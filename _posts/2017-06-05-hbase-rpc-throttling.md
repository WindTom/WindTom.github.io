---
layout: post
title: "HBase RPC throttling 源码分析"
description: ""
category: HBase
tags: [hbase,quotas,rpc]
---
* content
{:toc}


HBase客户端有对数据的请求和对表分裂等的请求，如果开启了Quotas功能，则会对这些请求的大小或者数量进行限制，也即RPC Throttling。






对数据请求的quotas限制在region server上进行。RSRpcServices收到RPC请求后，对于get,mutate,scan操作，其执行的RegionServerQuotaManager的checkQuota如下：

```java
  /**
   * Check the quota for the current (rpc-context) user. Returns the OperationQuota used to get the
   * available quota and to report the data/usage of the operation.
   * @param region the region where the operation will be performed
   * @param type the operation type
   * @return the OperationQuota
   * @throws ThrottlingException if the operation cannot be executed due to quota exceeded.
   */
  public OperationQuota checkQuota(final Region region, final OperationQuota.OperationType type)
      throws IOException, ThrottlingException {
    switch (type) {
    case SCAN:
      return checkQuota(region, 0, 0, 1);
    case GET:
      return checkQuota(region, 0, 1, 0);
    case MUTATE:
      return checkQuota(region, 1, 0, 0);
    default:
      throw new RuntimeException("Invalid operation type: " + type);
    }
  }
```
multi（multiple actions on a table: get, mutate, and/or execCoprocessor）操作，调用RegionServerQuotaManager的如下checkQuota方法进行检查：

```java
/**
   * Check the quota for the current (rpc-context) user. Returns the OperationQuota used to get the
   * available quota and to report the data/usage of the operation.
   * @param region the region where the operation will be performed
   * @param actions the "multi" actions to perform
   * @return the OperationQuota
   * @throws ThrottlingException if the operation cannot be executed due to quota exceeded.
   */
  public OperationQuota checkQuota(final Region region, final List<ClientProtos.Action> actions)
      throws IOException, ThrottlingException {
    int numWrites = 0;
    int numReads = 0;
    for (final ClientProtos.Action action : actions) {
      if (action.hasMutation()) {
        numWrites++;
      } else if (action.hasGet()) {
        numReads++;
      }
    }
    return checkQuota(region, numWrites, numReads, 0);
  }

```

我们可以看到上面两个方法都调用了同一个checkQuota方法，只是输入参数不同。这个方法才是核心所在：

```java
 /**
   * Check the quota for the current (rpc-context) user. Returns the OperationQuota used to get the
   * available quota and to report the data/usage of the operation.
   * @param region the region where the operation will be performed
   * @param numWrites number of writes to perform
   * @param numReads number of short-reads to perform
   * @param numScans number of scan to perform
   * @return the OperationQuota
   * @throws ThrottlingException if the operation cannot be executed due to quota exceeded.
   */
  private OperationQuota checkQuota(final Region region, final int numWrites, final int numReads,
      final int numScans) throws IOException, ThrottlingException {
    User user = RpcServer.getRequestUser(); //从RPC请求里获得user。
    UserGroupInformation ugi; 
    if (user != null) {   //如果RPC请求里没有User，是不是说明这个RPC是本地HBase Shell发出的？
      ugi = user.getUGI();  //如果用户存在，则获取user和group的对应信息
    } else {
      ugi = User.getCurrent().getUGI(); //如果用户不存在，则获取当前用户组信息。如果还是没有，返回空
    }
    TableName table = region.getTableDesc().getTableName();

    OperationQuota quota = getQuota(ugi, table); //拿到limiter，作为参数新建一个OperationQuota。OperationQuota有两种，一种是DefaultOperationQuota，另一种是NoopOperationQuota（也就是不做Quota限制）
    try {
      quota.checkQuota(numWrites, numReads, numScans);
    } catch (ThrottlingException e) {
      LOG.debug("Throttling exception for user=" + ugi.getUserName() + " table=" + table
          + " numWrites=" + numWrites + " numReads=" + numReads + " numScans=" + numScans + ": "
          + e.getMessage());
      throw e;
    }
    return quota;
  }
}
```

从上面代码中可以看到，这里最重要的两个方法就是getQuota和checkQuota。我们先看getQuota做了什么：

```java
  /**
   * Returns the quota for an operation. 这个方法的作用就是拿到limiter后将其作为参数新建一个OperationQuota。
   * @param ugi the user that is executing the operation
   * @param table the table where the operation will be executed
   * @return the OperationQuota
   */
  public OperationQuota getQuota(final UserGroupInformation ugi, final TableName table) {
    if (isQuotaEnabled() && !table.isSystemTable()) { //从这里我们看到系统表是不受Quota限制的
      UserQuotaState userQuotaState = quotaCache.getUserQuotaState(ugi); 
      QuotaLimiter userLimiter = userQuotaState.getTableLimiter(table);
      boolean useNoop = userLimiter.isBypass();
      if (userQuotaState.hasBypassGlobals()) { //异常情况判断，如果是为这个用户设置了byPass（也就是这个用户不受Quotas限制），默认使用NoopOperationQuota。
        if (LOG.isTraceEnabled()) {
          LOG.trace("get quota for ugi=" + ugi + " table=" + table + " userLimiter=" + userLimiter);
        }
        if (!useNoop) {                        //但是，如果不想使用NoopOperationQuota，那么就新建一个DefaultOperationQuota。
          return new DefaultOperationQuota(userLimiter);
        }
      } else { // 正常情况下
        QuotaLimiter nsLimiter = quotaCache.getNamespaceLimiter(table.getNamespaceAsString());
        QuotaLimiter tableLimiter = quotaCache.getTableLimiter(table);
        useNoop &= tableLimiter.isBypass() && nsLimiter.isBypass(); //这三者如果有一个为假，那useNoop就是假
        if (LOG.isTraceEnabled()) {
          LOG.trace("get quota for ugi=" + ugi + " table=" + table + " userLimiter=" + userLimiter
              + " tableLimiter=" + tableLimiter + " nsLimiter=" + nsLimiter);
        }
        if (!useNoop) {
          return new DefaultOperationQuota(userLimiter, tableLimiter, nsLimiter);
        }
      }
    }
    return NoopOperationQuota.get(); //如果是系统表就不做限制
  }
```
简单解释一下代码就是：首先，从Region Server的quotaCache中拿到关于此用户的UserQuotaState。然后从UserQuotaState中提取QuotaLimiter。至于UserQuotaState和QuotaLimiter的详细解释请见附录1和附录2。最后都是新建一个OperationQuota返回。

再来看DefaultOperationQuota的checkQuota方法，（NoopOperationQuota是个空架子，什么都不做）：

```java
@Override
  public void checkQuota(int numWrites, int numReads, int numScans) throws ThrottlingException {
    writeConsumed = estimateConsume(OperationType.MUTATE, numWrites, 100); //估计写操作大小
    readConsumed = estimateConsume(OperationType.GET, numReads, 100);      
    readConsumed += estimateConsume(OperationType.SCAN, numScans, 1000);  //读操作大小包括get操作和scan操作

    writeAvailable = Long.MAX_VALUE;
    readAvailable = Long.MAX_VALUE;
    for (final QuotaLimiter limiter : limiters) {  //哈哈，这些limiter就是getQuota方法中拿到的那些limiter。
      if (limiter.isBypass()) continue; //异常情况，先不管它

      limiter.checkQuota(writeConsumed, readConsumed); // 看看大小是不是超过了这个limiter的限制。这里limiter只有NoopQuotaLimiter和TimeBasedLimiter
      readAvailable = Math.min(readAvailable, limiter.getReadAvailable()); //当前读存量有多少
      writeAvailable = Math.min(writeAvailable, limiter.getWriteAvailable()); //当前写存量有多少
    }

    for (final QuotaLimiter limiter : limiters) {
      limiter.grabQuota(writeConsumed, readConsumed);
    }
  }
```

这个方法很重要，因为不管是什么操作类型，都会经过这个方法进行判断。方法中，首先估计各个操作的大小，这个估计的方法很简单，如下面代码所示。不管操作类型是什么，计算方法都一样：
```java
private long estimateConsume(final OperationType type, int numReqs, long avgSize) {
    if (numReqs > 0) {
      return avgSize * numReqs; //该操作的平均大小乘以请求次数。
    }
    return 0;
  }
```

回到DefaultOperationQuota的checkQuota方法，最重要的是`limiter.checkQuota(writeConsumed, readConsumed)`这段代码。进入TimeBasedLimiter的checkQuota（）方法：

```java
@Override
  public void checkQuota(long writeSize, long readSize) throws ThrottlingException {
    if (!reqsLimiter.canExecute()) { // 首先检查是否有足够的资源执行request limiter
      ThrottlingException.throwNumRequestsExceeded(reqsLimiter.waitInterval());
    }
    if (!reqSizeLimiter.canExecute(writeSize + readSize)) {
      ThrottlingException.throwRequestSizeExceeded(reqSizeLimiter
          .waitInterval(writeSize + readSize));
    }

    if (writeSize > 0) {
      if (!writeReqsLimiter.canExecute()) {
        ThrottlingException.throwNumWriteRequestsExceeded(writeReqsLimiter.waitInterval());
      }
      if (!writeSizeLimiter.canExecute(writeSize)) {
        ThrottlingException.throwWriteSizeExceeded(writeSizeLimiter.waitInterval(writeSize));
      }
    }

    if (readSize > 0) {
      if (!readReqsLimiter.canExecute()) {
        ThrottlingException.throwNumReadRequestsExceeded(readReqsLimiter.waitInterval());
      }
      if (!readSizeLimiter.canExecute(readSize)) {
        ThrottlingException.throwReadSizeExceeded(readSizeLimiter.waitInterval(readSize));
      }
    }
  }
```

注意到，这个函数会把所有的limiter都检查一遍，但凡有一个Limiter不满足条件，就抛出异常。如果所有的判断条件都通过，下面就走到了`limiter.grabQuota(writeConsumed, readConsumed)`这一步。这个方法的详细代码如下：
```java
  /* Removes the specified write and read amount from the quota. 
   * At this point the write and read amount will be an estimate, that will be later adjusted with a consumeWrite()/consumeRead() call.
   * 意思就是说扣除掉估算的写大小和读大小，但是这是估算的大小，具体大小稍后会通过调用consumeWrite()和consumeRead()进行调整
   */
  @Override
  public void grabQuota(long writeSize, long readSize) {
    assert writeSize != 0 || readSize != 0;

    reqsLimiter.consume(1);
    reqSizeLimiter.consume(writeSize + readSize);

    if (writeSize > 0) {
      writeReqsLimiter.consume(1);
      writeSizeLimiter.consume(writeSize);
    }
    if (readSize > 0) {
      readReqsLimiter.consume(1);
      readSizeLimiter.consume(readSize);
    }
  }
```
我们看到quotastate和limiter之间的包含关系可以总结为下图，虽然这样表示可能不标准，但能够说明自上而下的包含关系

<img src="https://github.com/WindTom/imagestom/blob/master/quota-throttling.png?raw=true">


至此，各个操作的Quota检查就算完成了。等等，不是说还要调整真正的读写消费量吗？现在是时候看究竟是谁调用的RegionServerQuotaManager的checkQuota（）方法了。文章开头提到，RSRPCServices的get,mutate,scan和multi方法会调用checkQuota方法。拿get方法举例：

```java
  /**
   * Get data from a table.
   *
   * @param controller the RPC controller
   * @param request the get request
   * @throws ServiceException
   */
  @Override
  public GetResponse get(final RpcController controller,
      final GetRequest request) throws ServiceException {
    long before = EnvironmentEdgeManager.currentTime();
    OperationQuota quota = null;  // 创建一个Quota对象
    try {
      checkOpen();
      requestCount.increment();
      rpcGetRequestCount.increment();
      Region region = getRegion(request.getRegion());

      GetResponse.Builder builder = GetResponse.newBuilder();
      ClientProtos.Get get = request.getGet();
      Boolean existence = null;
      Result r = null;
      quota = getQuotaManager().checkQuota(region, OperationQuota.OperationType.GET);  // 执行Quota检查

      if (get.hasClosestRowBefore() && get.getClosestRowBefore()) {
        if (get.getColumnCount() != 1) {
          throw new DoNotRetryIOException(
            "get ClosestRowBefore supports one and only one family now, not "
              + get.getColumnCount() + " families");
        }
        byte[] row = get.getRow().toByteArray();
        byte[] family = get.getColumn(0).getFamily().toByteArray();
        r = region.getClosestRowBefore(row, family);
      } else {
        Get clientGet = ProtobufUtil.toGet(get);
        if (get.getExistenceOnly() && region.getCoprocessorHost() != null) {
          existence = region.getCoprocessorHost().preExists(clientGet);
        }
        if (existence == null) {
          r = region.get(clientGet);
          if (get.getExistenceOnly()) {
            boolean exists = r.getExists();
            if (region.getCoprocessorHost() != null) {
              exists = region.getCoprocessorHost().postExists(clientGet, exists);
            }
            existence = exists;
          }
        }
      }
      if (existence != null){
        ClientProtos.Result pbr =
            ProtobufUtil.toResult(existence, region.getRegionInfo().getReplicaId() != 0);
        builder.setResult(pbr);
      } else  if (r != null) {
        ClientProtos.Result pbr;
        RpcCallContext call = RpcServer.getCurrentCall();
        if (isClientCellBlockSupport(call) && controller instanceof PayloadCarryingRpcController
            && VersionInfoUtil.hasMinimumVersion(call.getClientVersionInfo(), 1, 3)) {
          pbr = ProtobufUtil.toResultNoData(r);
          ((PayloadCarryingRpcController) controller)
              .setCellScanner(CellUtil.createCellScanner(r.rawCells()));
          addSize(call, r, null);
        } else {
          pbr = ProtobufUtil.toResult(r);
        }
        builder.setResult(pbr);
      }
      if (r != null) {
        quota.addGetResult(r); // quota
      }
      return builder.build();
    } catch (IOException ie) {
      throw new ServiceException(ie);
    } finally {
      if (regionServer.metricsRegionServer != null) {
        regionServer.metricsRegionServer.updateGet(
          EnvironmentEdgeManager.currentTime() - before);
      }
      if (quota != null) {
        quota.close();  //quota
      }
    }
  }
```

这段代码很长，我们只看与Quota相关的部分。checkQuota部分不必再看，奇怪的是出现两句代码`quota.addGetResult(r)`和`quota.close()`，它们是做什么用的呢？先看`quota.addGetResult(r)`所调用的方法：
```java 
  @Override
  public void addGetResult(final Result result) {
    operationSize[OperationType.GET.ordinal()] += QuotaUtil.calculateResultSize(result);
  }
```
从代码里不难看出，这是在计算get操作所返回的结果的真正大小，也就是真正的消费量。

直接看`quota.close()`所调用的方法：
```java
  @Override
  public void close() {
    // Adjust the quota consumed for the specified operation
    long writeDiff = operationSize[OperationType.MUTATE.ordinal()] - writeConsumed;
    long readDiff = operationSize[OperationType.GET.ordinal()] +
        operationSize[OperationType.SCAN.ordinal()] - readConsumed;

    for (final QuotaLimiter limiter: limiters) {
      if (writeDiff != 0) limiter.consumeWrite(writeDiff);
      if (readDiff != 0) limiter.consumeRead(readDiff);
    }
  }
```
到这里就非常清楚了，最后进行的步骤就是把真正消费量和估计消费量之间的差额给补齐，多退少补。


# 附录

## 附录1：UserQuotaState

QuotaState(class)：In-Memory state of table or namespace quotas
UserQuotaState(继承QuotaState)：In-Memory state of the user quotas

## 附录2：QuotaLimiter

TimeBasedLimiter和NoopQuotaLimiter（空架子，什么都不做）实现了QuotaLimiter接口。

## 附录3：RateLimiter

FixedIntervalRateLimiter和AverageIntervalRateLimiter继承了RateLimiter(abstract class)。具体使用FixedIntervalRateLimiter还是AverageIntervalRateLimiter，看HBase的配置hbase.quota.rate.limiter，默认是AverageIntervalRateLimiter

NoopQuotaLimiter：当user/table没有关联limiter的时候使用。





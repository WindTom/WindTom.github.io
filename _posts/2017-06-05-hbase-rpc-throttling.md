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

```java


```

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

下面看checkQuota方法都做了什么












<div align="center"><table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td><img src=""></td>
<td><img src=""></td>
</tr>

<tr>
<td><p><small><b> </b></small></p></td>
<td><p><small><b> </b></small></p></td>
</tr>

<br><br></table></div>
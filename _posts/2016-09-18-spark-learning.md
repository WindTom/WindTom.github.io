---
layout: post
title: "learning spark"
description: ""
category: 数据挖掘
tags: [spark,hadoop]
---
* content
{:toc}

<div align="center">
<image src="http://www.360sdn.com/uploadfile/2015/0330/20150330011728778.jpg">
</div>

因为有RDD的抽象概念，spark更适合于迭代运算比较多的ML和DM运算，提供了多种数据操作模型，但是由于RDD的特性，Spark不适用于异步细粒度更新状态的应用，比如Web服务的存储或或者是增量的Web爬虫和索引。

### 部署方式

目前spark支持四种分布式部署方式，分别为：amazon ec2、standalone、spark on mesos、spark on YARN。

Amazon ec2模式需要运行在amazon ec2上，一般可以不关注。

standalone模式自带完整服务，可单独部署到一个集群上，无需以来其他资源管理系统。

spark on mesos模式是很多公司采用的模式，也是`官方推荐的模式`。目前spark运行在mesos上要比在YARN上更加灵活。目前在Spark on mesos环境中，用户可以选择两种调度模式：1）粗粒度模式（Coarse-grained Mode）2）细粒度模式（Fine-grained Mode）。

```
1)粗粒度模式

任务运行之前分配资源（内存、CPU等），运行时一直占用这些资源，结束后才回收。资源浪费严重。

2)细粒度模式

资源按需、动态分配。优点是便于资源控制和隔离，缺点是段作业运行延迟大。
```

Spark On YARN模式是一种`最有前景`的部署模式，但限于YARN自身的发展，目前仅支持粗粒度模式，不过这个已经在YARN的开发计划中了

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
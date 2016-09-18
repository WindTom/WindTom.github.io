---
layout: post
title: "learning spark"
description: ""
category: 数据挖掘
tags: [spark,hadoop]
---
* content
{:toc}


### 部署方式

目前spark支持三种分布式部署方式，分别为：standalone、spark on mesos、spark on YARN。

standalone模式自带完整服务，可单独部署到一个集群上，无需以来其他资源管理系统。

spark on mesos模式是很多公司采用的模式，也是`官方推荐的模式`。目前spark运行在mesos上要比在YARN上更加灵活。目前在Spark on mesos环境中，用户可以选择两种调度模式：1）粗粒度模式（Coarse-grained Mode）2）细粒度模式（Fine-grained Mode）。

`
1)粗粒度模式

任务运行之前分配资源（内存、CPU等），运行时一直占用这些资源，结束后才回收。资源浪费严重。

2)细粒度模式

资源按需、动态分配。优点是便于资源控制和隔离，缺点是段作业运行延迟大。
`


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
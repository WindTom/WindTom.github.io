---
layout: post
title: "HBase Compaction"
description: "hbase"
category: 
tags: [hbase,compaction]
---
* content
{:toc}


# 综述

hbase的compaction分minor compaction和major compaction。那么问题是如果设置中关闭了major compaction，还会有minor compaction吗？会有的





major compaction可以选择自动关闭。同时保留手动触发。

> **hbase.hregion.majorcompaction**
> 
> Time between major compactions, expressed in milliseconds. Set to 0 to disable time-based automatic major compactions. User-requested and size-based major compactions will still run. This value is multiplied by hbase.hregion.majorcompaction.jitter to cause compaction to start at a somewhat-random time during a given window of time



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
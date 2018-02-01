---
layout: post
title: "HBase Compaction"
description: "hbase"
category: 
tags: [hbase,compaction]
---
* content
{:toc}


## 综述

hbase的compaction分minor compaction和major compaction。那么问题是如果设置中关闭了major compaction，还会有minor compaction吗？答案是会有的。

## compaction触发时机

* region server定期检查，一般7天。compaction请求被提交给compactSplitThread
* client compaction请求，被提交给compactSplitThread
* memstore flush的时候，会判断是否需要compact




## major compaction
major compaction可以选择自动关闭。同时保留手动触发。

将hbase.hregion.majorcompaction默认7天执行一次major compaction。将其设为0可关闭major compaction，但是这并不表示从此不会发生major compaction了。在下述情况下,major compaction仍会发生：

* hbase shell命令 major_compact进行触发
* compact when file <= sum(smaller_files) * 'hbase.hstore.compaction.ratio'。这种情况是指选中的文件数量等于store中的文件数量时，会由minor compact升级为major_compact。
* major compact时间间隔到期：after (now - min(StoreFile.timestamp)) >"hbase.hregion.majorcompaction" + rand() *hbase.hregion.majorcompaction.jitter"

至于split，并不是设置了hbase.hregion.max.filesize（默认10G）为很大就保证不split了，需要有以下的算法，参见：

IncreasingToUpperBoundRegionSplitPolicy是0.94.0默认region split策略。这里的split有一个判断条件，先计算这tableRegionsCount（regionserver上的这个table的online的region个数）， 
然后循环计算此region的所有store是否太大，这是通过getSizeToCheck方法计算出一个size,若当前的store总大小大于这个值，则表示此region需要split. getSizeToCheck的计算方法首先判断tableRegionsCount是否等于0，若是则返回hbase.hregion.max.filesize ，若不是，则计算Math.min(getDesiredMaxFileSize(), this.flushSize * (tableRegionsCount * tableRegionsCount)。

# major compaction发生的时间

1. 系统默认每隔7天执行一次major compaction
2. 用户手动触发。

# compaction policy

compaction policy指的是选择多少store files进行compact，以及compaction是major compaction还是minor compaction。

在0.96之前只有一个compaction policy，即RatioBasedCompactionPolicy。新版本的默认policy是ExploringCompactionPolicy。

这二者的区别是：RatioBasedCompactionPolicy选择第一组满足标准的sotre files，而ExploringCompactionPolicy选择最佳的一组。

## ExploringCompactionPolicy

有些storefiles不在本policy的考虑之列：
* 比hbase.hstore.compaction.max.size值大的store file。
* bulk-load操作产生的Hfile，要排除这些hfiles需要开启参数hbase.mapreduce.hfileoutputformat.compaction.exclude

hfile的选择方法：

1. 列出所有的HFile.
2. 选择hbase.hstore.compaction.min（默认3个）个以上，hbase.hstore.compaction.max以下的位置上连续的hfile列为一组，
3. 意思就是选择出最合适的一组。如果选不出来，就用备用的那一组。

## RatioBasedCompactionPolicy Algorithm

如果想使用的这个policy，在配置文件中将hbase.hstore.defaultengine.compactionpolicy.class置为RatioBasedCompactionPolicy 。

## hfile的选择方法

1. 列出所有不在compaction队列中的hfile作为候选，这些hfile按照sequenceID排序。
2. 检查本policy算法是否stuck住了，如果是，则执行major compaction。
3. major compaction允许的最大hfile数量为hbase.hstore.compaction.max。超出这些数量，就会执行minor compaction。不过，如果是用户执行的major请求，则即使hfile的数量超标，也会执行。
4. 如果hfile数量少于hbase.hstore.compaction.min，则不会执行minor compaction。注意：即使只有一个hfile,major compaction也能执行。
5. 0.96以前的major compaction间隔默认是24小时，0.96之后的版本改为7天(+-4.8)小时。有4.8是因为不想让major compaction同时发生。


如果被stuck住了，怎么办？

major compaction 的功能是将所有的store file合并成一个，触发major compaction的可能条件有：major_compact 命令、majorCompact() API、region server自动运行（相关参数：hbase.hregion.majoucompaction 默认为24 小时、hbase.hregion.majorcompaction.jetter 默认值为0.2 防止region server 在同一时间进行major compaction）。

hbase.hregion.majorcompaction.jetter参数的作用是：对参数hbase.hregion.majoucompaction 规定的值起到浮动的作用，假如两个参数都为默认值24和0,2，那么major compact最终使用的数值为：19.2~28.8 这个范围。

minor compaction的运行机制要复杂一些，它由一下几个参数共同决定：

* hbase.hstore.compaction.min :默认值为 3，表示至少需要三个满足条件的store file时，minor compaction才会启动
* hbase.hstore.compaction.max 默认值为10，表示一次minor compaction中最多选取10个store file
* hbase.hstore.compaction.min.size 表示文件大小小于该值的store file 一定会加入到minor compaction的store file中
* hbase.hstore.compaction.max.size 表示文件大小大于该值的store file 一定会被minor compaction排除
* hbase.hstore.compaction.ratio 将store file 按照文件年龄排序（older to younger），minor compaction总是从older store file开始选择，如果该文件的size小于它后面hbase.hstore.compaction.max个store file size之和乘以该ratio，则该store file也将加入到minor compaction 中。

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
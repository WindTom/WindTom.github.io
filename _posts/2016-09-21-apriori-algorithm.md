---
layout: post
title: "Apriori算法详解"
description: ""
category: 数据挖掘
tags: [算法,数据挖掘]
---
* content
{:toc}

## 目录
 算法来源   
 算法思想  
 算法特点  
 算法举例  
 算法源码  
 算法改进  




## 算法来源

Apriori算法是关联规则算法的一种，由Rakesh Agrawal和Ramakrishnan Srikant两位博士在1994年提出的算法。

关联规则的目的是在一个数据集中找出项与项之间的关系，也被称为购物篮分析(Market Basket analysis)。

关于这个算法有一个非常有名的故事：“尿布和啤酒”。故事大致为：美国的妇女们经常嘱咐丈夫下班后给孩子买尿布，而丈夫们买尿布的同时会买啤酒。因此啤酒和尿布的销量双双增加。

## 算法思想

Apriori算法的功能是找出频繁项集，那如何定义频繁？这里涉及到两个概念：支持度和置信度。


>设有关联项 $$ A\rightarrow B $$
>
>支持度： $$ P(A\cup B) $$
>
>> 支持度表示A与B同时出现的概率。如果A与B同时出现的概率小，说明A与B的关系不大；如果同时出现的概率大，说明A与B总是相关的。
>>
>> 关联规则有绝对支持度（个数）和相对支持度之分（百分比） 
>
> 置信度： $$ P(B\mid A) $$
>
>>置信度表示当A出现时，B也出现的概率。置信度太低说明A与B的关系不大。如果置信度为100%，那么在卖A的时候一定要捆绑销售B。

如果存在一条关联规则，它的支持度和置信度都大于预先定义好的最小支持度和置信度，则称它为强关联规则。关联分析的主要目的就是寻找强关联规则。所谓频繁项集即是支持度大于最小支持度的项集。

Apriori算法包括连接步和剪枝步

1）连接步

为找出$$L_{k}$$ （所有的频繁K项集所组成的集合），首先将 $$L_{k-1}$$与自身连接产生候选的K项集集合，记为$$C_{k}$$。Apriori规定频繁项集的所有子集应该也是频繁项集。所以$$C_{k}$$集中，每一个K项集，如果其任一子集不在$$L_{k-1}$$中，则去掉这个K项集，最后得到一个更新后的$$C_{k}$$。

2）剪枝步

通过扫描所有样本（交易），确定$$C_{k}$$中每个候选项的计数。计数（或百分比）大于最小支持度的，称为频繁的，予以保留；计数小于最小支持度的则去除，最后得到K项频繁项集$$L_{k}$$。

## 算法特点

## 算法举例



## 算法优化

FP-tree

垂直数据分布

FP-growth

## 参考资料

1. [数据挖掘算法-Apriori Algorithm（关联规则）](http://www.cnblogs.com/gaizai/archive/2010/03/31/1701573.html)
2. [Apriori算法 ](http://blog.sina.com.cn/s/blog_6e85bf420100ogn2.html)
3. [关联分析之Apriori算法](http://blog.csdn.net/rongyongfeikai2/article/details/40457827)
4. [关联规则（一）Apriori算法](http://blog.sina.com.cn/s/blog_6a17628d0100v83b.html)




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
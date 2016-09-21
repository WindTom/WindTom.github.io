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

关联规则的目的是在一个数据集中找出项与项之间的关系，也被成为购物篮分析(Market Basket analysis)。

关于这个算法有一个非常有名的故事：“尿布和啤酒”。故事大致为：美国的妇女们经常嘱咐丈夫下班后给孩子买尿布，而丈夫们买尿布的同时会买啤酒。因此啤酒和尿布的销量双双增加。

## 算法思想

Apriori算法的功能是找出频繁项集，那如何定义频繁？这里涉及到两个概念：支持度和置信度。


>设有关联项 $$ A\rightarrow B $$
>
>支持度： $$ P(A\cup B) $$
>
>> 支持度表示A与B同时出现的概率。如果A与B同时出现的概率小，说明A与B的关系不大；如果同时出现的概率大，说明A与B总是相关的。
>
>置信度： $$P(B|A)$$
>
>>置信度表示当A出现时，B也出现的概率。置信度太低说明A与B的关系不大。如果置信度为100%，那么在卖A的时候一定要捆绑销售B。




## 算法优化

FP-tree

垂直数据分布

FP-growth

## 参考资料

1. [数据挖掘算法-Apriori Algorithm（关联规则）](http://www.cnblogs.com/gaizai/archive/2010/03/31/1701573.html)
2. [Apriori算法 ](http://blog.sina.com.cn/s/blog_6e85bf420100ogn2.html)
3. [关联分析之Apriori算法](http://blog.csdn.net/rongyongfeikai2/article/details/40457827)




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
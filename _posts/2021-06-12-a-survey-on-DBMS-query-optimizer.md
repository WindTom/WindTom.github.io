---
layout: post
title: "A Survey on Advancing the DBMS Query Optimizer: Cardinality
Estimation, Cost Model, and Plan Enumeration"
description: ""
category: DBMS
tags: [Database,paper]
---
* content
{:toc}


本文分析查询优化器的重要组成部分（cardinality estimation、cost model和plan enumeration）上的缺点，总结了当前学术界对这些问题的解决办法，并指出了将来的可能的研究方向。

# 1 为什么查询优化器的重要组件仍不准确

## 1.1 cardinality estimation

Lohman指出cost model能引入30%的错误率，但cardinality estimation很容易就引入数量级的错误率。

cardinality estimation主要在以下三种情况下引入错误：

1. **查询中关于单表的错误**。数据库常以直方图反应数据分布，但直方图并不能完全真实的数据分布，人们常常假设数据是均匀分布、且不同列间是相互独立的。当这些假设不成立的时候，估计值往往不准确，从而导致优化器产生次优的执行计划。为利用表中各列之间的关系，人们还提出Multi-histograms，但此类方法对空间占用比较大。
2. **多表join中的错误**。不同表上的列可能存在一定的关联关系。当前多表Join cardinality采用自下而上的计算方法，先从子孙节点开始计算，但容易造成错误率自下而上地传递和放大。
3. **用户自定义函数（UDF）中的错误**。当查询条件中存在UDF时，很难估计满足条件的元组个数。

## 1.2 Cost Model

cost-based optimizer是一个代价模型，输出某个query的cost，该cost是查询中所有算子的cost之和。

单个算子的cost跟这些因子有关：

1. 所在系统的硬件
2. 算子的实现方式
3. 待该算子处理的行数
4. 当前数据库状态（buffer中数据、并发queries）

因此，当组合这些因子来计算cost时首先需要确定组合方法中的很多参数值。此外，当把数据库系统部署在分布式环境、云或跨平台查询引擎上时，cost model的复杂度还会剧增。而且，即使cardinality是真实值，一个query的cost estimation与运行时间并不成线性关系，可能造成优化器选择次优计划。

## 1.3 Plan enumeration

plan enumeration用来从众多计划中选择一个最优执行计划，这被证明是一个NP难问题。当面临多表join的查询时，穷举可能的执行计划对于大型数据库来说是个十分困难的事情。搜索空间中 的join树有可能是zigzag树，左深树、右深树、bushy tree，或者它们的子集。不同的系统join tree形式也不同。

传统数据库有三种枚举方法：

1. 动态规划的方法，自下而上join枚举，如System R
2. 自上而下join枚举， 如Volcano/Cascades
3. 随机方法，如PostgreSQL中的遗传算法。

Plan Enumeration当前有三个局限：

1. cardinality estimation和cost model中的错误。
2. prune 搜索空间的规则
3. query涉及的表非常多的时候。当表非常多的时候，优化器就得做出牺牲，采用启发式方法，确保优化时间在合理范围内，如PostgreSQL中的遗传算法、DB2中的谈心算法。这往往导致优化器选择次优计划。

应该注意到，cardinality estimation的错误会传导至cost model，导致次优计划。因此，构建一个好的优化器的前提是先解决cardinality estimation中的错误。

# 2. cardinality estimation

当前，cardinality estimation方法有三类。如图2所示。

![image-20210612181956242](C:\Users\W.J\AppData\Roaming\Typora\typora-user-images\image-20210612181956242.png)

## 2.1 Synopsis-based methods

引入新的数据结构存储统计信息。Histogram和sketch是其中应用最为广泛的方法。

### 2.1.1 Histogram

两种类型：1维和d维（d>=2）。

2003以后，直方图方面的研究可以划分为3类：

1. 寻找更快构建直方图的方法
2. 新的bucket划分方法以求更准确
3. 给予query feedback的直方图构建方法

### 2.1.2 scketch

Sketch将一个列作为一个矢量或矩阵来计算不同值的数量或元组频率

## 2.2 sampling-based methods



## 2.3 Learning‑Based Methods

有监督方法

无监督方法







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
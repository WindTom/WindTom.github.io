---
layout: post
title: "个性化推荐和排序算法"
description: ""
category: 数据挖掘
tags: [数据挖掘,排序算法,推荐算法]
---
* content
{:toc}


## 推荐算法



## 排序算法

LearningToRank

LDA(Latent Dirichlet Allocation)话题模型

## 关联分析中的置信度、支持度和提升度<sup>[[1]](http://www.360doc.com/content/15/0611/19/25802092_477451393.shtml)</sup>


## 权重系数的确定方法

综合评价中，根据样本数据的有无，可将计算方法分为定量和定性两大类。

**定量**的方法有：1）熵值法；2）灰色关联度法；3）主成分分析法；4）人工神经网络定权法；5）因子分析法；6）回归分析法 等。

**定性**的方法有：1）德尔菲法；2）层次分析法；3）模糊聚类法；4）比重法 等。

### 主成分分析法

#### 基本原理

通过用一些较小的新的数量指标（因子）代替原来较多的指标，这些新指标是原来指标的线性组合，并且能充分载有原来指标的信息，起到降维的作用，而且指标间不相关。可用新指标对原来信息的反映程度作为权。该方法客观性强，避免了人为赋权所造成的偏差。缺点是新指标不可能完全反映原来指标的信息，有一定的偏差，适用于有数据的样本。

#### 计算方法

1. 原始指标数据的标准化采集p维随机向量<img src="http://latex.codecogs.com/png.latex?x%3D%28x_%7B1%7D%2Cx_%7B2%7D%2C...%2Cx_%7Bp%7D%29%5E%7BT%7D">
2. gongshi
3. 
3. xiam \\(x=(x_{1},x_{2},...,x_{p})^{T}\\)
$$(x=(x_{1},x_{2},...,x_{p})^{T}$$

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}

$$

上面是公式

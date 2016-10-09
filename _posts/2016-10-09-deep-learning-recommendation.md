---
layout: post
title: "基于深度学习思想的个性化推荐系统和框架"
description: ""
category: 数据挖掘
tags: [deep learning,recommendation]
---
* content
{:toc}

个性化推荐将是深度学习最重要的应用之一





original article from [here](https://www.quora.com/Has-there-been-any-work-on-using-deep-learning-for-recommendation-engines)

正如[Suvash Sedhain](https://www.quora.com/profile/Suvash-Sedhain) 提到，深度学习最近被应用到了个性化推荐。他在[NIPs](http://machinelearning.wustl.edu/mlpapers/paper_files/NIPS2013_5004.pdf)提到的工作被用于Spotify的一个实验中（见[Recommending music on Spotify with deep learning](http://benanne.github.io/2014/08/05/spotify-cnns.html)）。最近Spotify上还有一个实验将Recurrent Neural Networks用于基于协同过滤的个性化推荐（[Recurrent Neural Networks for Collaborative Filtering](https://www.quora.com/Has-there-been-any-work-on-using-deep-learning-for-recommendation-engines)）。同样在音乐领域，谷歌发布了使用深度学习方法学习标签用于推荐的方法（[Page on ismir.net](http://www.ismir2011.ismir.net/papers/PS6-13.pdf)）

在其他领域，当我在Netflix的时候，我们描述了一个分布式ANN结构([distributed Neural Networks with GPUs in the AWS Cloud](http://techblog.netflix.com/2014/02/distributed-neural-networks-with-gpus.html))

---
最近出现的方法有使用Deep content based的**音乐推荐方法**（[发表于NIPs2013](http://machinelearning.wustl.edu/mlpapers/paper_files/NIPS2013_5004.pdf)）。该方法的主要思想是首先用传统矩阵因子化（Matix factorization）方法学习生成用户和物品因子，然后学习一个深度卷积网络，将音频特征作为输入，对应的物品潜在因子作为输出。所以，他们是使用深度卷积网络学习一个将物品内容特征映射到MF潜在因子的方法。最后，还是按照标准MF方法进行推荐（物品和用户潜在因子的乘积）。这种方法在**冷启动**时非常有效，冷启动时仅有很少的针对物品的用户行为，物品潜在因子可以从其内容中生成。

有篇[文章](http://www.cs.utoronto.ca/~hinton/absps/netflixICML.pdf)提到在Netflix prize数据集上使用受限的Boltzmann machines。该文“future extension”章节讨论了在Netflix prize数据集上使用深度模型的几种可能方法。此外，Edwin Chen在[博客](http://blog.echen.me/2011/07/18/introduction-to-restricted-boltzmann-machines/)中详细介绍了一个基于RBM的协同过滤推荐方法。

Chris Nicholson开发了[deeplearning4j](http://deeplearning4j.org/welldressed-recommendation-engine.html)项目。

Domonkos Tikk最近出品一篇[RNN session-based 推荐算法](https://arxiv.org/abs/1511.06939)，提交给ICLR 2016。他将RNN应用于实际推荐系统，使用的数据为短期，而不是长期历史数据。这是一个典型的冷启动问题，算法经证明较矩阵因子化更为精确。

这里有两篇文章([Page on ucsc.edu](http://classes.soe.ucsc.edu/cmps290c/Spring13/proj/kailu_report.pdf),[Page on arxiv.org](https://arxiv.org/pdf/1409.2944v1.pdf))将深度学习和RBM（restricted Boltzmann machine）应用在协同推荐里。

Ali Mamdouh Elkahky将[深度学习用于微软的新闻和Apps推荐](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=KB3S8RoAAAAJ&citation_for_view=KB3S8RoAAAAJ%3AIjCSPb-OGe4C)，效果较好。

---

## 谷歌新开源--宽度&深度学习框架：结合记忆和归纳实现更优推荐

人类的大脑能够通过记忆日常生活的种种事件形成规则，并归纳这些学习以应用到我们从未见过的事务上。当我们探索推荐机器只能的方法

将深度神经网络（用于归纳）与宽线性模型（用于记忆）联合进行训练，对带有稀疏输入的一般大规模回归和分类问题（带有大量可能特征值的类别特征）很有用，比如推荐系统、搜索和排名问题。

对推荐系统而言，记忆形成和归纳两者都非常重要，使用产品间的特征转换，宽线性模型能有效记忆稀疏特征交互，同时深度神经网络能通过低维度嵌入推广到先前未见过的特征交互。宽度&深度学习框架结合了这两类模型的优势。在大型的商业应用商店Google Play推荐系统上对这一框架进行了投入产品的测试以及评估。在线测试结果显示宽度&深度学习模型相比于单一的宽度或者深度模型，在应用购买上有了极大的改进。

谷歌将其宽度&深度学习实现方法作为Tensor Flow Learn API的一部分进行了开源。

源码地址：[地址1](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/learn/python/learn)

教程地址：

[地址2](https://www.tensorflow.org/tutorials/wide/)
[地址3](https://www.tensorflow.org/tutorials/wide_and_deep/)

---
作者：谷文栋
链接：https://www.zhihu.com/question/20830906/answer/104390007

毋庸质疑，推荐系统将成为 Deep Learning 最重要的应用领域之一。

也许是最早的，算是与 Deep Learning 沾点儿边的推荐算法，是在 Netflix Prize 竞赛后半程异军突起的`Restricted Boltzmann Machine(RBM)`算法。当时以 SVD++ 为核心的模型几乎已经陷入了僵局，大家基本进入到了比拼trick与融合模型数量的体力活阶段了。RBM 的出现推动整个竞赛上了一个新台阶，相关的论文「Restricted Boltzmann Machines for Collaborative Filtering」在此<sup>[1]</sup>。但 RBM 本身大家并不认为和 Deep Learning 有太大关系，因为它太“浅”了，官方论文里面最后也提到了RBM一个重要的扩展方向就是"Learning Deep Generative Models"。Netflix 在2014年发表了一篇结合使用GPU和AWS搞分布式神经网络的博客`"Distributed Neural Networks with GPUs in the AWS Cloud"` <sup>[2]</sup>，昭告了一下除 Google 之外他们在 deep learning领域也不容小觑，然而并没有透露太多的应用细节。再多说几句 Netflix Prize，有关 Netflix Prize 对 Netflix Recommendation 带来的改变，可以看看 Netflix 自己的官方博客<sup>[3][4]</sup>，这几乎也可以看做是推荐系统领域最佳的入门资料。`关于RBM，对工业界同学们更具参考价值的是Edwin Chen的这篇"Introduction to Restricted Boltzmann Machines"` <sup>[5]</sup>，更浅显易懂，还有开源代码实现。然后就是最近，有人号称使用 deep learning取得了比Netflix Prize大奖方案`更好的结果` <sup>[6]</sup>。

其他一些值得看看的内容： 
   
`音乐`是deep learning适合发挥优势的领域之一，与Spotify相关的deep learning应用有两篇报导，一篇是"Recommending music on Spotify with deep learning" <sup>[7]</sup> 很详尽，另外一篇是把deep learning与经典的collaboritive filtering结合的尝试，"Recurrent Neural Networks for Collaborative Filtering"[8]。

Google 也发表了一篇音乐相关的 deep learning 论文，"Temporal Pooling and Multiscale Learning for Automatic Annotation and Ranking of Music Audio" <sup>[9]</sup>。
Netflix 的一位算法研究员作为作者之一的"Session-based Recommendations with Recurrent Neural Networks"[10]。

基于 deeplearning4j 的一个推荐引擎"The WellDressed Recommendation Engine"[11]，据说，使用了这个玩意儿的电商网站把 ad coverage 提升了200%。

微软同学在 WWW2015 上的一篇文章，"A Multi-View Deep Learning Approach for Cross Domain User Modeling in Recommendation Systems"[12]，讲在新闻和应用推荐领域使用deep learning的一些心得。

最后重磅推荐，Netflix 前推荐引擎总监 Xavier Amatriain 在 KDD2014 上的压轴分享，「The Recommender Problem Revisited」[13]，得认真啃一啃。

今年的 RecSys 会议，将会第一次专门针对 Deep Learning 与 Recommender Systems 设立一个专门的 workshop，将于今年9月15日在 Boston 举办，[14]这里是一些方向性的题目，有启发。

另外，就是 Amazon 刚刚宣布开源了 DSSTNE -- Deep Scalable Sparse Tensor Network Engine，DSSTNE 是 Amazon 用来`开发深度学习模型的一套框架`，Amazon 最新的推荐引擎就在使用 DSSTNE。我在知乎专栏里发表了一篇相关介绍`"关于Amazon开源的深度学习框架DSSTNE"`，看这一篇就够了！ - ResysChina - 知乎专栏」

参考资料：

[1] [cs.utoronto.ca/~hinton](https://link.zhihu.com/?target=http%3A//www.cs.utoronto.ca/%7Ehinton/absps/netflixICML.pdf)   
[2] [techblog.netflix.com/2014](https://link.zhihu.com/?target=http%3A//techblog.netflix.com/2014/02/distributed-neural-networks-with-gpus.html)    
[3] [techblog.netflix.com/2012](https://link.zhihu.com/?target=http%3A//techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html)     
[4] [techblog.netflix.com/2012](https://link.zhihu.com/?target=http%3A//techblog.netflix.com/2012/06/netflix-recommendations-beyond-5-stars.html)    
[5] [Introduction to Restricted Boltzmann Machines](https://link.zhihu.com/?target=http%3A//blog.echen.me/2011/07/18/introduction-to-restricted-boltzmann-machines/)           
[6] [Deep learning solution for netflix prize](https://link.zhihu.com/?target=https%3A//karthkk.wordpress.com/2016/03/22/deep-learning-solution-for-netflix-prize/)               
[7] [Recommending music on Spotify with deep learning](https://link.zhihu.com/?target=http%3A//benanne.github.io/2014/08/05/spotify-cnns.html)         
[8] [erikbern.com/wordpress](https://link.zhihu.com/?target=http%3A//erikbern.com/wordpress.php%3Fp%3D589)                 
[9] [ismir2011.ismir.net](https://link.zhihu.com/?target=http%3A//ismir2011.ismir.net/papers/PS6-13.pdf)    
[10] [arxiv.org/abs/1511.06939](https://link.zhihu.com/?target=http%3A//arxiv.org/abs/1511.06939)             
[11] [The WellDressed Recommendation Engine](https://link.zhihu.com/?target=http%3A//deeplearning4j.org/welldressed-recommendation-engine.html)       
[12] [msr-waypoint.com/pubs](https://link.zhihu.com/?target=http%3A//msr-waypoint.com/pubs/238334/frp1159-songA.pdf)     
[13] [The Recommender Problem Revisited](https://link.zhihu.com/?target=http%3A//www.kdd.org/kdd2014/tutorials/KDD)            
[14] [Call for papers](https://link.zhihu.com/?target=http%3A//dlrs-workshop.org/dlrs-2016/cfp/)
              












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
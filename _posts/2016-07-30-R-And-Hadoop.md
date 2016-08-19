---
layout: post
title:  "数据挖掘平台-R语言与Hadoop"
categories: 数据挖掘
tags: R Hadoop 大数据 数据挖掘
---
* content  
{:toc}

**（注：本文资料来源于互联网）**

## 几种流行的开源数据挖掘平台

### R
[R](http://www.r-projectorg)是用于**统计分析**和**图形化**的计算机语言及分析工具。Ｒ支持一系列分析技术，包括统计检验，预测建模，数据可视化等等。在官网上能找到众多的开源扩展包。




### Weka
[Weka](http://www.cs.waikato.ac.nz/ml/weka/)可能是名气最大的开源机器学习和数据挖掘软件。高级用户可以通过Java编程和命令行来调用其分析组件。同时Weka也提供图形化界面。和R相比，Weka在统计分析方面较弱，但在机器学习方面强得多。在[Weka论坛](http://weka.wikispaces.com/Related+Projects)可以找到很多扩展包，包括文本挖掘、可视化、网格计算等等。很多其它开源数据挖掘软件也支持调用Weka的分析功能。

### Tanagra
[Tanagra](http://eric.univ-lyon2.fr/~ricco/tanagra/en/tanagra.html)是使用图形界面的数据挖掘软件，采用了树状结构来组织分析组件。Tanagra缺乏高级的可视化能力，但它的强项是统计分析，提供了众多的有参和无参检验方法，同时它的特征选取方法也很多。

### RapidMiner

[RapidMiner](https://rapidminer.com/)是预测性分析和数据挖掘软件，使用Java语言开发，具有拖曳操作、无需编程、运算速度快的特点，有开源和商业两种版本。RapidMiner可以被用来搭建推荐系统和评论挖掘系统，它有一些很有用的扩展包。比如，推荐系统扩展包rmx_irbrecommender-ANY-5.0.4.jar可以直接实现基于内容的和基于协同过滤的推荐系统。信息抽取扩展包rapidminer-Information-Extraction-1.0.2.jar可以用于实现特征和观点词的提取，若再配合RapidMiner提供的文本分类功能，应该可以实现一个评论挖掘原型系统（作者xiaoli的观点，她同时提供了用RapidMiner搭建推荐系统和评论挖掘系统的[具体方法](http://blog.sina.com.cn/s/blog_73de143c010156sb.html)。同时她的博客也提出了一些关于学习推荐系统应该思考的问题）。

### KNIME
[KNIME](http://www.knime.org/)是基于Eclipse开发环境精心开发的数据挖掘工具。工具无需安装，也是基于Java开发，可以扩展使用Weka中的挖掘算法。和YALE不同的是，KNIME采用的是类似数据流的方式建立分析挖掘流程。挖掘流程由一系列功能节点组成，每个节点有输入/输出端口（port），用于接收数据或模型、导出结果。[网上文章](http://www.360doc.com/content/11/0212/19/5696310_92510422.shtml)指出KNIME比Weka的KnowledgeFlow更好用，连接节点时很方便，直接用鼠标拖拽连接端口即可。而Weka中则需要在节点上按鼠标右键，再选择后续节点，比较麻烦。

### Orange
[Orange](http://orange.biolab.si/)是类似KNIME和Weka KnowledgeFlow的数据挖掘工具，它的图形环境称为Orange画布(OrangeCanvas)，用户可以在画布上放置分析控件，然后把控件连接起来组成挖掘流程。Orange的强项在于提供了大量可视化方法，可以对数据和模型进行多种图形化展示，并能智能搜索合适的可视化形式，支持对数据的交互式探索。弱项在于传统统计分析能力不强，不支持统计检验，报表能力也有限。Orange允许用户使用Python脚本语言进行扩展开发。

### GGobi
用于交互式可视化的开源软件，使用Brushing的方法。GFobi可以用作R软件的插件，或者通过Perl,Python等脚本语言来调用。网上文章(链接见上文)推荐使用KNIME，同时安装Weka和R扩展包。

## R语言大数据挖掘应用 

使用R语言在Hadoop平台上进行分析，可采用的方式有：
Ｒ语言的RJDBC包链接Hive传递SQL进行数据的查询和导入，得到的数据在R语言里面进行数据预处理于算法建模，再把分析模型的结果存储到本地的MySQL，然后提供作为PHP的数据查询，从而让PHP进行分析挖掘后台的数据可视化处理。这样就形成了一条“可视化数据挖掘闭环”(Visual Data Mining Closed Loop)

### R语言是否适用企业级大数据挖掘应用

作者[刘思喆](http://www.bjt.name/)在文章[R语言企业级大数据挖掘应用](http://www.thebigdata.cn/JieJueFangAn/12031.html)中提出一个看似另类的解决方案——在企业级数据挖掘中使用R语言＋Hadoop的方式。

之所以使用R语言是因为开源软件可以根据业务的变化进行调整，但商业软件往往很难做到。

使用R语言进行大数据处理需要注意到是避免使用C++或Java式的编程思想，而是要使用类似于matlab的`向量化`编程思想。

R语言能够处理GB级数据，但是底层数据是PB级应该怎么办？刘给出了如下图示：

<div align="center">
<img src="http://www.thebigdata.cn/upload/2014-10/141008110780412.png" >
<br>
</div>

从图上看出，针对不同业务场景的数据集市，通过数据清洗已经下降到TB级，再往上是针对特定任务的分析和挖掘模块，数据被整理到GB级，这时候R语言就能够比较方便地处理了。

### Hadoop家族

Hadoop的家族成员：Hive, HBase, Zookeeper, Avro, Pig, Ambari, Sqoop, Mahout, Chukwa

**Hive**: 是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。

**Pig**: 是一个基于Hadoop的大规模数据分析工具，它提供的SQL-LIKE语言叫Pig Latin，该语言的编译器会把类SQL的数据分析请求转换为一系列经过优化处理的MapReduce运算。

**HBase**: 是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，利用HBase技术可在廉价PC Server上搭建起大规模结构化存储集群。

**Sqoop**: 是一个用来将Hadoop和关系型数据库中的数据相互转移的工具，可以将一个关系型数据库（MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。

**Zookeeper**: 是一个为分布式应用所设计的分布的、开源的协调服务，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，简化分布式应用协调及其管理的难度，提供高性能的分布式服务

**Mahout**: 是基于Hadoop的机器学习和数据挖掘的一个分布式框架。Mahout用MapReduce实现了部分数据挖掘算法，解决了并行挖掘的问题。

**Avro**: 是一个数据序列化系统，设计用于支持数据密集型，大批量数据交换的应用。Avro是新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制

**Ambari**: 是一种基于Web的工具，支持Hadoop集群的供应、管理和监控。

**Chukwa**: 是一个开源的用于监控大型分布式系统的数据收集系统，它可以将各种各样类型的数据收集成适合 Hadoop 处理的文件保存在 HDFS 中供 Hadoop 进行各种 MapReduce 操作。

### R语言如何与Hadoop结合？

作者[张丹](http://www.36dsj.com/archives/6468)提到，当前已经有很多比较好的结合点出现。

1. RHadoop。
RHadoop是Hadoop与R语言结合的开源产品。RHadoop包含三个R包 (rmr，rhdfs，rhbase)，分别对应Hadoop系统架构中的MapReduce, HDFS, HBase三个部分。

2. RHiveRHive是一款通过R语言直接访问Hive的工具包，由韩国公司NexR研发。

3. 重写Mahout。用R语言重写Mahout的实现也是一种结合的思路，我也做过相关的尝试。

4. Hadoop调用R。上面说的都是R如何调用Hadoop，当然我们也可以反相操作，打通JAVA和R的连接通道，让Hadoop调用R的函数。但是，这部分还没有商家做出成形的产品。

5. R和Hadoop在实际中的案例

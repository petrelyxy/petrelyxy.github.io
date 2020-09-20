	layout: post
	title: "MapReduce"
	date:  2020-03-26 21:20:20 +0800
	desc: "keep up"
	keywords: ""
	categories: [bigdata]
	tags: [bigtable,MapReduce,distributed system]
	icon: icon-html
# MapReduce:Simplified Data Processing on Large Clusters

## 摘要

MapReduce是一种处理大规模数据集的编程模型及实现。用户需要实现一个 ***Map***函数处理key/value对并产生另一个key/value的中间集，并实现一个 ***reduce***函数来将这些中间数据根据相同的key进行合并。按照这种编程风格写出的程序能够在大型的商用集群上被并行执行。由运行的系统来处理分割输入数据、在集群机器间调度任务、处理机器失败和管理集群内部机器的数据交互等细节。这使得没有任何并行、分布式系统处理经验的编程人员也能够简单地使用大型分布式系统资源。

## 1 Introduction

略



## 2 Programming Model

计算接受key/value对的输入集并产生key/value对的输出集。MapReduce library的用户通过两个函数来表示这样的计算：Map 和 Reduce  

  *Map*： 由用户编写，接受一个输入的key/value pair并产生一个key/value pair的中间结果集合。MapReduce将这些有着相同中间key *I*  的中间value聚集在一起并传递给Reduce函数处理。

  *Reduce*: 同样由用户编写，接受一个中间key *I* 和一个与这个key相关的value的集合。reduce将这些value合并并由此组成一个可能规模更小的value集合。通常来说一个Reduce函数产生0或1个输出value。中间value通过迭代器传递给reduce函数来应对value数据集过大时不能存储在内存中的情况。



## 3 Implementation

根据使用场景及部署环境的不同，可以有许多对应的实现。这一章介绍针对google计算环境的实现：由交换以太网相互连接的商用PC的大规模集群。

1. 每台机器内存约2-4GB
2. 集群内网络环境良好
3. machine failure应当被考虑
4. 存储介质廉价。因此建立在不可靠硬件上的file system通过副本的方式来提供高可用、可靠性。
5. 用户将计算jobs提交给一个调度系统。每一个job由一系列的task组成并通过调度器分配给集群内的可用机器。

### 3.1 Execution Overview


---
layout: post
title: "分布式项目相关"
date:  2017-07-06 14:30:20 +0800
desc: "Always be fimiliar with these basic skills"
keywords: "Distributed, ZooKeeper, Duobbo"
categories: [Java]
tags: [Distributed, ZooKeeper, Duobbo]
icon: icon-html
---
## 什么是分布式？ （一切从定义出发）
  A distributed system is one in which components located at networked computers communicate and coordinate actions only by passing messages.
分布式系统:分布在网络连接计算上的组件仅通过传递消息来运行和协同。  可以近似它成多机的多进程。分布式系统整体从外部来看就像是一个超级计算机。

## 网络通信基础知识
  TCP/IP,七层网络模型，网络IO实现方式：BIO(Blocking IO),NIO(Nonblocking IO,Reactor模式),AIO(AsynchronousIO,异步IO)
  session sticky和session数据集中存储
  读写分离：读多写少，数据读压力大
  读源：不仅仅是数据库，例如可以是搜索引擎建立的索引，读源如何与主库保持数据一致，什么时候走搜索，什么时候走数据库。构建搜索用的索引的过程就是一个数据复制的过程。

## 搜索引擎
构建索引的方式划分：1.全量/增量 （全量：第一次建立索引，可能是新建也可能是重建。增量：在全量的基础上持续更新索引。）
		    2.实时/非实时
## 缓存
数据缓存：主要用于分担读的压力，目的上看类似分库和搜索引擎。存放“热”数据。
页面缓存

## 消息中间件
异步和解耦

远程过程调用和对象访问中间件：
消息中间件：
数据访问中间件

ThreadPoolExecutor

动态代理 反射机制 

LVS:linux virtual server

网络通信，异步调用：序列化、反序列化，NIO，**Future**，Future可以用于避免等待和合并运算(避免重复计算)。。

服务框架可以使应用从集中式走向分布式，解决了当网站功能越来越丰富时，单个应用越来越庞大的问题(服务拆分，将**为多方提供公共功能**的拆分为服务)。

负载均衡方式：随机，轮询，权重。在机器性能相对均衡时，随机和轮询相对容易实现且效率不错，在机器性能不均衡时，权重可以较好地平衡负载提升效率。

## 互联网应用的性质决定了分布式架构势在必行。
  1. 高访问量 --> 高并发性，高负载量 -->负载均衡
  2. 可扩展性 --> 注重项目结构, 接口等定义是非常重要的
  3. 

## ZooKeeper
  服务提供者和服务消费者通过注册中心交互，通过注册中心实现了服务提供者和服务消费者之间的解耦。显然这也是一个生产者-消费者模式。

## 负载均衡
硬负载均衡：硬件实现
软负载均衡：hash，
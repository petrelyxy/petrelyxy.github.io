---
layout: post
title: "开源监控:skywalking笔记"
date:  2020-09-20 09:53:20 +0800
desc: "skywalking整体框架介绍，作为目录指引。"
keywords: "anime"
categories: [application]
tags: [apm, monitoring, agent, moduleDefine]
icon: icon-html
---



---

# SkyWalking

开源的apm监控系统， 设计参照open tracing实现。 年初的时候，由于组内需要实现全链路追踪， 根据skywalking源码做了部分改造来部署， 能达到预期效果。 做一些笔记做备忘录和后续看书指引。

推荐解析：http://www.iocoder.cn/categories/SkyWalking/

## 整体架构

agent(sniffer) -> collector(stream processing) -> data storage（various  storage system available）

1. 客户端：即被监控应用， 使用java instrumentation、java agent机制来做数据传输类和方法的增强(代理模式)，通过增强插件采集客户端的请求信息。

2. 服务端:收集处理客户端上传的请求信息，并进行链路关系、拓扑关系、异常信息等分析。

   

## 代码结构

客户端：插件扫描加载、模块(插件)定义、请求对象、信息上传(grpc+protobuf)。

服务端： 解析处理信息。

优先看插件扫描、插件定义(模块定义)以及java instrumentation和java agent的代理增强思想。

基于skywalking 8.0 版本

客户端主要模块为apm-sniffer：以下都在apm-sniffer子模块中查找

1. 入口为模块apm-agent的SkyWalkingAgent类的premain方法。
2. 链路信息的内部抽象在apm-agent-core模块下的context包下面。
3. 插件定义在apm-sdk-plugin， 有大多数常见的组件插件。【可插拔式插件以及对模块的定义(各插件模块下resource文件中的skywalking-plugin.def)】

服务端看一下以及ModuleManager（org.skywalking.apm.collector.core.module.ModuleManager）、oap-server模块下的server-receiver-plugin模块下的skywalking-trace-receiver-plugin中的模块定义方式，即ModuleDefine、ModuleProvider。

## 值得看的点

java instrumentation、java agent、代理模式、open tracing、 grpc+protobuf、spi机制(Service Provider Interface)
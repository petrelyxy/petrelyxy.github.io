---
layout: post
title: "notes of the world beyond batch and streaming 102"
date:  2020-03-12 22:34:20 +0800
desc: "keep up"
keywords: "streaming,robust out-of order data processing, beam"
categories: [Java]
tags: [streaming, big data]
icon: icon-html
---
[https://www.oreilly.com/radar/the-world-beyond-batch-streaming-102/](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-102/ "the world beyond batch and streaming 102")的一些笔记



数据处理博客系列的第二部分：[the-world-beyond-batch-streaming-102/](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-102/ "the world beyond batch and streaming 102")的一些笔记,原文中有大量动图，建议阅读原文，本文只做概述参考。简单回顾一下上一部分，作者主要讨论了三个主题：

1. **术语**：准确定义了作者认为的“streaming”等术语具体指的是什么
2. **batch versus streaming**: 比较了这两类系统理论上的能力范围，由此推论出streaming想要取代现有的batch系统只需要两件事：**correctness** and **tools for reasoning about time**.
3. **数据处理模式**：介绍了batch与streaming系统在处理有界及无界数据时的基本方法。

本文作者将重点放在数据处理模式上。在上一篇文章101中作者介绍了两个处理无界数据集时相关的基本概念。第一个是**event time与processing time的重要区别**, 第二个则是 **windowing**(根据时间边界来划分数据集)。 本文将会在这两个概念的基础上更进一步，详细地讨论三个概念：

1.  **watermarks**: 在某一个event time的输入已经完成了标志。一个带有时间X值的watermark表示“所有event time小于X的事件都已经被观测到了”。因此，watermark被用作观测无界数据的进度指标。
2. **Triggers**：trigger是根据某些外部信号来判断宣告一个window的输出应该被实化(materialized)的机制。trigger提供了选择什么时候输出应该被产生时的灵活性。也使得一个不断演变(evolves)的窗口的输入能够被观测多次。这使得窗口结果值能够随时间推进被完善(后文会讲到late data),当数据到达时产生推测结果，并处理随时间的上游数据的变化，还能够处理相对watermark的late data(如移动设备场景，某人的手机在offline状态下记录了他的大量相关活动数据及事件时间， 然后在重新连接之后将这些数据上传处理)。
3. **Accumulation**：Accumulation的模式指明同一个窗口被观测到的多个结果值之间的关系。这些结果可能是完全独立无关的，也可能结果之间会有交叠。不同的accumulation模式有着不同的语义与不同的计算代价。

为了更好地理解这些概念之间的关系，作者提出四个处理无界数据的重要问题，通过回答这四个问题来进一步展开（what、where、when、how）。

- ***<u>What</u>* results are calculated？**  这个问题的答案取决于处理流水线中transformations的类型。这个问题涵盖了例如计算求和、求直方图(building histograms)、训练机器学习模型等。这也是传统批处理系统所解决的问题。
- ***<u>Where</u>* in event time are results calculated?**  这个问题由处理线对event-time windowing的使用方式决定。包含了前文streaming 101中所提到的常用windowing(fix, slidiing与sessions)、没有windowing相关表示的一些使用场景(例如streaming 101中的time-agnostic处理， 经典的批处理也在这类中)、一些更复杂的windowing(例如time-limited auctions)。需要注意的是这其中也可能包含有processing-time windowing， 比如将记录进入系统的时间作为记录的event time的情景。
- ***<u>When</u>* in processing time are results materialized? ** 这个问题取决与对watermark及triggers的使用。在这个主题上可以有无穷的变化，但最常用的模式是使用watermark来标志什么时候一个给定窗口的输入已经完整了，使用triggers来推论early results(在窗口完成前推论产生部分结果)和late results(用于watermark只是输入完整性的估计， 当watermark已经声明给定窗口的数据已经完整后又更多的输入数据到达的场景)。
- ***<u>How</u>* do redinements of results relate?** 这个问题由使用的accumulation模式决定：discarding(用于results之间独立无关的场景)、accumulating(后续的results依赖与之前的)、accumulating and retracting(这种场景下accumulating值和上一个触发值的retraction都会被产生)。

## Streaming 101 redux

### **What** ： transformations

经典的批处理中使用的transformations给出了”**What results are calculated**“?的答案， 在这一章节，作者将介绍一个简单的例子：computing keyed integer sums over a simple data set consisting of 10 values. 可以把它想象为通过对队员的得分求和来计算一只队伍的游戏总分。作者使用Dataflow的伪代码来描述处理过程, 所以先解释Dataflow中的两个基本原语

- **PCollections**, 表示数据集， parallel transformations可以在PCollections上执行
- **PTransforms**, 作用在PCollections上来产生新的PCollections.
- 






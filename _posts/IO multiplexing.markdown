---
layout: post
title: "I/O multiplexing "
date:  2021-10-20 22:26:20 +0800
desc: "Always be familiar with these basic skills"
keywords: "I/O"
categories: [network]
tags: [IO,job]
icon: icon-html
---

See**《UNIX Network Programming》volume 1 ,Chapter 6.2.** 

I/O models:

1. BIO(Blocking IO): 阻塞IO

2. NIO(Non-Blocking IO): 非阻塞IO
3. I/O multiplexing :IO多路复用， 使用的系统调用：select、 poll、epoll
4. signal driven I/O：
5. asynchronous I/O: 与同步IO的重要区别： **回调通知**



数据准备的**两个阶段**：

1. wait for the data to be ready;
2. copying the data from the kernel to the process.

For an input operation on a socket, the first step normally involves waiting for data to
arrive on the network. When the packet arrives, it is copied into a buffer within the kernel.
The second step is copying this data from the kernel's buffer into our application buffer.



POSIX defines these two terms as follows:
 A synchronous I/O operation **causes the requesting process to be blocked until that**
**I/O operation completes.**
 An asynchronous I/O operation **does not cause the requesting process to be blocked**.
Using these definitions, the first four I/O models blocking, nonblocking, I/O multiplexing,
and signal-driven I/O are all synchronous because the actual I/O operation **(recvfrom)**
**blocks the process**. Only the asynchronous I/O model matches the asynchronous I/O
definition.
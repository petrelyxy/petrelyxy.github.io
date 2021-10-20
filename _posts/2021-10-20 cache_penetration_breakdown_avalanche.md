---
layout: post
title: "缓存雪崩、击穿、穿透"
date:  2021-10-20 21:30:20 +0800
desc: "Always be fimiliar with these basic skills"
keywords: "cache"
categories: [Java]
tags: [cache, redis, job]
icon: icon-html
---



参考文章:[What is cache penetration, cache breakdown and cache avalanche?](https://www.pixelstech.net/article/1586522853-What-is-cache-penetration-cache-breakdown-and-cache-avalanche)

这三个问题都是在缓存中没有找到请求值，因此将请求压力给到了底层的数据库，可能导致数据库承受过大的压力。

## 缓存雪崩（Cache avalanche）

大量的缓存数据同时过期失效或者缓存服务突然宕机， 此时如果有大量的对这些过期数据的请求就会将压力给到数据库。

对应方案：

1. 不要将大量缓存的过期时间设置在同一时间，可以加一些随机的delay
2. 缓存的高可用方案， 以redis举例， 使用redis集群模式而不是单个redis实例。
3. 使用一些限流和熔断机制来保护数据库， 如hystrix

## 缓存击穿(Cache breakdown)

缓存数据失效的同时有大量对该失效缓存数据进行请求， 这些请求将直接击中数据库。这种情况通常在高并发情况下发生。

对应方案：

1. 热点数据最好不要失效，视业务场景：如果可以接受一些数据更新延时，可以使用一个维护线程周期更新缓存数据， 或者直接不过期。
2. 对搜索的key加锁，这样就只有一个请求会打到数据库层。当数据更新后释放锁，其他请求就能获取到缓存数据了。

## 缓存穿透（Cache penetration）

请求的数据在底层数据库不存在，因此在缓存中当然也就不存在，每次请求都会直接打到数据库。 通常这样的请求由攻击者发起。

对应方案：

1. 接口层鉴权校验，做好安全防范。
2. 使用bloom filter来做存在性校验， 如果key对应的数据在数据库中存在则请求，否则就直接返回。
3. 从缓存中没有拿到的数据，在数据库层也没有渠道，这是也可以短暂的将key-val设为key-null,设一个短暂的过期时间。

## 缓存一致性

如何保证缓存与数据库的数据一致性。

这里显然是针对更新操作的， 简单的方案就是对于更新操作，先删除对应的缓存数据再更新数据库。   
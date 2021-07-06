# Redis

## 什么是Redis？

REmote DIctionary Server(Redis) 是一个由 Salvatore Sanfilippo 写的 **key-value 存储系统**，是**跨平台的非关系型数据库**。

Redis 是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库，并提供多种语言的 API。

Redis 通常被称为数据结构服务器，因为值（value）可以是字符串(String)、哈希(Hash)、列表(list)、集合(sets)和有序集合(sorted sets)等类型。

## nosql介绍

以前的一个基本网站访问量一般不大，服务器根本没有太大压力，单个数据库完全足够。
由于一般的系统任务中通常不会存在高并发的情况，所以这样看起来并没有什么问题，可是一旦涉及大数据量的需求，比如一些商品抢购的情景，或者是主页访问量瞬间较大的时候，单一使用数据库来保存数据的系统会因为面向磁盘，磁盘读/写速度比较慢的问题而存在严重的性能弊端，一瞬间成千上万的请求到来，需要系统在极短的时间内完成成千上万次的读/写操作，这个时候往往不是数据库能够承受的，极其容易造成数据库系统瘫痪，最终导致服务宕机的严重生产问题。

为了克服上述的问题，Java Web项目通常会引入NoSQL技术，这是一种基于内存的数据库，并且提供一定的持久化功能。

Redis和MongoDB是当前使用最广泛的NoSQL，而就Redis技术而言，它的**性能十分优越**，可以支持**每秒十几万此的读/写操作**，其性能远超数据库，并且还支持集群、分布式、主从同步等配置，原则上可以无限扩展，让更多的数据存储在内存中，它还支持一定的事务能力，这保证了高并发的场景下数据的安全和一致性。

### Redis 在 Java Web 主要有两个应用场景：

- 存储 **缓存** 用的数据；
- 需要高速读/写的场合**使用它快速读/写**；


## Redis安装
**不推荐在Win系统下安装**

下载地址：https://github.com/tporadowski/redis/releases

Redis官网（英文）：https://redis.io/

Redis官网（中文）：http://www.redis.cn/

## Redis基础知识

redis默认有16个数据库

redis默认端口为6379

默认使用的是第0个

可以使用`select`进行切换数据库

`keys *`查看所有的key

`flushdb`清空当前库

`flushall`清空所有库

> redis是单线程的

redis是基于内存操作，cpu不是redis的瓶颈，redis的瓶颈是根据机器的内存和网络带宽决定的（6.0版本为多线程）

##　五大数据类型

### Redis-key

```redis
keys *              # 查看所有的key

set [key] [value]   # 设置k-v

exists [key]        # 判断当前key是否存在

move [key]          # 移除当前的key

expire [key]        # 设置key的过期时间，单位：秒

ttl [key]           # 查看当前key的剩余时间

type [key]          # 查看key的类型
```

### String(字符串)
```redis
append [key] [" "]      # 追加字符串，如果当前key不存在，则相当于setkey

strlen [key]            # 获取字符串长度

setex 					# 设置过期时间

setnx 					# 不存在在设置（分布式锁）
```

## Jedis

> 使用Java操作redis


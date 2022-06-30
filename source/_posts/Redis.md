---
abbrlink: eef5bf5d
title: Redis学习笔记
date: 2022-05-22
updated: 
categories: Redis
tags: Redis
---

# Redis学习笔记

## 零. 前言

### 基于教程

[【尚硅谷】Redis 6 入门到精通 超详细 教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Rv41177Af)（讲师是王泽老师，视频录制时间为2021年4月份）

### 参考笔记

[Redis - tsuiraku](https://tsuiraku.com/2021/08/19/redis/)

### 官网资料

Redis官方网站：https://redis.io/（注意是`.io`不是`.com`，后者是Redis Cloud的官网）

Redis中文官方网站：http://redis.cn/

* [Redis文档中心](http://redis.cn/documentation.html)
  * [Redis 数据类型](http://redis.cn/topics/data-types.html)
  * [Redis 数据类型介绍](http://redis.cn/topics/data-types-intro.html)

* [Redis命令中心](http://redis.cn/commands.html)
  * http://redis.cn/commands.html#string

### 环境版本

* Redis For Linux 6.2.1
* CentOS 7.9.2009

## 一. NoSQL数据库简介

### 1.1 编程技术的分类

![不同类型的技术](Redis/不同类型的技术.png)

**Redis**就是一种典型的NoSQL数据库。

### 1.2 为什么需要NoSQL数据库

传统的网络应用：客户端 --> 单体的应用服务器（CPU和内存压力） --> 单体的数据库服务器（IO压力）

现在的网络应用：负载均衡器+**服务器集群**

此时如何解决**不同服务器之间的数据共享问题**？比如说如何共享同一用户的session？

* 方案1：将数据存储到客户端cookie中
  * 缺点：不安全
* 方案2：session复制
  * 缺点：session数据冗余。如果集群中有N个服务器，1份数据就要占用N份内存空间，服务器越多，空间浪费越大。
* 方案3：独立的文件服务器或数据库服务器
  * 缺点：IO效率是瓶颈

* 方案4：独立的NoSQL数据库服务器

  * 优点1：数据存放在内存中，不需要经过IO。起到类似缓存的作用，故又称“缓存数据库”。

  * 优点2：缓解原来应用服务器的CPU和内存压力。1份数据只需要占用1份内存空间
  * 优点3：缓解原来数据库服务器的IO压力。普通的数据库，即“SQL数据库”，做的是磁盘的读写操作，而NoSQL数据库的操作是内存级别的，不需要经过IO，速度极快。

[为什么要用Redis？Redis为什么这么快？ - 知乎](https://zhuanlan.zhihu.com/p/81195864)

> 一篇非常通俗易懂的文章，关于Redis入门你需要了解的所有东西，尽在此文，小白必读！

### 1.3 NoSQL数据库概述

NoSQL(non-relational / Not Only SQL )，意即"不仅仅是SQL"，泛指非关系型的数据库。

NoSQL的存储不依赖于业务逻辑，而是以简单的key-value模式存储。

* 不遵循SQL标准
* 不支持跨多个数据元素的ACID事务
* 提供远超于SQL的性能

适用场景：

* 数据的高并发读写
* 海量数据的读写
* 要求具备高可扩展性TODO

### 1.4 NoSQL数据库比较

#### Memcache

![Memcache](Redis/Memcache.png)

#### Redis

![Redis](Redis/Redis.png)

#### MongoDB

![MongoDB](Redis/MongoDB.png)

#### 数据库排行榜

来自网站：https://db-engines.com/en/ranking
根据流行度进行排名，每月实时更新。

> 截图中没有出现Memcache是因为此时它排在30名开外。

![数据库排行榜](Redis/数据库排行榜.png)

### 1.5 行式数据库与列式数据库

[一图理解行式数据库和列式数据库的区别 - 简书](https://www.jianshu.com/p/ad2533e5cfaa)

## 二. 初识Redis

### 2.1 在Linux环境中安装Redis

> 如果想在Windows环境安装Redis，直接从[Releases · tporadowski/redis](https://github.com/tporadowski/redis/releases)下载一份解压版即可

#### 安装命令

下载redis-6.2.1.tar.gz放到/opt目录下，然后打开终端：

```shell
#
cd /opt
# 确认是否已经安装了gcc
gcc -version
# 安装gcc（GNU Compiler Collection，GNU编译器套件）
yum -y install gcc
# 检查gcc是否安装成功
gcc -version
# 解压
tar -zxvf redis-6.2.1.tar.gz
#
cd redis-6.2.1
# 编译
make
# 安装
make install
# 默认安装路径为/usr/local/bin
[root@hadoop100 bin]# cd /usr/local/bin
[root@hadoop100 bin]# ls
redis-benchmark  redis-check-rdb  redis-sentinel
redis-check-aof  redis-cli        redis-server
```

#### 安装目录

Redis安装目录下都有些什么？

![Redis默认安装目录](Redis/Redis默认安装目录.png)

### 2.2 启动Redis

#### 前台启动方式

```shell
redis-server
```

不推荐。独占一个Shell，无法运行别的命令（对于图形化界面，关闭终端窗口后Redis无法继续运行）

#### 后台启动方式

准备工作：

```shell
# 复制redis.conf
cp /opt/redis-6.2.1/redis.conf /etc/redis.conf
#
cd /etc
# 将配置文件中的daemonize no改为daemonize yes（守护线程）
vim redis.conf
```

启动：

```shell
redis-server /etc/redis.conf
```

进入redis命令行客户端程序：

```shell
redis-cli
```

### 2.3 关闭Redis

> 前提当然是以“后台启动方式”启动的

* 直接执行`redis-cli shutdown`

* 在Redis命令行中执行`shutdown`
* 先通过`ps -ef | grep redis`查看进程信息，再根据进程号，执行`kill PID`直接杀死进程

## 三. 常用5大数据类型

### 3.1 Redis键(key)基本操作

以下操作均在redis命令行内进行

| 命令     | 参数        | 作用                                                         | 示例    |
| -------- | ----------- | ------------------------------------------------------------ | ------- |
| keys     | pattern     | 查看当前库所有匹配pattern的key                               | keys *  |
| exists   | key         | 判断key是否存在                                              | key k1  |
| type     | key         | 查看key的类型                                                | type k1 |
| del      | key         | 删除指定的key数据                                            |         |
| unlink   | key         | 根据value选择非阻塞删除，仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作 |         |
| expire   | key seconds | 为key设置过期时间second，单位为秒                            |         |
| ttl      | key         | 查看key的过期时间（time to live），返回-1代表                |         |
| select   | index       | 切换数据库（默认共16个，下标从0到15）                        |         |
| dbsize   | 无          | 查看当前库的key的数量                                        |         |
| flushdb  | 无          | 清空当前库                                                   |         |
| flushall | 无          | 通杀全部库                                                   |         |

#### UNLINK key [key ...]

来源：http://redis.cn/commands/unlink.html

> **起始版本：4.0.0**
>
> **时间复杂度：**O(1) for each key removed regardless of its size. Then the command does O(N) work in a different thread in order to reclaim memory, where N is the number of allocations the deleted objects where composed of.
>
> 该命令和`DEL`十分相似：删除指定的key(s),若key不存在则该key被跳过。但是，相比`DEL`会产生阻塞，该命令会在另一个线程中回收内存，因此它是非阻塞的。 这也是该命令名字的由来：仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。
>
> **返回值**
>
> [integer-reply](http://redis.cn/topics/protocol.html#integer-reply)：unlink的keys的数量.

#### EXPIRE key seconds

来源：http://redis.cn/commands/expire.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 设置`key`的过期时间，超过时间后，将会自动删除该`key`。在Redis的术语中一个`key`的相关超时是不确定的。
>
> 超时后只有对`key`执行[DEL](http://redis.cn/commands/del.html)命令或者[SET](http://redis.cn/commands/set.html)命令或者[GETSET](http://redis.cn/commands/getset.html)时才会清除。 这意味着，从概念上讲所有改变`key`的值的操作都会使他清除。 例如，[INCR](http://redis.cn/commands/incr.html)递增key的值，执行[LPUSH](http://redis.cn/commands/lpush.html)操作，或者用[HSET](http://redis.cn/commands/hset.html)改变hash的`field`所有这些操作都会触发删除动作。
>
> 使用[PERSIST](http://redis.cn/commands/persist.html)命令可以清除超时，使其变成一个永久的`key`。
>
> 如果`key`被[RENAME](http://redis.cn/commands/rename.html)命令修改，相关的超时时间会转移到新`key`上面。
>
> 如果`key`被[RENAME](http://redis.cn/commands/rename.html)命令修改，比如原来就存在`Key_A`,然后调用`RENAME Key_B Key_A`命令，这时不管原来`Key_A`是永久的还是设置为超时的，都会由`Key_B`的有效期状态覆盖。
>
> ##### 刷新过期时间
>
> 对已经有过期时间的`key`执行`EXPIRE`操作，将会更新它的过期时间。有很多应用有这种业务场景，例如记录会话的session。
>
> **返回值**
>
> [integer-reply](http://redis.cn/topics/protocol.html#integer-reply), 具体的:
>
> - `1` 如果成功设置过期时间。
> - `0` 如果`key`不存在或者不能设置过期时间。

#### TTL key

来源：http://redis.cn/commands/ttl.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 返回key剩余的过期时间。 这种反射能力允许Redis客户端检查指定key在数据集里面剩余的有效期。
>
> 在Redis 2.6和之前版本，如果key不存在或者已过期时返回`-1`。
>
> 从Redis2.8开始，错误返回值的结果有如下改变：
>
> - 如果key不存在或者已过期，返回 `-2`
> - 如果key存在并且没有设置过期时间（永久有效），返回 `-1` 。
>
> 另见[PTTL](http://redis.cn/commands/pttl.html)命令返回相同的信息，只不过他的时间单位是毫秒（仅适用于Redis 2.6及更高版本）。
>
> **返回值**
>
> [Integer reply](http://redis.cn/topics/protocol.html#integer-reply)： key有效的秒数（TTL in seconds）,或者一个负值的错误 (参考上文)。

### 3.2 Redis字符串(String)

#### 简介

来源：http://redis.cn/topics/data-types.html

> 字符串是一种最基本的Redis值类型。Redis字符串是二进制安全的，这意味着一个Redis字符串能包含任意类型的数据，例如： 一张JPEG格式的图片或者一个序列化的Ruby对象。
>
> 一个字符串类型的值最多能存储512M字节的内容。
>
> 你可以用Redis字符串做许多有趣的事，例如你可以：
>
> - 利用INCR命令簇（[INCR](http://redis.cn/commands/incr.html), [DECR](http://redis.cn/commands/decr.html), [INCRBY](http://redis.cn/commands/incrby)）来把字符串当作原子计数器使用。
> - 使用[APPEND](http://redis.cn/commands/append.html)命令在字符串后添加内容。
> - 将字符串作为[GETRANGE](http://redis.cn/commands/getrange.html) 和 [SETRANGE](http://redis.cn/commands/setrange.html)的随机访问向量。
> - 在小空间里编码大量数据，或者使用 [GETBIT](http://redis.cn/commands/getbit.html) 和 [SETBIT](http://redis.cn/commands/setbit.html)创建一个Redis支持的Bloom过滤器。
>
> 查看所有可用的[字符串命令](http://redis.cn/commands.html#string)获取更多信息, 或者进一步阅读 [Redis数据类型介绍](http://redis.cn/topics/data-types-intro.html).

#### 数据结构

String底层的数据结构为简单动态字符串（Simple Dynamic String, SDS），类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。

扩展阅读：[Redis内部数据结构详解(2)——sds - 铁蕾的个人博客](http://zhangtielei.com/posts/blog-redis-sds.html)

#### SET key value [EX seconds] [PX milliseconds] [NX|XX]

来源：http://redis.cn/commands/set.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> Set `key` to hold the string `value`. If `key` already holds a value, it is overwritten, regardless of its type. Any previous time to live associated with the key is discarded on successful `SET` operation.
>
> 将键`key`设定为指定的“字符串”值。
>
> 如果 key 已经保存了一个值，那么这个操作会直接覆盖原来的值，并且忽略原始类型。
>
> 当`set`命令执行成功之后，之前设置的过期时间都将失效
>
> ##### 选项
>
> 从2.6.12版本开始，redis为`SET`命令增加了一系列选项:
>
> - `EX` *seconds* – Set the specified expire time, in seconds.
> - `PX` *milliseconds* – Set the specified expire time, in milliseconds.
> - `NX` – Only set the key if it does not already exist.
> - `XX` – Only set the key if it already exist.
> - `EX` *seconds* – 设置键key的过期时间，单位时秒
> - `PX` *milliseconds* – 设置键key的过期时间，单位时毫秒
> - `NX` – 只有键key不存在的时候才会设置key的值
> - `XX` – 只有键key存在的时候才会设置key的值
>
> **注意:** 由于`SET`命令加上选项已经可以完全取代[SETNX](http://redis.cn/commands/setnx.html), [SETEX](http://redis.cn/commands/setex.html), [PSETEX](http://redis.cn/commands/psetex.html)的功能，所以在将来的版本中，redis可能会不推荐使用并且最终抛弃这几个命令。
>
> ##### 返回值
>
> [simple-string-reply](http://redis.cn/topics/protocol.html#simple-string-reply):如果`SET`命令正常执行那么回返回`OK`，否则如果加了`NX` 或者 `XX`选项，但是没有设置条件。那么会返回nil。

#### GET key

来源：http://redis.cn/commands/get.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 返回`key`的`value`。如果key不存在，返回特殊值`nil`。如果`key`的`value`不是string，就返回错误，因为`GET`只处理string类型的`values`。
>
> ##### 返回值
>
> [simple-string-reply](http://redis.cn/topics/protocol.html#simple-string-reply):key对应的value，或者nil（key不存在时）

#### APPEND key value

来源：http://redis.cn/commands/append.html

> **起始版本：2.0.0**
>
> **时间复杂度：**O(1)。均摊时间复杂度是O(1)， 因为redis用的动态字符串的库在每次分配空间的时候会增加一倍的可用空闲空间，所以在添加的value较小而且已经存在的 value是任意大小的情况下，均摊时间复杂度是O(1) 。
>
> 如果 `key` 已经存在，并且值为字符串，那么这个命令会把 `value` 追加到原来值（value）的结尾。 如果 `key` 不存在，那么它将首先创建一个空字符串的`key`，再执行追加操作，这种情况 [APPEND](http://redis.cn/ommands/append.html) 将类似于 [SET](http://redis.cn/ommands/set.html) 操作。
>
> ##### 返回值
>
> [Integer reply](http://redis.cn/topics/protocol.html#integer-reply)：返回append后字符串值（value）的长度。

#### STRLEN key

来源：http://redis.cn/commands/strlen.html

> **起始版本：2.2.0**
>
> **时间复杂度：**O(1)
>
> 返回key的string类型value的长度。如果key对应的非string类型，就返回错误。
>
> ##### 返回值
>
> [integer-reply](http://redis.cn/topics/protocol#integer-reply)：key对应的字符串value的长度，或者0（key不存在）

#### INCR key

来源：http://redis.cn/commands/incr.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 对存储在指定`key`的数值执行原子的加1操作。
>
> 如果指定的key不存在，那么在执行incr操作之前，会先将它的值设定为`0`。
>
> 如果指定的key中存储的值不是字符串类型（fix：）或者存储的字符串类型不能表示为一个整数，
>
> 那么执行这个命令时服务器会返回一个错误(eq:(error) ERR value is not an integer or out of range)。
>
> 这个操作仅限于64位的有符号整型数据。
>
> **注意**: 由于redis并没有一个明确的类型来表示整型数据，所以这个操作是一个字符串操作。
>
> 执行这个操作的时候，key对应存储的字符串被解析为10进制的**64位有符号整型数据**。
>
> 事实上，Redis 内部采用整数形式（Integer representation）来存储对应的整数值，所以对该类字符串值实际上是用整数保存，也就不存在存储整数的字符串表示（String representation）所带来的额外消耗。
>
> ##### 返回值
>
> [integer-reply](http://redis.cn/topics/protocol.html#integer-reply):执行递增操作后`key`对应的值。

#### DECR key

来源：http://redis.cn/commands/decr.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 对key对应的数字做减1操作。如果key不存在，那么在操作之前，这个key对应的值会被置为0。如果key有一个错误类型的value或者是一个不能表示成数字的字符串，就返回错误。这个操作最大支持在64位有符号的整型数字。
>
> 查看命令[INCR](http://redis.cn/commands/incr.html)了解关于增减操作的额外信息。
>
> ##### 返回值
>
> 数字：减小之后的value

#### INCRBY key increment

来源：http://redis.cn/commands/incrby.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 将key对应的数字加decrement。如果key不存在，操作之前，key就会被置为0。如果key的value类型错误或者是个不能表示成数字的字符串，就返回错误。这个操作最多支持64位有符号的正型数字。
>
> 查看命令[INCR](http://redis.cn/commands/incr.html)了解关于增减操作的额外信息。
>
> ##### 返回值
>
> [integer-reply](http://redis.cn/topics/protocol.html#integer-reply)： 增加之后的value值。

#### DECRBY key decrement

来源：http://redis.cn/commands/decrby.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 将key对应的数字减decrement。如果key不存在，操作之前，key就会被置为0。如果key的value类型错误或者是个不能表示成数字的字符串，就返回错误。这个操作最多支持64位有符号的正型数字。
>
> 查看命令[INCR](http://redis.cn/commands/incr.html)了解关于增减操作的额外信息。似。
>
> ##### 返回值
>
> 返回一个数字：减少之后的value值。

#### MSET key value [key value ...]

来源：http://redis.cn/commands/mset.html

> **起始版本：1.0.1**
>
> **时间复杂度：**O(N) where N is the number of keys to set.
>
> 对应给定的keys到他们相应的values上。`MSET`会用新的value替换已经存在的value，就像普通的[SET](http://redis.cn/commands/set.html)命令一样。如果你不想覆盖已经存在的values，请参看命令[MSETNX](http://redis.cn/commands/msetnx.html)。
>
> `MSET`是原子的，所以所有给定的keys是一次性set的。客户端不可能看到这种一部分keys被更新而另外的没有改变的情况。
>
> ##### 返回值
>
> [simple-string-reply](http://redis.cn/topics/protocol.html#simple-string-reply)：总是OK，因为MSET不会失败。

#### MGET key [key ...]

来源：http://redis.cn/commands/mget.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(N) where N is the number of keys to retrieve.
>
> 返回所有指定的key的value。对于每个不对应string或者不存在的key，都返回特殊值`nil`。正因为此，这个操作从来不会失败。
>
> ##### 返回值
>
> [array-reply](http://redis.cn/topics/protocol.html#array-reply): 指定的key对应的values的list

#### GETRANGE key start end

来源：http://redis.cn/commands/getrange.html

> **起始版本：2.4.0**
>
> **时间复杂度：**O(N) N是字符串长度，复杂度由最终返回长度决定，但由于通过一个字符串创建子字符串是很容易的，它可以被认为是O(1)。
>
> **警告**：这个命令是被改成GETRANGE的，在小于2.0的Redis版本中叫SUBSTR。 返回key对应的字符串value的子串，这个子串是由start和end位移决定的（两者都在string内）。可以用负的位移来表示从string尾部开始数的下标。所以-1就是最后一个字符，-2就是倒数第二个，以此类推。
>
> 这个函数处理超出范围的请求时，都把结果限制在string内。
>
> **返回值**
>
> [bulk-reply](http://redis.cn/topics/protocol.html#bulk-reply)

#### SETRANGE key offset value

来源：http://redis.cn/commands/setrange.html

> **起始版本：2.2.0**
>
> **时间复杂度：**O(1), not counting the time taken to copy the new string in place. Usually, this string is very small so the amortized complexity is O(1). Otherwise, complexity is O(M) with M being the length of the value argument.
>
> 这个命令的作用是覆盖key对应的string的一部分，从指定的offset处开始，覆盖value的长度。如果offset比当前key对应string还要长，那这个string后面就补0以达到offset。不存在的keys被认为是空字符串，所以这个命令可以确保key有一个足够大的字符串，能在offset处设置value。
>
> 注意，offset最大可以是229-1(536870911),因为redis字符串限制在512M大小。如果你需要超过这个大小，你可以用多个keys。
>
> **警告**：当set最后一个字节并且key还没有一个字符串value或者其value是个比较小的字符串时，Redis需要立即分配所有内存，这有可能会导致服务阻塞一会。在一台2010MacBook Pro上，set536870911字节（分配512MB）需要～300ms，set134217728字节(分配128MB)需要～80ms，set33554432比特位（分配32MB）需要～30ms，set8388608比特（分配8MB）需要8ms。注意，一旦第一次内存分配完，后面对同一个key调用[SETRANGE](http://redis.cn/commands/setrange.html)就不会预先得到内存分配。
>
> ##### 模式
>
> 正因为有了[SETRANGE](http://redis.cn/commands/setrange.html)和类似功能的[GETRANGE](http://redis.cn/commands/getrange.html)命令，你可以把Redis的字符串当成线性数组，随机访问只要O(1)复杂度。这在很多真实场景应用里非常快和高效。
>
> ##### 返回值
>
> [integer-reply](http://redis.cn/topics/protocol.html#integer-reply)：该命令修改后的字符串长度

#### SETEX key seconds value

来源：http://redis.cn/commands/setex.html

> **起始版本：2.0.0**
>
> **时间复杂度：**O(1)
>
> 设置key对应字符串value，并且设置key在给定的seconds时间之后超时过期。这个命令等效于执行下面的命令：
>
> ```
> SET mykey value
> EXPIRE mykey seconds
> ```
>
> SETEX是原子的，也可以通过把上面两个命令放到[MULTI](http://redis.cn/commands/multi.html)/[EXEC](http://redis.cn/commands/exec.html)块中执行的方式重现。相比连续执行上面两个命令，它更快，因为当Redis当做缓存使用时，这个操作更加常用。
>
> ##### 返回值
>
> [simple-string-reply](http://redis.cn/topics/protocol.html#simple-string-reply)

#### GETSET key value

来源：http://redis.cn/commands/getset.html

> **起始版本：1.0.0**
>
> **时间复杂度：**O(1)
>
> 自动将key对应到value并且返回原来key对应的value。如果key存在但是对应的value不是字符串，就返回错误。
>
> 设计模式
>
> [GETSET](http://redis.cn/commands/getset.html)可以和[INCR](http://redis.cn/commands/incr.html)一起使用实现支持重置的计数功能。举个例子：每当有事件发生的时候，一段程序都会调用[INCR](http://redis.cn/commands/incr.html)给key mycounter加1，但是有时我们需要获取计数器的值，并且自动将其重置为0。这可以通过GETSET mycounter “0”来实现：
>
> ```
> INCR mycounter
> GETSET mycounter "0"
> GET mycounter
> ```
>
> ##### 返回值
>
> [bulk-string-reply](http://redis.cn/topics/protocol.html#bulk-string-reply): 返回之前的旧值，如果之前`Key`不存在将返回`nil`。

### ---

3.1和3.2的笔记太过详细了，从官网复制粘贴太多没必要放在笔记里的东西（本来就可以很轻易就查到的）。

到3.3“悬崖勒马”。

### 3.3 Redis列表(List)

#### 3.3.1 简介

> Redis列表是简单的字符串列表，按照插入顺序排序。 你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
>
> [LPUSH](http://redis.cn/commands/lpush.html) 命令插入一个新元素到列表头部，而[RPUSH](http://redis.cn/commands/rpush.html)命令 插入一个新元素到列表的尾部。当 对一个空key执行其中某个命令时，将会创建一个新表。 类似的，如果一个操作要清空列表，那么key会从对应的key空间删除。这是个非常便利的语义， 因为如果使用一个不存在的key作为参数，所有的列表命令都会像在对一个空表操作一样。
>
> 一些列表操作及其结果：
>
> ```
> LPUSH mylist a   # now the list is "a"
> LPUSH mylist b   # now the list is "b","a"
> RPUSH mylist c   # now the list is "b","a","c" (RPUSH was used this time)
> ```
>
> 一个列表最多可以包含232-1个元素（4294967295，每个表超过40亿个元素）。
>
> 从时间复杂度的角度来看，Redis列表主要的特性就是支持时间常数的 插入和靠近头尾部元素的删除，即使是需要插入上百万的条目。 访问列表两端的元素是非常快的，但如果你试着访问一个非常大 的列表的中间元素仍然是十分慢的，因为那是一个时间复杂度为 **O(N)** 的操作。
>
> 你可以用Redis列表做许多有趣的事，例如你可以：
>
> - 在社交网络中建立一个时间线模型，使用[LPUSH](http://redis.cn/commands/lpush.html)去添加新的元素到用户时间线中，使用[LRANGE](http://redis.cn/commands/lrange.html)去检索一些最近插入的条目。
> - 你可以同时使用[LPUSH](http://redis.cn/commands/lpush.html)和[LTRIM](http://redis.cn/commands/ltrim.html)去创建一个永远不会超过指定元素数目的列表并同时记住最后的N个元素。
> - 列表可以用来当作消息传递的基元（primitive），例如，众所周知的用来创建后台任务的Resque Ruby库。
> - 你可以使用列表做更多事，这个数据类型支持许多命令，包括像[BLPOP](http://redis.cn/commands/blpop.html)这样的阻塞命令。请查看所有可用的列表操作命令获取更多的信息。
>
> 查看完整的 [列表（Lists）](http://redis.cn/commands.html#list) 获取更多信息, 或者进一步阅读 [Redis数据类型介绍](http://redis.cn/topics/data-types-intro.html).

#### 3.3.2 数据结构

> ## Redis Lists
>
> 要说清楚列表数据类型，最好先讲一点儿理论背景，在信息技术界List这个词常常被使用不当。例如”Python Lists”就名不副实（名为Linked Lists），但他们实际上是数组（同样的数据类型在Ruby中叫数组）
>
> 一般意义上讲，列表就是有序元素的序列：10,20,1,2,3就是一个列表。但用数组实现的List和用Linked List实现的List，在属性方面大不相同。
>
> Redis lists基于Linked Lists实现。这意味着即使在一个list中有数百万个元素，在头部或尾部添加一个元素的操作，其时间复杂度也是常数级别的。用LPUSH 命令在十个元素的list头部添加新元素，和在千万元素list头部添加新元素的速度相同。
>
> 那么，坏消息是什么？在数组实现的list中利用索引访问元素的速度极快，而同样的操作在linked list实现的list上没有那么快。
>
> Redis Lists用linked list实现的原因是：对于数据库系统来说，至关重要的特性是：能非常快的在很大的列表上添加元素。另一个重要因素是，正如你将要看到的：Redis lists能在常数时间取得常数长度。
>
> 如果快速访问集合元素很重要，建议使用可排序集合(sorted sets)。可排序集合我们会随后介绍。
>
> ...
>
> ## key 的自动创建和删除
>
> 目前为止，在我们的例子中，我们没有在推入元素之前创建空的 list，或者在 list 没有元素时删除它。在 list 为空时删除 key，并在用户试图添加元素（比如通过 `LPUSH`）而键不存在时创建空 list，是 Redis 的职责。
>
> 这不光适用于 lists，还适用于所有包括多个元素的 Redis 数据类型 – Sets, Sorted Sets 和 Hashes。
>
> 基本上，我们可以用三条规则来概括它的行为：
>
> 1. 当我们向一个聚合数据类型中添加元素时，如果目标键不存在，就在添加元素前创建空的聚合数据类型。
> 2. 当我们从聚合数据类型中移除元素时，如果值仍然是空的，键自动被销毁。
> 3. 对一个空的 key 调用一个只读的命令，比如 `LLEN` （返回 list 的长度），或者一个删除元素的命令，将总是产生同样的结果。该结果和对一个空的聚合类型做同个操作的结果是一样的。

扩展阅读：[Redis内部数据结构详解(5)——quicklist - 铁蕾的个人博客](http://zhangtielei.com/posts/blog-redis-quicklist.html)

#### 3.3.3 命令

`LPUSH key value [value ...]`

`RPUSH key value [value ...]`

将所有指定的值插入到存于 key 的列表的头部（left push）或尾部（right push）。如果 key 不存在，那么在进行 push 操作前会创建一个空列表。

`LPOP key`

`RPOP key`

移除并且返回 key 对应的 list 的第一个（left pop）或最后一个（right pop）元素。

`RPOPLPUSH source destination`

原子性地返回并移除存储在 source 的列表的最后一个元素（列表尾部元素）， 并把该元素放入存储在 destination 的列表的第一个元素位置（列表头部）。

`LRANGE key start stop`

返回存储在 key 的列表里指定范围内的元素。 start 和 end 偏移量都是基于0的下标，即list的第一个元素下标是0（list的表头），第二个元素下标是1，以此类推。

偏移量也可以是负数，表示偏移量是从list尾部开始计数。 例如， -1 表示列表的最后一个元素，-2 是倒数第二个，以此类推。

`LINDEX key index`

返回列表里的元素的索引 index 存储在 key 里面。 下标是从0开始索引的，所以 0 是表示第一个元素， 1 表示第二个元素，并以此类推。 

负数索引用于指定从列表尾部开始索引的元素。在这种方法下，-1 表示最后一个元素，-2 表示倒数第二个元素，并以此往前推。

`LLEN key`

返回存储在 key 里的list的长度。 如果 key 不存在，那么就被看作是空list，并且返回长度为 0。

`LINSERT key BEFORE|AFTER pivot value`

把 value 插入存于 key 的列表中在基准值 pivot 的前面或后面。

当 key 不存在时，这个list会被看作是空list，任何操作都不会发生。

`LREM key count value`

从存于 key 的列表里移除前 count 次出现的值为 value 的元素。 这个 count 参数通过下面几种方式影响这个操作：

- count > 0: 从头往尾移除值为 value 的元素。
- count < 0: 从尾往头移除值为 value 的元素。
- count = 0: 移除所有值为 value 的元素。

比如， LREM list -2 “hello” 会从存于 list 的列表里移除最后两个出现的 “hello”。

需要注意的是，如果list里没有存在key就会被当作空list处理，所以当 key 不存在的时候，这个命令会返回 0。

`LSET key index value`

设置 index 位置的list元素的值为 value。

### ---

突然想起评论区有人说这个课程基本上就是照着《Redis开发与运维》这本书讲的，我去找了电子书一翻，发现确实，而且书里更详细，还不用做重复性的低价值的笔记……

还是等暑假回家买书吧，以书为基础，视频为学习参考，来学习。

纯视频的学习先暂停。

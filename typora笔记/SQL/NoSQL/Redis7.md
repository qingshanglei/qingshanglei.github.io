# 关于NoSQl数据库产生的原因：

**Memcached(缓存)+MySQL+垂直拆分**：

  随着访问量的上升，几乎大部分使用MySQL架构的网站在数据库上都开始出现了性能问题，web程序不再仅仅专注在功能上，同时也在追求性能。程序员们开始大量的使用缓存技术来缓解数据库的压力，优化数据库的结构和索引。开始比较流行的是通过文件缓存来缓解数据库压力，但是当访问量继续增大的时候，多台web机器通过文件缓存不能共享，大量的小文件缓存也带了了比较高的IO压力。在这个时候，Memcached就自然的成为一个非常时尚的技术产品。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Memcached(%E7%BC%93%E5%AD%98)+MySQL+%E5%9E%82%E7%9B%B4%E6%8B%86%E5%88%86.png)

 **Mysql主从读写分离**:

   由于数据库的写入压力增加，Memcached 只能缓解数据库的读取压力。读写集中在一个数据库上让数据库不堪重负，大部分网站开始使用主从复制技术来达到读写分离，以提高读写性能和读库的可扩展性。Mysql的master-slave模 式成为这个时候的网站标配了。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Mysql%E4%B8%BB%E4%BB%8E%E8%AF%BB%E5%86%99%E5%88%86%E7%A6%BB.png)

**分表分库+水平拆分+mysql集群**:

  在Memcached的高速缓存，MySQL的主从复制， 读写分离的基础之上，这时MySQL主库的写压力开始出现瓶颈，而数据量的持续猛增，由于MyISAM使用表锁，在高并发下会出现严重的锁问题，大量的高并发MySQL应用开始使用InnoDB引擎代替MyISAM。
  同时，开始流行使用分表分库来缓解写压力和数据增长的扩展问题。这个时候，分表分库成了一个热门技术，是面试的热门问题也是业界讨论的热门技术问题。也就在这个时候，MySQL推出了还不太稳定的表分区，这也给技术实力一般的公司带来了希望。虽然MySQL推出了MySQL Cluster集群，但性能也不能很好满足互联网的要求，只是在高可靠性上提供了非常大的保证。

**现在使用：**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E7%A8%8B%E5%BA%8F%E6%B5%81%E7%A8%8B.png)



# NoSQL特点：

1. **易扩展**：NoSQL数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。数据之间无关系，易扩展。但在架构的层面上带来了可扩展的能力。
2. **大数据量高性能**：NoSQL数据库都具有非常高的读写性能，尤其在大数据量下，同样表现优秀。 这得益于它的无关系性，数据库的结构简单。
       一般MySQL使用Query Cache，每次表的更新Cache就失效，是一种大粒度的Cache，在针对web2.0的交互频繁的应用，Cache性能不高。
    而NoSQL的Cache是记录级的，是一种细粒度的Cache，所以NoSQL在这个层面上来说就要性能高很多了。

3. **多样灵活的数据模型**：NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式。

NoSQL( Not Only SQL)，意为“不仅仅是SQL”，泛指非关系型的数据库。
NoSQL 数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。



**传统RDBMS VS NOSQL
RDBMS**

- 高度组织化结构化数据
- 结构化查询语言(SQL)
- 数据和关系都存储在单独的表中
- 数据操纵语言，数据定义语言
- 严格的一致性
- 基础事务

**NoSQL**

- 代表着不仅仅是SQL
- 没有声明性查询语言
- 没有预定义的模式
- 键-值对存储，列存储，文档存储，图形数据库
- 最终一致性，而非ACID属性
- 非结构化和不可预知的数据:
- CAP定理
- 高性能，高可用性和可伸缩性

# 概念：

Redis是一款key-value存储结构的内存级NoSQL数据库

- 支持多种数据存储格式
- 支持持久化
- 支持集群

Rrmote Dictionary Server(**远程字典服务**)是完全开源的，使用**ANSIC语言**编写遵守**DSD协议**,是一个高性能的Key-Value数据库提供了丰富的数据结构，例**String、Hash、List、Set、SortedSet**等，数据是存在内存中的，同时Redis支持事务、持久化、**LUA脚本**、发布/订阅、缓存淘汰、流技术等多种功能特性提供了主从模式、**Redis Sentinel**和**Redis Cluster**集群架构方案。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E7%89%B9%E7%82%B9.png)

 

mySQL与Redis数据库的关系：    

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/mySQL%E4%B8%8ERedis%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%85%B3%E7%B3%BB.png)



## 与传统数据库的关系：

- Redis是key-value数据库(NoSQL一种)，mySQL是关系型数据库。
- Redis数据库操作主要在内存上，而mySQL主要操作(存储)在硬盘/磁盘上。
- Redis在某一些场景使用中要明显优于mysql。 例计时器，排行榜等方面。
- Redis通常用于一些特定场景，需要与mysql一起配合使用。
- **<font color='#ff79a5'> Redis和mySQL两者并不是相互替换和竟争关系，而是共有和配合使用。 </font>**

## 主流功能与应用:

1. 分布式缓存，挡在mysql数据库之前的带刀侍卫。
2. 内存存储和持久化(RDB+AOF),redis支持异步将内存中的数据写到硬盘上，同时不影响继续服务.
3. 高可用架构搭配: ①单机、②主从  ③哨兵   ④集群
4. 缓存穿透、击穿、雪崩
5. 分布式锁
6. 队列： Reids提供list和set操作，这使得Redis能作为一个很好的消息队列平台来使用。我们常通过Reids的队列功能做购买限制。比如到节假日或者推广期间，进行一些活动，对用户购买行为进行限制，限制今天只能购买几次商品或者一段时间内只能购买一次。也比较适合适用。
7. 排行版+点赞：在互联网应用中，有各种各样的排行榜，如电商网站的月度销量排行榜、社交APP的礼物排行榜、小程序的投票排行榜等等。Redis提供的zset数据类型能够快速实现这些复杂的排行榜。比如小说网站对小说进行排名，根据排名，将排名靠前的小说推荐给用户

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E4%B8%BB%E6%B5%81%E5%8A%9F%E8%83%BD.png)

优点:
   ①性能极高 – Redis能读的速度是110000次/秒,写的速度是81000次/秒
   ②Redis数据类型丰富，不仅仅支持简单的Key-value类型的数据，同时还提供list,set,zset,hash等数据结构的存储。
  ③Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。  （ 注意：Redis关机/断电数据会写进硬盘，开机数据写进内存。）
   ④Redis支持数据的备份，即master-slave模式的数据备份。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E4%BC%98%E7%82%B9.png)



[Redis英文官网](https://redis.io/)  [Redis中文官网](http://www.redis.cn/)    [Redis中文官网2](https://www.redis.com.cn/documentation.html)

[ Redis命令参考](http://doc.redisfans.com/)   [Redis命令测试网站](https://try.redis.io/)  [GitHut官网](https://github.com/redis/redis)

核心部分：

1. 多种数据类型基本操作和配置

2. 持久化和复制，RDB/AOF

3. 事务的控制

4. 复制，集群等
    [Redis官网路径](https://redis.io/docs/about/)

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E6%A0%B8%E5%BF%83%E9%83%A8%E5%88%86.png)

## Redis版本说明:

5.0版本是直接升级到6.0版本，2022年4月27日Redis正式发布了7.0更新
（其实早在2022年1月31日，Redis已经预发布了7.0rc-1,确认没重大Bug才会正式发布)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E5%8E%86%E5%8F%B2%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E.png)



Redis版本命名规则：
   版本号第二位如果是奇数，则为非稳定版本 如2.7x、2.9x、3.1x
   版本号第二位如果是偶数，则为稳定版本 如2.6x、2.8x、3.0x、3.2x
当前奇数版本就是下一个稳定版本的开发版本，如2.9版本是3.0版本的开发版本
历史发布版本的源码：https://download.redis.io/releases/

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis%E5%8E%86%E5%8F%B2%E6%BA%90%E7%A0%81.png)



## Redis7.0新特性：

   2022 年 4 月正式发布的 Redis 7.0 是目前 Redis 历史版本中变化最大的版本。首先，它有超过 50 个以上新增命令；其次，它有大量核心特性的新增和改进。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7.png)

1. Redis Functions
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-Redis%20Functions.png)
2. Client-eviction
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-Client-eviction.png)
4. Multi-part AOF
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-Multi-part%20AOF.png)
5. ACL V2
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-ACL%20V2.png)
6. 新增命令： listpack 是用来替代 ziplist 的新数据结构，在 7.0 版本已经没有 ziplist 的配置了（6.0版本仅部分数据类型作为过渡阶段在使用）
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-%E6%96%B0%E5%A2%9E%E5%91%BD%E4%BB%A4.png)
7. listpack替代ziplist
8. 底层性能提升(和编码关系不大)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis7%E6%96%B0%E7%89%B9%E6%80%A7-%E5%BA%95%E5%B1%82%E6%80%A7%E8%83%BD%E6%8F%90%E5%8D%87.png)

**总体概述：**  大体和之前的redis版本保持一致和稳定，主要是自身底层性能和资源利用率上的优化和提高，如果你生产上系统稳定，不用着急升级到最新的redis7版本;  新系统，直接上Redis7.0-GA版。

| 多AOF文件支持                     | 7.0 版本中一个比较大的变化就是 aof 文件由一个变成了多个，主要分为两种类型：基本文件(base files)、增量文件(incr files)，请注意这些文件名称是复数形式说明每一类文件不仅仅只有一个。在此之外还引入了一个清单文件(manifest) 用于跟踪文件以及文件的创建和应用顺序（恢复） |
| --------------------------------- | ------------------------------------------------------------ |
| config命令增强                    | 对于Config Set 和Get命令，支持在一次调用过程中传递多个配置参数。例如，现在我们可以在执行一次Config Set命令中更改多个参数： config set maxmemory 10000001 maxmemory-clients 50% port 6399 |
| 限制客户端内存使用Client-eviction | 一旦 Redis 连接较多，再加上每个连接的内存占用都比较大的时候， Redis总连接内存占用可能会达到maxmemory的上限，可以增加允许限制所有客户端的总内存使用量配置项，redis.config 中对应的配置项// 两种配置形式：指定内存大小、基于 maxmemory 的百分比。maxmemory-clients 1gmaxmemory-clients 10% |
| listpack紧凑列表调整              | listpack 是用来替代 ziplist 的新数据结构，在 7.0 版本已经没有 ziplist 的配置了（6.0版本仅部分数据类型作为过渡阶段在使用）listpack 已经替换了 ziplist 类似 hash-max-ziplist-entries 的配置 |
| 访问安全性增强ACLV2               | 在redis.conf配置文件中，protected-mode默认为yes，只有当你希望你的客户端在没有授权的情况下可以连接到Redis server的时候可以将protected-mode设置为no |
| Redis Functions                   | Redis函数，一种新的通过服务端脚本扩展Redis的方式，函数与数据本身一起存储。简言之，redis自己要去抢夺Lua脚本的饭碗 |
| RDB保存时间调整                   | 将持久化文件RDB的保存规则发生了改变，尤其是时间记录频度变化  |
| 命令新增和变动                    | Zset (有序集合)增加 ZMPOP、BZMPOP、ZINTERCARD 等命令Set (集合)增加 SINTERCARD 命令LIST (列表)增加 LMPOP、BLMPOP ，从提供的键名列表中的第一个非空列表键中弹出一个或多个元素。 |
| 性能资源利用率、安全、等改进      | 自身底层部分优化改动，Redis核心在许多方面进行了重构和改进主动碎片整理V2：增强版主动碎片整理，配合Jemalloc版本更新，更快更智能，延时更低HyperLogLog改进：在Redis5.0中，HyperLogLog算法得到改进，优化了计数统计时的内存使用效率，7更加优秀更好的内存统计报告 ==如果不为了API向后兼容，我们将不再使用slave一词......(政治正确)== |

```sh
getconf LONG_BIT # 查看Linux系统多少位 (返回是多少就是几位)
```



# 安装&启动Redis:

  gcc是linux下的一个编译程序，是C程序的编译工具。
  GCC(GNU Compiler Collection) 是 GNU(GNU's Not Unix) 计划提供的编译器家族，它能够支持 C, C++, Objective-C, Fortran, Java 和 Ada 等等程序设计语言前端，同时能够运行在 x86, x86-64, IA-64, PowerPC, SPARC和Alpha 等等几乎目前所有的硬件平台上。鉴于这些特征，以及 GCC 编译代码的高效性，使得 GCC 成为绝大多数自由软件开发编译的首选工具。虽然对于程序员们来说，编译器只是一个工具，除了开发和维护人员，很少有人关注编译器的发展，但是 GCC 的影响力是如此之大，它的性能提升甚至有望改善所有的自由软件的运行效率，同时它的内部结构的变化也体现出现代编译器发展的新特征。

### 前提：

①Linux环境安装Redis必须安装gcc编译环境。
           ②安装Redis必须安装c++库环境。

```sh
# 安装gcc和c++环境
yum -y install gcc-c++ 

gcc -v # 检查是否安装gcc

redis-serve -v # 查看Redis版本命令（若按照有版本号）
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AE%89%E8%A3%85gcc%E5%92%8Cc++%E5%BA%93.png)



注意：Redis别使用6.0.8版本，建议都升级到6.0.8版本以上。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis6.0.8%E7%89%88%E6%9C%AC%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E.png)

### 下载并解压Redis安装包

```sh
# 下载并解压Redis安装包
wget https://download.redis.io/releases/redis-7.0.0.tar.gz
```

###  解压后在redis目录下执行make命令：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AE%89%E8%A3%85Redis.png)

### 将默认的redis.conf复制到/myredis路径下：

  复制此文件是怕了改坏，留个备份。。。

### 修改/myredis目录下redis.conf配置文件做初始化设置:

redis.conf配置文件，改完后确保生效，记得重启
```
 默认daemonize no        改为 daemonize yes
 默认protected-mode yes   改为 protected-mode no（取消保护模式）
 默认bind 127.0.0.1       改为 直接注释掉(默认bind 127.0.0.1只能本机访问)或改成本机IP地址，否则影响远程IP连接
 添加redis密码           改为 requirepass 密码
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AE%89%E8%A3%85Redis2.png)

### 启动&连接服务:

  启动服务: /usr/local/bin目录下运行redis-server，启用/myredis目录下的redis.conf文件 

```sh
ps -ef|grep redis|grep -v grep   # 查询Redis端口是否启动
ping # 检查是否安装Redis成功，出现PONG安装成功。
quit # 退出Redis客户端

redis-cli -a 密码 -p 端口   # 连接服务  （注意：端口可不写，默认6379）
redis-cli -a 密码 -p 端口  2>/dev/null #连接服务，并去除warning警告

在Redis服务器关闭客户端： shutdown 
关闭_单实例关闭：redis-cli -a 密码 shutdown
关闭_多实例关闭(指定端口关闭)：redis-cli  -p 6379 shutdown
lsof -i: 6379  # 查询端口是否关闭

redis-server # 前台启动(不推荐)
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AE%89%E8%A3%85Redis3.png)



## 查看默认安装目录：usr/local/bin

   注意：①执行make命令后Redis会安装在`usr/local/bin`目录
              ②Linux下的/usr/local类似win系统的C:\Program Files目录

1. redis-benchmark:性能测试工具，服务启动后运行该命令，查看电脑性能如何
2. redis-check-aof：修复有问题的AOF文件
3. redis-check-dump：修复有问题的dump.rdb文件
4. redis-cli：客户端，操作入口
5. redis-sentinel：redis集群使用
6. redis-server：Redis服务器启动命令

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/make%E5%91%BD%E4%BB%A4%E5%90%8ERedis%E5%AE%89%E8%A3%85%E5%85%B7%E4%BD%93%E5%86%85%E5%AE%B9.png)





# 卸载Redis:

1. 停止redis-server 服务
![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%8D%B8%E8%BD%BDRedis.png)
3. 删除/usr/local/lib目录下与redis相关的文件

```sh
ls -l /usr/local/bin/redis-* # 查看bin及子目录下所有的redis数据
rm -rf /usr/local/bin/redis-* # 删除bin及子目录下所有的redis数据
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%8D%B8%E8%BD%BDRedis2.png)

# Redis10大数据类型：

注意：此处的数据类型是value的数据类型，key的类型都是字符串。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis10%E5%A4%A7%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.png)

10大数据类型分别为：

1. redis字符串（String）
2. redis列表（List）
3. redis哈希表（Hash）
4. redis集合（Set）
5. redis有序集合（ZSet）
6. redis地理空间（GEO）
7. redis基数统计（HyperLogLog）
8. redis位图（bitmap）
9. redis位域（bitfield）
10. redis流（Stream）

## 常用命令：

 查看Redis常见数据类型操作命令： [英文官网](https://redis.io/commands/)     [中文官网](http://www.redis.cn/commands.html)

注意：命令不区分大小写，而key是区分大小写的。

```sh
keys *  	  # 查看当前库所有key
exists key    # 判断某个key是否存在
type key      # 查看你的key是什么类型
del key  	  # 删除指定的key的数据
unlink key    # 非阻塞删除，仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作
expire key 10 # 10秒钟：为给定的key设置过期时间（单位：秒）
ttl key		  # 查看还有多少秒过期，-1表示永不过期，-2表示已过期

select dbindex(数据库）  # 切换数据库 [0-15个数据库，默认为0]
move key dbindex # 将当前数据库的 key移动到给定的数据库中
dbsize	     # 查看当前数据库的key的数）
flushdb 	  # 清空当前数据库
flushall 	  # 清空所有库数据

```



**Redis 的过期时间设置有四种形式：**

```sh
expire key seconds(秒) [NX|XX|GT|LT]

 EXPIRE 秒——设置指定的过期时间(秒)，表示的是时间间隔。
 PEXPIRE 毫秒——设置指定的过期时间，以毫秒为单位，表示的是时间间隔。
 EXPIREAT 时间戳-秒——设置指定的 Key 过期的 Unix 时间，单位为秒，表示的是时间/时刻。
 PEXPIREAT 时间戳-毫秒——设置指定的 Key 到期的 Unix 时间，以毫秒为单位，表示的是时间/时刻。
```

## 帮助命令：

```sh
help @string   # 查询所有string类型的命令
help @list     # 查询所有list类型的命令 
help @hash     # ...
help @hyperloglog    # 
。。。
```



## 字符串(String):

- string是redis最基本的类型，一个key对应一个value。

- string类型是二进制安全的，意思是redis的string可以包含任何数据，比如jpg图片或者序列化的对象 。

- string类型是Redis最基本的数据类型，一个redis中字符串value最多可以是512M。

- 特点：单值单Value。

- [Redis Strings官网路径](https://redis.io/docs/data-types/strings/)

   

使用场景：

1.   比如抖音无限点赞某个视频或者商品，点一下加一次
2.   是否喜欢的文章

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AD%97%E7%AC%A6%E4%B8%B2-String-%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF.png)



### 命令详解：

```sh
 set key value [NX|XX] [GET] [EX seconds|PX milliseconds|EXAT unix-time-seconds|PXAT unix-time-milliseconds|KEEPTTL]
 
#   NX:当数据库中key不存在时，可以将key-value添加数据库
#   XX:当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
#   EX:key的超时秒数
#   PX:key的超时毫秒数，与EX互斥
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AD%97%E7%AC%A6%E4%B8%B2-String0.png)

基本命令：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AD%97%E7%AC%A6%E4%B8%B2-String.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AD%97%E7%AC%A6%E4%B8%B2-String2.png)



```sh
set key value # 添加键值对
get <key> # 根据键查询值
setnx <key><value> # 只有在key不存在时 设置key的值
getset  <key>  <value>  # getset(先get再set)

# 同时设置/获取多个键值 
mset key value[key value ...]
mset key [key ...]
mset/mget/msetnx 

# 获取/设置指定区间范围内的值   注意：（零到负一表示全部）
getrange key  0  -1  
setrange key  1  value  # 从key的索引值为1开始设置

# 获取字符串长度和内容追加
strlen <key> # 获得键值的长度
append <key> <value> #将给定的value追加到原值的末尾

# 分布式锁 --还没看。。。
setnx key value 
setex(set with expire)键秒值/setnx(set if not exist)

```

### 数值增减：

```sh
# 数值增减  注意：一定是数字才行。
incr <key> # 将key中存储的数字值递增1，如果为空，新增值为1
decr <key> # 递减数值
incrby/decrby <key> <步长> # 根据key值增减数字值
```

使用场景：
    ①抖音无限点赞某个视频或商品，点一下加一下。
    ②是否喜欢的文章。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%AD%97%E7%AC%A6%E4%B8%B2-String-%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF.png)



## 列表(List)：

​    Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）它的底层实际是个双端链表，最多可以包含 2^32 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。
​    它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的结点性能会较差。

  特点:单键多值, 值可重复。

 使用场景：微信公众号订阅消息。

```sh
#按照索引下标获得元素（从左到右） （0 -1表示获取所有）
lrange <key> <start> <stop>
# 0左边第一个，-1右边第一个，
lrange mylist 0 -1

#按照索引获得元素（从左到右）
lindex <key> <index>
#获得列表长度
llen <key>

#从左边/右边查询一个值。值在键在，值光键亡。
lpop/rpop <key>

#从左边/右边新增值
lpush/rpush <key> <value1><value2><value3>...

#从1列表右边获取一个值，插到2列表左边
rpoplpush <key1> <key2>

#从左边删除n个value（从左到右，删除N个等于value的值）
lrem  <key> <n> <value>
# 截取指定范围内的值后在赋值给key
ltrim  <key> <开始索引> <结束索引>

#将列表下标为index的值替换成value
lset <key> <index> <value>
# 在value的前面插入new值（-1代表插入失败）
linsert <key> before <value> <newvalue>
```



## 哈希表(Hash):

​    Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。 Redis 中每个 hash 可以存储 2^32 - 1 键值对（40多亿）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%93%88%E5%B8%8C%E8%A1%A8(Hash).png)

KV模式不变，但V是一个键值对,Map<String,Map<Object,Object>>

```sh
# 设置、获取哈希值
hset/hget  <field> <value>
# 批量新增、查询、删除哈希值
hmset/hmget/hdel <key> <field1><value1> <field2><value2>... 
hkeys/hvals <key> # 查看哈希表key中所有的key/valus。
hgetall   <key>  # 获取key的所有key和val

hlen key  # 获取key的长度
# 判断field是否已存在（存在1，不存在0）
hexists <key> <field> 
# 当域 field 不存在时，将哈希表 key 中的域 field 的值设置为 value 
hsetnx <key> <field> <value>

# 为哈希表key 中的域 field 的值加上增量1 -1 (hincrbyfloat返回值带小数)
hincrby/hincrbyfloat <key> <field> <increment>
```

使用场景：JD早期购物车(目前不再采用)，当前小中厂可用
1. 新增商品 → hset shopcar:uid1024 334488 1
2. 新增商品 → hset shopcar:uid1024 334477 1
3. 增加商品数量 → hincrby shopcar:uid1024 334477 1
4. 商品总数 → hlen shopcar:uid1024
5. 全部选择 → hgetall shopcar:uid1024

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%93%88%E5%B8%8C%E8%A1%A8%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF.png)











## 集合(Set):

- Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据，集合对象的编码可以是 intset 或者 hashtable。
- Redis 中Set集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
   - 集合中最大的成员数为 2^32 - 1 (4294967295, 每个集合可存储40多亿个成员)
- 特点：单值多value，且无重复，可快速查找、添加、删除
- 例： set   k1  v1 v2 v3

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/redis%E9%9B%86%E5%90%88(Set).png)



```sh
# 新增/删除集合
sadd/srem <key> member [member...]

smembers  <key> # 获取集合中的所有数据
sismember  <key> <member> # 判断元素是否存在集合中
scard  <key> # 获取集合里元素
srandmember <key> <count> # 从集合中随机获取一个元素
spop <key> <count># 从集合中随机获取一个元素，出一个删一个。
smove <key1> <key2> <value>  # 将key1里的一个值移动到key2


# 集合运算   例： A--> abc12    B-->123ax
sdiff key [key...]  #  差集运算A-B(属于A但不属于)
sunion key  [key...] # 并集运算A U B(属于A或者属于B的元素合并后的集合)
sinter key  [key...] # 交集运算A ∩ B
sintercard numkeys key [key...] [limit limit] # redis7新命令,它不返回结果集，而只返回结果的基数。返回由所有给定集合的交集产生的集合的基数

```



使用场景：
1. 微信抽奖小程序。

   ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E9%9B%86%E5%90%88(Set)%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF1.png)

 微信抽奖小程序

| 1 用户ID，立即参与按钮                    | sadd key 用户ID                                              |
| ----------------------------------------- | ------------------------------------------------------------ |
| 2 显示已经有多少人参与了，上图23208人参加 | SCARD key                                                    |
| 3 抽奖(从set中任意选取N个中奖人)          | SRANDMEMBER key 2    随机抽奖2个人，元素不删除SPOP key 3             随机抽奖3个人，元素会删除 |

2. 微信朋友圈点赞查看同赞朋友。
    ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E9%9B%86%E5%90%88(Set)%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF2.png)

  微信朋友圈点赞

| 1 新增点赞                               | sadd pub:msgID 点赞用户ID1 点赞用户ID2 |
| ---------------------------------------- | -------------------------------------- |
| 2 取消点赞                               | srem pub:msgID 点赞用户ID              |
| 3 展现所有点赞过的用户                   | SMEMBERS pub:msgID                     |
| 4 点赞用户数统计，就是常见的点赞红色数字 | scard pub:msgID                        |
| 5 判断某个朋友是否对楼主点赞过           | SISMEMBER pub:msgID 用户ID             |

3. QQ内推可能认识的人。

![]()

## 有序集合(ZSet):

- set(sorted set：有序集合) Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
- 不同的是每个元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序。
- zset的成员是唯一的,但分数(score)却可以重复。
- zset集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。 集合中最大的成员数为 2^32 - 1
- 例：   zset  k1  score1  v1  score2  v2

```sh
# 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
zadd <key> <score1><value1> [score2 value2…]
  例：zadd zkey1 60 v1 70 v2 80 v3
#  返回索引从start到stop之间的所有元素
zrange <key> <start> <stop> [WITHSCORES] 
# 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max 的成员。有序集成员按 score 值递增(从小到大)次序排列。
#     ①WITHSCORES。  ②"("：不包含。 ③limit：返回限制。
zrangebyscore <key> min max [withscores] [limit offset count]
#同上，改为从大到小排列。  
zrevrangebyscore <key> max min [withscores] [limit offset count] 
# 在排序的设置返回的成员范围，通过索引获取分数
zrevrange  <key> <start> <stop> [WITHSCORES] 
zincrby <key> <increment> <value> 
# 为元素的score加上增量
zrem <key><value>
# 删除该集合下，指定值的元素
zcount <key> <min> <max> # 获取分数范围内的元素

zrank <key> <value> # 获取集合中的索引

```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E6%9C%89%E5%BA%8F%E9%9B%86%E5%90%88(ZSet)%E5%91%BD%E4%BB%A4.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E6%9C%89%E5%BA%8F%E9%9B%86%E5%90%88(ZSet)%E5%91%BD%E4%BB%A42.png)

使用场景：根据商品销售对商品进行排序显示。

​     思路：定义商品销售排行榜(sorted set集合)，key为goods:sellsort，分数为商品销售数量。

| 商品编号1001的销量是9，商品编号1002的销量是15    | zadd goods:sellsort 9 1001 15 1002   |
| ------------------------------------------------ | ------------------------------------ |
| 有一个客户又买了2件商品1001，商品编号1001销量加2 | zincrby goods:sellsort 2 1001        |
| 求商品销量前10名                                 | ZRANGE goods:sellsort 0 9 withscores |





## 地理空间(GEO) 3.2+:

- Redis GEO 主要用于存储地理位置信息，并对存储的信息进行操作，包括

   添加地理位置的坐标。获取地理位置的坐标。计算两个位置之间的距离。根据用户给定的经纬度坐标来获取指定范围内的地理位置集合

​    地球上的地理位置是使用二维的经纬度表示，经度范围 (-180, 180]，纬度范围 (-90, 90]，只要我们确定一个点的经纬度就可以名取得他在地球的位置。

```sql
  # 滴滴打车,在数据库中查找距离我们(坐标x0,y0)附近r公里范围内部的车辆 
   select taxi from position where x0-r < x < x0 + r and y0-r < y < y0+r
```

问题：

1. 查询性能问题，如果并发高，数据量大这种查询是要搞垮数据库的

2. 这个查询的是一个矩形访问，而不是以我为中心r公里为半径的圆形访问。

3. 精准度的问题，我们知道地球不是平面坐标系，而是一个圆球，这种矩形计算在长距离计算时会有很大误差

     注意： 核心思想就是将球体转换为平面，区块转换为一点。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%9C%B0%E7%90%86%E7%A9%BA%E9%97%B4(GEO).png)

   [地理空间(GEO)原理](https://baike.baidu.com/item/%E7%BB%8F%E7%BA%AC%E7%BA%BF/5596978?fr=aladdin)



```sh
  GEORADIUS key longitude(多个经度) latitude(维度) radius <M|KM|FT|MI> [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count [ANY]] [ASC | DESC] [STORE key] [STOREDIST key]  --
              
   GEORADIUSBYMEMBER key member radius <M | KM | FT | MI> [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count [ANY]] [ASC | DESC] [STORE key] [STOREDIST key]
       
   geoadd city 113.331084 23.112223 "广州塔" 113.331077 23.109816 "广州塔羊城广场"  # 注意：百度地图获取的经纬度要去","   例： 天安门位置：113.331077,23.109816    改为 113.331077 23.109816才能使用。
```



使用场景：

1. 美团地图附近位置的酒店推送
2. 高德地图附近位置的核酸检查点



## redis基数统计(HyperLogLog)  2.8.9+:

- HyperLogLog 是用来去重复统计功能的基数估计算法。
- 优点：在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定且是很小的。
- 在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。
- 但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

   基数： 是一种数据集，去重复后的真实个数。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E5%9F%BA%E6%95%B0.png)

使用场景：
1.   统计某个网站的UV、统计某个文章的UV(Unique Visitor，独立访客，一般理解为客户端IP,==需要去重考虑==)
2.   用户搜索网站关键词的数量
3.   统计用户每天搜索不同词条个数
4.   天猫网站首页亿级UV的Redis统计方案

```sh
# 添加指定元素到HyperLogLog
pfadd key element [element...]
# 返回给定 HyperLogLog的基数估算值
pfcount key  [key...]
# 将多个 HyperLogLog合并为一个HyperLogLog
pfmerge  destkey sourcekey  [sourc...] 
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/redis%E5%9F%BA%E6%95%B0%E7%BB%9F%E8%AE%A1(HyperLogLog).png)



## redis位图(bitmap):

原理：由0和1状态表现的二进制位的bit数组。
说明：用String类型作为底层数据结构实现的一种统计二值状态的数据类型
​          位图本质是数组，它是基于String数据类型的按位的操作。该数组由多个二进制位组成，每个二进制位都对应一个偏移量(我们称之为一个索引)。
         Bitmap支持的最大位数是2^32位，它可以极大的节约存储空间，使用512M内存就可以存储多达42.9亿的字节信息(2^32 = 4294967296)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E4%BD%8D%E5%9B%BE-BitMap.png)

使用场景：

1. 用户是否登陆过Y、N，比如京东每日签到送京豆。
2. 电影、广告是否被点击播放过
3. 钉钉打卡上下班，签到统计等
4. 一年365天，全年天天登陆占用多少字节



```sh
# 新增   注意：Bitmap的偏移量是从零开始算的。
setbit key offset(偏移位) value(只能0或1)

strlen  <key> # 统计key的长度
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/redis%E4%BD%8D%E5%9B%BE(bitmap).png)





## redis流(Stream) 5.0+:

- Redis Stream 是 Redis 5.0 版本新增加的数据结构。
- Redis Stream 主要用于消息队列（MQ，Message Queue），Redis 本身是有一个 Redis 发布订阅 (pub/sub) 来实现消息队列的功能，但它有个缺点就是消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃。
- 简单来说发布订阅 (pub/sub) 可以分发消息，但无法记录历史消息。
- 而 Redis Stream 提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。
- ==Redis版的MQ消息中间件+阻塞队列==
- ==Stream还是不能100%等价于Kafka、RabbitMQ来使用的，生产案例少，慎用==

功能：
1. 实现消息队列，它支持消息的持久化、支持自动生成全局唯一 ID、支持，
2. ack确认消息的模式、支持消费组模式等，让消息队列更加的稳定和可靠。

原理：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/redis%E6%B5%81(Stream)%E5%8E%9F%E7%90%86.png)

一个消息链表，将所有加入的消息都串起来，每个消息都有一个唯一的 ID 和对应的内容

| 1    | Message Content   | 消息内容                                                     |
| ---- | ----------------- | ------------------------------------------------------------ |
| 2    | Consumer group    | 消费组，通过XGROUP CREATE 命令创建，同一个消费组可以有多个消费者 |
| 3    | Last_delivered_id | 游标，每个消费组会有个游标 last_delivered_id，任意一个消费者读取了消息都会使游标 last_delivered_id 往前移动。 |
| 4    | Consumer          | 消费者，消费组中的消费者                                     |
| 5    | Pending_ids       | 消费者会有一个状态变量，用于记录被当前消费已读取但未ack的消息Id，如果客户端没有ack，这个变量里面的消息ID会越来越多，一旦某个消息被ack它就开始减少。这个pending_ids变量在Redis官方被称之为 PEL(Pending Entries List)，记录了当前已经被客户端读取的消息，但是还没有 ack (Acknowledge character：确认字符），它用来确保客户端至少消费了消息一次，而不会在网络传输的中途丢失了没处理 |



```sh
# 四个特殊符号
- +  # 最小和最大可能出现的Id
$ # $ 表示只消费新的消息，当前流中最大的 id，可用于将要到来的信息
> # 用于XREADGROUP命令，表示迄今还没有发送给组中使用者的信息，会更新消费者组的最后 ID
* # 用于XADD命令中，让系统自动生成 id
```







## 位域(bitfield)：

- 通过bitfield命令可以一次性操作多个比特位域(指的是连续的多个比特位)，它会执行一系列操作并返回一个响应数组，这个数组中的元素对应参数列表中的相应操作的执行结果。
- 通过bitfield命令我们可以一次性对多个比特位域进行操作。

​    ==位域(bitfield)了解即可。。。。==




```sql
   exists key --判断某个key是否存在  （存在：1，不存在：0，其他个数：存在多少个key-values）   
  
   unlink key --非阻删除。（删除大内存文件）  成功：0，失败1
   8000毫秒 == 8秒
   
   user:001 id 11 name lise age 25
   user:002 id 22 name zangsan age 23

   getrande key 0 -1  --获取指定key的val的值(0到-1获取到key的全部值)
   setrange key 3 xxx --从3开启设置/修改key的值为xxx。
   sentnx key 秒 val  -- set key  + expire key 秒
   
   hset user:001 id 001 name zsan age 23
   
   zincrby key increment member -- 增加某个元素的分数(有则相加，无则新增)
   
   hset uid:map 0 uid-yh001
   hset uid:map 1 uid-yh002
   
   redis6.2及7  3600秒(60分钟)
   
    注意：dump文件名称最好加端口号，因为后面要配置多台Redis。（改完配置最后重启Redis）
    
    config get 名称 --读取redis-conf文件指定的名称
    cnfig set 名称 --修改redis-conf文件指定的名称
    
  
  Redis事务： 
     全体连坐：编译间出错。
     冤头债主：编译间不出错，结果出错。
   --pipeline可以将一批命令进行打包，然后发送给服务器，服务器执行完按顺序打包返回，这样就减少了频繁交互往返的时间。
管道(Pipelining)

Redis发布与订阅：可以使用“ * ”和“ ？”来批量订阅。

RedisReplica（复制）：
CentOS 7-2  192.168.111.140      端口：6379   
CentOS 7-3  192.168.111.141      端口：6380   
CentOS 7-4  192.168.111.142      端口：6381   
注意：
   ①从机只能读，不能写。
   ②从机初次加载会加载主机的全部数据，后续跟随，master写，slave跟。
   ③主机shutdown后：从机不动，原地待命，从机数据可以正常使用；等待主机重启动归来。
   
 哨兵(sentinel):
   注意：哨兵的端口和Redis的端口不同。
            哨兵的端口： 26379(默认)
            Redis的端口： 6379(默认)
   
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Sentinel%E5%93%A8%E5%85%B5.png)

| 选项         | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| longitude    | 经度                                                         |
| latitude     | 维度                                                         |
| member       | 经纬度名称                                                   |
| radius       | 距离当前位置多少米的人                                       |
| withcoord    | 将位置元素的经度和维度也一并返回。                           |
| withdist     | 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。 |
| ~~withhash~~ | 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大 |
| count        | 限定返回的记录数。                                           |
|              |                                                              |
|              |                                                              |
|              |                                                              |

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Stream(%E6%B5%81)%E6%B5%81%E7%A8%8B%E5%9B%BE.png)





​    1的二进制数：0011 0001

   无符号整数（unsigned integer），计算机术语，计算机里的数是用[二进制](https://baike.baidu.com/item/二进制/361457?fromModule=lemma_inlink)表示的，最左边的这一位一般用来表示这个数是正数还是负数，这样的话这个数就是有符号整数。无符号整数包括0和正数。

```sql
bitfield key set i8 被修改位数（字节）   要修改数字/单词（十进制）  --（i8表示有符号8位二进制，范围(-128-127)）
    -- int是有符号的，unsigned是无符号的。
    
    flushall -- 删除所有库数据，会RDB一份rdb文件(但无数据)。
    
  redis开机会自动加载RDB数据。
```



# Redis持久化:

https://redis.io/docs/manual/persistence/

## RDB(Redis DataBase):

### 概念：

   RDB（Redis DataBase/Redis 数据库）：RDB 持久性以指定的时间间隔执行数据集的时间点快照。

   在指定的时间间隔，执行数据集的时间点快照。
   实现类似照片记录效果的方式，就是把某一时刻的数据和状态以文件的形式写到磁盘上，也就是快照。这样一来即使故障宕机，快照文件也不会丢失，数据的可靠性也就得到了保证。这个快照文件称为RDB文件(dump.rdb)。
​    Redis的数据都在内存中，保存备份时它执行的是==全量快照==（把内存中的所有数据都记录到磁盘中）。

#### 优点：

1. 适合大规模的数据恢复
2. 按照业务定时备份
3. 对数据完整性和一致性要求不高
4. RDB 文件在内存中的加载速度要比 AOF 快得多

#### 缺点：

1. 在一定间隔时间做一次备份，所以如果redis意外down掉的话，就会丢失从当前至最近一次快照期间的数据，==快照之间的数据会丢失==
2. 内存数据的全量同步，如果数据量太大会导致I/0严重影响服务器性能
3. RDB依赖于主进程的fork，在更大的数据集中，这可能会导致服务请求的瞬间延迟。fork的时候内存中的数据被克隆了一份，大致2倍的膨胀性，需要考虑

#### 哪些情况会触发RDB快照：

1. 配置文件中默认的快照配置
2. 手动save/bgsave命令
3. 执行flushall/flushdb命令也会产生dump.rdb文件，但里面是空的，无意义
4. 执行shutdown且没有设置开启AOF持久化
5. 主从复制时，主节点自动触发

#### 禁用快照：

1.    动态停止所有RDB保存规则的方法：redis-cli config set save ""
2.   配置文件禁用：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E7%A6%81%E7%94%A8%E5%BF%AB%E7%85%A7.png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96.png)

### 持久化说明：

持久化类似：①手动持久化。   ②自动持久化。

Redis6.0.16以下：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis6.0.16%E4%BB%A5%E4%B8%8B%E6%8C%81%E4%B9%85%E5%8C%96%E8%AF%B4%E6%98%8E.png)

Redis6.2及Redis-7.0.0+:

​	![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/Redis6.2%E5%8F%8ARedis-7.0.0+%E6%8C%81%E4%B9%85%E5%8C%96%E8%AF%B4%E6%98%8E.png)

### 自动备份/触发：

   本次案例Redis7版本5秒内2次修改触发备份。

1.修改自动备份时间

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BD1.png)

2.修改dump文件保存路径 及文件名称

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BD2.png)

3.触发备份

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BD3.png)

​    恢复数据： 将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可恢复数据。

   注意：不可以把备份文件dump.rdb和生产redis服务器放在同一台机器，必须分开各自存储，以防生产机物理损坏后备份文件也挂了。

物理恢复，一定服务和备份==分机隔离==



### 手动备份/触发： 

1.  save在主程序中执⾏==会阻塞==当前redis服务器，直到持久化工作完成，执行save命令期间，Redis不能处理其他命令，线上禁止使用。
2. Redis会使用bgsave对当前内存中的所有数据做快照，这个操作是子进程在后台完成的，这就允许主进程同时可以修改数据。

```sh
save #手动备份1（注意：会阻塞，线上禁止使用）  
bgsave #手动备份2-默认 
lastsave # 获取最后一次成功执行快照的时间-时间戳格式
```

在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，尽量避免膨胀。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E6%89%8B%E5%8A%A8%E5%A4%87%E4%BB%BD.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96-%E6%89%8B%E5%8A%A8%E5%A4%87%E4%BB%BD_save.png)



### 数据丢失案例：

1. 正常录入数据
2. kill -9 端口故意模拟意外down机
3. redis重启恢复，查看数据是否丢失

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E6%8C%81%E4%B9%85%E5%8C%96%E6%95%B0%E6%8D%AE%E4%B8%A2%E5%A4%B1%E6%A1%88%E4%BE%8B.png)

### RDB优化配置项:

  RDB优化配置项详解:配置文件SNAPSHOTTING模块

1. save <seconds> <changes>
2. dbfilename
3. dir
4. stop-writes-on-bgsave-error
5. rdbcompression
6. rdbchecksum
7. rdb-del-sync-files

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/RDB%E4%BC%98%E5%8C%96%E9%85%8D%E7%BD%AE%E9%A1%B9.png)



## AOF(Append Only File):

 AOF(Append Only File,默认关闭):   以日志的形式来记录每个写操作,将Redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作.

  开启AOF功能需要设置配置：appendonly yes
  Aof保存的是appendonly.aof文件





## RDB-AOF混合持久化：





# 集群:

3主3从Redis集群：

```js
bind 0.0.0.0
daemonize yes
protected-mode no
port 6385
logfile "/opt/redis-7.0.9/myRedis7/Cluster/log/cluster6385.log"
pidfile /opt/redis-7.0.9/myRedis7/Cluster/pid/cluster6385.pid
dir /opt/redis-7.0.9/myRedis7/rdb/Cluster
dbfilename dump6385.rdb
appendonly yes
appendfilename "appendonly6385.aof"
requirepass 071800
masterauth 071800
 
cluster-enabled yes #开启集群
cluster-config-file nodes-6385.conf #打开集群的配置文件(节点配置文件)
cluster-node-timeout 5000 # 集群的访问和超时时间
```



```js
redis-cli -a 071800 --cluster create --cluster-replicas 1 192.168.139.140:6381  192.168.139.140:6382  192.168.139.141:6383 192.168.139.141:6384 192.168.139.142:6385 192.168.139.142:6386 192.168.139.142:6387 192.168.139.142:6388
```



```
cluster nodes
cluster info
```



```js
redis-cli -a 密码 -p 端口 -c(防止路由失效) 

cluster keyslot key // 查看该key对应的槽位值

redis-cli -a 071800 --cluster add-node 192.168.139.142:6385 192.168.139.140:6382 --cluster-slave --cluster-master-id 385c21c7617fbe3e338d52ebdd9a4bf896ed26ba
```



|                                                              |      |
| ------------------------------------------------------------ | ---- |
| [ggkt后台管理系統 (zwf123.top)](https://zwf123.top/html/#/dashboard) |      |
|                                                              |      |
|                                                              |      |



# SpringBoot整合Redis:

   Jedis，Lettuce，RedisTemplate都是Redis的客户端。

## Jedis:



## Lettuce:



## RedisTemplate:



导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### 使用RedisTemplate:

#### RedisTemplate:



#### 解决RedisTemplate序列化问题：

解决RedisTemplate序列化问题(Redis服务端中文乱码)：

Config/RedisConfig:

```java
/** 解决RedisTemplateTests的序列化问题 */
@Configuration // @Configuration：配置类 代替 ‘applicationContext.xml’文件
public class RedisConfig {

    /**
     * redis序列化的工具配置类，下面这个请一定开启配置
     * 127.0.0.1:6379> keys *
     * 1) "ord:102"  序列化过
     * 2) "\xac\xed\x00\x05t\x00\aord:102"   野生，没有序列化过
     * this.redisTemplate.opsForValue(); //提供了操作string类型的所有方法
     * this.redisTemplate.opsForList(); // 提供了操作list类型的所有方法
     * this.redisTemplate.opsForSet(); //提供了操作set的所有方法
     * this.redisTemplate.opsForHash(); //提供了操作hash表的所有方法
     * this.redisTemplate.opsForZSet(); //提供了操作zset的所有方法
     *
     * @param lettuceConnectionFactory
     * @return
     */
    @Bean
    public RedisTemplate<String, Object> redisTemplate(LettuceConnectionFactory lettuceConnectionFactory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();

        redisTemplate.setConnectionFactory(lettuceConnectionFactory);
        //设置key序列化方式string
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //设置value的序列化方式json，使用GenericJackson2JsonRedisSerializer替换默认序列化
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.afterPropertiesSet();

        return redisTemplate;
    }
}
```



#### StringRedisTemplate：



### 单机：







### 集群：

```yaml
spring:
  redis:
    cluster: # 集群Ip 
      nodes: 192.168.139.140:6381,
        192.168.139.140:6382,
        192.168.139.141:6383,
        192.168.139.141:6384,
        192.168.139.142:6385,
        192.168.139.142:6386,
        192.168.139.142:6387,
        192.168.139.142:6388
    password: 071800
    database: 0 # 选择数据库
    lettuce:
      pool: # 连接池最大连接数
        max-active: 8
        max-wait: -1ms
        max-idle: 8
        min-idle: 0
```



### 解决集群中duan机情况：

   故障现象：SpringBoot客户端没有动态感应到Redis Cluster的最新集群信息。

   导致原因：SpringBoot 2.X 版本， Redis默认的连接池采用 Lettuce,当Redis 集群节点发生变化后，Letture默认是不会刷新节点拓扑。

解决方法：

1.排除Lettuce采用jedis（不推荐）：

2.重写连接工厂实例(极度不推荐)：

3.刷新节点集群拓扑动态感应:

   ①.调用RedisClusterClient.reloadPartitions。
   ②后台基于时间间隔的周期刷新。
   ③后台基于持续的**断开**和**移动、重定向**的自适应更新。

yml写入以下配置：

```yaml
spring:
  redis:
    lettuce:
      cluster:
        refresh:
          adaptive: true # 支持集群拓扑动态感应刷新,自适应拓扑刷新是否使用所有可用的更新，默认false（解决集群shutdown机，不及时刷新问题）
          period: 2000 # 定时刷新
```





微服务技术：BootCloud+docker+nginx+juc+jmeter....









导入：

| MindManager |      xmind      |      |
| :---------: | :-------------: | ---- |
|             | MindManager格式 |      |
|             |  FreeMind格式   |      |
|             |     Lighten     |      |
|             |    MindNode     |      |
|             |    Markdown     |      |
|             |      OPML       |      |
|             |   TextBundle    |      |
|             | Word（仅docx）  |      |
|             |                 |      |

llettcus与jedis区别：

1.  jedis连接Redis服务器是直连模式，当多线程模式下使用jedis会存在线程安全问题，解决方案可以通过配置连接池使每个连接专用，这样整体性能就大受影响。
2. lettcus(默认)基于Netty框架进行与Redis服务器连接，底层设计中采用StatefulRedisConnection。 StatefulRedisConnection自身是线程安全的，可以保障并发访问安全问题，所以一个连接可以被多线程复用。当然lettcus也支持多连接实例一起工作。


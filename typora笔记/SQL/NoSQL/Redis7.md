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
- [Redis Strings官网路径](https://redis.io/docs/data-types/strings/)

```sh
1.set <key><value> #添加键值对
*NX:当数据库中key不存在时，可以将key-value添加数据库
*XX:当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
*EX:key的超时秒数
*PX:key的超时毫秒数，与EX互斥
get <key> #查询对应键值

append <key><value> #将给定的value追加到原值的末尾

strlen <key> #获得键值的长度

setnx <key><value> #只有在key不存在时 设置key的值

6.incr <key> #将key中存储的数字值增1
			 #只能对数字值操作，如果为空，新增值为1
7.decr <key> #减1

8.incrby/decrby <key> <步长> #增减数字值

```



![graphic](data:application/octet-stream;base64,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)

1. redis列表（List）:Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）它的底层实际是个双端链表，最多可以包含 2^32 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

2. redis哈希表（Hash）：Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。 Redis 中每个 hash 可以存储 2^32 - 1 键值对（40多亿）

4. redis集合（Set）: 
    - Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据，集合对象的编码可以是 intset 或者 hashtable。
- Redis 中Set集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
   - 集合中最大的成员数为 2^32 - 1 (4294967295, 每个集合可存储40多亿个成员)

4. redis有序集合（ZSet）

    - set(sorted set：有序集合) Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
    - 不同的是每个元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序。
    - zset的成员是唯一的,但分数(score)却可以重复。
    - zset集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。 集合中最大的成员数为 2^32 - 1

5. redis地理空间（GEO）

    - Redis GEO 主要用于存储地理位置信息，并对存储的信息进行操作，包括

       添加地理位置的坐标。获取地理位置的坐标。计算两个位置之间的距离。根据用户给定的经纬度坐标来获取指定范围内的地理位置集合

6. redis基数统计（HyperLogLog）:

    - HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定且是很小的。
    - 在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。
    - 但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

7. redis位图（bitmap）: 由0和1状态表现的二进制位的bit数组。

    ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/Redis/%E4%BD%8D%E5%9B%BE-BitMap.png)

8. redis位域（bitfield）：

    - 通过bitfield命令可以一次性操作多个比特位域(指的是连续的多个比特位)，它会执行一系列操作并返回一个响应数组，这个数组中的元素对应参数列表中的相应操作的执行结果。
    -  通过bitfield命令我们可以一次性对多个比特位域进行操作。

9. redis流（Stream）

    - Redis Stream 是 Redis 5.0 版本新增加的数据结构。
    - Redis Stream 主要用于消息队列（MQ，Message Queue），Redis 本身是有一个 Redis 发布订阅 (pub/sub) 来实现消息队列的功能，但它有个缺点就是消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃。
    - 简单来说发布订阅 (pub/sub) 可以分发消息，但无法记录历史消息。
    - 而 Redis Stream 提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。




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
   
   
 
                                                                     GEORADIUS key longitude latitude radius <M|KM|FT|MI> [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count [ANY]] [ASC | DESC] [STORE key] [STOREDIST key]  --
              
   GEORADIUSBYMEMBER key member radius <M | KM | FT | MI> [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count [ANY]] [ASC | DESC] [STORE key] [STOREDIST key]
              
              geoadd city 113.331084 23.112223 "广州塔" 113.331077 23.109816 "广州塔羊城广场"  --注意：百度地图获取的经纬度要去"," 例： 天安门位置：113.331077,23.109816    改为 113.331077 23.109816才能使用。
   
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


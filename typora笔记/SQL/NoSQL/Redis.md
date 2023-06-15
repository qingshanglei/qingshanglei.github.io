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

# 主流功能与应用:

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




Redis6.0,6.7版本。

注意：第三方软件一般安装在Liunx系统的opt文件里。

```shell
ps -ef|grep redis|grep -v grep   # 查询Redis端口是否启动
ping # 检查是否安装Redis成功，出现PONG安装成功。
quit # 退出Redis客户端
在Redis服务器关闭客户端： shutdown 
关闭_单实例关闭：redis-cli -a 密码 shutdown
lsof -i: 6379  # 查询端口是否关闭

protected-mode改为no，# 取消保护模式
redis-cli -a 密码 -p 端口   # 连接服务  （注意：端口可不写，默认6379）
```



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
| [ggkt后台管理系統 - Vue Admin Template (zwf123.top)](https://zwf123.top/html/#/dashboard) |      |
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


Redis是一款key-value存储结构的内存级NoSQL数据库

- 支持多种数据存储格式
- 支持持久化
- 支持集群

Rrmote Dictionary Server(**远程字典服务**)是完全开源的，使用**ANSIC语言**编写遵守**DSD协议**,是一个高性能的Key-Value数据库提供了丰富的数据结构，例**String、Hash、List、Set、SortedSet**等，数据是存在内存中的，同时Redis支持事务、持久化、**LUA脚本**、发布/订阅、缓存淘汰、流技术等多种功能特性提供了主从模式、**Redis Sentinel**和**Redis Cluster**集群架构方案。

  注意：Redis关机(断电)数据会写进硬盘，开机数据写进内存。

## mySQL与Redis数据库的关系：

​     分布式缓存，挡在mysql数据库之前的带刀侍卫。



## 与传统数据库的关系：

Redis是key-value数据库(NoSQL一种)，mySQL是关系型数据库。

Redis数据库操作主要在内存上，而mySQL主要操作(存储)在硬盘/磁盘上。

Redis在某一些场景使用中要明显优于mysql。 例计时器，排行榜等方面。

Redis通常用于一些特定场景，需要与mysql一起配合使用。

   **<font color='#ff79a5'> Redis和mySQL两者并不是相互替换和竟争关系，而是共有和配合使用。 </font>**



[Redis 命令参考 — Redis 命令参考 (redisfans.com)](http://doc.redisfans.com/)

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


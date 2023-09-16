MQ的相关概念

## 什么是MQ

  MQ(message queue)，本质是个队列，FIFO 先入先出，只不过队列中存放的内容是 message(消息) 而已，还是一种跨进程的通信机制，用于上下游传递消息。在互联网架构中，MQ 是一种非常常 见的上下游「逻辑解耦 + 物理解耦」的消息通信服务。使用了 MQ 之后，消息发送上游只需要依赖 MQ，不用依赖其他服务。

## 为什么要用MQ

MQ 的优、缺点：

| **优点**： | *缺点*：         |
| ---------- | ---------------- |
| ⚫应用解耦  | ⚫ 系统可用性降低 |
| ⚫ 异步提速 | ⚫ 系统复杂度提高 |
| ⚫ 削峰填谷 | ⚫ 一致性问题     |

### 优点：

1. 流量消峰

   例如：如果订单系统最多能处理一万次订单，这个处理能力应付正常时段的下单时绰绰有余，正常时段我们下单一秒后就能返回结果。但是在高峰期，如果有两万次下单操作系统是处理不了的，只能限制订单超过一万后不允许用户下单。使用消息队列做缓冲，我们可以取消这个限制，把一秒内下的订单分 散成一段时间来处理，这时有些用户可能在下单十几秒后才能收到下单成功的操作，但是比不能下单的体验要好。

   ​    流量消峰：使用MQ后，可以提高系统稳定性。

   

   ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%B5%81%E9%87%8F%E6%B6%88%E5%B3%B0.jpg)

2. 应用解耦

      例如：电商应用中有订单系统、库存系统、物流系统、支付系统。用户创建订单后，如果耦合调用库存系统、物流系统、支付系统，任何一个子系统出了故障，都会造成下单操作异常。当转变成基于消息队列的方式后，系统间调用的问题会减少很多，比如物流系统因为发生故障，需要几分钟来修复。在这几分钟的时间里，物流系统要处理的内存被缓存在消息队列中，用户的下单操作可以正常完成。当物流系统恢复后，继续处理订单信息即可，中单用户感受不到物流系统的故障，提升系统的可用性。

   ​      简单的说： 在系统崩溃的期间请求消息等被缓存在消息队列中，用户可正常下的；系统恢复后，用户可继续使用。
   
   ​      应用解耦：使用 MQ 使得应用间解耦，提升容错性和可维护性。
   
   注意：系统的耦合性越高，容错性就越低，可维护性就越低。
   
   ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BA%94%E7%94%A8%E8%A7%A3%E8%80%A6.png)

3.异步处理

   有些服务间调用是异步的，例如 A 调用 B，B 需要花费很长时间执行，但是 A 需要知道 B 什么时候可以执行完。

以前一般有两种方式：
​       ①A 过一段时间去调用 B 的查询 api 查询。
​       ②或者 A 提供一个 callback api，B 执行完之后调用 api 通知 A 服务。这两种方式都不是很优雅。

 异步处理：提升用户体验和系统吞吐量（单位时间内处理请求的数目）。

使用消息总线后：

​    A 调用 B 服务后，只需要监听 B 处理完成的消息，当 B 处理完成后，会发送一条消息给 MQ，MQ 会将此消息转发给 A 服务。这样 A 服务既不用循环调用 B 的查询 api，也不用提供 callback api。同样 B 服务也不用做这些操作。A 服务还能及时的得到异步处理成功的消息。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BC%82%E6%AD%A5%E5%A4%84%E7%90%86.png)

### 缺点：

1. 系统可用性降低：系统引入的外部依赖越多，系统稳定性越差。
2. 系统复杂度提高：MQ 的加入大大增加了系统的复杂度，以前系统间是同步的远程调用，现在是通过 MQ 进行异步调用。
3. 一致性问题：A 系统处理完业务，通过 MQ 给B、C、D三个系统发消息数据，如果 B 系统、C 系统处理成功，D 系统处理失败。



## MQ的分类

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/MQ%E5%88%86%E7%B1%BB%E5%AF%B9%E6%AF%94.jpg)

- **ActiveMQ：**

  优点：单机吞吐量万级，时效性 ms 级，可用性高，基于主从架构实现高可用性，消息可靠性较 低的概率丢失数据

  缺点：官方社区现在对 ActiveMQ 5.x 维护越来越少，高吞吐量场景较少使用

- **Kafka：**

     大数据的杀手锏，谈到大数据领域内的消息传输，则绕不开 Kafka，这款为大数据而生的消息中间件，以其百万级 TPS 的吞吐量名声大噪，迅速成为大数据领域的宠儿，在数据采集、传输、存储的过程中发挥着举足轻重的作用。目前已经被 LinkedIn，Uber，Twitter，Netflix 等大公司所采纳。

  ​    优点: 性能卓越，单机写入 TPS 约在百万条/秒，最大的优点，就是吞吐量高。时效性 ms 级可用性非常高，kafka 是分布式的，一个数据多个副本，少数机器宕机，不会丢失数据，不会导致不可用,消费者采用 Pull 方式获取消息，消息有序，通过控制能够保证所有消息被消费且仅被消费一次；有优秀的第三方Kafka Web 管理界面 Kafka-Manager；在日志领域比较成熟，被多家公司和多个开源项目使用；功能支持：功能 较为简单，主要支持简单的 MQ 功能，在大数据领域的实时计算以及日志采集被大规模使用

  缺点：Kafka 单机超过 64 个队列/分区，Load 会发生明显的CPU飙高现象，队列越多，load 越高，发送消息响应时间变长，使用短轮询方式，实时性取决于轮询间隔时间，消费失败不支持重试；支持消息顺序，但是一台代理宕机后，就会产生消息乱序，社区更新较慢

- **RocketMQ：**

  RocketMQ 出自阿里巴巴的开源产品，用 Java 语言实现，在设计时参考了 Kafka，并做出了自己的一些改进。被阿里巴巴广泛应用在订单，交易，充值，流计算，消息推送，日志流式处理，binglog 分发等场景。

  ​    优点：单机吞吐量十万级，可用性非常高，分布式架构,消息可以做到 0 丢失,MQ 功能较为完善，还是分布式的，扩展性好,支持 10 亿级别的消息堆积，不会因为堆积导致性能下降，源码是 java 我们可以自己阅读源码，定制自己公司的 MQ

  ​    缺点：支持的客户端语言不多，目前是 java 及 c++，其中 c++ 不成熟；社区活跃度一般,没有在 MQ 核心中去实现 JMS 等接口,有些系统要迁移需要修改大量代码

- **RabbitMQ：**

  2007 年发布，是一个在AMQP(高级消息队列协议)基础上完成的，可复用的企业消息系统，是当前最主流的消息中间件之一。

  ​    优点：由于 erlang 语言的高并发特性，性能较好；吞吐量到万级，MQ 功能比较完备,健壮、稳定、易用、跨平台、支持多种语言 如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP 等，支持 AJAX 文档齐全；开源提供的管理界面非常棒，用起来很好用,社区活跃度高；更新频率相当高

  ​    缺点：商业版需要收费,学习成本较高。
  
- **ZeroMQ、MetaMq ......**

**MQ 的选择** 

1.Kafka

Kafka 主要特点是基于 Pull 的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输，适合产生**大量数据**的互联网服务的数据收集业务。**大型公司**建议可以选用，如果有**日志采集**功能，首选 kafka 。

2.RocketMQ

天生为**金融互联网**领域而生，对于可靠性要求很高的场景，尤其是电商里面的订单扣款，以及业务削峰，在大量交易涌入时，后端可能无法及时处理的情况。RoketMQ 在稳定性上可能更值得信赖，这些业务场景在阿里双 11 已经经历了多次考验，如果你的业务有上述并发场景，建议可以选择 RocketMQ。

3.RabbitMQ

结合 erlang 语言本身的并发优势，性能好**时效性微秒级**，**社区活跃度也比较高**，管理界面用起来十分方便，如果你的**数据量没有那么大**，中小型公司优先选择功能比较完备的 RabbitMQ。

# RabbitMQ 介绍：

​     RabbitMQ 是一个消息中间件：它接受并转发消息。
​     你可以把它当做一个快递站点，当你要发送一个包裹时，你把你的包裹放到快递站，快递员最终会把你的快递送到收件人那里，按照这种逻辑 RabbitMQ 是 一个快递站，一个快递员帮你传递快件。
​    RabbitMQ 与快递站的主要区别在于，它不处理快件而是接收，存储和转发消息数据。

​      AMQP，即 Advanced Message Queuing Protocol（高级消息队列协议），是一个网络协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。2006年，AMQP 规范发布。类比HTTP。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/AMQP%E6%B5%81%E7%A8%8B.jpg)

​     2007年，Rabbit 技术公司基于 AMQP 标准开发的 RabbitMQ 1.0 发布。RabbitMQ 采用 Erlang 语言开发。
   注意：Erlang 语言由 Ericson 设计，专门为开发高并发和分布式系统的一种语言，在电信领域使用广泛。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%20%E5%9F%BA%E7%A1%80%E6%9E%B6%E6%9E%84%E6%B5%81%E7%A8%8B.jpg)







## 四大核心概念

  生产者：产生数据发送消息的程序。
  交换机：是 RabbitMQ 非常重要的一个部件，一方面它接收来自生产者的消息，另一方面它将消息 推送到队列中。交换机必须确切知道如何处理它接收到的消息，是将这些消息推送到特定队列还是推送到多个队列，亦或者是把消息丢弃，这个得有交换机类型决定。
  队列：是 RabbitMQ 内部使用的一种数据结构，尽管消息流经 RabbitMQ 和应用程序，但它们只能存储在队列中。队列仅受主机的内存和磁盘限制的约束，本质上是一个大的消息缓冲区。许多生产者可以将消息发送到一个队列，许多消费者可以尝试从一个队列接收数据。这就是我们使用队列的方式。

  消费者：消费与接收具有相似的含义。消费者大多时候是一个等待接收消息的程序。   注意：生产者、消费者和消息中间件很多时候并不在同一机器上。同一个应用程序既可以是生产者又是可以是消费者。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/MQ%E7%9A%84%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6.jpg)


## **RabbitMQ 核心部分**

```
  7大核心部分/7大模式
  1.简单模式
  2.工作模式
  3.发布、订阅模式
  4.路由模式
  5.主题模式
  6.请求应答模式  RPC    # 远程调用，不太算 MQ；暂不作介绍）
  7.发布确认模式
```

  官网地址：https://www.rabbitmq.com/getstarted.html

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E6%A0%B8%E5%BF%83%E9%83%A8%E5%88%86%E6%B5%81%E7%A8%8B.jpg)

1. 简单模式 HelloWorld：一个生产者、一个消费者，不需要设置交换机（使用默认的交换机）。

2. 工作队列模式 Work Queue：一个生产者、多个消费者（竞争关系），不需要设置交换机（使用默认的交换机）。

3. 发布订阅模式 Publish/subscribe需要设置类型为 fanout 的交换机，并且交换机和队列进行绑定，当发送消息到交换机后，交换机会将消息发送到绑定的队列。

4. 路由模式 Routing：需要设置类型为 direct 的交换机，交换机和队列进行绑定，并且指定 routing key，当发送消息到交换机后，交换机会根据 routing key 将消息发送到对应的队列。

5. 通配符模式 Topic：需要设置类型为 topic 的交换机，交换机和队列进行绑定，并且指定通配符方式的 routing key，当发送消息到交换机后，交换机会根据 routing key 将消息发送到对应的队列。
6. RPC 远程调用模式（远程调用，不太算 MQ；暂不作介绍）



## RabbitMQ原理：



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.jpg)

- **Broker**(RabbitMQ Server 或Message Broker)：接收和分发消息的应用。
- **Virtual host**：出于多租户和安全因素设计的，把 AMQP 的基本组件划分到一个虚拟的分组中，类似于网络中的 namespace(命名空间) 概念。当多个不同的用户使用同一个 RabbitMQ server 提供的服务时，可以划分出多个 vhost，每个用户在自己的 vhost 创建 exchange／queue 等。
- Connection**：publisher／consumer 和 broker 之间的 TCP 连接**
- Channel**：如果每一次访问 RabbitMQ 都建立一个 Connection，在消息量大的时候建立 TCP Connection 的开销将是巨大的，效率也较低。Channel 是在 connection 内部建立的逻辑连接，如果应用程序支持多线程，通常每个 thread 创建单独的 channel 进行通讯，AMQP method 包含了 channel id 帮助客户端和 message broker 识别 channel，所以 channel 之间是完全隔离的。**Channel 作为轻量级的Connection 极大减少了操作系统建立 **TCP connection** **的开销** 。
- **Exchange**：message 到达 broker 的第一站，根据分发规则，匹配查询表中的 routing key，分发消息到 queue 中去。常用的类型有：direct/直接交换机 (point-to-point), topic/主题交换机 (publish-subscribe) and fanout (multicast)
- **Queue**：消息最终被送到这里等待 consumer(消费者) 取走。
- **Binding**：exchange 和 queue 之间的虚拟连接，binding 中可以包含 routing key，Binding 信息被保存到 exchange (交换机)中的查询表中，用于 message(消息) 的分发依据。

JMS:

JMS 即 Java 消息服务（JavaMessage Service）应用程序接口，是一个 Java 平台中关于面向消息中间件的API。

⚫ JMS 是 JavaEE 规范中的一种，类似JDBC。

⚫ 很多消息中间件都实现了JMS规范，例如：ActiveMQ。   RabbitMQ 官方没有提供 JMS 的实现包，但是开源社区有。





```sh

https://frxcat.fun/pages/e645d9/ #笔记
https://www.bozhu12.cc/backend/afbo8k/ # 笔记1
https://blog.csdn.net/lyyrhf/article/details/120159288  #笔记2
https://zhangc233.github.io/2021/07/30/RabbitMQ%E9%9D%A2%E8%AF%95%E9%A2%98/  # 面试题

https://www.wolai.com/nnRjHcUSv2mrRbFKZUpBMS # 代码重工-笔记
https://heavy_code_industry.gitee.io/code_heavy_industry/ #代码重工2
```



# RabbitMQ 安装

## 版本关系:

 RabbitMQ和Erlang 版本对比：https://www.rabbitmq.com/which-erlang.html
     本次采用 RabbitMQ的 3.8.8 版本，需要的 Erlang 最小版本是21.3。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E7%89%88%E6%9C%AC%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB.jpg)





springCloud整合：

```yaml
spring:
  cloud:
    stream:
      # 配置输入输出绑定
      bindings:
        input:
          destination: my-input-queue   # 输入目的地
          content-type: application/json   # 消息内容类型
        output:
          destination: my-output-exchange   # 输出目的地
          content-type: application/json   # 消息内容类型
          producer:
            exchangeType: topic   # 交换类型为主题
            requiredGroups: my-consumer-group   # 指定必需的消费者组名称
      # 使用RabbitMQ作为Binder
      rabbit:
        bindings:
          input:
            consumer:
              queueNameGroupOnly: true   # 输入绑定的队列名称只包含组名
上述代码创建了两个绑定（input和output），并指定它们的目的地。对于输出绑定，我们还指定了交换类型为主题（topic），并指定了必需的消费者组名称（required-groups）。使用RabbitMQ作为Binder时，我们指定了输入绑定的队列名称只包含组名。
```



## 角色默认四种级别：

```sh
  administrator（超级管理员）：可以登录控制台、查看所有信息、并对rabbitmq进行管理
  monToring(监控者):登录控制台，查看所有信息
  policymaker(策略制定者):  登录控制台指定策略,无法查看节点的相关信息.
  managment(普通管理员): 登录控制
  其他:无法登陆管理控制台，通常就是普通的生产者和消费者.
```



## 其他命令：

```sh
# 删除单个队列
rabbitmqadmin delete queue name=myqueue

# 删除多个队列
rabbitmqadmin delete queue name=myqueue1 name=myqueue2 name=myqueue3
# 使用通配符删除多个队列
rabbitmqadmin delete queue name="myqueue.*"

# 删除所有队列（谨慎执行）
rabbitmqadmin list queues name | awk '{print $2}' | xargs -I qn rabbitmqadmin delete queue name=qn
```



## 单机：

 RabbitMQ官网：https://www.rabbitmq.com/
 RabbitMQ中文文档： http://rabbitmq.mr-ping.com/

 RocketMQ官网：https://rocketmq.apache.org/

```sh
# 采用Linux系统安装
https://packagecloud.io/rabbitmq # erlang，RabbitMQ软件下载网站 
```

1.必须先具备依赖环境：

```sh
yum install socat -y # 先安装依赖
```

2.上传并安装Erlang和RabbitMQ

```sh
rpm -ivh erlang-21.3-1.el7.x86_64.rpm
rpm -ivh rabbitmq-server-3.8.8-1.el7.noarch.rpm
```

启动：

```sh
   chkconfig rabbitmq-server on # 添加开机自启服务
   /sbin/service rabbitmq-server start/status/stop # 安装完必须先启动,再关闭，然后开启 web 管理插件
 
   rabbitmq-plugins enable rabbitmq_management # 开启web管理插件，装完重启rabbitmq-server 

uname -a # 查看系统版本
# 浏览器：192.168.139.140:15672  # 
#  默认账号、密码：guest (注意需要权限才能登录，默认无权限)
```

用户：

```sh
# 查看所有用户和角色
rabbitmqctl list_users  
# 创建账号
rabbitmqctl add_user qsl(用户名) 123456(密码)  
# 设置用户角色
rabbitmqctl set_user_tags qsl administrator
# 设置用户权限
set_permissions [-p <vhostpath>] <user> <conf-读> <write-写> <read-执行>
   例：rabbitmqctl set_permissions -p "/" qsl ".*" ".*" ".*"
 
 # 查看rabbit1节点的所有用户和角色，用户角色、权限等类似。
 rabbitmqctl --node rabbit1 list_users 
```

浏览器访问：http://192.168.139.140:15672/



重启：

```sh
# 关闭应用的命令
rabbitmqctl stop_app
# 清除所有的账号和密码
rabbitmqctl reset
# 重新启动命令
rabbitmqctl start_app
```





## 集群：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E9%9B%86%E7%BE%A4%E6%B5%81%E7%A8%8B.jpg)

### 伪集群服务端：

集群：单台机器(伪集群)使用端口区分，多台机器使用Ip来区分。

   RabbitMQ这款消息队列中间件产品本身是基于Erlang编写，Erlang语言天生具备分布式特性（通过同步Erlang集群各节点的magic cookie来实现）。因此，RabbitMQ天然支持Clustering。这使得RabbitMQ本身不需要像ActiveMQ、Kafka那样通过ZooKeeper分别来实现HA方案和保存集群的元数据。集群是保证可靠性的一种方式，同时可以通过水平扩展以达到增加消息吞吐量能力的目的。

```sh
   # 本次两个集群(节点)：首先单机先安装一个RabbitMQ，再根据RabbitMQ启动多个节点(端口)搭建集群。
http://192.168.139.140:15672/#/
http://192.168.139.140:15673/#/
```

1.安装RabbitMQ的15673

```sh
service rabbitmq-server stop # 停止RabbitMq

#  启动rabbit1节点(端口)  注意：启动完需要克隆一个会话再启动rabbit1节点和web管理插件。
RABBITMQ_NODE_PORT=5673 RABBITMQ_NODENAME=rabbit1 rabbitmq-server start # 注意：此处使用的是web管理插件默认端口号。

# 启动rabbit2节点，并指定其web管理插件占用的端口号。 注意：启动完又需要克隆一个会话才能使用了。
RABBITMQ_NODE_PORT=5674 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15673}]" RABBITMQ_NODENAME=rabbit2 rabbitmq-server start

# 浏览器分别访问：  
http://192.168.139.140:15672/#/
http://192.168.139.140:15673/#/
```

2. 将15673加入集群中

```sh
# 停止rabbit2节点服务
rabbitmqctl -n rabbit2 stop  
# 清除rabbit2节点信息。 主要是为了清除账号等信息，不清有点问题。
rabbitmqctl -n rabbit2 reset 

# rabbit2(从节点)节点加入到rabbit1(主节点)节点中
rabbitmqctl -n rabbit2 join_cluster rabbit1@'主机名' [选项] 
# hostname: 查看主机名
   选项： 
      ① --ram:  # 将此节点加入到集群中的内存节点。
      ② --disc: # 将节点加入到集群中的磁盘节点中。
    
# 启动rabbit2节点服务
rabbitmqctl -n rabbit2 start  

# 查看rabbit1集群状态
rabbitmqctl cluster_status -n rabbit1
```

浏览器访问：
​     http://192.168.139.140:15672/#/
 ​    http://192.168.139.140:15673/#/

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%9B%86%E7%BE%A4%E6%90%AD%E5%BB%BA%E6%88%90%E5%8A%9F.jpg)



移除集群等操作。。。

```sh

# 将节点从集群中删除，允许离线执行。
rabbitmqctl forget_cluster_node [–offline]

# 取消队列queue同步镜像的操作。
rabbitmqctl cancel_sync_queue [-p vhost] {queue}

# 在集群中的节点应用启动前咨询clusternode节点的最新信息，并更新相应的集群信息。这个和join_cluster不同，它不加入集群
rabbitmqctl update_cluster_nodes {clusternode}

# 设置集群名称。集群名称在客户端连接时会通报给客户端。Federation和Shovel插件也会有用到集群名称的地方。集群名称默认是集群中第一个节点的名称，通过这个命令可以重新设置。
rabbitmqctl set_cluster_name {name}
```



### 集群：

   根据单机先安装三个RabbitMQ,

```sh
192.168.139.140
192.168.139.141
192.168.139.142

# 安装RabbitMQ好后在140分别拉取141和142的RabbitmqCookie
scp /var/lib/rabbitmq/.erlang.cookie root@192.168.193.141:/var/lib/rabbitmq/.erlang.cookie

# 三台机器分别启动 RabbitMQ 服务
rabbitmq-server -detached

# 141，142分别执行以下命令
rabbitmqctl stop_app # 关闭RabbitMQ
rabbitmqctl join_cluster rabbit@192.168.139.140 # 将一个RabbitMQ节点加入到集群中。  注意：在加入集群之前，您需要确保新节点与现有集群具有相同的erlang.cookie文件和相同的版本号。
rabbitmqctl start_app # 重启应用

rabbitmqctl cluster_status # 查看集群状态
```



### 客户端：

​     RabbitMQ客户端默认不支持负载均衡，必须使用**Haproxy+Keepalive**等实现高可用负载均衡。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E9%9B%86%E7%BE%A4-%E5%AE%A2%E6%88%B7%E7%AB%AF.png)



​    HAProxy 提供高可用性、负载均衡及基于 TCPHTTP 应用的代理，支持虚拟主机，它是免费、快速并且可靠的一种解决方案，包括 Twitter,Reddit,StackOverflow,GitHub 在内的多家知名互联网公司在使用。HAProxy 实现了一种事件驱动、单一进程模型，此模型支持非常大的井发连接数。
​     nginx,lvs,haproxy 之间的区别: http://www.ha97.com/5646.html

1.Haproxy安装略。。。

2.生产者

```java
public class Producer {
    public static final String QUEUE_NAME = "hello"; // 队列名

    //    发消息
    public static void main(String[] args) throws Exception {
        // 创建一个连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 连接Happroxy的服务端来调用RabbitMQ集群
        factory.setHost("192.168.139.140");
        factory.setPort(5672); //  连接Happroxy：端口
        // 创建连接
        Connection connection = factory.newConnection();
        // 获取信道
        Channel channel = connection.createChannel();

        /* 生成一个队列
         * 1.队列名称
         * 2. 是否开启持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 要发送的消息
        String message = "hello world";

        /* 发送一个消息
         * 1.发送指定的交换机
         * 2.路由的Key值，  本次是队列的名称
         * 3.其他参信息
         * 4.发送消息的消息体 */
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());  // getBytes: 二进制转换
        System.out.println("消息发送完毕");

        // 释放资源
        channel.close();
        connection.close();
    }
}
```



## 卸载：

```sh
# 停止RabbitMQ服务
systemctl stop rabbitmq-server.service

# 禁用RabbitMQ服务
systemctl disable rabbitmq-server.service

# 卸载RabbitMQ软件包
yum remove rabbitmq-server

# 删除RabbitMQ数据和配置文件夹
rm -rf /var/lib/rabbitmq
rm -rf /etc/rabbitmq
# 自此卸载成功
```









## 安装异常：

   解决方法：不要使用ME浏览器，换成谷歌浏览器问题解决。

```java
  Uncaught TypeError: Cannot set properties of undefined (setting '_innerDate')
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/RabbitMQ/%E5%AE%89%E8%A3%85RabbitMQ%E5%BC%82%E5%B8%B8.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/RabbitMQ/%E5%AE%89%E8%A3%85RabbitMQ%E5%BC%82%E5%B8%B81.png)





# 简单模式：

## 依赖：

```xml
<dependencies>
    <!--rabbitmq 依赖客户端-->
    <dependency>
        <groupId>com.rabbitmq</groupId>
        <artifactId>amqp-client</artifactId>
        <version>5.7.3</version>
    </dependency>
    <!--操作文件流的一个依赖-->
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
    </dependency>
    <dependency>
        <!-- logback-classic 是一个用于记录日志的 Java 类库,提供了一个与 SLF4J 框架无缝集成的实现-->
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>

<!--指定 jdk 编译版本-->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>8</source>
                <target>8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## 生产者：

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/** 生产者 */
public class Producer {
    public static final String QUEUE_NAME = "hello"; // 队列名

    //    发消息
    public static void main(String[] args) throws Exception {
        // 创建一个连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 连接RabbitMQ队列： 工厂IP
        factory.setHost("192.168.139.140");
        //用户名
        factory.setUsername("qsl");
        // 密码
        factory.setPassword("123456");
        // 创建连接
        Connection connection = factory.newConnection();
        // 获取信道
        Channel channel = connection.createChannel();
        /* 生成一个队列
         * 1.队列名称
         * 2. 是否开启持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 要发送的消息
        String message = "hello world";

        /* 发送一个消息
         * 1.消息发送到指定的交换机
         * 2.路由的Key值，  本次是队列的名称
         * 3.其他参信息
         * 4.发送消息的消息体 */
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());  // getBytes: 二进制转换
        System.out.println("消息发送完毕");
    }
}
```

浏览器访问：http://192.168.139.140:15672/#/queues

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81%E4%B8%8E%E6%8E%A5%E6%94%B6.jpg)





## 消费者：

```java
import com.rabbitmq.client.*;

/* 消费者*/
public class Consumer {

    private static final String QUEUE_NAME = "hello"; //队列名

    public static void main(String[] args) throws Exception {
        // 创建一个连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 连接RabbitMQ队列， 工厂IP
        factory.setHost("192.168.139.140");
        // 用户名
        factory.setUsername("qsl");
        //密码
        factory.setPassword("123456");
        // 创建连接
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        //    声明-接收的消费
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody());
            System.out.println(message);
        };

        // 取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消费消息被中断");
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者成功消费的回调
         * 4.消费者取消消费的回调
         */
        channel.basicConsume(QUEUE_NAME, true, deliverCallback, cancelCallback);
    }
```

浏览器访问：http://192.168.139.140:15672/#/queues

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81%E4%B8%8E%E6%8E%A5%E6%94%B6.jpg)



# Work Queues(工作模式)：

​    工作队列/模式(任务队列)的主要思想是避免立即执行资源密集型任务，而不得不等待它完成。相反我们安排任务在之后执行。我们把任务封装为消息并将其发送到队列。在后台运行的工作进程将弹出任务并最终执行作业。当有多个工作线程时，这些工作线程将一起处理这些任务。

## **轮询分发消息(默认)**

```java
// 生产者在同一个队列中的多个消费者之间，轮询发送消息。
Tack01 // 生产者
Worker01  // 消费者
Worker02  // 消费者
```

### 生产者：

```java
import com.qsl.rabbitmq.utils.RabbitMqUtils;
import com.rabbitmq.client.Channel;

import java.util.Scanner;

/*生产者*/
public class Tack01 {
    private static final String QUEUE_NAME = "poll"; // 队列

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类—信道—信道
        Channel channel = RabbitMqUtils.getChannel();

        /* 生成一个队列
         * 1.队列名称
         * 2. 是否开启持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 从控制台中接收消息
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String message = scanner.next();
            /* 发送一个消息
             * 1.消息发送到指定的交换机
             * 2.路由的Key值，  本次是队列的名称
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
            System.out.println("已发送消息：" + message);
        }
    }
}
```

### 消费者：

```java
import com.qsl.rabbitmq.utils.RabbitMqUtils;
import com.rabbitmq.client.CancelCallback;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.DeliverCallback;

/**
 * 消费者
 */
public class Worker01 {
    private static final String QUEUE_NAME = "poll"; // 队列名

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类—信道
        Channel channel = new RabbitMqUtils().getChannel();

        //    声明-接收的消费
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody());
            System.out.println("接收的消息： " + message);
        };

        // 取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        System.out.println("C3 消费者启动等待消费......");   // 使用多线程，模拟3个线程：c1,c2,c3 （主要是懒得写三个一样的类了）
        /**
   * 消费者-消费者消息
   * 1.消费那个队列
   * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
   * 3.消费者为成功消费的回调
   * 4.消费者录取消费的回调*/
        channel.basicConsume(QUEUE_NAME, false, deliverCallback, cancelCallback);

    }
}
```

在当前模块复制多线程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%B8%80%E4%B8%AA%E6%A8%A1%E5%9D%97%E8%BF%90%E8%A1%8C%E5%A4%9A%E7%BA%BF%E7%A8%8B.jpg)



结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E8%BD%AE%E8%AF%A2%E5%88%86%E5%8F%91%E6%B6%88%E6%81%AF.jpg)



## **消息应答**:

​      产生原因： RabbitMQ 一旦向消费者传递了一条消息，便立即将该消息标记为删除，而消费者在消费过程中消费者宕机了等，我们将丢失正在处理的消息。以及后续发送给该消费者的消息。

   为了保证消息在发送过程中不丢失，引入消息应答机制，消息应答：**消费者在接收到消息并且处理该消息之后，告诉 rabbitmq(ack) 可以把该消息删除了。**



**消息应答的方法** ：

**A**.Channel.basicAck(用于肯定确认)

**B**.Channel.basicNack(用于否定确认) 

**C**.Channel.basicReject(用于否定确认，少个参数) 

​    肯定确认：RabbitMQ 已知道该消息并且成功的处理消息，可以将其丢弃了。

​    否定确认: 不处理该消息了直接拒绝，可以将其丢弃了。

### 自动应答（默认）：

​    消息发送后立即被认为已经传送成功，这种模式需要在**高吞吐量和数据传输安全性方面做权衡**,因为这种模式如果消息在接收到之前，消费者那边出现连接或者 channel 关闭，那么消息就丢失了,当然另一方面这种模式消费者那边可以传递过载的消息，**没有对传递的消息数量进行限制**，使得消费者这边由于接收太多消息，导致这些消息的积压，最终使得内存耗尽，最终这些消费者线程被操作系统杀死，**所以这种模式仅适用在消费者可以高效并以, 某种速率能够处理这些消息的情况下使用。**



### 消息手动应答：

消息手动应答流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%B6%88%E6%81%AF%E5%BA%94%E7%AD%94-%E6%89%8B%E5%8A%A8%E5%BA%94%E7%AD%94%E6%B5%81%E7%A8%8B.png)

​    手动应答的优点：可以批量应答并且减少网络拥堵。

消息自动重新入队 ：

  如果消费者宕机等原因失去连接(通道关闭，连接关闭或 TCP 连接丢失)，导致消息未发送 ACK 确认，RabbitMQ 收到未完全处理的消息，并将对其重新排队。如果此时其他消费者可以处理，它将消息重新分发给另一个消费者。这样，即使某个消费者偶尔死亡，也可以确保不会丢失任何消息。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%B6%88%E6%81%AF%E8%87%AA%E5%8A%A8%E9%87%8D%E6%96%B0%E5%85%A5%E9%98%9F%E6%B5%81%E7%A8%8B.png)

​    注意：手动应答默认开启**消息自动重新入队**,就是手动应答消息是不丢失的，消息丢失时会返回队列中重新消费。

消费者：

```java
........
    //    手动应答
    /** 1.消息的标记  tag(ID）
      * 2.是否批量应答  
         false:只应答接收到的消息: 
         true：应答所有消息包括传递过来的消息:*/
    channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
........

    boolean autoAck = false; //手动应答
/**
 * 消费者-消费者消息
 * 1.消费那个队列
 * 2.消费成功后是否使用自动应答，true:自动应答（默认）   false：手动应答
 * 3.消费者成功消费的回调
 * 4.消费者取消消费的回调 */
channel.basicConsume(QUEUE_NAME, autoAck, deliverCallback, cancelCallback);
```



​    生产者者发送消息 dd，C2 消费者消费过程中宕机，此时C2 还没有执行 ack ，RabbitMQ 收到未完全处理的消息，并将对其重新排队。消费者C1正常运行，RabbitMQ转发消息给 消费者C1，消费者C1消费消息。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%89%8B%E5%8A%A8%E5%BA%94%E7%AD%94%E7%BB%93%E6%9E%9C.png)







## 持久化：

   默认情况下 RabbitMQ 退出或系统崩溃时，它忽视了队列和消息。
   确保消息不会丢失需要做两件事：**我们需要将队列和消息都标记为持久化。**

### 队列持久化：

​    队列非持久化：RabbitMQ 如果重启的后，该队列就会被删除掉，如果要队列实现持久化需要在声明队列的时候把 durable 参数设置为true，代表开启持久化。

生产者：

  ```java
/* 生成一个队列
   * 1.队列名称
   * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
   * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
   * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
   * 5.其他参数*/
channel.queueDeclare(QUEUE_NAME,true,false,false,null);
  ```

  

  队列持久化:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%98%9F%E5%88%97%E6%8C%81%E4%B9%85%E5%8C%96.jpg)



   注意：若已存在不持久化队列，则报以下异常，RabbitMQ删除队列 、重启即可。

````java
     Caused by: com.rabbitmq.client.ShutdownSignalException: channel error; protocol method: #method<channel.close>(reply-code=406, reply-text=PRECONDITION_FAILED - inequivalent arg 'durable' for queue 'ack_queue' in vhost '/': received 'true' but current is 'false', class-id=50, method-id=10)
````



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/RabbitMQ%E5%AF%B9%E5%AD%98%E5%9C%A8%E4%B8%8D%E6%8C%81%E4%B9%85%E5%8C%96%E9%98%9F%E5%88%97%E5%BC%82%E5%B8%B8.jpg)



### 消息持久化：

  注意：将消息标记为持久化并不能完全保证不会丢失消息。尽管它告诉 RabbitMQ 将消息保存到磁盘，但是这里依然存在消息存储在磁盘的时候，MQ宕机消息丢失的情况。此时消息并没 有真正写入磁盘。持久性保证并不强，但是对于我们的简单任务队列而言，这已经绰绰有余了。

​    如果需要更强有力的持久化策略，参考发布确认章节。

消息生产者：

```java
/* 发送一个消息
     * 1.消息发送到指定的交换机
     * 2.路由的Key值，  本次是队列的名称
     * 3.其他参信息   （消息持久化：MessageProperties.PERSISTENT_TEXT_PLAIN ）
     * 4.发送消息的消息体 */
channel.basicPublish("",QUEUE_NAME, MessageProperties.PERSISTENT_TEXT_PLAIN,message.getBytes("UTF-8")); // 二进制，UTF-8编码
```



### 不公平分发、负载均衡:

​     产生原因： 有两个消费者在处理任务，其中有个**消费者 1** 处理任务的速度非常快，而另外一个**消费者 2** 处理速度却很慢，这个时候我们还是采用轮询分发的化就会 到这处理速度快的这个消费者很大一部分时间处于空闲状态，而处理慢的那个消费者一直在干活，这种分配方式在这种情况下其实就不太好，但是 RabbitMQ 并不知道这种情况它依然很公平的进行分发。

消费者：

```java
  .....
// 0：轮询(默认)   1:负载均衡     2以上为预取值
channel.basicQos(1); // 不公平分发/负载均衡:1 
boolean autoAck = false; //手动应答
  .....
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%B8%8D%E5%85%AC%E5%B9%B3%E5%88%86%E5%8F%91%E3%80%81%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1.jpg)



结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%B8%8D%E5%85%AC%E5%B9%B3%E5%88%86%E5%8F%91%E3%80%81%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A11.jpg)



# basicQos：

```java
// 0：轮询(默认)   1:负载均衡     2以上为预取值
channel.basicQos(0); // 不公平分发/负载均衡
```

1. 轮询(默认) ： 生产者发送消息，消费者轮询接收消息。
2. 负载均衡：生产者发送消息，消费者根据自身情况接收消息。
3. 预取值：

   本身消息的发送就是异步发送的，所以在任何时候，channel(信道) 上肯定不止只有一个消息另外来自消费者的手动确认本质上也是异步的。因此这里就存在一个未确认的消息缓冲区。 
    注意：必须限制此缓冲区的大小，以避免缓冲区里面有无限制的未确认消息问题。   通过使用 basic.qos 方法设置“预取计数”值完成限制此缓冲区的大小。

​    预取值：该值定义通道上允许的未确认消息的最大数量。一旦数量达到配置的数量，RabbitMQ 将停止在该通道上传递更多消息，除非至少有一个未处理的消息被确认，例如，假设在通道上有4个未确认的消息 5、6、7，8，并且通道的预取计数设置为 4，此时 RabbitMQ 将不会在该通道上再传递任何消息，除非至少有一个未应答的消息被 ack。比方说 tag(Id)=6 这个消息刚刚被确认 ACK。

​    消息应答和 QoS 预取值对用户吞吐量有重大影响。通常，增加预取将提高向消费者传递消息的速度。**虽然自动应答传输消息速率是最佳的，但是，在这种情况下已传递但尚未处理的消息的数量也会增加，从而增加了消费者的** **RAM** **消耗**(随机存取存储器)应该小心使用具有无限预处理的自动确认模式或手动确认模式，消费者消费了大量的消息如果没有确认的话，会导致消费者连接节点的内存消耗变大。不同的负载该值取值也不同 100 到 300 范围内的值通常可提供最佳的吞吐量，并且不会给消费者带来太大的风险。预取值为 1 是最保守的。当然这将使吞吐量变得很低，特别是消费者连接延迟很严重的情况下，特别是在消费者连接等待时间较长的环境中。对于大多数应用来说，稍微高一点的值将是最佳的。

流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%A2%84%E5%8F%96%E5%80%BC%E6%B5%81%E7%A8%8B.png)



案例流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%A2%84%E5%8F%96%E5%80%BC%E6%B5%81%E7%A8%8B.jpg)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%A2%84%E5%8F%96%E5%80%BC.jpg)





# Publisher Confirms(发布确认模式)：

  

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E3%80%81%E8%AE%A2%E9%98%85%E6%A8%A1%E5%BC%8F%E6%B5%81%E7%A8%8B.jpg)

​      生产者发布消息到 RabbitMQ 后，需要 RabbitMQ 返回==ACK(已收到)==给生产者，这样生产者才知道自己生产的消息成功发布出去。

​    

## 发布确认：

  生产者将信道设置成 confirm(确认) 模式，一旦信道进入 confirm 模式，**所有在该信道上面发布的**消息都将会被指派一个唯一的 ID(从 1 开始)，一旦消息被投递到所有匹配的队列之后，broker就会发送一个确认给生产者(包含消息的唯一 ID)，这就使得生产者知道消息已经正确到达目的队列了。
     注意：如果消息和队列是持久化的，那么确认消息会在将消息写入磁盘之后发出，broker 回传给生产者的确认消息中 delivery-tag 域包含了确认消息的序列号，此外 broker 也可以设置basic.ack 的 multiple 域，表示到这个序列号之前的所有消息都已经得到了处理。confirm 模式最大的好处在于他是异步的，一旦发布一条消息，生产者应用程序就可以在等信道返回确认的同时继续发送下一条消息，当消息最终得到确认之后，生产者应用便可以通过回调方法来处理该确认消息，如果 RabbitMQ 因为自身内部错误导致消息丢失，就会发送一条 nack 消息，生产者应用程序同样可以在回调方法中处理该 nack 消息。

```java
// 发布确认默认关闭，
Channel channel = connection.createChannel();
channel.confirmSelect(); // 开启发布确认
```



### 单个发布确认：

​    单个发布确认：发送消息后只有它被确认发布，后续的消息才能继续发布。
​    waitForConfirmsOrDie(long)这个方法只有在消息被确认的时候才返回，如果在指定时间范围内这个消息没有被确认那么它将抛出异常。

​     缺点：`发布速度特别的慢`，因为如果没有确认发布的消息就会阻塞所有后续消息的发布，这种方式最多提供每秒不超过数百条发布消息的吞吐量。

生产者：

```java
public class publishMessageIndividually() throws Exception {
    private static final int MESSAGE_COUNT = 1000; // 要发送的消息
    // 消息发送到指定的交换机—信道
    Channel channel = RabbitMqUtils.getChannel();
    // 队列名
    String queueName = UUID.randomUUID().toString();
    /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
    channel.queueDeclare(queueName, false, false, true, null);
    // 开启发布确认
    channel.confirmSelect();
    // 开始时间
    long begin = System.currentTimeMillis();

    // for循环发送消息-单个确认发布
    for (int i = 0; i < MESSAGE_COUNT; i++) {
        String message = i + "";
        /* 发送一个消息
             * 1.消息发送到指定的交换机
             * 2.路由的Key值，  本次是队列的名称
             * 3.其他参信息
             * 4.发送消息的消息体 */
        channel.basicPublish("", queueName, null, message.getBytes("UTF-8")); // 二进制，UTF-8编码
        //            单个消息确认发布
        boolean flag = channel.waitForConfirms();
        if (flag) {
            System.out.println("消息发送成功");
        }
    }
    long end = System.currentTimeMillis();
    System.out.println(("发布" + MESSAGE_COUNT + "条单独确认消息,耗时" + (end - begin)));
}
```



### 批量发布确认：

  批量发布确认：先发布一批消息然后一起确认。
   优点：合理的吞吐量。
   缺点：当发生故障导致发布出现问题时，不知道是哪个消息出现问题了，我们必须将整个批处理保存在内存中，以记录重要的信息而后重新发布消息。当然这种方案仍然是同步的，也一样阻塞消息的发布。

生产者：

```java
public void publishMessageBatch() throws Exception {
    private static final int MESSAGE_COUNT = 1000; // 要发送的消息
    // 消息发送到指定的交换机—信道
    Channel channel = RabbitMqUtils.getChannel();
    // 队列名
    String queueName = UUID.randomUUID().toString();
    // 定义队列
    /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
    channel.queueDeclare(queueName, false, false, false, null);
    // 开启发布确认
    channel.confirmSelect();
    // 开始时间
    long begin = System.currentTimeMillis();

    // for循环发送消息-批量确认发布
    for (int i = 0; i < MESSAGE_COUNT; i++) {
        String message = i + "";
        /* 发送一个消息
             * 1.消息发送到指定的交换机
             * 2.路由的Key值，  本次是队列的名称
             * 3.其他参信息
             * 4.发送消息的消息体 */
        channel.basicPublish("", queueName, null, message.getBytes());

        // 消息达到100条的时候，批量确认一次
        if (i % 100 == 0) {
            // 发布确认
            boolean flag = channel.waitForConfirms();
            if (flag) {
                System.out.println("消息发送成功");
            }
        }
    }
    long end = System.currentTimeMillis();
    System.out.println(("发布" + MESSAGE_COUNT + "条批量确认发布,耗时" + (end - begin)));
}
```



### 异步确认发布：

​     缺点：异步确认编程逻辑比上两个要复杂
​     优点：最佳性能和资源使用，在出现错误的情况下可以很好地控制，但是实现起来稍微难些。  他是利用回调函数来达到消息可靠性传递的，这个中间件也是通过函数回调来保证是否投递成功。

​       **如何处理异步未确认消息**：把未确认的消息放到一个基于内存的能被发布线程访问的队列，比如说用 ConcurrentLinkedQueue 这个队列在 confirm callbacks 与发布线程之间进行消息的传递。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E7%A1%AE%E8%AE%A4-%E5%BC%82%E6%AD%A5%E7%A1%AE%E8%AE%A4%E5%8F%91%E5%B8%83%E6%B5%81%E7%A8%8B.png)

```java
public void publishMessageAsync2() throws Exception {
    private static final int MESSAGE_COUNT = 1000; // 要发送的消息

    // 消息发送到指定的交换机—信道
    Channel channel = RabbitMqUtils.getChannel();
    String queueName = UUID.randomUUID().toString();// 队列名
    // 定义队列
    /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
    channel.queueDeclare(queueName, false, false, true, null);
    // 开启发布确认
    channel.confirmSelect();

    /** ① 定义对象
         *  线程安全有序的一个哈希表，适用于高并发的情况下
         *  优点： 1.轻松的将序号与消息进行管理
         *        2.轻松批量删除条目，只要给到序号
         *        3.支持高并发（多线程） */
    ConcurrentSkipListMap<Long, String> outstandingConfirms = new ConcurrentSkipListMap<>();

    // 监听发送成功的消息-回调函数： 成功的消息，是否为批量确认
    ConfirmCallback ackCallback = (deliveryTag, multiple) -> {
        if (multiple) {
            // 获取未确认的消息
            ConcurrentNavigableMap<Long, String> confirmed = outstandingConfirms.headMap(deliveryTag);
            confirmed.clear(); //删除已
        } else {
            outstandingConfirms.remove(deliveryTag);
        }
        System.out.println("已确认消息：" + deliveryTag + "，" + multiple);
    };
    //  发送失败的消息-回调函数
    ConfirmCallback nackCallback = (deliveryTag, multiple) -> {
        /* ④获取未确认的消息*/
        String message = outstandingConfirms.remove(deliveryTag);
        System.out.println("未确认消息：" + message + "未确认消息的Tag：" + deliveryTag + "，" + multiple);
    };
    // 异步确认发布-消息监听器： 监听发送成功的消息，发送失败的消息
    channel.addConfirmListener(ackCallback, nackCallback);

    // 开始时间
    long begin = System.currentTimeMillis();

    // for循环发送消息-异步确认发布
    for (int i = 0; i < MESSAGE_COUNT; i++) {
        String message = i + ""; // 消息
        /* 发送一个消息
             * 1.消息发送到指定的交换机
             * 2.路由的Key值，  本次是队列的名称
             * 3.其他参信息
             * 4.发送消息的消息体 */
        channel.basicPublish("", queueName, null, message.getBytes());
        // ②。记录下所有要发送的消息
        outstandingConfirms.put(channel.getNextPublishSeqNo(), message);
    }
    // 结束时间
    long end = System.currentTimeMillis();
    System.out.println(("发布" + MESSAGE_COUNT + "条异步确认发布,耗时" + (end - begin)));
}
```







## 消息的可靠投递：

rabbitmq 整个消息投递的路径为：

`producer--->rabbitmq broker--->exchange--->queue--->consumer`

⚫ 消息从 producer 到 exchange 则会返回一个 confirmCallback 。 

⚫ 消息从 exchange-->queue 投递失败则会返回一个 returnCallback 。

   注意：RabbitMQ中也提供了事务机制，但是性能较差，此处不做讲解。

```java
 使用channel下列方法，完成事务控制：    
txSelect() // 用于将当前channel设置成transaction模式
txCommit() // 用于提交事务
txRollback() // 用于回滚事务
```



### Confirms(确认)：

​     Confirms(确认)解决交换机异常。

1. 产生原因：在生产环境中由于一些不明原因，导致 RabbitMQ 重启，在 RabbitMQ 重启期间生产者消息投递失败，导致消息丢失，需要手动处理和恢复。

2. Confirms(确认)：交换机或队列因宕机等原因，怕消息掉失，把消息存储起来。

   交换机或队列因宕机等导致的异常:

```java
    应 用 [xxx] 在 [08-1516:36:04] 发 生 [ 错误日志异常 ] ， alertId=[xxx]。 由
[org.springframework.amqp.rabbit.listener.BlockingQueueConsumer:start:620] 触发。
    应用 xxx 可能原因如下服务名为： 异常为：org.springframework.amqp.rabbit.listener.BlockingQueueConsumer:start:620, 产 生 原 因 如 下 :1.org.springframework.amqp.rabbit.listener.QueuesNotAvailableException: Cannot prepare queue for listener. Either the queue doesn"'"t exist or the broker will not allow us to use it.||Consumer received fatal=false exception on startup:
```

 

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E7%A1%AE%E8%AE%A4%E6%A8%A1%E5%BC%8F-%E7%A1%AE%E8%AE%A4.png)



#### yml：

```yaml
spring:
  rabbitmq: # 整合RabbitMQ
    host: 192.168.139.140 # ip
    port: 5672 # 端口
    username: qsl
    password: 123456
    publisher-confirm-type: correlated  # 设置发布确认-确认
```

⚫NONE: 禁用发布确认模式(默认)
⚫CORRELATED:发布消息成功到交换器后会触发回调方法
⚫SIMPLE:有两效果，一效果和 CORRELATED 值一样会触发回调方法，
二在发布消息成功后使用 rabbitTemplate 调用 waitForConfirms 或waitForConfirmsOrDie 方法,等待 broker 节点返回发送结果，根据返回结果来判定下一步的逻辑，
    注意：waitForConfirmsOrDie 方法如果返回 false 则会关闭 channel(信道)，接下来则无法发送消息到 broker。



#### Config/ConfirmsConfig:

```java
import org.springframework.amqp.core.*;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/* 生产者-发布确认模式_确认*/
@Configuration //配置类
public class ConfirmsConfig {
    // 交换机名称
    public static final String CONFIRM_EXCHANGE = "confirm.exchange";
    // 队列名称
    public static final String CONFIRM_QUEUE = "confirm.queue";
    // routingKey
    public static final String CONFIRM_ROUTING_KEY = "key1";

    // 定义交换机
    @Bean
    public DirectExchange confirmExchange() {
    }

    // 定义队列
    @Bean
    public Queue confirmQueue() {
        return QueueBuilder.durable(CONFIRM_QUEUE).build();
    }


    //绑定  交换机 绑定 队列
    @Bean // 注入交换机和 队列
    public Binding queueBindingExchange(@Qualifier("confirmQueue") Queue confirmQueue,
                                        @Qualifier("confirmExchange") DirectExchange confirmExchange){
        // 队列，交换机，routingKey
        return BindingBuilder.bind(confirmQueue).to(confirmExchange).with(CONFIRM_ROUTING_KEY);
    }
}
```

#### Config/MyCallBack:

```java
mport lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;

/**
 * 发布确认-确认: 消息生产者发布消息后的回调接口
 * 注意：只要生产者发布消息，交换机不管是否收到消息，都会调用该类的 confirm 方法
 */
@Slf4j
@Component // 默认bean
public class MyCallBack implements RabbitTemplate.ConfirmCallback {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    /** 当前类注入到RabbitTemplate的ConfirmCallback方法中
     * 注意：   @PostConstruct: 此注解在其他注解执行完，才执行。
     */
    @PostConstruct
    public void init() {
        rabbitTemplate.setConfirmCallback(this);
    }

    /**
     * 交换机不管是否收到消息都会回调此方法
     * 1. 交换机收到的消息
     * correlationData： 保存了回调信息的Id相关信息
     * ack:  true:交换机收到消息   false:交换机未收到消息
     * cause： 未收到消息的原因
     */
    @Override
    public void confirm(CorrelationData correlationData, boolean ack, String cause) {
        String id = correlationData.getId() != null ? correlationData.getId() : "";

        if (ack) { // 成功接收消息
            log.info("交换机已经收到了ID为:{}的消息", id);
        } else {
            log.info("交换机已经收到了ID为:{}的消息", id);
        }
    }
}
```

#### 生产者：

```java
import com.qsl.rabbitmq.config.ConfirmsConfig;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@Slf4j
@Api(tags = "生产者-发布确认模式_确认")
@RestController // Controller
@RequestMapping("confirm")
public class ConfirmsController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @ApiOperation("生产者发送消息-发布确认模式_确认") // Swagger注释
    @GetMapping("/sendMessageConfirm/{message}")
    public String sendMessageConfirm(@PathVariable("message") String message) {
        // 指定消息 id 为 1
        CorrelationData correlationData = new CorrelationData("1");

        // 占位符
        log.info("当前时间：{}，发送消息给confirm队列:{}" + new Date().toString(), message);
        //  交换机，routingKey,要发送的消息,id等参数
        rabbitTemplate.convertAndSend(ConfirmsConfig.CONFIRM_EXCHANGE, ConfirmsConfig.CONFIRM_ROUTING_KEY,
                                      "消息来自发布确认模式_确认：" + message, correlationData);
        return "生产者发送消息:" + message;
    }
}
```

#### 消费者：

consumer/ConfirmsConsumer:

```java
import com.qsl.rabbitmq.config.ConfirmsConfig;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;


/*消费者-发布确认模式_确认*/
@Slf4j
@Component //默认bean
public class ConfirmsConsumer {


    /**消费者-发布确认模式_确认
     * @param message
     */
    @RabbitListener(queues = ConfirmsConfig.CONFIRM_QUEUE)  // 监听器-监听confirm队列
    public void receiveConfirmMessage(Message message) {
        String msg = new String(message.getBody());
        log.info("接受到的队列confirm.queue消息:{}",msg);
    }
}
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E7%A1%AE%E8%AE%A4%E6%A8%A1%E5%BC%8F-%E7%A1%AE%E8%AE%A4%E7%BB%93%E6%9E%9C.png)

注意：队列丢弃的消息交换机是不知道的，需要解决告诉生产者消息传送失败。



  

### Return(消息回退):

消息回退:

1. 在仅开启了生产者确认机制的情况下，交换机接收到消息后，会直接给消息生产者发送确认消息，如果发现该消息不可路由，那么消息会被直接丢弃，此时生产者是不知道消息被丢弃这个事件的。通过设置 mandatory 参数可以在当消息传递过程中 报异常等地时将消息返回给生产者。
2. 不可路由：无队列或队列丢失，交换机路由不到队列，导致消息丢失。

​    消息回退解决队列异常。

#### 依赖：

```yaml
spring:
  rabbitmq: # 整合RabbitMQ
    host: 192.168.139.140 # ip
    port: 5672 # 端口
    username: qsl
    password: 123456
    publisher-confirm-type: correlated  # 设置发布确认-确认
    publisher-returns: true # 开启消息回退
```

#### config/MyCallBack:

```java

/**
 * 发布确认模式-确认，消息回退
 * 确认: 消息生产者发布消息后的回调接口
 * 注意：只要生产者发布消息，交换机不管是否收到消息，都会调用该类的 confirm 方法
 */
@Slf4j
@Component // 默认bean
public class MyCallBack implements RabbitTemplate.ConfirmCallback, RabbitTemplate.ReturnsCallback {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    /**
     * 当前类注入到RabbitTemplate的ConfirmCallback方法中
     * 注意：   @PostConstruct: 此注解在其他注解执行完，才执行。
     */
    @PostConstruct
    public void init() {
        rabbitTemplate.setConfirmCallback(this); // 确认注入
        rabbitTemplate.setReturnsCallback(this); // 消息回退注入
    }

    /**
     * 确认
     * 交换机不管是否收到消息都会回调此方法
     * 1. 交换机收到的消息
     * correlationData： 保存了回调信息的Id相关信息
     * ack:  true:交换机收到消息   false:交换机未收到消息
     * cause： 未收到消息的原因
     */
    @Override
    public void confirm(CorrelationData correlationData, boolean ack, String cause) {
        String id = correlationData.getId() != null ? correlationData.getId() : "";

        if (ack) { // 成功接收消息
            log.info("交换机已经收到了ID为:{}的消息", id);
        } else {
            log.info("交换机还未收到ID为:{}的消息，由于原因:{}", id, cause);
        }
    }

    //只有不可达目的地（报异常等）的时候 才进行回退   注意： 消息回退给生产者
    /**
     * 消息回退
     * 当消息无法路由的时候的回调方法
     * message      消息
     * replyCode    编码
     * replyText    退回原因
     * exchange     从哪个交换机退回
     * routingKey   通过哪个路由 key 退回
     */
    @Override
    public void returnedMessage(ReturnedMessage returned) {
        log.error("消息{},被交换机{}退回，退回原因:{},路由key:{}", returned.getMessage().getBody().toString()
                  , returned.getExchange(), returned.getReplyText(), returned.getRoutingKey());
    }
}
```

 生产者及消费者参考确认。。。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E7%A1%AE%E8%AE%A4%E6%A8%A1%E5%BC%8F-%E6%B6%88%E6%81%AF%E5%9B%9E%E9%80%80.png)



## 备份交换机：

有了mandatory 参数和回退消息，我们获得了对无法投递消息的感知能力，有机会在生产者的消息无法被投递时发现并处理。但有时候，我们并不知道该如何处理这些无法路由的消息，最多打个日志，然后触发报警，再来手动处理。而通过日志来处理这些无法路由的消息是很不优雅的做法，特别是当生产者所在的服务有多台机器的时候，手动复制日志会更加麻烦而且容易出错。而且设置mandatory参数会增加生产者的复杂性，需要添加处理这些被退回的消息的逻辑。

如果既不想丢失消息，又不想增加生产者的复杂性  ，不可路由消息根本没有机会进入到队列，因此无法使用死信队列来保存消息。

备份交换机：为某一个交换机设置备份交换机时，当交换机接收到一条不可路由消息时，将会把这条消息转发到备份交换机中，由备份交换机来进行转发和处理，通常备份交换机的类型为Fanout，这样就能把所有消息都投递到与其绑定的队列中，然后我们在备份交换机下绑定两个队列：①备份队列：备份那些原交换机无法被路由的消息。②报警队列：用独立的消费者来进行监测和报警。



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%A4%87%E4%BB%BD%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B.png)

  注意：备份交换机 优先级高于 消息回退。

### Config/BackupConfig:

```java
import org.springframework.amqp.core.*;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

/* 备份交换机 */
@Component // 配置类
public class BackupConfig {
    /*======交换机名称========*/
    // 普通交换机名称
    public static final String CONFIRM_EXCHANGE = "confirm.exchange";
    // 备份交换机
    public static final String BACKUP_EXCHANGE = "backup.exchange";
    /*======队列名称========*/
    // 普通队列名称
    public static final String CONFIRM_QUEUE = "confirm.queue";
    // 备份队列名称
    public static final String BACKUP_QUEUE = "backup.queue";
    // 报警队列名称
    public static final String WARNING_QUEUE = "warning.queue";
    // 普通交换机routingKey
    public static final String CONFIRM_ROUTING_KEY = "key1";

    /*============== 交换机 ==============  */
    // 定义普通交换机
    @Bean
    public DirectExchange confirmExchange() {
        //        普通交换机 连接 备份交换机     durable：是否持久
        return ExchangeBuilder.directExchange(CONFIRM_EXCHANGE).durable(true)
            .withArgument("alternate-exchange", BACKUP_EXCHANGE).build();
    }

    // 备份交换机
    @Bean
    public FanoutExchange backupExchange() {
        return new FanoutExchange(BACKUP_EXCHANGE);
    }

    /*============== 队列 ==============  */
    // 定义队列
    @Bean
    public Queue confirmQueue() {
        return QueueBuilder.durable(CONFIRM_QUEUE).build();
    }

    // 定义备份队列
    @Bean
    public Queue backupQueue() {
        return QueueBuilder.durable(BACKUP_QUEUE).build();
    }

    // 定义报警队列
    @Bean
    public Queue warningQueue() {
        return QueueBuilder.durable(WARNING_QUEUE).build();
    }

    /*============== 绑定 ==============  */
    //绑定  普通交换机 绑定 普通队列
    @Bean // 注入交换机和 队列
    public Binding queueBindingExchange(@Qualifier("confirmQueue") Queue confirmQueue,
                                        @Qualifier("confirmExchange") DirectExchange confirmExchange) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(confirmQueue).to(confirmExchange).with(CONFIRM_ROUTING_KEY);
    }

    //绑定  备份交换机 绑定 备份队列
    @Bean
    public Binding backupExchangeBindingbackupQueue(@Qualifier("backupExchange") FanoutExchange backupExchange,
                                                    @Qualifier("backupQueue") Queue backupQueue) {
        return BindingBuilder.bind(backupQueue).to(backupExchange);
    }

    //绑定  备份交换机 绑定 报警队列
    @Bean
    public Binding backupExchangeBindingwarningQueue(@Qualifier("warningQueue") Queue warningQueue,
                                                     @Qualifier("backupExchange") FanoutExchange backupExchange) {
        return BindingBuilder.bind(warningQueue).to(backupExchange);
    }
}
```

### 生产者：

Controller/BackupController:

```java
import com.qsl.rabbitmq.config.BackupConfig;
import com.qsl.rabbitmq.config.ConfirmsConfig;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@Slf4j
@Api(tags = "生产者-备份交换机")
@RestController // Controller
@RequestMapping("backup")
public class BackupController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @ApiOperation("生产者发送消息-备份交换机") // Swagger注释
    @GetMapping("/sendMessageConfirm/{message}")
    public String sendMessageConfirm(@PathVariable("message") String message) {
        // 占位符
        log.info("当前时间：{}，发送消息给confirm队列:{}" + new Date().toString(), message);
        //  交换机，routingKey,要发送的消息,id等参数
        rabbitTemplate.convertAndSend(BackupConfig.CONFIRM_EXCHANGE, BackupConfig.CONFIRM_ROUTING_KEY,
                                      "消息来自发布确认模式_确认：" + message);
        return "生产者发送消息:" + message;
    }
}
```

### 消费者：

Consumer/BackupConsumer:

```java
import com.qsl.rabbitmq.config.BackupConfig;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/*备份交换机*/
@Slf4j
@Component // 配置类
public class BackupConsumer {

    /* 消费者-备份交换机_备份队列 */
    @RabbitListener(queues = BackupConfig.CONFIRM_QUEUE) // 监听器-监听普通队列
    public void receiveconfirmessage(Message message) {
        String msg = new String(message.getBody());
        log.info("接受到的队列confirm.queue消息:{}",msg);

    }

    /*消费者-备份交换机_备份队列*/
    @RabbitListener(queues = BackupConfig.BACKUP_QUEUE) // 监听器-监听备份队列
    public void receiveBackupMessage(Message message) {
        String msg = new String(message.getBody());
        log.info("接受到的队列backup.queue消息:{}",msg);

    }

    /*消费者-备份交换机_报警队列*/
    @RabbitListener(queues = BackupConfig.WARNING_QUEUE) // 监听器-监听报警队列
    public void receiveWarningMessage(Message message) {
        String msg = new String(message.getBody());
        log.info("接受到的队列wirning.queue消息:{}",msg);
    }
}
```



### 结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%A4%87%E4%BB%BD%E4%BA%A4%E6%8D%A2%E6%9C%BA%E7%BB%93%E6%9E%9C.png)



一个消息只消费一次：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E7%AE%80%E5%8D%95%E3%80%81%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F%E3%80%81%E7%AE%80%E5%8D%95%E3%80%81%E5%B7%A5%E4%BD%9C%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.jpg)

一个消息消费多次：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%8F%91%E5%B8%83%E3%80%81%E8%AE%A2%E9%98%85%E6%A8%A1%E5%BC%8F%E6%B5%81%E7%A8%8B.jpg)





## 临时队列:

​    注意： 消费者根据队列名称去消费指定队列内的消息。

​     临时队列： 生成一个临时队列，队列的名称随机生成；当消费者断开和该队列的连接，队列将被自动删除。

```java
// 创建临时队列
String queueName = channel.queueDeclare().getQueue();
// 定义队列
/* 定义一个队列
    * 1.队列名称
    * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
    * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
    * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
    * 5.其他参数*/
channel.queueDeclare(queueName, false, false, true, null);
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%B8%B4%E6%97%B6%E9%98%9F%E5%88%97.png)





## bindings(绑定):

​    binding: exchange 和那个queue进行了绑定关系。其实就是 exchange(交换机) 和 queue(队列)之间的桥梁。

​    X 与 Q1 和 Q2 进行了绑定：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E7%BB%91%E5%AE%9A%E6%B5%81%E7%A8%8B.jpg)

wen页面绑定：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%BA%A4%E6%8D%A2%E6%9C%BA%E7%BB%91%E5%AE%9A-bindings%E9%98%9F%E5%88%97.jpg)

编码绑定：

```java
// 交换机名称
private static final String EXCHAGE_NAME = "direct_logs"; 
// 队列名称
private static final String QUEUE_NAME = "queue_logs"; 

/* 定义一个交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.DIRECT);
System.out.println("消费者1接收消息： ");

// 定义队列
/* 定义一个队列
     * 1.队列名称
     * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
     * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
     * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
     * 5.其他参数*/
channel.queueDeclare(QUEUE_NAME, false, false, true, null);

/* 绑定交换机: 队列，交换机，routingKey(可使用表达式)  */
channel.queueBind(QUEUE_NAME, EXCHAGE_NAME, "info");//方法1
channel.queueBind(queueName, EXCHAGE_NAME, "*.orange.*");//方式2
```



# 交换机：

## 原理：

​    RabbitMQ 消息传递模型的核心思想是: **生产者生产的消息从不会直接发送到队列**。实际上，通常生产者甚至都不知道这些消息传递传递到了哪些队列中。
​    相反，**生产者只能将消息发送到交换机(exchange)**，交换机工作的内容非常简单，一方面它接收来自生产者的消息，另一方面将它们推入队列。

  交换机必须确切知道如何处理收到的消息。 而消息是由交换机的类型来决定怎么处理消息。  例：应该把这些消息放到特定队列，还是说把他们放到许多队列中,还是说应该丢弃它们。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B6%88%E6%81%AF%E7%9A%84%E5%A4%84%E7%90%86%E6%B5%81%E7%A8%8B.png)

##       交换机类型：

1. ​      Direct（无名/直接/路由类型,==默认交换机==）：最简单的一种交换机，它会将消息直接发送到指定的队列。它通过Routing Key来确定消息需要被发送到哪个队列，只有当消息的Routing Key与绑定Queue的Routing Key完全匹配时，消息才会被发送到该队列。

2. ​      Fanout（扇形/发布订阅类型）:Fanout交换机会把所有发送到该交换机的消息路由到所有与之绑定的队列中，忽略Routing Key。所以所有连接到这个交换机的队列都会收到相同的消息。 ==注意：Fanout 交换机转发消息是最快的。==

3. ​      Topic（主题交换机）：Topic交换机根据Routing Key和通配符来确定消息应该被发送到哪个队列。其中，Routing Key包含一个或多个单词，以“.”分隔，例如：“us.news.sports”；而通配符则有两种：*（匹配一个单词）和#（匹配零个或多个单词）。通过使用这些通配符，Topic交换机可以将消息路由到多个队列中。

4. ​     Headers（标题/报头交换机）：Headers交换机不依赖于Routing Key来路由消息，而是根据消息头部信息的键值对来匹配队列。当消息头部信息中的键值对与队列绑定的参数完全匹配时，消息才会被路由到该队列中。



 生产者：
     注意： 消息能路由发送到队列中其实是由 routingKey(bindingkey)绑定 key 指定的，如果它存在的话。

```java
/* 发送一个消息
  * 1.发送指定的交换机  空字符串表示默认或无名称交换机
  * 2.路由的Key值，  本次是队列的名称
  * 3.其他参信息
  * 4.发送消息的消息体 */
channel.basicPublish("", queueName, null, message.getBytes());
```

mq交换机类型对应图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%BA%A4%E6%8D%A2%E6%9C%BA%E7%B1%BB%E5%9E%8B.jpg)







### Fanout(扇形/发布订阅)：

​     ”发布/订阅模式”: 假设工作队列背后，每个任务都恰好交付给一个消费者(工作进程)。在这一部分中，我们将做一些完全不同的事情-我们将消息传达给多个消费者，为了说明这种模式，我们将构建一个简单的日志系统。它将由两个程序组成:第一个程序将发出日志消息，第二个程序是消费者。其中我们会启动两个消费者，其中一个消费者接收到消息后把日志存储在磁盘，另外一个消费者接收到消息后把消息打印在屏幕上，事实上第一个程序发出的日志消息将广播给所有消费者者。

> 一个发送，多个接受，发布/订阅模式.

​    Logs(交换机)和临时队列的绑定关系图:

​                      ==注意：此处Routing Key为空字符串。==

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Fanout%E6%89%87%E5%BD%A2%E4%BA%A4%E6%8D%A2%E6%9C%BA%E7%BB%91%E5%AE%9A%E9%98%9F%E5%88%97.png)

Fanout扇形交换机流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Fanout%E6%89%87%E5%BD%A2%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B.png)

```js
EmitLog //生产者：发送消息
ReceiveLogs01 //消费者：将接收到的消息打印在控制台
ReceiveLogs02 //消费者：将接收到的消息打印在控制台,并写入到文件
```

#### 生产者：

```java
/** 生产者-交换机_Fanout(扇形/发布订阅)*/
public class EmitLog {
    private static final String EXCHANGE_NAME = "logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者你请输入消息： ");

        /* 定义一个交换机：  交换机名称， 交换机类型-扇形/发布订阅(枚举，字符串都行）*/
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
        //        channel.exchangeDeclare(EXCHANGE_NAME, "fanout");

        //        控制台获取消息
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String message = scanner.next();
            //            发送消息
            /*
             * 1.发送指定的交换机
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(EXCHANGE_NAME, "", null, message.getBytes("UTF-8"));
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

#### 消费者：

##### ReceiveLogs01:

```java
/** 消费者1：交换机_Fanout(扇形/发布订阅)*/
public class ReceiveLogs01 {

    private static final String EXCHANGE_NAME = "logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");


        //  创建临时队列:   生成一个临时队列，队列的名称随机生成；当消费者断开和该队列的连接，队列将被自动删除。
        String queueName = channel.queueDeclare().getQueue();

        /* 绑定交换机与队列: 队列，交换机，routingKey  */
        channel.queueBind(queueName, EXCHANGE_NAME, "");


        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String mag = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + mag);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        // 发送消息
        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(queueName, false, deliverCallback, cancelCallback);
    }
}
```

##### ReceiveLogs02:

```java
/* 消费者2：交换机_Fanout(扇形/发布订阅)*/
public class ReceiveLogs02 {

    private static final String EXCHANGE_NAME = "logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者2接收消息： ");

        //  创建临时队列:   生成一个临时队列，队列的名称随机生成；当消费者断开和该队列的连接，队列将被自动删除。
        String queueName = channel.queueDeclare().getQueue();

        /* 绑定交换机与队列: 队列，交换机，routingKey  */
        channel.queueBind(queueName,EXCHANGE_NAME,"");


        //  声明-接收的消费
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
            // 消息写入文件
            File file = new File("src/main/java/com/qsl/rabbitmq/a.txt");
            FileUtils.writeStringToFile(file, msg, "UTF-8");
            System.out.println("数据成功写入文件");
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        // 发送消息
        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(queueName, false, deliverCallback, cancelCallback);
    }
}
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Fanout-%E6%89%87%E5%BD%A2%E3%80%81%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85.jpg)



### Direct(无名/直接/路由类型,==默认交换机==):

​       日志系统: 例如我们希望将日志消息写入磁盘的程序仅接收严重错误(errros)，而不存储哪些警告(warning)或信息(info)日志 消息避免浪费磁盘空间。Fanout 这种交换类型并不能给我们带来很大的灵活性-它只能进行无意识的广播，在这里我们将使用 direct 这种类型来进行替换，这种类型的工作方式是，消息只去到它绑定的 routingKey 队列中去。

```java
// 定义交换机名称  
private static final String EXCHAGE_NAME = "direct_logs"; 

/* 定义一个交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.DIRECT);
```

#### 单个绑定：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Direct_%E6%97%A0%E5%90%8D%E3%80%81%E7%9B%B4%E6%8E%A5%E3%80%81%E8%B7%AF%E7%94%B1%E7%B1%BB%E5%9E%8B,%E9%BB%98%E8%AE%A4%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B.jpg)

​     在这种绑定情况下，生产者发布消息到 exchange(交换机) 上，绑定键为 orange 的消息会被发布到队列 Q1。绑定键为 black和green的消息会被发布到队列 Q2，其他消息类型的消息将被丢弃。

#### 多重绑定：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Direct_%E6%97%A0%E5%90%8D%E3%80%81%E7%9B%B4%E6%8E%A5%E3%80%81%E8%B7%AF%E7%94%B1%E7%B1%BB%E5%9E%8B,%E9%BB%98%E8%AE%A4%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B-%E5%A4%9A%E9%87%8D%E7%BB%91%E5%AE%9A.jpg)

​      若 exchange(交换机)的绑定类型是direct，**但是它绑定的多个队列的 key 如果都相同**，在这种情况下虽然绑定类型是 direct **但是它表现的就和 fanout 有点类似了**，就跟广播差不多。



#### 多重绑定2：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Direct_%E6%97%A0%E5%90%8D%E3%80%81%E7%9B%B4%E6%8E%A5%E3%80%81%E8%B7%AF%E7%94%B1%E7%B1%BB%E5%9E%8B,%E9%BB%98%E8%AE%A4%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B22.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Direct_%E6%97%A0%E5%90%8D%E3%80%81%E7%9B%B4%E6%8E%A5%E3%80%81%E8%B7%AF%E7%94%B1%E7%B1%BB%E5%9E%8B,%E9%BB%98%E8%AE%A4%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B22-1.png)

   注意：生产者生产消息后，如果没有对应的消费者接收，则该消息被遗弃。

```apl
C1 消费者：绑定 console 队列，routingKey 为 info、warning
C2 消费者：绑定 disk 队列，routingKey 为 error
```



##### 生产者：

```java
public class DirectLogs {

    private static final String EXCHAGE_NAME = "direct_logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者你请输入消息： ");

        /* 定义一个交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        //        channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.DIRECT); // 此处可不声明

        //  控制台获取消息
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String message = scanner.next();
            //   发送消息
            /*
             * 1.消息发送到指定的交换机
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(EXCHAGE_NAME, "error", null, message.getBytes("UTF-8"));
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

##### 消费者：

###### ReceiveLogsDirect01：

```java
public class ReceiveLogsDirect01 {

    private static final String EXCHAGE_NAME = "direct_logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道
        Channel channel = RabbitMqUtils.getChannel();

        /* 定义一个交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.DIRECT);
        System.out.println("消费者1接收消息： ");

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare("console", false, false, true, null);

        /* 绑定交换机: 队列，交换机，routingKey  */
        channel.queueBind("console", EXCHAGE_NAME, "info");
        channel.queueBind("console", EXCHAGE_NAME, "warning");

        //  消费者成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        // 发送消息
        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否使用自动应答，  true:自动应答   false：手动应答
         * 3.消费者成功消费的回调
         * 4.消费者取消消费的回调
         */
        channel.basicConsume("console", false, deliverCallback, cancelCallback);
    }
}
```



###### ReceiveLogsDirect02：

```java
public class ReceiveLogsDirect02 {

    private static final String EXCHAGE_NAME = "direct_logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道_信道
        Channel channel = RabbitMqUtils.getChannel();

        /* 定义一个交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.DIRECT);
        System.out.println("消费者2接收消息： ");


        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare("disk", false, false, true, null);

        /* 绑定交换机: 队列，交换机，routingKey  */
        channel.queueBind("disk", EXCHAGE_NAME, "error");

        //  消费者成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        // 发送消息
        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否使用自动应答，  true:自动应答   false：手动应答
         * 3.消费者成功消费的回调
         * 4.消费者取消消费的回调
         */
        channel.basicConsume("disk", false, deliverCallback, cancelCallback);
    }
}
```



### Topic(主题交换机)：

​      使用要求：发送到类型是 topic 交换机的消息的 routing_key 不能随意写，必须满足一定的要求，它必须是**一个单词列表**，**以点号分隔开**。这些单词可以是任意单词比如说："stock.usd.nyse", "nyse.vmw", "quick.orange.rabbit" 这种类型的。当然这个单词列表最多不能超过 255 个字节。

```java
// 定义交换机名称
private static final String EXCHAGE_NAME = "Topics_logs"; 

/* 定义一个交换机：  交换机名称， 交换机类型-交换机_Topics(主题交换机)(枚举，字符串都行）*/
channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.TOPIC);

```



在这个规则列表中，其中有两个替换符：
- ***(星号)可以代替一个位置**
- **#(井号)可以替代零个或多个位置**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Topic_%E4%B8%BB%E9%A2%98%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B.jpg)

  web页面:topic_logs(交换机)和临时队列的绑定关系图

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Topic_%E4%B8%BB%E9%A2%98%E4%BA%A4%E6%8D%A2%E6%9C%BA%E7%BB%91%E5%AE%9A%E9%98%9F%E5%88%97.png)

- Q1-->绑定的是
  - 中间带 orange 带 3 个单词的字符串 `(*.orange.*)`
- Q2-->绑定的是
  - 最后一个单词是 rabbit 的 3 个单词 `(*.*.rabbit)`
  - 第一个单词是 lazy 的多个单词 `(lazy.#)`

| 例子                     | 说明                                       |
| ------------------------ | ------------------------------------------ |
| quick.orange.rabbit      | 被队列 Q1Q2 接收到                         |
| lazy.orange.elephant     | 被队列 Q1Q2 接收到                         |
| quick.orange.fox         | 被队列 Q1 接收到                           |
| lazy.brown.fox           | 被队列 Q2 接收到                           |
| lazy.pink.rabbit         | 虽然满足两个绑定但只被队列 Q2 接收一次     |
| quick.brown.fox          | 不匹配任何绑定不会被任何队列接收到会被丢弃 |
| quick.orange.male.rabbit | 是四个单词不匹配任何绑定会被丢弃           |
| lazy.orange.male.rabbit  | 是四个单词但匹配 Q2                        |

 注意:当队列绑定关系是下列这种情况时
    当一个队列绑定键是#,那么这个队列将接收所有数据，就有点像fanout了。
    如果队列绑定键当中没有#和出现，那么该队列绑定类型就是direct了。

#### 生产者：

```java
public class EmitLog {

    private static final String EXCHAGE_NAME = "Topics_logs"; // 定义交换机名称
    private static Map<String, String> bindingKeyMap;

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道
        Channel channel = RabbitMqUtils.getChannel();

        /* 定义一个交换机：  交换机名称， 交换机类型-交换机_Topics(主题交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(EXCHAGE_NAME, BuiltinExchangeType.TOPIC);

        // 定义要发送的消息
        Map<String, String> bindingKeyMap = new HashMap<>();
        bindingKeyMap.put("quick.orange.rabbit", "被队列 Q1Q2 接收到");
        bindingKeyMap.put("lazy.orange.elephant", "被队列 Q1Q2 接收到");
        bindingKeyMap.put("quick.orange.fox", "被队列 Q1 接收到");
        bindingKeyMap.put("lazy.brown.fox", "被队列 Q2 接收到");
        bindingKeyMap.put("lazy.pink.rabbit", " 虽然满足两个绑定但只被队列 Q2 接收一次");
        bindingKeyMap.put("quick.brown.fox", " 不匹配任何绑定不会被任何队列接收到会被丢弃");
        bindingKeyMap.put("quick.orange.male.rabbit", " 是四个单词不匹配任何绑定会被丢弃");
        bindingKeyMap.put("lazy.orange.male.rabbit", " 是四个单词但匹配 Q2");
        System.out.println("生产者发送消息： " + bindingKeyMap);

        for (Map.Entry<String, String> bindingKeyEntry : bindingKeyMap.entrySet()) {
            String routingKey = bindingKeyEntry.getKey(); // key
            String message = bindingKeyEntry.getValue(); // 消息

            //   发送消息
            /*
             * 1.消息发送到指定的交换机
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(EXCHAGE_NAME, routingKey, null, message.getBytes("UTF-8"));
        }
    }
}
```

#### 消费者：

##### ReceiveLogs01：

```java
public class ReceiveLogs01 {

    private static final String EXCHAGE_NAME = "Topics_logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");
        //  声明队列
        String queueName = "Q1";

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(queueName, false, false, true, null);
        // 绑定
        channel.queueBind(queueName, EXCHAGE_NAME, "*.orange.*");

        //  消费者成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
            System.out.println("接收队列：" + queueName + "绑定键：" + message.getEnvelope().getRoutingKey());
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否使用自动应答，  true:自动应答   false：手动应答
         * 3.消费者成功消费的回调
         * 4.消费者取消消费的回调
         */
        channel.basicConsume(queueName, false, deliverCallback, cancelCallback);
    }
}
```



##### ReceiveLogs02：

```java
public class ReceiveLogs02 {

    private static final String EXCHAGE_NAME = "Topics_logs"; // 定义交换机名称

    public static void main(String[] args) throws Exception {
        // 消息发送到指定的交换机—信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者2接收消息： ");
        //  声明队列
        String queueName = "Q2";

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(queueName,false,false,true,null);
        // 绑定
        channel.queueBind(queueName, EXCHAGE_NAME, "*.*.rabbit");
        channel.queueBind(queueName, EXCHAGE_NAME, "lazy.#");

        //  消费者成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
            System.out.println("接收队列：" + queueName + "绑定键：" + message.getEnvelope().getRoutingKey());
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println("消息被中断");
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否使用自动应答，  true:自动应答   false：手动应答
         * 3.消费者成功消费的回调
         * 4.消费者取消消费的回调
         */
        channel.basicConsume(queueName, false, deliverCallback, cancelCallback);
    }
}
```



# 队列：

​    队列具备两种模式：default(默认)和 lazy 3.6.0+。

​     lazy模式即为惰性队列的模式，可以通过调用 channel.queueDeclare 方法的时候在参数中设置，也可以通过Policy 的方式设置，如果一个队列同时使用这两种方式设置的话，那么 Policy 的方式具备更高的优先级。如果要通过声明的方式改变已有队列的模式的话，那么只能先删除队列，然后再重新声明一个新的。



## 死信队列:

   死信：无法被消费的消息。

​     一般来说，producer 将消息投递到 broker(交换机/队列)或者直接到queue(队列) 里了，consumer(消费者) 从 queue 取出消息 进行消费，但某些时候由于特定的原因**导致 queue 中的某些消息无法被消费**，这样的消息如果没有后续的处理，就变成了死信，有死信自然就有了死信队列。

​    应用场景：为了保证订单业务的消息数据不丢失，需要使用到 RabbitMQ 的死信队列机制，当消息消费发生异常时，将消息投入死信队列中。还有比如说：用户在商城下单成功并点击去支付后在指定时间未支付时自动失效。

 消息成为死信的三种情况:

- 消息 TTL(Time To Live/存活时间) 过期
- 队列达到最大长度(队列满了，无法再添加数据到 MQ 中)
- 消息被拒绝消费消息，(basic.reject 或 basic.nack) 并且不把消息重新放入原模板队列， requeue = false。

流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%AD%BB%E4%BF%A1%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.png)





### TTL(消息存活时间/过期时间)：

   TTL 是 RabbitMQ 中一个消息或者队列的属性：当消息到达存活时间后，还没有被消费，会被自动清除。

作用：
     如果一条消息设置了 TTL 属性或者进入了设置 TTL 属性的队列，那么这条消息如果在 TTL 设置的时间内没有被消费，则会成为「死信」。

   注意：若同时配置队列 和消息的 TTL，那么较小的那个值将会被使用。

**两者的区别** 

  如果设置了队列的 TTL 属性，那么一旦消息过期，就会被队列丢弃(如果配置了死信队列被丢到死信队列中)，而第二种方式，消息即使过期，也不一定会被马上丢弃，因为**消息是否过期是在即将投递到消费者**之前判定的，如果当前队列有严重的消息积压情况，则已过期的消息也许还能存活较长时间；另外，还需要注意的一点是，如果不设置 TTL，表示消息永远不会过期，如果将 TTL 设置为 0，则表示除非此时可以直接投递该消息到消费者，否则该消息将会被丢弃。



#### TTL的两种设置:

**队列TTL**：

 消费者：创建队列的时候设置队列的 x-message-ttl 属性。

```java
Map<String, Object> params = new HashMap<>();
params.put("x-message-ttl",5000);
return QueueBuilder.durable("QA").withArguments(args).build(); 
// QA 队列的最大存活时间位 5000 毫秒
```

**消息TTL**:

生产者：针对每条消息设置 TTL。

```java
rabbitTemplate.converAndSend("X","XC",message,correlationData -> {
    correlationData.getMessageProperties().setExpiration("5000");
});
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B2.png)

````
    先启动消费者 C1，创建出队列，然后停止该 C1 的运行，则 C1 将无法收到队列的消息，无法收到的消息 10 秒后进入死信队列。启动生产者 producer 生产消息；启动 消费者 C2，消费死信队列里面的消息
````



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%AD%BB%E4%BF%A1%E9%98%9F%E5%88%97-TTL%E6%B5%81%E7%A8%8B.jpg)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%AD%BB%E4%BF%A1%E9%98%9F%E5%88%97-TTL%E6%B5%81%E7%A8%8B2.png)



#### 生产者：

```java
/* 生产者-死信队列_消息TTL(Time To Live/存活时间) 过期*/
public class Producer {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者生成消息： ");

        // 设置死信消息过期时间-方法1:10秒
        AMQP.BasicProperties properties =
                new AMQP.BasicProperties().builder().expiration("10000").build();

        // 发送10条消息
        for (int i = 1; i < 11; i++) {
            String message = "info：" + i;
            //   发送消息
            /*
             * 1.发送指定的交换机-默认：Direct
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(NORMAL_EXCHANGE, "zhnagsan", properties, message.getBytes("UTF-8"));
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

#### 消费者：

##### Consumer01:

```java
/** 消费者-死信队列_消息TTL(Time To Live/存活时间) 过期*/
public class Consumer01 {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";
    // 交换机-死信交换机名称
    private static final String DEAD_EXCHANGE = "dead_exchange";
    // 队列-普通队列名称
    private static final String NORMAL_QUEUE = "normal_queue";
    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");

        // 声明普通队列
        Map<String, Object> arguments = new HashMap<>();
        // 设置死信消息过期时间-方法2    10秒
//        arguments.put("x-message-ttl","10000");
        //正常的队列设置死信交换机
        arguments.put("x-dead-letter-exchange", DEAD_EXCHANGE);//图中红箭头
        //设置死信routingKey
        arguments.put("x-dead-letter-routing-key", "lisi");

        /* 定义交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(NORMAL_EXCHANGE, BuiltinExchangeType.DIRECT); // 普通交换机
        channel.exchangeDeclare(DEAD_EXCHANGE, BuiltinExchangeType.DIRECT); //死信交换机

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(NORMAL_QUEUE, false, false, false, arguments); // 普通队列
        channel.queueDeclare(DEAD_QUEUE, false, false, false, null); // 死信队列

        // 绑定
        channel.queueBind(NORMAL_QUEUE, NORMAL_EXCHANGE, "zhnagsan");  // 绑定普通交换机与普通队列
        channel.queueBind(DEAD_QUEUE, DEAD_EXCHANGE, "lisi"); // 绑定死信的交换机与死信的队列

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(NORMAL_QUEUE, false, deliverCallback, cancelCallback);
    }
}
```

##### Consumer04:

```java
/* 消费者2-死信队列_消息TTL(Time To Live/存活时间) 过期*/
public class Consumer02 {
    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者2-死信队列接收消息： ");

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("死信队列接收消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(DEAD_QUEUE, false, deliverCallback, cancelCallback);

    }
}
```







### 队列达到最大长度:

  注意：修改队列参数，需要把原先队列删除。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%AD%BB%E4%BF%A1%E9%98%9F%E5%88%97-%E9%98%9F%E5%88%97%E8%BE%BE%E5%88%B0%E6%9C%80%E5%A4%A7%E9%95%BF%E5%BA%A6.png)

#### 生产者：

```java
/* 生产者-死信队列_队列达到最大长度 */
public class Producer {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者生成消息： ");

        // 发送10条消息
        for (int i = 1; i < 11; i++) {
            String message = "info：" + i;
            //   发送消息
            /*
             * 1.发送指定的交换机-默认：Direct
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(NORMAL_EXCHANGE, "zhnagsan", null, message.getBytes("UTF-8"));
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

#### 消费者：

##### Consumer01:

```java
 /* 消费者1-死信队列_队列达到最大长度 */
public class Consumer01 {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";
    // 交换机-死信交换机名称
    private static final String DEAD_EXCHANGE = "dead_exchange";
    // 队列-普通队列名称
    private static final String NORMAL_QUEUE = "normal_queue";
    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");

        // 声明普通队列
        Map<String, Object> arguments = new HashMap<>();
        // 设置死信消息过期时间-方法2    10秒
//        arguments.put("x-message-ttl","10000");
        //正常的队列设置死信交换机
        arguments.put("x-dead-letter-exchange", DEAD_EXCHANGE);//图中红箭头
        //设置死信routingKey
        arguments.put("x-dead-letter-routing-key", "lisi");
        // 设置正常队列长度的限制，例如发送10个消息，6个为正常，4个为死信
        arguments.put("x-max-length", 6);

        /* 定义交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(NORMAL_EXCHANGE, BuiltinExchangeType.DIRECT); // 普通交换机
        channel.exchangeDeclare(DEAD_EXCHANGE, BuiltinExchangeType.DIRECT); //死信交换机

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(NORMAL_QUEUE, false, false, false, arguments); // 普通队列
        channel.queueDeclare(DEAD_QUEUE, false, false, false, null); // 死信队列

        // 绑定
        channel.queueBind(NORMAL_QUEUE, NORMAL_EXCHANGE, "zhnagsan");  // 绑定普通交换机与普通队列
        channel.queueBind(DEAD_QUEUE, DEAD_EXCHANGE, "lisi"); // 绑定死信的交换机与死信的队列

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(NORMAL_QUEUE, false, deliverCallback, cancelCallback);

    }
}
```

##### Consumer02:

```java
/* 消费者2-死信队列_队列达到最大长度 */
public class Consumer02 {

    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者2-死信队列接收消息： ");

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("死信队列接收消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(DEAD_QUEUE, false, deliverCallback, cancelCallback);
    }
}
```



### 消息被拒:

消息被拒:生产者

1. 需求：消费者 C1 拒收消息 "info5"，开启手动应答

   注意：  自动应答不存在消息被拒。

#### 生产者：

```java
/* 生产者-死信队列_消息被拒 */
public class Producer {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者生成消息： ");

        // 发送10条消息
        for (int i = 1; i < 11; i++) {
            String message = "info" + i;
            //   发送消息
            /*
             * 1.发送指定的交换机-默认：Direct
             * 2.路由的Key值
             * 3.其他参信息
             * 4.发送消息的消息体 */
            channel.basicPublish(NORMAL_EXCHANGE, "zhnagsan", null, message.getBytes("UTF-8"));
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

#### 消费者：

Consumer01：

```java
/*消费者1-死信队列_消息被拒*/
public class Consumer01 {
    // 交换机-普通交换机名称
    private static final String NORMAL_EXCHANGE = "normal_exchange";
    // 交换机-死信交换机名称
    private static final String DEAD_EXCHANGE = "dead_exchange";
    // 队列-普通队列名称
    private static final String NORMAL_QUEUE = "normal_queue";
    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");

        // 声明普通队列
        Map<String, Object> arguments = new HashMap<>();
        // 设置死信消息过期时间-方法2    10秒
        //        arguments.put("x-message-ttl","10000");
        //正常的队列设置死信交换机
        arguments.put("x-dead-letter-exchange", DEAD_EXCHANGE);//图中红箭头
        //设置死信routingKey
        arguments.put("x-dead-letter-routing-key", "lisi");

        /* 定义交换机：  交换机名称， 交换机类型-交换机_Direct(无名/直接/路由类型,默认交换机)(枚举，字符串都行）*/
        channel.exchangeDeclare(NORMAL_EXCHANGE, BuiltinExchangeType.DIRECT); // 普通交换机
        channel.exchangeDeclare(DEAD_EXCHANGE, BuiltinExchangeType.DIRECT); //死信交换机

        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数*/
        channel.queueDeclare(NORMAL_QUEUE, false, false, false, arguments); // 普通队列
        channel.queueDeclare(DEAD_QUEUE, false, false, false, null); // 死信队列

        // 绑定
        channel.queueBind(NORMAL_QUEUE, NORMAL_EXCHANGE, "zhnagsan");  // 绑定普通交换机与普通队列
        channel.queueBind(DEAD_QUEUE, DEAD_EXCHANGE, "lisi"); // 绑定死信的交换机与死信的队列

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            // 拒绝info：5的消息
            if (msg.equals("info5")) {
                System.out.println("消费者1接收到消息： " + msg+"并拒收此消息");
                // requeue 设置为 false为拒绝重新入队， 该队列如果配置了死信交换机将发送到死信队列中
                channel.basicReject(message.getEnvelope().getDeliveryTag(),false);
            }else {
                System.out.println("消费者1接收消息： " + msg);
                // 消费成功后是否要自动应答，  true:自动应答   false：手动应答
                channel.basicAck(message.getEnvelope().getDeliveryTag(),false);
            }
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(NORMAL_QUEUE, false, deliverCallback, cancelCallback);
    }
}
```

Consumer02：

```java
/* 消费者2-死信队列_消息被拒 */
public class Consumer02 {

    // 队列-死信队列名称
    private static final String DEAD_QUEUE = "dead_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者2-死信队列接收消息： ");

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("死信队列接收消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(DEAD_QUEUE, false, deliverCallback, cancelCallback);
    }
}
```

结果：

注意：图中生产者1和生产者2为消费者，写错了。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%AD%BB%E4%BF%A1%E9%98%9F%E5%88%97-%E6%B6%88%E6%81%AF%E8%A2%AB%E6%8B%92.png)



## 延迟队列：

​      延迟队列(死信队列+TTL)：设置TTL在延迟多久之后消息成为死信，另一方面，成为死信的消息都会被投递到死信队列里，这样只需要消费者一直消费死信队列里的消息就完事了，因为里面的消息都是希望被立即处理的消息。

 注意：延迟队列基于死信队列。
           队列内部是有序的。
           延时队列就是用来存放需要在指定时间被处理的 元素的队列；需要在某个事件发生之后或者之前的指定时间点完成某一项任务。

使用场景：
1. 订单在十分钟之内未支付则自动取消。

2. 新创建的店铺，如果在十天内都没有上传过商品，则自动发送消息提醒。

3. 用户注册成功后，如果三天内没有登陆则进行短信提醒。

4. 用户发起退款，如果三天内没有得到处理则通知相关运营人员。

5. 预定会议后，需要在预定的时间点前十分钟通知各个与会人员参加会议。




![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.jpg)

​    延迟队列代码看SpringBoot整合RabbitMQ



## 队列优先级：

​     系统中有一个**订单催付**的场景，我们的客户在天猫下的订单,淘宝会及时将订单推送给我们，如果在用户设定的时间内未付款那么就会给用户推送一条短信提醒，很简单的一个功能对吧，但是，tmall商家对我们来说，肯定是要分大客户和小客户的对吧，比如像苹果，小米这样大商家一年起码能给我们创造很大的利润，所以理应当然，他们的订单必须得到优先处理，而曾经我们的后端系统是使用 redis 来存放的定时轮询，而redis 只能用 List 做一个简单的消息队列，并不能实现一个优先级的场景，所以订单量大了后采用 RabbitMQ 进行改造和优化,如果发现是大客户的订单给一个相对比较高的优先级，否则就是默认优先级。



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E4%BC%98%E5%85%88%E7%BA%A7%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.jpg)

注意：优先级队列一般只使用0-10之间。优先级队列太大会浪费CPU和内存。



 web页面添加队列优先级：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%9D%A1%E4%BB%B6%E9%98%9F%E5%88%97%E4%BC%98%E5%85%88%E7%BA%A7-web%E9%A1%B5%E9%9D%A2.jpg)



编码实现队列优先级:

```java
// 队列优先级
Map<String, Object> params = new HashMap();
params.put("x-max-priority", 10);
channel.queueDeclare("hello", true, false, false, params);

// 消息优先级
AMQP.BasicProperties properties = new 
    AMQP.BasicProperties().builder().priority(5).build();
channel.basicPublish("", PRIORITY_QUEUE, properties, message.getBytes("UTF-8"));
```

   

  队列优先级三要素：

1. 队列需要设置为优先级队列。
2. 消息需要设置消息的优先级。
3. 消费者需要等待消息已经发送到队列中才去消费因为，这样才有机会对消息进行排序。



### 生产者：

注意：这个要想启动生产者发完消息了，再启动消费者，找了1个多小时，居然因为队列优先级的第三要素。o(╥﹏╥)o。

```java
/* 生产者-队列优先级 */
public class Producer {

    // 队列
    private static final String PRIORITY_QUEUE = "priority_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("生产者生成消息： ");
        Map<String, Object> arguments = new HashMap<>();
        arguments.put("x-max-priority", 10); //  设置队列优先级0-10     默认-0255
        // 定义队列
        /* 定义一个队列
         * 1.队列名称
         * 2. 队列是否持久化   true:持久化（磁盘），默认false(内存)
         * 3. 该队列是否只供一个消费者进行消费，是否进行消息共享。 true:共享， false:独食
         * 4. 是否自动删除，最后一个消费者断开连接后，该队列是否自动删除。 true:自动删除， false：不自动删除
         * 5.其他参数: 队列优先级，*/
        channel.queueDeclare(PRIORITY_QUEUE, false, false, false, arguments);

        // 发送10条消息
        for (int i = 1; i < 11; i++) {
            String message = "info" + i;
            if (i == 5) {
                // info5消息-设置队列优先级为:5
                System.out.println("555");
                AMQP.BasicProperties properties = new AMQP.BasicProperties().builder().priority(10).build();
                //   发送消息
                /* 1.发送指定的交换机-默认：Direct
                 * 2.路由的Key值
                 * 3.其他参信息: 队列优先级，
                 * 4.发送消息的消息体 */
                channel.basicPublish("", PRIORITY_QUEUE, properties, message.getBytes("UTF-8"));
            } else {
                //   发送消息
                channel.basicPublish("", PRIORITY_QUEUE, null, message.getBytes("UTF-8"));
            }
            System.out.println("生产者发出消息：" + message);
        }
    }
}
```

### 消费者：

```java
/**
 * 消费者-队列优先级
 */
public class Consumer01 {
    // 队列
    private static final String PRIORITY_QUEUE = "priority_queue";

    public static void main(String[] args) throws Exception {
        // 调用RabbitMQ工厂-工具类_信道
        Channel channel = RabbitMqUtils.getChannel();
        System.out.println("消费者1接收消息： ");

        //  消费者为成功消费的回调
        DeliverCallback deliverCallback = (consumerTag, message) -> {
            String msg = new String(message.getBody(), "UTF-8");
            System.out.println("接收的消息： " + msg);
        };

        //   取消消息时的回调
        CancelCallback cancelCallback = consumerTag -> {
            System.out.println();
        };

        /**
         * 消费者-消费者消息
         * 1.消费那个队列
         * 2.消费成功后是否要自动应答，  true:自动应答   false：手动应答
         * 3.消费者为成功消费的回调
         * 4.消费者录取消费的回调
         */
        channel.basicConsume(PRIORITY_QUEUE, false, deliverCallback, cancelCallback);
    }
}
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%98%9F%E5%88%97%E4%BC%98%E5%85%88%E7%BA%A7%E7%BB%93%E6%9E%9C.jpg)



## 惰性队列-3.6.0+：

  惰性队列会尽可能的将消息存入磁盘中，而在消费者消费到相应的消息时才会被加载到内存中，它的一个重要的设计目标是能够支持更长的队列，即支持更多的消息存储。
   惰性队列适合使用在消费者由于消费者下线、宕机亦或者是由于维护而关闭等原因，而致使长时间内不能消费消息造成堆积时。

   默认情况下，当生产者将消息发送到 RabbitMQ 的时候，队列中的消息会尽可能的存储在内存之中，这样可以更加快速的将消息发送给消费者。即使是持久化的消息，在被写入磁盘的同时也会在内存中驻留一份备份。当 RabbitMQ 需要释放内存的时候，会将内存中的消息换页至磁盘中，这个操作会耗费较长的时间，也会阻塞队列的操作，进而无法接收新的消息。虽然 RabbitMQ 的开发者们一直在升级相关的算法， 但是效果始终不太理想，尤其是在消息量特别大的时候。



|          | 默认队列         | 惰性队列         |
| -------- | ---------------- | ---------------- |
| 存储文件 | 消息保存在内存中 | 消息保存在磁盘中 |
| 速度     | 慢               | 快               |
|          |                  |                  |



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%BB%98%E8%AE%A4%E9%98%9F%E5%88%97VS%E6%83%B0%E6%80%A7%E9%98%9F%E5%88%97.png)

​    在发送 1 百万条消息，每条消息大概占 1KB 的情况下，普通队列占用内存是 1.2GB，而惰性队列仅仅占用 1.5MB

队列：

```java
Map<String, Object> args = new HashMap<String, Object>();
args.put("x-queue-mode", "lazy");  // 惰性队列

// 生成一个队列:队列名称,是否开启持久化,消息是否共享，队列是否自动删除，其他参数
channel.queueDeclare("myqueue", false, false, false, args)
```





#  SpringBoot整合RabbitMQ:

​      利用 RabbitMQ 实现延时队列的两大要素:死信队列和 TTL。

​      延时队列：不就是想要消息延迟多久被处理吗，TTL 则刚好能让消息在延迟多久之后成为死信，另一方面，成为死信的消息都会被投递到死信队列里，这样只需要消费者一直消费死信队列里的消息就完事了，因为里面的消息都是希望被立即处理的消息。

## 依赖：

```xml
<!--RabbitMQ 依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
<!--RabbitMQ 测试依赖-->
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-rabbit-test</artifactId>
    <scope>test</scope>
</dependency>
```

## yml:

```yaml
server:
  port: 8080
spring:
  rabbitmq: # 整合RabbitMQ
    host: 192.168.139.140 # ip
    port: 5672 # 端口
    username: qsl
    password: 123456
    # 以下可省略。。。
    publisher-confirm-type: CORRELATED  # 发布者确认类型，消息是否被成功发送到交换机
    publisher-returns: true  # 发布者返回true
    listener: # 监听器配置
      simple:  # 简单模式
        prefetch: 1  # 预取数量
        concurrency: 3 # 并发量
        acknowledge-mode: manual  # 确认模式-手动确认（消费端）
```



## 延迟队列/消息:

  延时队列在需要延时处理的场景下非常有用，使用 RabbitMQ 来实现延时队列可以很好的利用 RabbitMQ 的特性，如：消息可靠发送、消息可靠投递、死信队列来保障消息至少被消费一次以及未被正确处理的消息不会被丢弃。另外，通过 RabbitMQ 集群的特性，可以很好的解决单点故障问题，不会因为单个节点挂掉导致延时队列不可用或者消息丢失。

当然，延时队列还有很多其它选择，比如利用 Java 的 DelayQueue，利用 Redis 的 zset，利用 Quartz 或者利用 kafka 的时间轮，这些方式各有特点,看需要适用的场景。



#### 队列TTL实现延迟消息(消费者):

​     缺点：只能使用固定队列TTL，若新增一个需求，只能再写一个队列TTL，灵活性不高。

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B2.png)

##### config/TtlQueueConfig:

```java
import org.springframework.amqp.core.*;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/** 整合RabbitMQ
 * TTL队列:配置文件类代码 */
@Configuration // 配置类
public class TtlQueueConfig {
    // ============交换机名称
    // 普通交换机的名称
    public static final String X_EXCHANGE = "X";
    // 死信交换机的名称
    public static final String Y_DEAD_LETTER_EXCHANGE = "Y";
    // ============队列名称
    // 普通队列名称
    public static final String QUEUE_A = "QA";
    public static final String QUEUE_B = "QB";
    // 死信队列名称
    public static final String DEAD_LETTER_QUEUE = "QD";

    // ==================定义交换机==========
    // 普通交换机
    @Bean("XExchange") // bean 起别名
    public DirectExchange XExchange() {
        return new DirectExchange(X_EXCHANGE);
    }

    // 死信交换机
    @Bean("YExchange")
    public DirectExchange YExchange() {
        return new DirectExchange(Y_DEAD_LETTER_EXCHANGE);
    }

    // ==================定义队列==========
    // 普通队列A   ttl(存活时间)为5s
    @Bean("queueA")
    public Queue queueA() {
        Map<String, Object> arguments = new HashMap<>(3);
        arguments.put("x-dead-letter-exchange", Y_DEAD_LETTER_EXCHANGE); //普通交换机绑定死信交换机-死信交换机名
        arguments.put("x-dead-letter-routing-key", "YD"); // 普通交换机绑定死信交换机-key
        arguments.put("x-message-ttl", 5000); // 消息5秒过期
        return QueueBuilder.durable(QUEUE_A).withArguments(arguments).build();
    }

    // 普通队列B ttl(存活时间)为10s
    @Bean("queueB")
    public Queue queueB() {
        Map<String, Object> arguments = new HashMap<>(3);
        arguments.put("x-dead-letter-exchange", Y_DEAD_LETTER_EXCHANGE); //普通交换机绑定死信交换机-死信交换机名
        arguments.put("x-dead-letter-routing-key", "YD"); // 普通交换机绑定死信交换机-key
        arguments.put("x-message-ttl", 20000); // 消息20秒过期
        return QueueBuilder.durable(QUEUE_B).withArguments(arguments).build();
    }

    // 死信队列
    @Bean("queueD")
    public Queue queueY() {
        return QueueBuilder.durable(DEAD_LETTER_QUEUE).build();
    }

    // ==================队列绑定交换机==========
    //  QA 绑定 X交换机
    @Bean   // @Qualifier注解可以指定要注入的实现类的名称
    public Binding queueABindingX(@Qualifier("queueA") Queue queueA,
                                  @Qualifier("XExchange") DirectExchange XExchange) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(queueA).to(XExchange).with("XA");
    }

    //  QB 绑定 X交换机
    @Bean   //  @Qualifier：注入指定实例（bean）
    public Binding queueBBindingX(@Qualifier("queueB") Queue queueB,
                                  @Qualifier("XExchange") DirectExchange XExchange) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(queueB).to(XExchange).with("XB");
    }

    //  QD 绑定 Y交换机
    @Bean   //  @Qualifier：注入指定实例（bean）
    public Binding queueDBindingY(@Qualifier("queueD") Queue queueD,
                                  @Qualifier("YExchange") DirectExchange YExchange) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(queueD).to(YExchange).with("YD");
    }
}
```

##### 生产者：

controller/SendMsgController

```java
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

/* 生产者*/
@RestController // Controller
@RequestMapping("ttl")
public class SendMsgController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @GetMapping("sendMsg/{message}")
    public String  sendMsg(@PathVariable("message") String message) {
        log.info("当前时间：{}，发送消息给TTLd队列:{}" + new Date().toString(), message);
        // 占位符
        rabbitTemplate.convertAndSend("X", "XA", "消息来自ttl为5s队列：" + message); // 交换机，routingKey,要发送的消息
        rabbitTemplate.convertAndSend("X", "XB", "消息来自ttl为20s队列：" + message);
        return "生产者发送消息:" + message;
    }
}
```

##### 消费者:

```java
/* 消费者-TTl*/
@Slf4j
@Component // 默认bean
public class DeadLetterQueueConsumer {

    @RabbitListener(queues = "QD") // 监听器-监听死信队列
    public void receiveD(Message message, Channel channel) throws Exception {
        String msg = new String(message.getBody());
        log.info("当前时间:{},收到死信队列消息: {}", new Date().toString(), msg);
    }
}
```

浏览器访问：http://localhost:8080/ttl/sendMsg/嘻嘻嘻

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/springBoot%E6%95%B4%E5%90%88MQ-%E9%98%9F%E5%88%97TTL%E7%BB%93%E6%9E%9C.jpg)





#### 消息TTL实现延迟消息(生产者):

​    缺点：消息TTL只会根据发送请求的顺序执行TTL,并且只能处理完一个再处理一个。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B2-%E4%BC%98%E5%8C%96%E5%90%8E.png)

注意：以下普通队列QA,QB不写了，可看队列TTL的。



##### Config/MsgTtlQueueConfig:

```java
@Configuration
public class MsgTtlQueueConfig {
    // ============交换机名称
    // 普通交换机名称
    public static final String X_EXCHANGE = "X";
    //死信交换机名称-延迟
    public static final String Y_DEAD_LETTER_EXCHANGE="Y";

    // ============队列名称
    //普通队列名称
    public static final String QUEUE_C = "QC";
    //死信队列名称
    public static final String QUEUE_D = "QD";

    // ==================定义交换机
    // 普通交换机
    @Bean("XExchange") // bean 起别名
    public DirectExchange XExchange() {
        return new DirectExchange(X_EXCHANGE);
    }

    // 死信交换机
    @Bean("YExchange")
    public DirectExchange YExchange() {
        return new DirectExchange(Y_DEAD_LETTER_EXCHANGE);
    }

    // ============定义队列
    //声明QC队列
    @Bean("queueC") // bean 起别名
    public Queue QueueC(){
        Map<String,Object> arguments = new HashMap<>(3);
        //设置死信交换机
        arguments.put("x-dead-letter-exchange",Y_DEAD_LETTER_EXCHANGE);
        //设置死信RoutingKey
        arguments.put("x-dead-letter-routing-key","XC");
        return QueueBuilder.durable(QUEUE_C).withArguments(arguments).build();
    }

    // 死信队列
    @Bean("queueD")
    public Queue queueY() {
        return QueueBuilder.durable(DEAD_LETTER_QUEUE).build();
    }

    // ============队列绑定交换机
    //声明队列 QC 绑定 X 交换机
    @Bean   // @Qualifier：注入指定实例（bean）
    public Binding queueCBindingX(@Qualifier("queueC") Queue queueC,@Qualifier("xExchange")DirectExchange xExchange){
        return BindingBuilder.bind(queueC).to(xExchange).with("XC");
    }

    //  QD队列 绑定 Y交换机
    @Bean   //  @Qualifier：注入指定实例（bean）
    public Binding queueDBindingY(@Qualifier("queueD") Queue queueD,@Qualifier("YExchange") DirectExchange YExchange) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(queueD).to(YExchange).with("YD");
    }
}
```

##### 生产者：

```java
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

/* 生产者*/
@RestController // Controller
@RequestMapping("ttl")
public class SendMsgController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @ApiOperation("生产者发送消息-消息TTL实现延迟消息_生产者") 
    @GetMapping("sendMsg/{message}/{ttlTime}")
    public String sendMsg(@PathVariable("message") String message,
                          @PathVariable("ttlTime") String ttlTime) {
        log.info("当前时间：{}，发送一条时长是{}毫秒TTL信息给队列QC：:{}" + new Date().toString(), ttlTime, message);
        // 占位符
        rabbitTemplate.convertAndSend("X", "XC", message, msg->{
            // 设置延迟队列时间-生产者
            msg.getMessageProperties().setExpiration(ttlTime);
            return  msg;
        }); // 交换机，routingKey,要发送的消息
        return "生产者发送消息:" + message;
    }
}
```



##### 消费者：

```java
import com.qsl.rabbitmq.config.DelayedQueueConfig;
import com.rabbitmq.client.Channel;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

import java.util.Date;

/* 消费者*/
@Slf4j
@Component // 默认bean
public class DeadLetterQueueConsumer {

    /* 监听消息-TTl */
    @RabbitListener(queues = "QD") // 监听器-监听死信队列
    public void receiveD(Message message, Channel channel) throws Exception {
        String msg = new String(message.getBody());
        log.info("当前时间:{},收到死信队列消息: {}", new Date().toString(), msg);
    }
}

```



发送两条消息：

http://localhost:8080/ttl/sendExpirationMsg/你好 1/20000
http://localhost:8080/ttl/sendExpirationMsg/你好 2/2000

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/springboot%E6%95%B4%E5%90%88mq-%E9%98%9F%E5%88%97TTL.png)



#### 延迟插件实现延迟消息：

  通过**rabbitmq_delayed_message_exchange**(延迟)插件:实现延迟队列。

  RabbitMQ插件：  https://www.rabbitmq.com/community-plugins.html

#####  安装延迟插件：

下载并上传到rabbitmq_server/plugins:

  RabbitMQ安装路径： /usr/lib/rabbitmq/lib/rabbitmq_server-3.8.8/plugins

```sh
# RabbitMQ安装延迟插件 （注意：不需要版本号和文件后缀）
rabbitmq-plugins enable rabbitmq_delayed_message_exchange

# 安装
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
# 重启服务
systemctl restart rabbitmq-server
```

成功图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Rabbitmq%E5%BB%B6%E8%BF%9F%E6%8F%92%E4%BB%B6%E5%AE%9E%E7%8E%B0%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97.png)



延迟插件实现延迟消息流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Rabbitmq%E5%BB%B6%E8%BF%9F%E6%8F%92%E4%BB%B6%E5%AE%9E%E7%8E%B0%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.png)

##### config/DelayedQueueConfig:

```java
import org.springframework.amqp.core.*;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.HashMap;
import java.util.Map;

/* 整合RabbitMQ-RabbitMQ插件实现延迟消息 */
@Component //默认bean
public class DelayedQueueConfig {
    // 延迟交换机名称
    public static final String DELAYED_EXCHANGE = "delayed.exchange";
    // 延迟队列名称
    public static final String DELAYED_QUEUE = "delayed.queue";
    // routingKey
    public static final String DELAYED_ROUTING_KEY="delayed.routingkey";

    // 定义延迟交换机
    @Bean    //  自定义交换机：CustomExchange
    public CustomExchange delayedExchange() {
        Map<String, Object> arguments = new HashMap<>();
        arguments.put("x-delayed-type", "direct"); //延迟交换机绑定直接交换机，

        /**
         * 1.交换机的名称
         * 2.交换机的类型 x-delayed-message(本次延迟交换机),
         * 3.是否需要持久化
         * 4.是否需要自动删除
         * 5.其他的参数
         */
        return new CustomExchange(DELAYED_EXCHANGE, "x-delayed-message", false, false, arguments);
    }

    // 定义延迟队列
    @Bean
    public Queue delayedQueue() {
        return new Queue(DELAYED_QUEUE);
    }

    // 队列 绑定 交换机
    @Bean     // @Qualifier： 注入 (注意：不名称则使用默认名称注入)
    public Binding DelayedQueueBindingDelayedQueue(@Qualifier("delayedExchange") Exchange delayedExchange,
                                                   @Qualifier("delayedQueue") Queue delayedQueue) {
        // 队列，交换机，routingKey
        return BindingBuilder.bind(delayedQueue).to(delayedExchange).with(DELAYED_ROUTING_KEY).noargs();
    }
}
```

##### 生产者：

```java
/** 生产者*/
@Slf4j
@Api(tags = "生产者")
@RestController // Controller
@RequestMapping("ttl")
public class SendMsgController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @ApiOperation("生产者发送消息-延迟插件实现延迟消息_生产者") // Swagger注释
    @GetMapping("sendDelayMsg/{message}/{delayTime}")
    public String sendDelayMsg(@PathVariable("message") String message,
                               @PathVariable("delayTime") Integer delayTime) {
        log.info("当前时间：{}，延迟插件实现延迟消息-发送一条时长是{}毫秒给延迟队列：:{}" + new Date().toString(), delayTime, message);
        // 占位符
        rabbitTemplate.convertAndSend(DelayedQueueConfig.DELAYED_EXCHANGE, DelayedQueueConfig.DELAYED_ROUTING_KEY, message, msg->{
            // 设置延迟队列时间-生产者
            msg.getMessageProperties().setDelay(delayTime);
            return  msg;
        }); // 交换机，routingKey,要发送的消息
        return "生产者发送消息:" + message;
    }
}
```

##### 消费者：

consumer/DeadLetterQueueConsumer:

```java
@Component // 默认bean
public class DeadLetterQueueConsumer {

    /**
     * 监听消息-延迟插件实现延迟消息
     */
    @RabbitListener(queues = DelayedQueueConfig.DELAYED_QUEUE)
    public void receiveDelayQueue(Message message) {
        String msg = new String(message.getBody());
        log.info("当前时间:{},收到死信队列消息: {}", new Date().toString(), msg);
    }
}  
```

发起两次请求：

http://localhost:8080/ttl/sendDelayMsg/come on baby1/20000
http://localhost:8080/ttl/sendDelayMsg/come on baby2/2000

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/springboot%E6%95%B4%E5%90%88mq-%E5%BB%B6%E8%BF%9F%E6%8F%92%E4%BB%B6%E5%AE%9E%E7%8E%B0%E5%BB%B6%E8%BF%9F%E6%B6%88%E6%81%AF.png)



#  其他知识点:



## 幂等性

### 概念

​    用户对于同一操作发起的一次请求或者多次请求的结果是一致的，不会因为多次点击而产生了副作用。      例如：支付，用户购买商品后支付，支付扣款成功，但是返回结果的时候网络异常，此时钱已经扣了，用户再次点击按钮，此时会进行第二次扣款，返回结果成功，用户查询余额发现多扣钱了，流水记录也变成了两条。在以前的单应用系统中，我们只需要把数据操作放入事务中即可，发生错误立即回滚，但是在响应客户端的时候也有可能出现网络中断或者异常等等。

### 消息重复消费

消费者在消费 MQ 中的消息时，MQ 已把消息发送给消费者，消费者在给 MQ 返回 ack 时网络中断， 故 MQ 未收到确认信息，该条消息会重新发给其他的消费者，或者在网络重连后再次发送给该消费者，但实际上该消费者已成功消费了该条消息，造成消费者消费了重复的消息。

### 以上两个问题解决思路

   MQ 消费者的幂等性的解决一般使用唯一标识(UUID, 全局 ID,MQ消费自带Id等) ，每次消费消息时用该 id 先判断该消息是否已消费过。

### 解决消费端的幂等性

  在海量订单生成的业务高峰期，生产端有可能就会重复发生了消息，这时候消费端就要实现幂等性，即使我们收到了一样的消息，我们的消息永远不会被消费多次。

业界主流的幂等性有两种操作：

- 唯一 ID+ 指纹码机制,利用数据库主键去重
  指纹码：我们的一些规则或者时间戳加别的服务给到的唯一信息码,它并不一定是我们系统生成的，基本都是由我们的业务规则拼接而来，但是一定要保证唯一性，然后就利用查询语句进行判断这个 id 是否存在数据库中。

  ​    优点：实现简单就一个拼接，然后查询判断是否重复；
  ​    缺点：在高并发时，如果是单个数据库就会有写入性能瓶颈当然也可以采用分库分表提升性能，但也不是我们最推荐的方式。

- Redis 的原子性：利用 redis 执行 setnx 命令，天然具有幂等性。从而实现不重复消费



## 日志与监控：

```sh
# 查看队列
rabbitmqctl list_queues
#查看exchanges
rabbitmqctl list_exchanges
# 查看用户
rabbitmqctl list_users
# 查看连接
rabbitmqctl list_connections
# 查看消费者信息
rabbitmqctl list_consumers

# 查看环境变量
rabbitmqctl environment
# 查看未被确认的队列
rabbitmqctl list_queues name messages_unacknowledged
# 查看单个队列的内存使用
rabbitmqctl list_queues name memory
# 查看准备就绪的队列
rabbitmqctl list_queues name messages_ready
```



## 镜像集群：

​    镜像队列是基于普通的集群模式的，然后再添加一些策略，所以必须先配置普通集群，然后才能设置镜像队列。

   镜像队列(Mirror Queue): 可以将队列镜像到集群中的其他 Broker 节点之上，如果集群中的一个节点失效了，队列能自动地切换到镜像中的另一个节点上以保证服务的可用性。

1.镜像集群:

```sh
# 命令行实现镜像集群     my_ha名称备份所有
rabbitmqctl set_policy my_ha[名称] "^" '{"ha-mode":"all"}' 
```

web页面实现镜像集群：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%95%9C%E5%83%8F%E9%9B%86%E7%BE%A4.jpg)



测试镜像队列：

​    测试镜像队列： 添加mirrior-hello队列并发送一条消息。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%95%9C%E5%83%8F%E9%9B%86%E7%BE%A4%E6%B5%8B%E8%AF%95.png)





## Federation Exchange (联邦/联合交换机)

​     原因：现实中会在多个地方搭建服务器，服务器之间发送消息有很大的网络延迟，尤其是在开启了 publisherconfirm(发布确认模式)机制或者事务机制的情况下;进而必然造成这条发送线程的性能降低，甚至造成一定程度上的阻塞。



### 开启federation和federation_management插件:

​     注意：要使用Federation Exchange必须先开启rabbitmq_federation和rabbitmq_federation_management插件。

```sh
# 在集群里开启rabbitmq_federation插件
rabbitmq-plugins enable rabbitmq_federation
# 在集群里开启rabbitmq_federation_management插件
rabbitmq-plugins enable rabbitmq_federation_management
```

成功安装图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Federation%20Exchange_%E8%81%94%E9%82%A6%E3%80%81%E8%81%94%E5%90%88%E4%BA%A4%E6%8D%A2%E6%9C%BA%E5%AE%89%E8%A3%85.jpg)









### 配置联邦交换机：

流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/Federation%20Exchange_%E8%81%94%E9%82%A6%E3%80%81%E8%81%94%E5%90%88%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%B5%81%E7%A8%8B.png)

1.在 downstream(node2)配置 upstream(node1)：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%85%8D%E7%BD%AE%E8%81%94%E9%82%A6%E4%BA%A4%E6%8D%A2%E6%9C%BA1.png)

添加 policy:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%85%8D%E7%BD%AE%E8%81%94%E9%82%A6%E4%BA%A4%E6%8D%A2%E6%9C%BA2.png)

配置联邦交换机成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E9%85%8D%E7%BD%AE%E8%81%94%E9%82%A6%E4%BA%A4%E6%8D%A2%E6%9C%BA%E6%88%90%E5%8A%9F.png)



## Federation Queue(联邦队列)：

   联邦队列可以在多个 Broker 节点(或者集群)之间为单个队列提供均衡负载的功能。一个联邦队列可以连接一个或者多个上游队列(upstream queue)，并从这些上游队列中获取消息以满足本地消费者消费消息的需求。

注意：联邦交换机或联邦队列都可以实现。

流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E8%81%94%E9%82%A6%E9%98%9F%E5%88%97%E6%B5%81%E7%A8%8B.png)

新增联邦队列：

   新增联邦队列类似联邦交换机。

成功图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E8%81%94%E9%82%A6%E9%98%9F%E5%88%97.png)

## Shovel：

​    Shovel(铲子) :类似联邦交换机和联邦队列。
​    Federation 具备的数据转发功能类似，Shovel 够可靠、持续地从一个 Broker 中的队列(作为源端，即source)拉取数据并转发至另一个 Broker 中的交换器(作为目的端，即 destination)。作为源端的队列和作为目的端的交换器可以同时位于同一个 Broker，也可以位于不同的 Broker 上。Shovel 行为就像优秀的客户端应用程序能够负责连接源和目的地、负责消息的读写及负责连接失败问题的处理。

### 开启Shover插件:

​     注意：要使用Shover必须先开启rabbitmq_shovel和rabbitmq_shovel_management插件。

```sh
# 开启Shover插件(默认自带)
rabbitmq-plugins enable rabbitmq_shovel
rabbitmq-plugins enable rabbitmq_shovel_management
```

shovel插件安装成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/shovel%E6%8F%92%E4%BB%B6%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.png)



### shovel执行流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/shovel%E6%B5%81%E7%A8%8B.png)

### 新增shovel：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97/RibbimMQ/%E6%96%B0%E5%A2%9Eshovel.png)







#  HAProxy 安装：

HAProxy 未成功安装！！！！！先过！！！

```sh
# 前提：安装gcc vim wget等依赖环境
yum install gcc vim wget

# 上传并解压haproxy源码包
tar -zxvf haproxy-1.6.5.tar.gz -C /opt

# 进入haproxy-1.6.5目录、进行编译、安装
make TARGET=linux31 PREFIX=/opt/haproxy-1.6.5/haproxy
make install PREFIX=/opt/haproxy-1.6.5/haproxy

#  添加组，添加用户
groupadd -r -g 149 haproxy
useradd -g haproxy -r -s /sbin/nologin -u 149 haproxy

mkdir /etc/haproxy
# 创建haproxy配置文件
vim /etc/haproxy/haproxy.cfg
```

配置HAProxy:

​    配置文件路径：/etc/haproxy/haproxy.cfg

```sh
#logging options
global
	log 127.0.0.1 local0 info
	maxconn 5120
	chroot /opt/haproxy-1.6.5/haproxy
	uid 99
	gid 99
	daemon
	quiet
	nbproc 20
	pidfile /var/run/haproxy.pid

defaults
	log global
	
	mode tcp

	option tcplog
	option dontlognull
	retries 3
	option redispatch
	maxconn 2000
	contimeout 5s
   
     clitimeout 60s

     srvtimeout 15s	
#front-end IP for consumers and producters

listen rabbitmq_cluster  # 监听服务
	bind 0.0.0.0:5672 # 对外提供服务的端口
	
	mode tcp
	
	balance roundrobin
	   # RabbitMQ集群配置：ip,端口，最多重试2次、失败2次
        server node1 127.0.0.1:5673 check inter 5000 rise 2 fall 2
        server node2 127.0.0.1:5674 check inter 5000 rise 2 fall 2

listen stats # web管理插件：ip,端口
	bind 172.16.98.133:8100
	mode http
	option httplog
	stats enable
	stats uri /rabbitmq-stats  # 通过此路径访问web管理页面
	stats refresh 5s
```

启动HAproxy负载

```sh
# 启动Haproxy,并配置文件启动路径
/opt/haproxy-1.6.5/haproxy/sbin/haproxy -f /etc/haproxy/haproxy.cfg
//查看haproxy进程状态
ps -ef | grep haproxy

访问如下地址对mq节点进行监控
http://172.16.98.133:8100/rabbitmq-stats
```




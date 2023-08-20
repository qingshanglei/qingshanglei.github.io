技术要求：java8+maven+git、 github+nginx+RabbitMQ+SpringBoot2.0

JVM、JUC、JMM、GC、Nginx ......

# 概念：

​    Spring Cloud是一系列框架的集合。它利用Spring Boot的开发便利性简化了分布式系统基础设施的开发，如服务发现、服务注册、配置中心、消息总线、负载均衡、 熔断器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud只是将目前各家公司开发的比较成熟服务框架组合起来，通过SpringBoot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。         SpringCloud是一系列技术的总和。

​    ①微服务是一种框架模式，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调、互相配合，为用户提供最终价值。
​    ②每个服务运行在其独立的进程中，服务与服务间采用轻量级的通信机制互相协作（通常基于HTTP协议的RESTful API）。
​    ③每个服务都围腰者具体服务进行构建，并且能够被独立的部署到生成环境、类生产环境等。另外，应当尽量避免统一的、集中式的服务管理机制，对具体一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建。



SpringCloud=分布式微服务架构的一站式解决方案，是多种微服务建构落地技术的集合体，简称微服务全家桶。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud.png)

技术对应图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud%E9%9B%86%E7%BE%A4%E7%9B%AE%E5%BD%95.png)



## Cloud流程图：

​    打×的为停更，不推荐使用。

​    老系统：Docker做服务调用，Zookeeper做服务注册中心。

​    Hystrix：国内大规模使用，国外不推荐。

​    国内：Hystrix，Sentienl         国外：resilience4j

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Cloud%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

旧：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud6.png)

## Spring Cloud和Spring Boot关系

Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务，Spring Cloud是一个基于Spring Boot实现的开发工具；Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架； Spring Boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring Boot来实现，必须基于Spring Boot开发。可以单独使用Spring Boot开发项目，但是Spring Cloud离不开 Spring Boot。



## SpringCloud版本说明:

- Spring boot使用的是数字作为版本。官网强烈建议升级到2.0以上。
- Spring cloud使用的是字母作为版本，A-Z依次类推的形式来发布迭代版本,，伦敦地铁站站名。
- cloud版本决定了boot版本。

查看版本对应关系：https://start.spring.io/actuator/info

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/%E5%AE%98%E7%BD%91%E7%89%88%E6%9C%AC%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB.png)

查看版本对应关系：https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/

​    当SpringCloud的发布重大内容或者一个重大BUG被解决后，会发布一个"service releases"版本，简称SRX版本。 例：Greenwich.SR2

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud%E4%B8%8ESpringBoot%E7%89%88%E6%9C%AC%E5%85%B3%E7%B3%BB.png)

  教学版本：

| cloud         | Hoxton.SR1    |
| ------------- | ------------- |
| boot          | 2.2.2.RELEASE |
| cloud alibaba | 2.1.0.RELEASE |
| java          | java8         |
| maven         | 3.5及以上     |
| mysql         | 5.7及以上     |

   注意：Swagger 2.x 版本不支持 Spring Cloud Alibaba





## 微服务模块流程：

1.   建module
2.  改POM
3.  写YML
4. 主启动
5. 业务类（Service,Controller层）

### 编码：约定 > 配置 > 编码



### 开启Run DashBoard：(多个微服务):

  该项目路径/ .idea/ workspace.xml，改完重启IDEA。

```xml
<option name="configurationTypes"> 
    <set>
        <option value="SpringBootApplicationConfigurationType" />
    </set>
</option>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/%E5%BC%80%E5%90%AFrun%20DashBoard.png)

## yml文件配置：

<font color="red">applicaiton.yml</font>是用户级的资源配置项。
<font color="red">bootstrap.yml</font>是系统级的，优先级更加高。
  故先加载bootstrap.yml，再加载application.yml的。

   Spring Cloud会创建一个“Bootstrap Context”，作为Spring应用的`Application Context`的父上下文。初始化的时候，`Bootstrap Context`负责从外部源加载配置属性并解析配置。这两个上下文共享一个从外部获取的`Environment`。

​       Bootstrap`属性有高优先级，默认情况下，它们不会被本地配置覆盖。 `Bootstrap context`和`Application Context`有着不同的约定，所以新增了一个`bootstrap.yml`文件，保证`Bootstrap Context`和`Application Context`配置的分离。

**服务端和客户端说明：**

服务端：后端接口
客户端：浏览器、安卓、ios



# 服务注册：

## Eureka:

  [Eureka](https://github.com/Netflix/Eureka) 是 [Netflix](https://github.com/Netflix) 开发的，一个基于 REST 服务的，服务注册与发现的组件，以实现中间层服务器的负载平衡和故障转移。

Eureka 分为 Eureka Server 和 Eureka Client及服务端和客户端。**Eureka Server为注册中心，是服务端，而服务提供者和消费者即为客户端，消费者也可以是服务者，服务者也可以是消费者**。同时Eureka Server在启动时默认会注册自己，成为一个服务，所以Eureka Server也是一个客户端，这是搭建Eureka集群的基础。





### 原理：

####      服务治理：

​        在传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务于服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

​      Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务治理

####     服务注册与发现：

1.  Eureka采用了CS的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka的客户端连接到 Eureka Server并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

2. 在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息 比如 服务地址通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC调用RPC远程调用框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系(服务治理概念)。

   ​     注意： 在任何rpc远程框架中，都会有一个注册中心(存放服务地址相关信息(接口地址))

   

   Eureka与Dubbo的系统架构对比:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E4%B8%8EDubbo%E7%9A%84%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84%E5%AF%B9%E6%AF%94.png)



#### Eureka两个大组件：

1. Eureka Server提供服务注册服务：
   各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。
2. EurekaClient通过注册中心进行访问：
   是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/%E5%8E%9F%E7%90%86.png)



### 服务端：

##### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

<!-- 老版本,不推荐（2018） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

##### 主启动：

```java
package com.qsl.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
public class eurekaMain7002 {
    public static void main(String[] args) {
        SpringApplication.run(eurekaMain7002.class,args);
    }
}
```



##### yml:

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost # eureka服务端名称 （ ip ）
  client:
    register-with-eureka: false # 是否在注册中心注册显示  true:显示  ，false：不显示
    fetch-registry: false # false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

浏览器打开：http://localhost:7001/,出现以下页面代表成功。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka%E6%9C%8D%E5%8A%A1%E7%AB%AF.bmp)



### 客户端:

#### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!-- 老版本,不推荐（2018） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

#### 主启动：

```java
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient //开启Eureka客户端
public class PaymentMain8001
{
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
```



#### yml:

```yaml
server:
  port: 80
  
spring:
  application:
    name: cloud-order-server # 软件名称

eureka:
  client:
    register-with-eureka: true # 是否在EurekaServer显示，默认true
    fetch-registry: true # 是否从EurekaService抓取已有的注册信息，默认true。单节点无所谓，集群必须设置为true才能配合 “ribbon"使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka # 服务端地址
```

注意：先要启动EurekaServer（服务端）

浏览器打开：http://localhost:7001/,服务端监控客户端代表成功。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%88%90%E5%8A%9F%E7%BB%93%E6%9E%9C.bmp)



#### 访问信息有IP信息提示:

```yaml
eureka:
  client:
    register-with-eureka: true # 是否在EurekaServer显示，默认true
    fetch-registry: true # 是否从EurekaService抓取已有的注册信息，默认true。单节点无所谓，集群必须设置为true才能配合 “ribbon"使用负载均衡
    service-url:
      #      defaultZone: http://localhost:7001/eureka # 服务端地址-单机
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 服务端地址-集群
  instance:
    prefer-ip-address: true # 访问路径显示 IP地址
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka-%E8%AE%BF%E9%97%AE%E4%BF%A1%E6%81%AF%E6%9C%89IP%E4%BF%A1%E6%81%AF%E6%8F%90%E7%A4%BA.png)





### 单机：

#### 服务端：

##### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

<!-- 老版本,不推荐（2018） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

##### 主启动：

```java
package com.qsl.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
public class eurekaMain7002 {
    public static void main(String[] args) {
        SpringApplication.run(eurekaMain7002.class,args);
    }
}
```



##### yml:

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost # eureka服务端名称 （ ip ）
  client:
    register-with-eureka: false # 是否在注册中心注册显示  true:显示  ，false：不显示
    fetch-registry: false # false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

浏览器打开：http://localhost:7001/,出现以下页面代表成功。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka%E6%9C%8D%E5%8A%A1%E7%AB%AF.bmp)



#### 客户端:

##### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!-- 老版本,不推荐（2018） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

##### 主启动：

```java
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient //开启Eureka客户端
public class PaymentMain8001
{
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
```



##### yml:

```yaml
server:
  port: 80
  
spring:
  application:
    name: cloud-order-server # 软件名称

eureka:
  client:
    register-with-eureka: true # 是否在EurekaServer显示，默认true
    fetch-registry: true # 是否从EurekaService抓取已有的注册信息，默认true。单节点无所谓，集群必须设置为true才能配合 “ribbon"使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka # 服务端地址
```

注意：先要启动EurekaServer（服务端）

浏览器打开：http://localhost:7001/,服务端监控客户端代表成功。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%88%90%E5%8A%9F%E7%BB%93%E6%9E%9C.bmp)





### 微服务注册名配置说明：

```yaml
spring:
  application:
    name: cloud-payment-service

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka 
  instance:
    instance-id: payment8001
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/%E4%B8%BB%E6%9C%BA%E5%90%8D%E7%A7%B0-%E4%BF%AE%E6%94%B9%E6%9C%8D%E5%8A%A1%E5%90%8D%E7%A7%B0.png)









### 集群：

##### 原理：

问题：微服务RPC远程服务调用最核心的是什么：高可用。
 　      搭建Eureka注册中心集群 ，实现负载均衡+故障容错。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E9%9B%86%E7%BE%A4%E5%8E%9F%E7%90%86.bmp)

集群：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E9%9B%86%E7%BE%A43.png)





##### 服务端：

配置两台服务端：

```js
cloud-eureka-server7001
cloud-eureka-server7002
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E9%9B%86%E7%BE%A43.png)

###### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```



#### 主启动：

```java
package com.qsl.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
public class eurekaMain7002 {
    public static void main(String[] args) {
        SpringApplication.run(eurekaMain7002.class,args);
    }
}
```



##### yml:

```yaml
server:
  port: 7002

eureka:
  instance:
    hostname: eqreka7002.com # eureka服务端名称 （ ip,win10/hosts ）
  client:
    register-with-eureka: false # 是否向注册中心注册自己  true:向  ，false：不向
    fetch-registry: false # false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://eqreka7001.com:7001/eureka/

  server:
    enable-self-preservation: false # 自我保护机制-默认开启，保证不可用服务被及时踢除
    eviction-interval-timer-in-ms: 2000 # 发心跳时间，默认90秒
      
```

#### win10：

win10：C:\Windows\System32\drivers\etc\hosts文件新增映射路径。

```java
# springCloud-Eureka
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```

 浏览器访问：

```java
http://localhost:7001/
http://localhost:7002/
http://eureka7001.com:7001/
http://eureka7002.com:7002/
```

成功图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/EurekaService%E9%9B%86%E7%BE%A4.png)





#### 客户端:

配置两台客户端：

```
cloud-consumer-order80
cloud-provider-payment8001
```



##### 依赖：

```xml
<!-- 新版本（2020.2） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!-- 老版本,不推荐（2018） -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

##### 主启动：

```java
package com.atguigu.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient //开启Eureka客户端
public class PaymentMain8001
{
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
```



##### yml:

```yaml
eureka:
  client:
    register-with-eureka: true # 是否在EurekaServer显示，默认true
    fetch-registry: true # 是否从EurekaService抓取已有的注册信息，默认true。单节点无所谓，集群必须设置为true才能配合 “ribbon"使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 服务端地址-集群
  instance:
    instance-id: payment8001 # 修改服务端上名称
    prefer-ip-address: true # 访问路径显示 Ip地址
    lease-renewal-interval-in-seconds: 1 # 客户端向服务端发送心跳的时间间隔，默认30秒
    lease-expiration-duration-in-seconds: 2 # 服务端最大等待客户端心跳时间，默认90秒，超时将服务剔除
```

注意：先要启动EurekaServer（服务端）

浏览器打开：http://localhost:7001/,服务端监控客户端代表成功。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%88%90%E5%8A%9F%E7%BB%93%E6%9E%9C.bmp)





#### 服务发现：

​    保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务。

注意 ：Eureka自我保护属于CAP里面的AP分支。

#### 什么是自我保护模式？

   默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是当网络分区故障发生(延时、卡顿、拥挤)时，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是健康的，此时本不应该注销这个微服务。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。

#### 为什么会产生Eureka自我保护机制？

​    为了防止EurekaClient可以正常运行，但是 与 EurekaServer网络不通情况下，EurekaServer不会立刻将EurekaClient服务剔除

在自我保护模式中，Eureka Server会保护服务注册表中的信息，不再注销任何服务实例。
它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例

#### 优点：

1.   自我保护模式是一种应对网络异常的安全保护措施。
2.   它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留）也不盲目注销任何健康的微服务。
3.   使用自我保护模式，可以让Eureka集群更加的健壮、稳定。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/Eureka-%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0.png)

### 关闭Eureka自我保护

   注意：  自我保护机制默认开启

```yaml
eureka:
  instance:
    hostname: eureka7001.com
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
  server: #关闭自我保护机制，保证不可用服务被及时踢除
       enable-self-preservation: false
       eviction-interval-timer-in-ms: 2000
  instance:
    instance-id: payment8001 # 修改服务端上名称
    prefer-ip-address: true # 访问路径显示 Ip地址
    lease-renewal-interval-in-seconds: 1 # 客户端向服务端发送心跳的时间间隔，默认30秒
    lease-expiration-duration-in-seconds: 2 # 服务端最大等待客户端心跳时间，默认90秒，超时将服务剔除
```

成功关闭Eureka自我保护：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka/%E5%85%B3%E9%97%ADEureka%E8%87%AA%E6%88%91%E4%BF%9D%E6%8A%A4.png)



## Zookeeper:

​    zookeeper是一个分布式协调工具，可以实现注册中心功能。

​    注意：Zookeeper服务节点是临时节点。

​     默认端口：2181

### 服务端

#### 安装：



分别配置三台Zookeeper

```
192.168.139.140
192.168.139.141
192.168.139.142
```

##### 下载并解压Zookeeper

```shell
# 下载Zookeeper安装包
wget http://archive.apache.org/dist/zookeeper/zookeeper-3.5.9/apache-zookeeper-3.5.9-bin.tar.gz
```

##### 修改并打开/conf/zoo_sample.cfg为zoo.cfg

```shell
tickTime=2000
# zookeeper数据存储目录
dataDir=/opt/zookeeper/data # 数据存储位置
clientPort=2181 
initLimit=5
syncLimit=2
server.1=192.168.139.140:2888:3888
server.2=192.168.139.141:2888:3888
server.3=192.168.139.142:2888:3888
# server.A=B.C.D 
# 参数说明：A:代表几号服务器。
#    B:服务器地址。
#    C:服务器Follower与集群中的Leader服务器交换信息的端口。
#    D：集群中的Leader服务挂了执行选举时服务器相互通信的端口。
```

##### 配置`myid`:

```sh
# 1. 创建Zookeeper的数据目录
mkdir /export/server/zookeeper/data

# 2. 创建文件，分别填入1,2,3即可(其他lunux系统安装类似)
vim /export/server/zookeeper/data/myid
```



##### zookeeper/bin:

```shell
./zkServer.sh  start|stop|restart|status # 启动 Zookeeper服务端
jps # 检查 ，有QuorumPeerMain 进程即可

./zkCli.sh # 连接zookeeper
ls /  # 查询所有数据 结果：[zookeeper]
ls /zookeeper # 结果：原始节点[quota] 

get /zookeeper # 获取zookeeper节点信息
create -e /k1 v1 # 创建临时节点
```





### 客户端：

#### 依赖：

```xml
<!-- SpringBoot整合zookeeper客户端3.5.3 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
</dependency>

<dependency> <!-- 默认zookeeper，二选一-->
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.9</version>
</dependency>
```

#### yml:

```yaml
server:
  port: 8004
#服务别名----注册zookeeper到注册中心名称
spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.111.144:2181 # ip
```

#### 主启动：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient //Zookeeper,Consul,Nacos服务发现,服务注册中心;注意没有Eureka。
public class PaymentMain8004
{
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8004.class,args);
    }
}
```



#### Controller:

```java
@RestController
public class PaymentController
{
    @Value("${server.port}")
    private String serverPort;

    @RequestMapping(value = "/payment/zk")
    public String paymentzk()
    {
        return "springcloud with zookeeper: "+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

   浏览器访问：http://localhost:8004/payment/zk

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/zookeeper/Zookeeper%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E6%88%90%E5%8A%9F.png)



### Zookeeper版本冲突问题:

   解决方法：排除一个Zookeeper版本即可。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/zookeeper/Zookeeper%E7%89%88%E6%9C%AC%E5%86%B2%E7%AA%81%E9%97%AE%E9%A2%98.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/zookeeper/Zookeeper%E7%89%88%E6%9C%AC%E5%86%B2%E7%AA%81%E9%97%AE%E9%A2%981.png)

## Consul:

1. 1. ​     Consul 是一套开源的分布式服务发现和配置管理系统，由 HashiCorp 公司用 Go 语言开发。
      ​     提供了微服务系统中的服务治理、配置中心、控制总线等功能。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全方位的服务网格，Consul提供了一种完整的服务网格解决方案。
      ​  

   优点：
   
2. 基于 raft 协议，比较简洁；   

2. 支持健康检查, 同时支持 HTTP 和 DNS 协议 支持跨数据中心的 WAN 集群 提供图形界面 跨平台，支持 Linux、Mac、Windows。

   官网：https://developer.hashicorp.com/consul
   中文文档：https://www.springcloud.cc/spring-cloud-consul.html

功能：

1. 服务发现:提供HTTP和DNS两种发现方式。
2. 健康监测:支持多种方式，HTTP、TCP、Docker、Shell脚本定制化监控
3. KV存储：Key、Value的存储方式。
4. 多数据中心：Consul支持多数据中心。
5. 可视化Web界面

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Consul/Consul%E5%8A%9F%E8%83%BD.png)



### 客户端：

 win10：    Linux系统没用过。

官网下载：https://learn.hashicorp.com/consul/getting-started/install.html

下载后只有一个consul.exe文件，查看版本号信息

```sh
consul -version # 出现版本号即可
consul agent -dev  # 使用开发模式启动Consul-命令行启动
```

   浏览器访问：http://localhost:8500

​    成功页面：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Consul/Consul%E6%9C%8D%E5%8A%A1%E7%AB%AF%E9%A1%B5%E9%9D%A2.png)





### 服务端：

```java
cloud-providerconsul-payment8006  // 服务提供者
cloud-consumerconsul-order83 //服务消费者    
```



#### 依赖：

```xml
<!--SpringCloud consul-server -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>

<!-- SpringBoot整合Web组件（以下可不加） -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### yml:

```yaml
###consul服务端口号
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment # 软件名

  # consul
  cloud:
    consul:
      host: localhost # ip
      port: 8500
      discovery:
        service-name: ${spring.application.name} # 软件名
```

#### 主启动：

```java
@SpringBootApplication
@EnableDiscoveryClient // Zookeeper,Consul,Nacos服务发现,服务注册中心;注意没有Eureka。
public class paymentConsul8006 {
    public static void main(String[] args) {
        SpringApplication.run(paymentConsul8006.class, args);
    }
}
```

#### 配置Bean:

```java
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration // 配置类
public class ApplicationContextBean {

    @Bean
    @LoadBalanced // 开启负载均衡-轮询(默认)
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

#### Controller层:

##### providerconsul-payment8006:

```java
@RestController
public class OrderConsulController
{
    public static final String INVOKE_URL = "http://cloud-provider-payment"; //consul-provider-payment

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/consul")
    public String paymentInfo()
    {
        String result = restTemplate.getForObject(INVOKE_URL+"/payment/consul", String.class);
        System.out.println("消费者调用支付服务(consule)--->result:" + result);
        return result;
    }
}
```



##### consumerconsul-order83:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
public class OrderNacosController {

    @Autowired
    private RestTemplate restTemplate;
    @Value("${service-url.nacos-user-service}")
    private String serverURL; // 方式2

    @GetMapping("/consumer/payment/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Integer id) {
        return restTemplate.getForObject(serverURL+"/payment/nacos/"+id, String.class);
    }
}
```

浏览器访问：http://localhost:8500/ui/dc1/services

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Consul/Consul%E5%AE%A2%E6%88%B7%E7%AB%AF%E9%9B%86%E6%88%90.png)





## 三个注册中心异同点：



###   CAP：

1. C:Consistency（强一致性）

2. A:Availability（可用性）

3. P:Partition tolerance（分区容错性）  
 注意：①CAP理论关注粒度是数据，而不是整体系统设计的策略。

 ​           ②ACP最多只能同时较好的满足两个。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/CAP%E5%8E%9F%E7%90%86.png)


​    CAP核心：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，因此根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：

1. CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
2. CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。
3. AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。

经典CAP图：

   AP架构(Eureka)：当网络分区出现后，为了保证可用性，系统B可以返回旧值，保证系统的可用性。
   结论：违背了一致性C的要求，只满足可用性和分区容错，即AP

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/AP%E6%9E%B6%E6%9E%84%E6%B5%81%E7%A8%8B.png)

CP(Zookeeper/Consul):

   当网络分区出现后，为了保证一致性，就必须拒接请求，否则无法保证一致性
结论：违背了可用性A的要求，只满足一致性和分区容错，即CP。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/CP%E6%9E%B6%E6%9E%84%E6%B5%81%E7%A8%8B.png)



### Eureka、Zookeeper、Consul异同:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Eureka%E3%80%81Zookeeper%E3%80%81Consul%E5%BC%82%E5%90%8C.png)



# 服务调用：

## Ribbon:

 

Ribbon其实就是一个软负载均衡的客户端组件，

​    Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer（简称LB）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。

   RestTemplate提供了多种便捷访问远程Http服务的方法，是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具集。

​    使用restTemplate访问restful接口非常的简单粗暴无脑。(url, requestMap, ResponseBean.class)这三个参数分别代表 REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。

### 原理：

#### Ribbon本地负载均衡客户端 VS Nginx服务端负载均衡区别：

​      Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。

#### 本地负载均衡(进程内的LB),服务端负载均衡(集中式的LB)：

​     Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用技术。

#####     集中式LB：

即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5, 也可以是软件，如nginx), 由该设施负责把访问请求通过某种策略转发至服务的提供方；

##### 进程内LB：

将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

​    Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。



### Ribbon流程图：

负载均衡+RestTemplate调用

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Rabbion/Ribbon%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

Ribbon在工作时分成两步：
第一步先选择 EurekaServer ,它优先选择在同一个区域内负载较少的server.
第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。
其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。







### restTemplate服务调用的两种请求、方法：

官网查看：https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html

   其实还有其他的，但不多用。

```sh
# 注意分为ForObject，ForEntity,二者有分为get，post请求
postForObject: 返回Json数据（返回对象为响应体中数据转换成的对象）

postForEntity:返回ResponseEntity对象,包含了响应头、响应状态码、响应体等
```

#### get和post使用说明：

  cloud-provider-payment8001调用cloud-consumer-order80微服务。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Rabbion/get%E5%92%8Cpost%E8%AF%B7%E6%B1%82%E8%AF%B4%E6%98%8E.png)

#### get请求方法：

```java
// Object    
     <T> T getForObject(String url, Class<T> responseType, Object... uriVariables);
    <T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables);
    <T> T getForObject(URI url, Class<T> responseType);
// Entity
    <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Object... uriVariables);
    <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Map<String, ?> uriVariables);
  <T> ResponseEntity<T> getForEntity(URI var1, Class<T> responseType);
```

#### post请求方法：

```java
// Object
     <T> T postForObject(String url, @Nullable Object request, Class<T> responseType, Object... uriVariables);
    <T> T postForObject(String url, @Nullable Object request, Class<T> responseType, Map<String, ?> uriVariables);
    <T> T postForObject(URI url, @Nullable Object request, Class<T> responseType);
 
 // Entity
   <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request, Class<T> responseType, Object... uriVariables);
   <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request, Class<T> responseType, Map<String, ?> uriVariables);
   <T> ResponseEntity<T> postForEntity(URI url, @Nullable Object request, Class<T> responseType);
```









### ribbon使用：

```java
// 前提使用Eureka,Zookeeper,Consul,Nacos在注册中心注册微服务
cloud-consumer-order80    
cloud-provider-payment8001  // 80模块调用8001模块controller方法
```



#### 依赖:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```

注意：spring-cloud-starter-netflix-eureka-client自带了spring-cloud-starter-ribbon依赖。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Rabbion/eureka-client%E8%87%AA%E5%B8%A6ribbon%E4%BE%9D%E8%B5%96.bmp)



#### Config/ApplicationContextConfig：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/** RestTemplate*/
@Configuration // 配置类
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced // RestTemplate开启负载均衡 
    public RestTemplate restTemplate(){
        return  new RestTemplate();
    }
}
```



#### controller:

##### 调用流程：

   图为post请求的，不用在意。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Rabbion/get%E5%92%8Cpost%E8%AF%B7%E6%B1%82%E8%AF%B4%E6%98%8E.png)

##### consumer-order80：

```java
import com.qsl.springcloud.lb.LoadBalancer;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.net.URI;
import java.util.List;
import java.util.logging.Logger;

@RestController //bean
@RequestMapping("/consumer/payment")
public class OrderController {

    // 调用服务注册中心上的，微服务名称
    public static final String PaymentSrv_URL = "http://CLOUD-PAYMENT-SERVICE";

    @Autowired
    private RestTemplate restTemplate;
    @Autowired
    private DiscoveryClient discoveryClient;
    @Autowired // 注入自定义负载均衡
    private LoadBalancer loadBalancer;

    @GetMapping("/getPayment/{id}")
    public Result getPayment(@PathVariable Long id) {
        String url = PaymentSrv_URL + "/consumer/payment/getPayment/" + id;
        // 读操作    返回Json数据（返回对象为响应体中数据转换成的对象）
        return restTemplate.getForObject(url, Result.class, id);
    }
```



##### payment8001:

```java

import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.service.PaymentService;
import com.qsl.springcloud.utils.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.concurrent.TimeUnit;
@Slf4j
@RestController
@RequestMapping("/consumer/payment/")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;
    @Value("${server.port}")
    private String serverPort; // 端口
    @Autowired
    private DiscoveryClient discoveryClient;

    @GetMapping("getPayment/{id}")
    public Result<Payment> getPaymentById(@PathVariable("id") Long id) {
        Payment payment = paymentService.getPaymentById(id);
        log.info("*****查询结果:{}", payment);
        if (payment != null) {
            return new Result(200, "查询成功，服务端口：" + serverPort, payment);
        } else {
            return new Result(444, "没有对应记录,查询ID: " + id, null);
        }
    }
```





### 替换负载均衡规则：

​    IRule：根据特定算法中从服务列表中选取一个要访问的服务。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Rabbion/%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6IRule.png)

#### 引入包及具体说明：

```java
    com.netflix.loadbalancer.RoundRobinRule //轮询（默认）
    com.netflix.loadbalancer.RandomRule  //随机    
    com.netflix.loadbalancer.RetryRule  //先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试，获取可用的服务     
    WeightedResponseTimeRule  //对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择    
    BestAvailableRule  //   会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务
    AvailabilityFilteringRule //先过滤掉故障实例，再选择并发较小的实例
    ZoneAvoidanceRule  //   默认规则,复合判断server所在区域的性能和server的可用性选择服务器

```

​    注意：这个自定义配置类不能放在@ComponentScan所扫描的当前包(com.qsl.springcloud)下以及子包下，达不到特殊化定制的目的了

#### myrule/MySelfRule:

```java
import com.netflix.loadbalancer.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/** 修改负载均衡-IRule
 * 注意：必须不被@ComponentScan注解扫描到，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享。
 */
@Configuration // 配置类
public class MySelfRule {

    @Bean
    public IRule iRule() {
        // RoundRobinRule: 论询（默认）    RandomRule：随机     BestAvailableRule：会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务
        return new RandomRule(); //随机  
    }
}
```



#### 主启动：

```java
import com.qsl.myrule.MySelfRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)  // cloud-provider-payment8001模块，引入修改负载均衡
public class order80 {
    public static void main(String[] args) {
        SpringApplication.run(order80.class,args);
    }
}
```



config/：

```java
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * RestTemplate
 */
@Configuration // 配置类
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced // RestTemplate开启负载均衡  
    public RestTemplate restTemplate(){
        return  new RestTemplate();
    }

}
```





### 自定义负载均衡算法：

#### 原理：

​     负载均衡算法：rest接口第几次请求数 % 服务器集群总数量 = 实际调用服务器位置下标    （注意：每次服务重启动后rest接口计数从1开始。）

```java
   List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE"); 
 
如：   List [0] instances = 127.0.0.1:8002
　　 　List [1] instances = 127.0.0.1:8001
 
    8001+ 8002 组合成为集群，它们共计2台机器，集群总数为2， 按照轮询算法原理：
 
   当总请求数为1时： 1 % 2 =1 对应下标位置为1 ，则获得服务地址为127.0.0.1:8001
   当总请求数位2时： 2 % 2 =0 对应下标位置为0 ，则获得服务地址为127.0.0.1:8002
   当总请求数位3时： 3 % 2 =1 对应下标位置为1 ，则获得服务地址为127.0.0.1:8001
   当总请求数位4时： 4 % 2 =0 对应下标位置为0 ，则获得服务地址为127.0.0.1:8002
如此类推......

```







```java
// 前提使用Eureka,Zookeeper,Consul,Nacos在注册中心注册微服务
// 80模块调用8001模块controller方法，具体看ribbon使用。
cloud-consumer-order80    
cloud-provider-payment8001  
```

#### lb/LoadBalancer:

```java
import org.springframework.cloud.client.ServiceInstance;

import java.util.List;

/** 自定义负载均衡 */
public interface LoadBalancer {

    //  获取服务器信息  例：请求头，请求路径，端口。。。
    ServiceInstance instances(List<ServiceInstance> serviceInstances);

}
```



#### MyLB实现类:

```java
import com.qsl.springcloud.lb.LoadBalancer;
import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.logging.Logger;

/** 自定义负载均衡算法 */
@Component // 默认bean
@Slf4j
public class MyLB implements LoadBalancer {

    // 默认请求次数
    private AtomicInteger atomicInteger = new AtomicInteger(0);

    // 获取请求次数
    public final int getAndIncrement() {
        int current;// 请求次数
        int next;

        do { // 死循环
            current = this.atomicInteger.get(); //请求次数
            // 三目运行   2147483647：最大整数
            next = current >= 2147483647 ? 0 : current + 1;
        } while (!this.atomicInteger.compareAndSet(current, next));// 期望的值，有值返回true,false返回表示实际值不等于期望值
        System.out.println("****当前请求此次数：" + next);
        return next;
    }


    // 自定义负载均衡算法
    @Override
    public ServiceInstance instances(List<ServiceInstance> serviceInstances) {
//        负载均衡算法：rest接口第几次请求数 % 服务器集群总数量 = 实际调用服务器位置下标
        int index = getAndIncrement() % serviceInstances.size();// 请求数，集群数
        return serviceInstances.get(index);
    }
}
```

#### Config/ApplicationContextConfig:

```java
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/** RestTemplate*/
@Configuration // 配置类
public class ApplicationContextConfig {

    @Bean // 注意：此次不用使用@RestTemplate开启负载均衡
    public RestTemplate restTemplate(){
        return  new RestTemplate();
    }
}
```



#### Controller:

​    cloud-consumer-order80模块引用 cloud-provider-payment8001的 `lb方法`。

##### cloud-consumer-order80模块:

```java
import com.qsl.springcloud.lb.LoadBalancer;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.net.URI;
import java.util.List;
import java.util.logging.Logger;

@RestController //bean
@RequestMapping("/consumer/payment")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;
    @Autowired
    private DiscoveryClient discoveryClient;
    @Autowired // 注入自定义负载均衡
    private LoadBalancer loadBalancer;

    // 自定义负载均衡
    @GetMapping("lb")
    public String getPaymentLB() {
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE"); //根据服务名获取信息

        if (instances == null || instances.size() <= 0) {
            return null;
        }
        ServiceInstance serviceInstance = loadBalancer.instances(instances);
        URI uri = serviceInstance.getUri();

        return restTemplate.getForObject(uri + "/consumer/payment/lb", String.class); // 调用服务端返回数据-8001,8002
    }
}
```



##### cloud-provider-payment8001模块：

```java
@Slf4j
@RestController
@RequestMapping("/consumer/payment/")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;
    @Value("${server.port}")
    private String serverPort; // 端口
    
    // 获取端口
    @GetMapping("lb")
    public String getPaymentLB() {
        return serverPort;
    }
}
```





## OpenFeign：

​     Feign是一个声明式WebService客户端。使用Feign能让编写Web Service客户端更加简单。
​    它的使用方法是定义一个服务接口然后在上面添加注解。Feign也支持可拔插式的编码器和解码器。Spring Cloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用以支持负载均衡。

功能：

Feign旨在使编写Java Http客户端变得更容易。
前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。

Feign集成了Ribbon：
利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，通过feign只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用。



### Feign和OpenFeign的区别：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/OpenFeign/Feign%E5%92%8COpenFeign%E7%9A%84%E5%8C%BA%E5%88%AB.png)



```java
// 前提：必须开启微服务调用。
// 注意：Feign自带负载均衡，故无需配置RestTemplate来开启负载均衡。
cloud-consumer-feign-order80 //
    
```

### 整合OpenFeign:

#### 依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

#### 主启动：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients // 开启Feign服务调用
public class OrderFeignMain80
{
    public static void main(String[] args)
    {
        SpringApplication.run(OrderFeignMain80.class,args);
    }
}
```

#### Service：

```java
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE")//开启服务调用
public interface PaymentFeignService
{
    @GetMapping(value = "/payment/get/{id}")
    CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
}
```

#### controller:

```java
import com.atguigu.springcloud.entities.CommonResult;
import com.atguigu.springcloud.entities.Payment;
import com.atguigu.springcloud.service.PaymentFeignService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController 
public class OrderFeignController
{
    @Resource 
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        return paymentFeignService.getPaymentById(id);
    }
}
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/OpenFeign/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8.png)



#### 日志：



#### yml:

```yaml
# openFeign日志
logging:
  level:     # debug级别监控 PaymentFeignService接口
     com.qsl.springcloud.service.PaymentFeignService: debug
```







### OpenFeign超时控制:

OpenFeign默认等待1秒钟，超过后报错。 

```yaml
ribbon: # 设置feign客户端超时时间(OpenFeign默认支持ribbon)
  ReadTimeout: 5000  # 两端连接所用的时间
  # 指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000 # 最大超时连接时间
```

服务调用超时异常:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/OpenFeign/%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8%E8%B6%85%E6%97%B6%E5%BC%82%E5%B8%B8.png)





OpenFeign默认引入Ribbon依赖:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/OpenFeign/OpenFeign%E9%BB%98%E8%AE%A4%E5%BC%95%E5%85%A5Ribbon%E4%BE%9D%E8%B5%96.png)



### 开启OpenFeign日志:

日志级别：

```
NONE：不显示任何日志（默认）；
BASIC：打印请求方法、URL、响应状态码及执行时间；
HEADERS：打印请求和响应的头、BASIC中定义的信息等。
FULL：打印请求和响应的正文和元数据，HEADERS中定义的信息等。
```

#### 配置日志bean:

  Config/FeignConfig:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import feign.Logger;

@Configuration
public class FeignConfig
{
    @Bean
    Logger.Level feignLoggerLevel()
    { //打印请求和响应的正文和元数据，HEADERS中定义的信息等。
        return Logger.Level.FULL; 
    }
}
```

yml:

```yaml
logging: # openFeign日志
  level:
    # debug级别监控 PaymentFeignService接口
    com.atguigu.springcloud.service.PaymentFeignService: debug
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/OpenFeign/%E5%BC%80%E5%90%AFOpenFeign%E6%97%A5%E5%BF%97.png)



| 注解                | 说明               |
| ------------------- | ------------------ |
| @EnableFeignClients | 开启Feign客户端    |
| @FeignClient        | 开启调用微服务调用 |
|                     |                    |



# 服务降级：

## Hystrix:

###   简介：

Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

出现原因:

 服务雪崩：多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的“扇出”。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”.

功能：
1. 服务降级-fallback：
2. 服务熔断-break
3. 接近实时的监控
4. 服务限流-flowlimit，隔离。。。

​    优先级： 服务的降级->进而熔断->恢复调用链路。

哪些情况会出发降级：
①程序运行异常
②超时
③服务熔断触发服务降级
④线程池/信号量打满也会导致服务降级

​      服务降级-fallback：服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback。
​      服务熔断-break：类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示。
​     服务限流：秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行。

依赖：

```xml
<!--hystrix-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

### 服务降级：

服务降级服务端和客户端都行，但一般使用客户端。

   服务降级的优先级： 接口层 > 方法  >  controller、service层  。

##### 单个

###### 主启动：

```java
@SpringBootApplication
@EnableFeignClients // 开启服务调用-Feign
@EnableHystrix // 开启服务降级
public class OrderHystrix80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrix80.class, args);
    }
}
```

###### Controller层：

Service/Controller层都行。

```java
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import com.qsl.springcloud.service.PaymentHystrixService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController // bean
public class OrderHystirxController {

    @Autowired
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/feign/timeout/{id}")
    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod", commandProperties = {   // fallbackMethod：异常等执行此方法。
        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "1500") // 当前线程最大等待3秒(3秒以内执行正常业务)
    })
    public String paymentInfo_TimeOut(@PathVariable("id") Long id) {
        return paymentHystrixService.paymentInfo_TimeOut(id);
    }


    public String paymentTimeOutFallbackMethod(@PathVariable("id") Long id){
        return "我是消费者80,对方支付系统繁忙请10秒钟后再试,o(╥﹏╥)o";
    }
}
```



##### 全局：

图解：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Hystrix/%E6%9C%8D%E5%8A%A1%E9%99%8D%E7%BA%A7-%E5%85%A8%E5%B1%80.bmp)



###### 主启动：

```java
@SpringBootApplication
@EnableFeignClients // 开启服务调用-Feign
@EnableHystrix // 开启服务降级
public class OrderHystrix80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrix80.class, args);
    }
}
```

###### Controller/Service层:

```java

import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.qsl.springcloud.service.PaymentHystrixService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController // bean
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod") // 服务降级全局
public class OrderHystirxController {

    @GetMapping("/consumer/payment/feign/timeout/{id}")
    @HystrixCommand // 使用服务降级-默认全局
    public String paymentInfo_TimeOut(@PathVariable("id") Long id) {
        return paymentHystrixService.paymentInfo_TimeOut(id);
    }

    // 定义全局降级方法
    public String payment_Global_FallbackMethod() {
        return "全局降级,对方支付系统繁忙请10秒钟后再试,o(╥﹏╥)o";
    }

}
```



##### 接口：

###### yml:

```yaml
feign:
  hystrix: #用于在注解@FeignClient中添加服务降级-fallbackFactory属性值
    enabled: true  # 开启服务降级
```

###### service层：

```java
package com.qsl.springcloud.service;

import com.qsl.springcloud.service.impl.PaymentFallbackServiceImpl;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Service // bean
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackServiceImpl.class) // 根据Eureka服务端使用Feign调用
public interface PaymentHystrixService {

    @GetMapping("/payment/hystrix/ok/{id}")
    String paymentInfo_OK(@PathVariable("id") Long id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    String paymentInfo_TimeOut(@PathVariable("id") Long id);
}
```

###### Service Impl:

```java
package com.qsl.springcloud.service.impl;

import com.qsl.springcloud.service.PaymentHystrixService;
import org.springframework.stereotype.Component;

/**
 * 服务降级-统一返回异常处理
 */
@Component // 默认bean
public class PaymentFallbackServiceImpl implements PaymentHystrixService {


    @Override
    public String paymentInfo_OK(Long id) {
        return "PaymentFallbackServiceImpl-接口级_服务降级 —— paymentInfo_OK";
    }

    @Override
    public String paymentInfo_TimeOut(Long id) {
        return "PaymentFallbackServiceImpl-接口级_服务降级 —— paymentInfo_TimeOut";
    }
}

```





宕机：

代码膨胀，混乱。



### 服务熔断：

###### 主启动：

```java
@SpringBootApplication
@EnableFeignClients // 开启服务调用-Feign
@EnableHystrix // 开启服务降级
public class OrderHystrix80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrix80.class, args);
    }
}
```



###### Service层:

```java
@Service
public class PaymentServiceImpl implements PaymentService {
    /*    paymentCircuitBreaker_fallback: 异常调用方法，
          commandProperties： 10秒内10次请求有6次异常跳闸   */
    @HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback", commandProperties = {
        @HystrixProperty(name="circuitBreaker.enabled",value = "true"), // 是否使用断路器
        @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"), // 请求次数
        @HystrixProperty(name="circuitBreaker.sleepWindowInMilliseconds",value = "10000"), // 时间窗口期、时间段
        @HystrixProperty(name="circuitBreaker.errorThresholdPercentage",value = "60") // 失败率、异常达到多少后跳闸
    })
    public String paymentCircuitBreaker(@PathVariable("id") Integer id) {

        if (id < 0) {
            throw new RuntimeException("*********id 不能为负数");
        }
        String simpleUUID = IdUtil.simpleUUID(); // UUID

        return Thread.currentThread().getName() + "\t" + "调用成功，流水号：" + simpleUUID;
    }

    public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id) {
        return "id 不能为负数，o(╥﹏╥)o  id:" + id;
    }
}
```



###### Controller层：

```java
@Slf4j
@RestController // bean
public class PaymentController {
    .....
        
    @Autowired
    private PaymentService paymentServicel;
    @GetMapping("/payment/circuit/{id}")
    public String paymentCircuitBreaker(@PathVariable Integer id) {
        String result = paymentServicel.paymentCircuitBreaker(id);
        log.info("****result: "+result);
        return result;
    }
    .....
} 
```



### 服务监控：

依赖：

```xml
<!--   hystrix-dashboard依赖：服务监控     -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>


<!--web-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- actuator监控信息完善 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



##### 服务监控异常：

服务监控异常：
      ①Unable to connect to Command Metric Stream.
      ②404

注意:新版本Hystrix需要在主启动类MainAppHystrix8001中指定监控路径。

主启动：

```java
@SpringBootApplication
@EnableEurekaClient //本服务启动后会自动注册进eureka服务中
@EnableCircuitBreaker//对hystrixR熔断机制的支持
public class MainAppHystrix8001
{
    public static void main(String[] args)
    {
        SpringApplication.run(MainAppHystrix8001.class,args);
    }

    /**
 *此配置是为了服务监控而配置，与服务容错本身无关，springcloud升级后的坑
 *ServletRegistrationBean因为springboot的默认路径不是"/hystrix.stream"，
 *只要在自己的项目里配置上下面的servlet就可以了
 */
    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }

}

```



## Sentienl:

​     Sentinel 是面向分布式、多语言异构化服务架构的流量治理组件，主要以流量为切入点，从流量路由、流量控制、流量整形、熔断降级、系统自适应过载保护、热点流量防护等多个维度来帮助开发者保障微服务的稳定性。

  Sentinel 是轻量级的流量控制、熔断降级Java库。

Sentinel 分为两个部分:

- 核心库（Java 客户端）不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持。
- 控制台（Dashboard）基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。



Sentinel 的主要特性：

​    简单概况：服务雪崩，服务降级，服务熔断，服务限流。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/Sentinel%E7%9A%84%E4%B8%BB%E8%A6%81%E7%89%B9%E6%80%A7%EF%BC%9A.png)

Sentinel 的开源生态：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/Sentinel%20%E7%9A%84%E5%BC%80%E6%BA%90%E7%94%9F%E6%80%81%EF%BC%9A.png)









### 安装：

  sentienl为一个jar包无需安装，使用java -jar命令即可启动。

注意：Sentienl的默认端口8080。

​          账号、密码为sentienl。

Sentienl启动页面：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/sentinel%E7%9A%84jar%E5%8C%85%E5%90%AF%E5%8A%A8%E6%88%90%E5%8A%9F.png)



依赖：

```xml
<!--SpringCloud ailibaba sentinel -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
<!--SpringCloud ailibaba sentinel-datasource-nacos 后续做持久化用到-->
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

主启动：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient // 开启客户端
public class SentienlService8401 {
    public static void main(String[] args) {
        SpringApplication.run(SentienlService8401.class, args);
    }
}
```

yml:

```yaml
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  #  SpringCloud Sentinel
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848  # Nacos服务注册中心
    sentinel:
      transport:
        dashboard:  localhost:8080 # sentinel ip
        port: 8719 # 默认端口8719，若端口别占用从该端口+1扫描,直至找到未被占用的端口。

management:
  endpoints:
    web:
      exposure:
        include: "*" # 暴露所有端点
```

Controller层:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // bean
public class FlowLimitController {

    @GetMapping("/testA")
    public String testA() {
        return "------testA";
    }

    @GetMapping("/testB")
    public String testB() {
        return "------testB";
    }
}
```

路径：http://localhost:8080/#/login

注意：Sentinel使用的是懒加载，必须调用controller层接口才行。

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/Sentinel%E6%9C%8D%E5%8A%A1%E7%AB%AF.bmp)





### 流控规则：

流量限制、控制规则。

#### 流控模式

##### 直接(默认)：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-%E7%9B%B4%E6%8E%A5.png)



##### 关联：

​       关联：儿子(A)犯错，父亲(B)赔罪。

​      当关联资源/testB的qps阀值超过1时，就限流/testA的Rest访问地址，当关联资源到阈值后限制配置好的资源名。

​    注意：资源名称可以使用get请求方法和@SentinelResource注释名称。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-%E5%85%B3%E8%81%94.png)

使用postman高并发测试：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-%E5%85%B3%E8%81%942.png)



##### 链路：

​    链路省略：

​      一人犯错，满门抄斩。



#### 流控效果：



##### Warm Up(预热)：

Warm Up(预热/限流 冷启动)：

​      公式：阈值除以coldFactor(默认值为3),经过预热时长后才会达到阈值。

​        默认 coldFactor 为 3，即请求QPS从(threshold / 3) 开始，经多少预热时长才逐渐升至设定的 QPS 阈值。
​        案例，阀值为10+预热时长设置5秒。
系统初始化的阀值为10 / 3 约等于3,即阀值刚开始为3；然后过了5秒后阀值才慢慢升高恢复到10

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-WarmUp(%E5%86%B7%E5%90%AF%E5%8A%A8).png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-WarmUp(%E5%86%B7%E5%90%AF%E5%8A%A8)1.png)

​    使用场景：秒杀系统在开启的瞬间，会有很多流量上来，很有可能把系统打死，预热方式就是把为了保护系统，可慢慢的把流量放进来，慢慢的把阀值增长到设置的阀值。



##### 排队等待：

原理：

​       匀速排队（`RuleConstant.CONTROL_BEHAVIOR_RATE_LIMITER`）方式会严格控制请求通过的间隔时间，也即是让请求以均匀的速度通过，对应的是漏桶算法。
​      匀速排队，让请求以均匀的速度通过。
注意：①匀速排队的阀值类型必须设成QPS，否则无效。
​           ②匀速排队模式暂时不支持 QPS > 	的场景。
​       使用场景：主要用于处理间隔性突发的流量。在某一秒有大量的请求到来，而接下来的几秒则处于空闲状态，我们希望系统能够在接下来的空闲期间逐渐处理这些请求，而不是在第一秒直接拒绝多余的请求。例如消息队列。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-%E5%8C%80%E9%80%9F%E6%8E%92%E9%98%9F%E5%8E%9F%E7%90%86.png)



应用：

​    设置含义：/testA每秒1次请求，超过的话就排队等待，等待的超时时间为20000毫秒。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E6%B5%81%E6%8E%A7%E8%A7%84%E5%88%99-%E5%8C%80%E9%80%9F%E6%8E%92%E9%98%9F.png)





### 降级/熔断规则:

降级规则就是当满足什么条件的时候，对服务进行降级。

降级策略：

- **慢调用比例：**当资源的响应时间超过最大RT（单位:ms，最大RT即最大响应时间）之后，资源进入准降级状态。如果接下来1s内持续进入5个请求（最小请求数），它们的RT都持续超过这个阈值，那么在接下来的熔断时长之内，就会对这个方法进行服务降级。  注意：“比例阈值”设置无效，下次编辑会重置成1*

​      注意Sentinel默认统计的RT上限是4900ms，超出此阈值的都会算作4900ms，更大的需要通过启动配置项-Dcsp.sentinel.statistic.max.rt=xxx来配置。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E9%99%8D%E7%BA%A7%E7%AD%96%E7%95%A5-%E6%85%A2%E8%B0%83%E7%94%A8%E6%AF%94%E4%BE%8B.png)



异常比列（秒级）：①当资源的每秒请求数大于等于最小请求数，并且异常总数占通过量的比例超过比例阈值时，资源进入降级状态。
    QPS >= 5 且异常比例（秒）超过阈值时，触发降级；时间窗口结束后，关闭降级

   当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。异常比率的阈值范围是 `[0.0, 1.0]`，代表 0% - 100%。

​     最大RT:最长时间处理本次请求，没处理完，断电/限流。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E9%99%8D%E7%BA%A7%E7%AD%96%E7%95%A5-%E6%85%A2%E8%B0%83%E7%94%A8%E6%AF%94%E4%BE%8B1.png)



异常数（分钟级）
     异常数（分钟统计）超过阈值时，触发降级；时间窗口(熔断时长)结束后，关闭降级。

注意：熔断时长一定要大于等于60秒。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E9%99%8D%E7%BA%A7%E7%AD%96%E7%95%A5-%E5%BC%82%E5%B8%B8%E6%95%B0.png)



### 热点规则：

​     热点参数限流会统计传入参数中的热点参数，并根据配置的限流阈值与模式，对包含热点参数的资源调用进行限流。热点参数限流可以看做是一种特殊的流量控制，仅对包含热点参数的资源调用生效。

#### 普通：

Controller层：

```java
@Slf4j
@RestController // bean
public class FlowLimitController {


    @GetMapping("/testHotKey")
    @SentinelResource(value = "testHotKeys", blockHandler = "deal_testHotKey") //使用服务降级：降级名称，降级方法
    public String testHotKey(@RequestParam(value = "p1", required = false) String p1,  //可选参数：required
                             @RequestParam(value = "p2", required = false) String p2) {
        return "====> testHotKey p1: "+p1+" p2:"+p2;
    }

    // 降级方法
    public String deal_testHotKey(String p1, String p2, BlockException e){
        return  "------ deal_testHotKey,o(╥﹏╥)o"; // Sentinel系统默认的提示：Blocked by Sentinel (flow limiting)
    }
}
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E7%83%AD%E7%82%B9%E8%A7%84%E5%88%99-%E6%97%A0%E9%AB%98%E7%BA%A7%E9%80%89%E9%A1%B9.png)



#### 高级选项：

注意：热点参数的注意点，参数必须是基本类型或者String

Controller层：

```java
@Slf4j
@RestController // bean
public class FlowLimitController {

    @GetMapping("/testHotKey")
    @SentinelResource(value = "testHotKeys", blockHandler = "deal_testHotKey") //使用服务降级：降级名称，降级方法
    public String testHotKey(@RequestParam(value = "p1", required = false) String p1,  //可选参数：required
                             @RequestParam(value = "p2", required = false) String p2) {
        return "====> testHotKey p1: "+p1+" p2:"+p2;
    }

    // 降级方法
    public String deal_testHotKey(String p1, String p2, BlockException e){
        return  "------ deal_testHotKey,o(╥﹏╥)o"; // Sentinel系统默认的提示：Blocked by Sentinel (flow limiting)
    }
}
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E7%83%AD%E7%82%B9%E8%A7%84%E5%88%99-%E9%AB%98%E7%BA%A7%E9%80%89%E9%A1%B9.png)



### 系统规则

系统保护规则是从应用级别的入口流量进行控制，从单台机器的 load、CPU 使用率、平均 RT、入口 QPS 和并发线程数等几个维度监控应用指标，让系统尽可能跑在最大吞吐量的同时保证系统整体的稳定性。

系统保护规则是应用整体维度的，而不是资源维度的，并且**仅对入口流量生效**。入口流量指的是进入应用的流量（`EntryType.IN`），比如 Web 服务或 Dubbo 服务端接收的请求，都属于入口流量。

系统规则支持以下的模式：

- **Load 自适应**（仅对 Linux/Unix-like 机器生效）：系统的 load1 作为启发指标，进行自适应系统保护。当系统 load1 超过设定的启发值，且系统当前的并发线程数超过估算的系统容量时才会触发系统保护（BBR 阶段）。系统容量由系统的 `maxQps * minRt` 估算得出。设定参考值一般是 `CPU cores * 2.5`。
- **CPU usage**（1.5.0+）：当系统 CPU 使用率超过阈值即触发系统保护（取值范围 0.0-1.0），比较灵敏。
- **平均 RT**：当单台机器上所有入口流量的平均 RT 达到阈值即触发系统保护，单位是毫秒。
- **并发线程数**：当单台机器上所有入口流量的并发线程数达到阈值即触发系统保护。
- **入口 QPS**：当单台机器上所有入口流量的 QPS 达到阈值即触发系统保护。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E7%B3%BB%E7%BB%9F%E8%A7%84%E5%88%99.png)





### 自定义降级：

#### 降级参数说明：

​    注意： @SentinelResource 注解不支持 private 方法,若有 private 会使用默认的降级服务。

​     Sentinel三个主要核心Api：①SphU定义资源，②Tracer定义统计，③ContextUtil定义了上下文。

优先级：fallback > blockHandler。

| 参数               | 说明                           |
| ------------------ | ------------------------------ |
| value              | 服务降级Id                     |
| blockHandlerClass  | 服务降级类                     |
| blockHandler       | 服务降级方法                   |
| fallback           | 运行时异常                     |
| exceptionsToIgnore | 指定异常不进行服务降级(可多个) |
|                    |                                |



#### 方法：

controller层:

```java
@Slf4j
@RestController // bean
public class FlowLimitController {


    @GetMapping("/testHotKey")
    @SentinelResource(value = "testHotKeys", blockHandler = "deal_testHotKey") //使用服务降级：降级名称，降级方法
    public String testHotKey(@RequestParam(value = "p1", required = false) String p1,  //可选参数：required
                             @RequestParam(value = "p2", required = false) String p2) {
        return "====> testHotKey p1: "+p1+" p2:"+p2;
    }

    // 降级方法
    public String deal_testHotKey(String p1, String p2, BlockException e){
        return  "------ deal_testHotKey,o(╥﹏╥)o"; // Sentinel系统默认的提示：Blocked by Sentinel (flow limiting)
    }
}
```



#### 类：

myHandler.CustomerBlockHandler:

```java
package com.qsl.springcloud.myHandler;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.qsl.springcloud.utils.Result;

/**
 * 自定义降级方法
 */
public class CustomerBlockHandler {

    public static Result handleException2(BlockException exception) {
        return new Result(4444, "自定义的限流处理信息2......CustomerBlockHandler");
    }
}
```



controller层：

```java
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.qsl.springcloud.myHandler.CustomerBlockHandler;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // bean
public class RateLimitController {
    
    /*** 自定义降流信息*/
    @GetMapping("/rateLimit/byUrl")
    @SentinelResource(value = "byUrl", blockHandlerClass = CustomerBlockHandler.class, blockHandler = "handleException2")
    public Result byUrl() {
        return new Result(200, "自定义降流", new Payment(2020L, "serial002"));
    }
}
```



流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%99%8D%E7%BA%A7%E4%BF%A1%E6%81%AF.png)



# 服务网关：

有无网关前后对比:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E6%9C%89%E6%97%A0%E7%BD%91%E5%85%B3%E5%89%8D%E5%90%8E%E5%AF%B9%E6%AF%94.png)

## Zuul:

Zuul是一种提供动态路由、监视、弹性、安全性等功能的边缘服务。
Zuul是Netflix出品的一个基于JVM路由和服务端的负载均衡器。
​     Zuul技术太老，略。。。



## Gateway(网关):

​     SpringCloud  Gateway是在Spring生态系统之上构建的API网关服务，基于Spring 5，Spring Boot 2和 Project Reactor等技术开发的网关。
​        Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的过滤器功能， 例如：熔断、限流、重试等。
​     Gateway旨在提供统一的路由方式且基于 Filter 链的方式提供了网关基本的功能，例如：安全，监控/指标，限流等。
​    Gateway是基于WebFlux框架实现的，而WebFlux框架底层则使用了高性能的Reactor模式通信框架Netty，大大提升网关的性能。
​     Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/Gateway%E5%BA%95%E5%B1%82%E4%BD%BF%E7%94%A8Netty%E9%80%9A%E8%AE%AF%E6%A1%86%E6%9E%B6.png)

功能：①反向代理  ②鉴权  ③流量控制  ④熔断 ⑤日志监控

项目流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E7%BD%91%E5%85%B3%E6%B5%81%E7%A8%8B%E5%9B%BE.png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E7%BD%91%E5%85%B3%E6%B5%81%E7%A8%8B%E5%9B%BE2.png)



Gateway是基于异步非阻塞模型上进行开发的，性能方面不需要担心。

**Gateway 具有如下特性：**

基于Spring Framework 5, Project Reactor 和 Spring Boot 2.0 进行构建；
1. 动态路由：能够匹配任何请求属性；
2. 可以对路由指定 Predicate（断言）和 Filter（过滤器）；
3. 集成Hystrix的断路器功能；
4. 集成 Spring Cloud 服务发现功能；
5. 易于编写的 Predicate（断言）和 Filter（过滤器）；
6. 请求限流功能；
7. 支持路径重写。

### Gateway核心概念：

网关提供API全托管服务，丰富的API管理功能，辅助企业管理大规模的API，以降低管理成本和安全风险，包括协议适配、协议转发、安全策略、防刷、流量、监控日志等贡呢。一般来说网关对外暴露的URL或者接口信息，我们统称为路由信息。网关的核心是Filter以及Filter Chain（Filter责任链）。Sprig Cloud Gateway也具有路由和Filter的概念。

**（1）Route(路由): **路由是网关最基础的部分，路由信息有一个ID、一个目的URL、一组断言和一组Filter组成。如果断言路由为true，则请求的URL和配置匹配。

**（2）Predicate(断言):** Java8中的断言函数。Spring Cloud Gateway中的断言函数输入类型是Spring5.0框架中的ServerWebExchange。Spring Cloud Gateway中的断言函数允许开发者去定义匹配来自于http request中的任何信息，比如请求头和参数等。**如果请求与断言相匹配则进行路由.**

**（3）Filter(过滤)： **一个标准的Spring webFilter。Spring cloud gateway中的filter分为两种类型的Filter，分别是Gateway Filter和Global Filter。过滤器Filter将会对请求和响应进行修改处理

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E8%B7%AF%E7%94%B1%E3%80%81%E6%96%AD%E8%A8%80%E3%80%81%E8%BF%87%E6%BB%A4%E6%B5%81%E7%A8%8B.png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E7%BD%91%E5%85%B3%E6%B5%81%E7%A8%8B%E5%9B%BE3.png)


  客户端向 Spring Cloud Gateway 发出请求。然后在 Gateway Handler Mapping 中找到与请求相匹配的路由，将其发送到 Gateway Web Handler。

​    Handler 再通过指定的过滤器链来将请求发送到我们实际的服务执行业务逻辑，然后返回。
过滤器之间用虚线分开是因为过滤器可能会在发送代理请求之前（“pre”）或之后（“post”）执行业务逻辑。

​    Filter在“pre”类型的过滤器可以做参数校验、权限校验、流量监控、日志输出、协议转换等，在“post”类型的过滤器中可以做响应内容、响应头的修改，日志的输出，流量监控等有着非常重要的作用



### gateway9527模块：

#### 依赖：

```xml
<!--gateway-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>


<dependency> <!--eureka：配置eureka的目的主要使用动态路由 -->
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### 主启动：

```java

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
public class Gateway9527 {
    public static void main(String[] args) {
        SpringApplication.run(Gateway9527.class, args);
    }
}
```

#### 网关路由：

##### 静态网关路由：

###### yml配置：

```yaml
spring:
  application:
    name: cloud-gateway
  cloud: # 网关配置1——yml配置
    gateway:
      routes:
        - id: payment_routh #payment_route    #路由的ID，建议配合服务名
          uri: http://localhost:8001   #匹配后提供服务的路由地址
          predicates:
            - Path=/consumer/payment/getPayment/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，建议配合服务名
          uri: http://localhost:8001   #匹配后提供服务的路由地址
          predicates:
            - Path=/consumer/payment/lb # 断言，路径相匹配的进行路由
```

图解：

 `Gateway9527模块`在 `provider-payment8001模块` 设置网关

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/%E9%9D%99%E6%80%81%E7%BD%91%E5%85%B3%E8%B7%AF%E7%94%B1-yml.png)

###### bean配置：

```java
@Configuration // 配置类
public class  GatewayConfig {

    /** 网关配置2-bean配置 */
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        RouteLocatorBuilder.Builder routes = builder.routes();
        // 原百度官网崩了，换csdm的测试但图片排版等有点问题。
        routes.route("path_route_CSDN", r -> r.path("/nav/back-end").uri("https://blog.csdn.net/nav/back-end")).build(); //后端
        routes.route("path_route_CSDN", r -> r.path("/nav/web").uri("https://blog.csdn.net/nav/web")).build(); // 前端
        return routes.build();
    }
}
```



#### 动态路由：

  前提知识：
​    uri以lb://开头（lb代表从注册中心获取服务），后面接的就是你需要转发到的服务名称，lb是负载均衡的意思。
   路由的三种方式：
①ws://host:port（websocket方式）
②http://host:port（HTTP方式）
③lb://微服务名

yml:

```yaml
  cloud: # 静态路由配置1——yml配置
    gateway:
      discovery:
        locator:
          enabled: true # 开启从服务中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh     #路由的ID，建议配合服务名
          uri: lb://CLOUD-PAYMENT-SERVICE # 根据微服务名匹配路由地址
          predicates: # 断言
            - Path=/consumer/payment/getPayment/**         # Controller,Service请求路径
        - id: payment_routh2     #路由的ID，建议配合服务名
          uri: lb://CLOUD-PAYMENT-SERVICE # 根据微服务名匹配路由地址
          predicates:
            - Path=/consumer/payment/lb  # 断言，路径相匹配的进行路由
            
 eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```





## predicates九大参数：

```sh
  curl http://localhost:9527/payment/lb -H "X-Request-Id:123" #带请求头    
  
  Host  接收一组参数，一组匹配的域名列表，这个模板是一个 ant 分隔的模板，用.号作为分隔符。它通过参数中的主机地址作为匹配规则。
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Gateway/predicates%E4%B9%9D%E5%A4%A7%E5%8F%82%E6%95%B0.png)

```yaml
  cloud: # 静态路由配置1——yml配置
    gateway:
      discovery:
        locator:
          enabled: true # 开启从服务中心动态创建路由功能，利用微服务名进行路由
      routes:
        - id: payment_routh2 #路由的ID
          uri: lb://CLOUD-PAYMENT-SERVICE  # 根据微服务名匹配路由地址
          predicates:   # 断言
            - Path=/consumer/payment/lb # Controller,Service请求路径
            - After=2022-04-13T22:22:18.827-04:00[America/Cuiaba] # 当前时间段之后才能访问 （可用在指定时间版本更新）
            - Cookie=username,zzyy  # 带Cookie请求。 Cookie需要两个参数，一个是 Cookie name ,一个是正则表达式。
#  路由规则会通过获取对应的 Cookie name 值和正则表达式去匹配，如果匹配上就会执行路由，如果没有匹配上则不执行
            - Header=X-Request-Id, \d+  # 请求头要有X-Request-Id，并且值为整数的正则表达式。
            - Host= **.qsl.com # 域名匹配
            - Method=GET # get请求 
#            - Query=username   # 两个参数：属性名，属性值（属性值可以是正则表达式）
```



### filter:

单个Filter：

全局Filter：

#### 自定义Filter：



### 网关解决跨域问题：

config/GatewayCorsConfig:

```java
package com.qsl.ggktparent.gateway.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.reactive.CorsWebFilter;
import org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource;
import org.springframework.web.util.pattern.PathPatternParser;

/**
 * 网关解决跨域问题:
 * 覆盖@CrossOrigin：请求支持跨域注解；
 * 注意：网关跨域方案不能与@CrossOrigin注解一起使用，否则跨域请求会失败。
 */
@Configuration // 配置类
public class GatewayCorsConfig {

    // 网关处理跨域问题
    @Bean
    public CorsWebFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedMethod("*"); //
        config.addAllowedOrigin("*");
        config.addAllowedHeader("*"); // 所有请求头
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource(new PathPatternParser());
        source.registerCorsConfiguration("/**", config);
        return new CorsWebFilter(source);
    }
}
```





## Gateway 与 Zuul的区别:

​      在SpringCloud Finchley 正式版之前，Spring Cloud 推荐的网关Zuul。

1、Zuul 1.x，是一个基于阻塞 I/ O 的 API Gateway
2、Zuul 1.x 基于Servlet 2. 5使用阻塞架构它不支持任何长连接(如 WebSocket) Zuul 的设计模式和Nginx较像，每次 I/ O 操作都是从工作线程中选择一个执行，请求线程被阻塞到工作线程完成，但是差别是Nginx 用C++ 实现，Zuul 用 Java 实现，而 JVM 本身会有第一次加载较慢的情况，使得Zuul 的性能相对较差。
3、Zuul 2.x理念更先进，想基于Netty非阻塞和支持长连接，但SpringCloud目前还没有整合。 Zuul 2.x的性能较 Zuul 1.x 有较大提升。在性能方面，根据官方提供的基准测试， Spring Cloud Gateway 的 RPS（每秒请求数）是Zuul 的 1. 6 倍。
4、Spring Cloud Gateway 建立 在 Spring Framework 5、 Project Reactor 和 Spring Boot 2 之上， 使用非阻塞 API。
5、Spring Cloud Gateway 还 支持 WebSocket， 并且与Spring紧密集成拥有更好的开发体验。

# 服务配置：

Config执行流程:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Config/Config%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B.png)

##  Config：

### 服务端：

#### 依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

####  主启动：

```java
@SpringBootApplication
@EnableEurekaClient // 开启Eureka客户端
@EnableConfigServer // 开启SpringCloud-Config服务端
public class ConfigCenter3344 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigCenter3344.class, args);
    }
}

```

#### yml:

   注意：SpringCloud-Config使用Gitee的Https连接的话需要把仓库状态设置为公开。

```yaml
server:
  port: 3344
  
# gitee
spring:
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/blue-shirt-tears/springcloud-config.git # gitee仓库ssh路径（ssh路径报错）
          force-pull: true
          username: "青衫泪"
          password: 138138Ss
          search-paths: # 搜索目录
            - springcloud-config
      label: master # 读取分支
```

#### 结果：

```js
http://localhost:3344/master/config-test.yml
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Config/Config%E6%9C%8D%E5%8A%A1%E7%AB%AF%E8%BF%9E%E6%8E%A5Gitee.png)



### 配置读取规则：



##### 配置说明:

1. label：分支(branch)
2. name ：服务名
3. profiles：环境(dev/test/prod)

```shell
/{label}/{application}-{profile}.yml  #方法1
/{application}-{profile}.yml #方法2
/{application}/{profile}[/{label}] #方法3

#   方法1-请求路径：http://localhost:3344/master/config-dev.yml
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Config/%E6%9C%8D%E5%8A%A1%E7%AB%AF%E9%85%8D%E7%BD%AE%E8%AF%BB%E5%8F%96%E8%A7%84%E5%88%99%E7%BB%93%E6%9E%9C.png)



### 客户端：

#### 依赖：

```xml
<!--   spring-cloud-config： 客户端      -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

#### yml:

注意：这里文件名是boostrap.yml。

```yaml
server:
  port: 3355
  
spring:
  application:
    name: config-client
#   SpringCloud-Config
  cloud:
    config:
      label: master # 分支名
      name: config # 配置文件名称
      profile: dev # 开发环境名称   上述3个综合：master分支上config-dev.yml的配置文件被读取 http://localhost:3344/master/config-dev.yml
      uri: http://localhost:3344 # SpringCloud-Config服务端ip地址
```



#### Controller层：

```java
@RestController
public class ConfigClientController
{
    @Value("${config.info}") //读取gitee文件中的config.info信息
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo()
    {
        return configInfo;
    }
}
```

请求路径：http://localhost:3355/configInfo

结果：    

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Config/%E6%9C%8D%E5%8A%A1%E7%AB%AF%E9%85%8D%E7%BD%AE%E8%AF%BB%E5%8F%96%E8%A7%84%E5%88%99%E7%BB%93%E6%9E%9C.png)



### 客户端读取Gitee流程:



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Config/%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%AF%BB%E5%8F%96Gitee%E6%B5%81%E7%A8%8B.png)





### 解决客户端无法动态读取Gitee数据问题：

#### 静态刷新：

依赖：

```xml
<dependency> <!-- actuator监控信息 -->
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

yml:

```yaml
# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"  # 所有
```

Controller:

```java

import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.beans.factory.annotation.Value;

@RestController
@RefreshScope // 根据Gitee,动态刷新配置文件
public class ConfigClientController
{
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo() {
        return configInfo;
    }
}
```

Post请求：

```sh
# 注意：必须是发送以下路径的Post请求。
curl -X POST "http://localhost:3355/actuator/refresh"

```





# 服务中线(Bus):

### 动态刷新：

  动态刷新：使用SpringCloud-Bus+Config解决客户端无法动态读取Gitee数据问题

####  方法1：

##### 原理：

1）利用消息总线触发一个客户端/bus/refresh,而刷新所有客户端的配置。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Bus/SpringCloud%20Bus%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0%E5%85%A8%E5%B1%80%E5%B9%BF%E6%92%AD.png)

##### 缺点：

1. 打破了微服务的职责单一性，因为微服务本身是业务模块，它本不应该承担配置刷新的职责。
2. 破坏了微服务各节点的对等性。
3. 有一定的局限性。例如，微服务在迁移时，它的网络地址常常会发生变化，此时如果想要做到自动刷新，那就会增加更多的修改



#### 方法2：

##### 原理：

2）利用消息总线触发一个服务端ConfigServer的/bus/refresh端点，而刷新所有客户端的配置。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Bus/SpringCloud%20Bus%E5%8A%A8%E6%80%81%E5%88%B7%E6%96%B0%E5%85%A8%E5%B1%80%E5%B9%BF%E6%92%AD-%E6%96%B9%E6%B3%952.png)



##### 服务端：

依赖：

```xml
<!-- RabbitMQ,SpringCloud-Bus -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency> <!--HystrixDashboard-服务监控-actuator-->
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

yml:

```yaml
  # 解决SpringCloud-Config无法动态读取Gitee数据问题——动态刷新：
spring:
  rabbitmq:
    host: 192.168.139.140
    port: 5672   # 注意：15672是管理界面的端口；5672是MQ访问的端口。
    username: qsl
    password: 123456
# 解决SpringCloud-Config无法动态读取Gitee数据问题：
management:
  endpoints:
    web:
      exposure: # 暴露监控端点
        include: "bus-refresh"  # 暴露所有监控端点
```





##### 客户端：

依赖：

```xml
<!-- RabbitMQ,SpringCloud-Bus -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency> <!--HystrixDashboard-服务监控-actuator-->
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

yml:

bootstrap.yml

```yaml
  # 解决SpringCloud-Config无法动态读取Gitee数据问题——动态刷新：
spring:
  rabbitmq:
    host: 192.168.139.140 # ip地址
    port: 5672 # 注意：15672是管理界面的端口；5672是MQ访问的端口。
    username: qsl
    password: 123456
```

发送Post请求：

一次修改，广播通知，处处生效。

```js
// 命令:
http://localhost:服务端端口号/actuator/bus-refresh [服务名:端口号]

// 向服务端发送请求-全局通知
curl -X POST "http://localhost:3344/actuator/bus-refresh" 

// 指定通知
curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"

```





# 消息驱动(Stream):

 Spring Cloud Stream 是一个构建消息驱动微服务的框架。

​     SpringCloud-Stream:屏蔽底层消息中间件的差异,降低切换成本，统一消息的编程模型。

应用程序通过 inputs 或者 outputs 来与 Spring Cloud Stream中binder对象交互。
通过我们配置来binding(绑定) ，而 Spring Cloud Stream 的 binder对象负责与消息中间件交互。

Spring Integration来连接消息代理中间件以实现消息事件驱动。
Spring Cloud Stream 为一些供应商的消息中间件产品提供了个性化的自动化配置实现，引用了发布-订阅、消费组、分区的三个核心概念。

==目前仅支持RabbitMQ、Kafka。==

[SpringCloud官网](https://spring.io/projects/spring-cloud-stream#overview),[中文指导手册](https://m.wang1314.com/doc/webapp/topic/20971999.html)

Binder:绑定器，绑定器对象。

<font color="red"></font>



## 服务端：

### 依赖：

```xml
<!--   springCloud-stream ：RabbimMQ 。
         整合kafka的话，rabbit换掉kafka就行    -->
<dependency> 
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```



### yml:

```yaml
spring:
  application:
    name: cloud-stream-provider
  cloud:
    stream:
      binders: # 配置要绑定的RabbitMQ的服务信息；
        defaultRabbit:
          type: rabbit # 消息组件类型: RabbitMQ,kafka
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq: #  RabbitMQ
                host: 192.168.139.140
                port: 5672
                username: qsl
                password: 123456
      bindings: # 服务的整合处理
        output: # 输出消息通道
          destination: myExchange # 定义Exchange名称
          content-type: application/json # 设置消息类型为json，文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置
```



### ServiceI:

```java
public interface MessageProvider {

    public String send();
}
```



### ServiceImpl:

```java
import com.qsl.springcloud.service.MessageProvider;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.messaging.MessageChannel;
import org.springframework.messaging.support.MessageBuilder;

import java.util.UUID;

@EnableBinding(Source.class) // 开启消息发送管道
public class MessageProviderImpl implements MessageProvider {

    @Autowired
    private MessageChannel output; // 消息发送管道

    @Override
    public String send() {
        UUID serial = UUID.randomUUID();
        this.output.send(MessageBuilder.withPayload(serial).build());
        return serial.toString();
    }
}
```



## 客户端：

### 依赖：

```xml
<!--   springCloud-stream ：RabbimMQ     -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

### yaml:

```yaml
spring:
  application:
    name: cloud-stream-RabbitMQ-consumer
  #      SpringCloud
  cloud:
    stream:
      binders: # 配置要绑定的RabbitMQ的服务信息；
        defaultRabbit:
          type: rabbit # 消息组件类型: RabbitMQ,kafka
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq: #  RabbitMQ
                host: 192.168.139.140
                port: 5672
                username: qsl
                password: 123456
      bindings: # 服务的整合处理
        input: # 输入（接收）消息通道
          destination: myExchange # 根据消息名称接收消息
          content-type: application/json # 设置消息类型为json，文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置
          group: "消费组B" #  设置分组 （默认不同组）
```



### Controller层：

```java
@Component // 默认bean
@EnableBinding(Sink.class) // 开启接收消息通道
public class ReceiveMessageListener {

    @Value("${server.port}")
    private String serverPort;

    @StreamListener(Sink.INPUT) // 监听队列，用于消费者的队列消息接收
    public void input(Message<String> message) {
        System.out.println("消费1号,-----接收的消息：" + message.getPayload() + "\t port:" + serverPort);
    }

}
```

消息发送自动接收了。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud-Stream/%E6%B6%88%E6%81%AF%E6%8E%A5%E6%94%B6.png)



## 分组：

### 原理：

   ①微服务应用放置于同一个group中，就能够保证消息只会被其中一个应用消费一次。
    ②不同的组是可以消费的，同一个组内会发生竞争关系，只有其中一个可以消费。



### 持久化：

​    无分组属性配置：没有持久化。
​    有分组属性配置：有持久化。（注意：必须是不同的组才行，同组只有一个能接受消息）

未持久化：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud-Stream/%E6%8C%81%E4%B9%85%E5%8C%96.png)

已持久化：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/SpringCloud-Stream/%E6%8C%81%E4%B9%85%E5%8C%962.png)



# 分布式请求链路跟踪(Sleuth)：

​     原因： 在微服务框架中，一个由客户端发起的请求在后端系统中会经过多个不同的的服务节点调用来协同产生最后的请求结果，每一个前段请求都会形成一条复杂的分布式服务调用链路，链路中的任何一环出现高延时或错误都会引起整个请求最后的失败。

​     例： 订单-->支付，库存，积分，物流，仓储。。。

   优点：
①Spring Cloud Sleuth提供了一套完整的服务跟踪的解决方案。
②在分布式系统中提供追踪解决方案并且兼容支持了zipkin。

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/SpringCloud-Sleuth%E6%B5%81%E7%A8%8B%E5%9B%BE.bmp)

完整的调用链路:
​     一请求链路，一条链路通过Trace Id唯一标识，Span标识发起的请求信息，各span通过parent id 关联起来。
​      Trace:类似于树结构的Span集合，表示一条调用链路，存在唯一标识。
​      span:表示调用链路来源，通俗的理解span就是一次请求信息。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/%E5%AE%8C%E6%95%B4%E7%9A%84%E8%B0%83%E7%94%A8%E9%93%BE%E8%B7%AF%20.png)

​     一条链路通过Trace Id唯一标识，Span标识发起的请求信息，各span通过parent id 关联起来。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/%E5%AE%8C%E6%95%B4%E7%9A%84%E8%B0%83%E7%94%A8%E9%93%BE%E8%B7%AF1%20.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/%E5%AE%8C%E6%95%B4%E7%9A%84%E8%B0%83%E7%94%A8%E9%93%BE%E8%B7%AF2%20.png)

## 服务端：

### zipkin:

​     服务端搭建链路监控zipkin。

​    zip版本库：https://repo1.maven.org/maven2/io/zipkin/zipkin-server/

​    注意：SpringCloud从F版起已不需要自己构建Zipkin Server了，只需调用jar包即可。

```java
java -jar zipkin-server-2.24.0-exec.jar //运行jar包
```

启动成功：
​      注意：左右是两个不同的zipkin，老师那个找不到。

​    浏览器访问：http://127.0.0.1:9411/zipkin/

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/%E8%BF%90%E8%A1%8Czipkin%E7%9A%84jar%E5%8C%85.png)

## 客户端：

### 依赖：

```xml
<!--包含了sleuth+zipkin-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

### yml:

```yaml
spring:
  application:
    name: cloud-payment-service
# SpringCloud-Cleuth
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
     probability: 1  #采样率值介于 0 到 1 之间，1 则表示全部采集
```



### controller层：

```java

import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import com.qsl.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@Slf4j
@RestController
@RequestMapping("/consumer/payment/")
public class PaymentController {

    @GetMapping("/zipkin")
    public String paymentZipkin() {
        return "paymentSpringCloud-Cleuth-Server，O(∩_∩)O哈哈~";
    }
}
```

浏览器访问：http://localhost:9411  

（注意：我的版本要点击RUN QUERY刷新链路）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/zipkin%E5%AE%A2%E6%88%B7%E7%AB%AF.png)

查看:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/zipkin%E5%AE%A2%E6%88%B7%E7%AB%AF-%E6%9F%A5%E7%9C%8B.png)

查看依赖关系:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sleuth/zipkin%E5%AE%A2%E6%88%B7%E7%AB%AF-%E6%9F%A5%E7%9C%8B%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.png)



# alibaba:

使用Alibaba的原因:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Alibaba/%E4%BD%BF%E7%94%A8Alibaba%E7%9A%84%E5%8E%9F%E5%9B%A0.png)

维护模式：

​    官网解析：https://spring.io/blog/2018/12/12/spring-cloud-greenwich-rc1-available-now

   进入维护模式意味着Spring Cloud Netflix 将不再开发新功能/组件。以后将以维护和Merge分支Full Request为主。
   新组件功能将以其他替代平代替的方式实现。 

维护期间替代图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Alibaba/%E7%BB%B4%E6%8A%A4%E6%9C%9F%E9%97%B4%E6%9B%BF%E4%BB%A3%E5%9B%BE.png)

## 优点：

1. 服务限流降级：默认支持 Servlet、Feign、RestTemplate、Dubbo 和 RocketMQ 限流降级功能的接入，可以在运行时通过控制台实时修改限流降级规则，还支持查看限流降级 Metrics 监控。
2. 服务注册与发现：适配 Spring Cloud 服务注册与发现标准，默认集成了 Ribbon 的支持。
3. 分布式配置管理：支持分布式系统中的外部化配置，配置更改时自动刷新。
   消息驱动能力：基于 Spring Cloud Stream 为微服务应用构建消息驱动能力。
4. 阿里云对象存储：阿里云提供的海量、安全、低成本、高可靠的云存储服务。支持在任何时间、任何地点、任何应用存储和访问任意类型的数据。
5. 分布式任务调度：提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。同时提供分布式的任务执行模型，如网格任务。网格任务支持海量子任务均匀分配到所有 Worker（schedulerx-client）上执行。





依赖：

```xml
<!--spring cloud alibaba 2.1.0.RELEASE-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.1.0.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```



## Nacos:

### 概念：

​    一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，能快速实现动态服务发现、服务配置、服务元数据及流量管理。Nacos 能更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

​    Nacos就是注册中心 + 配置中心的组合

​    Nacos = Eureka+Config +Bus

   [官网文档]：(https://nacos.io/zh-cn/index.html)

   官网文档2：https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_nacos_discovery

​    

### 单机安装：

 下载解压即安装，打开nacos/bin:

```java
//  本次1.4.5 和 2.0.4 版本: (standalone代表着单机模式运行，非集群模式)
// win10启动
startup.cmd -m standalone // 单机模式启动nacos（默认：集群模式）
  
// linux启动
sh startup.sh -m standalone
```

访问：http://192.168.42.100:8848/nacos

​    默认账号名/密码为 nacos/nacos

==注意：2.2.1启动版本报错。==

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Alibaba/Nacos/Nacos2.2.1%E7%89%88%E6%9C%AC%E5%90%AF%E5%8A%A8%E6%8A%A5%E9%94%99.png)

  原因：2.2.1版本原默认配置的秘钥移除了，手动配上就好。

[秘钥参考官方文档：](https://nacos.io/zh-cn/docs/v2/guide/user/auth.html)

```
 官网提供的秘钥（可手动生成）： SecretKey012345678901234567890123456789012345678901234567890123456789
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Alibaba/Nacos/%E8%A7%A3%E5%86%B3Nacos2.2.1%E7%89%88%E6%9C%AC%E5%90%AF%E5%8A%A8%E6%8A%A5%E9%94%99.png)





### 集群安装 1.1.4 ：

必备环境：JDK1.8+,Maven3.2+
注意：3个或3个以上Nacos节点才能构成集群。

​       案例：1个Nginx+3个nacos注册中心+1个mysql。

1.上传并解压Nacos。

2 切换Derby数据库为Mysql.

1. 导入nacos的/conf/nacos-mysql.sql文件到mysql数据库。
2. application.properties 文件新增Mysql配置。

```properties 
###***************修改Derby数据库为Mysql***************###
# 注意: mysql8.0版本必须加上时区，不然nacos报 No DataSource set错.
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```

3.nacos/cluster.conf.example文件新增集群

```sh
# ========Nacos集群配置=======
192.168.139.141:8848  
192.168.139.141:8849
192.168.139.141:8850 # 注意：此IP不能写127.0.0.1，必须是Linux-hostname命令能识别的。
```

4.nacos/bin/startup.sh:

   nacos-1.1.11版本

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Alibaba/%E9%85%8D%E7%BD%AENacos%E9%9B%86%E7%BE%A4startup.sh%E6%96%87%E4%BB%B6.png)

注意：nacos版本1.14的startup.sh已经存在并且 p：关键字，使用port关键字替换就行，而且还有在while上面配置新加 d: 对应的名称。





默认账号名/密码为 nacos/nacos

   wdnmd，这节我卡了3天，给兄弟们个特别提示：一定要和老师的版本一致，其他版本的集群搭建方式已经不仅仅是修改配置文件了！！！2.0版本以后需要复制多个nacos文件



依赖：

```xml
<!--SpringCloud ailibaba nacos: 服务注册中心 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>

<!--nacos-config：服务配置中心 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

### 服务注册中心：







### 服务配置中心：





## 服务限流：





## 服务调用：

​    sentinel整合ribbon+openFeign+fallback。

### Ribbon系列：



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/1.png)

### OpenFeign系列：

依赖：

```xml
<!--SpringCloud openfeign -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

yml:

```yaml
# 激活Sentinel对Feign的支持
feign:
  sentinel:
    enabled: true  
```

Service层:

```java
import com.qsl.springcloud.Service.impl.PaymentSentienlOpenFeignServiceImpl;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * 使用 fallback 方式是无法获取异常信息的，
 * 如果想要获取异常信息，可以使用 fallbackFactory参数
 */
@FeignClient(value = "nacos-payment-provider", fallback = PaymentSentienlOpenFeignServiceImpl.class)//调用中关闭9003服务提供者
public interface PaymentSentienlOpenFeignService {

    // 掉用9003/9004模块的方法
    @GetMapping("/paymentSQL/{id}")
    public Result<Payment> paymentSQL(@PathVariable("id") Long id);

}
```

Service Impl层:

```java
import com.qsl.springcloud.Service.PaymentSentienlOpenFeignService;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.stereotype.Component;

@Component // 默认bean
public class PaymentSentienlOpenFeignServiceImpl implements PaymentSentienlOpenFeignService {

    @Override
    public Result<Payment> paymentSQL(Long id) {
        return new Result<>(444,"服务降级返回,没有该流水信息",new Payment(id, "errorSerial......"));
    }
}
```

Controller层：

```java
    @RestController //bean
    public class CircleBreakerController {
        @Autowired // 自动注入-默认按byName
        private PaymentSentienlOpenFeignService paymentSentienlOpenFeignService;

        @GetMapping("/consumer/paymentSQL/{id}")
        public Result<Payment> paymentSQL(@PathVariable("id") Long id) {
            if (id == 4) {
                throw new RuntimeException("没有该id");
            }
            return paymentSentienlOpenFeignService.paymentSQL(id);
        }
    }
}
```



## Sentinel 规则数据持久化：

   原理：把Sentinel 规则数据配置到Nacos中。

依赖：

```xml
<!--SpringCloud ailibaba sentinel-datasource-nacos -->
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

yml:

```yaml
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
cloud:
 nacos:
   discovery:
   sentinel:
      datasource: # sentinel规则吃持久化配置
        ds1:
          nacos:
            server-addr: localhost:8848  # Nacos服务注册中心
            dataId: ${spring.application.name}
            groupId: DEFAULT_GROUP  # 分组
            data-type: json # Json数据
            rule-type: flow
```

Controller层：

```java
import com.alibaba.csp.sentinel.annotation.SentinelResource;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.qsl.springcloud.myHandler.CustomerBlockHandler;
import com.qsl.springcloud.pojo.Payment;
import com.qsl.springcloud.utils.Result;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // bean
public class RateLimitController {

    /**
     * 自定义降流信息
     *
     * @return
     */
    @GetMapping("/rateLimit/byUrl")
    @SentinelResource(value = "byUrl", blockHandlerClass = CustomerBlockHandler.class, blockHandler = "handleException2")
    private Result byUrl() {
        return new Result(200, "自定义降流", new Payment(2020L, "serial002"));
    }

    //  注意：资源名称可以使用get请求方法和@SentinelResource注释名称。
    @GetMapping("/byResource")
    @SentinelResource(value = "byResource", blockHandler = "handleException")// 服务降级名称,服务降级方法
    private Result byResource() {
        return new Result(200, "按资源名称限流测试OK", new Payment(2020L, "serial001"));
    }

    // 服务降级方法
    public Result handleException(BlockException e) {
        return new Result(444, e.getClass().getCanonicalName() + "服务不可用");
    }
}
```

Nacos：

```json
[
    {
        "resource": "/rateLimit/byUrl",
        "limitApp": "default",
        "grade": 1, 
        "count": 1,
        "strategy": 0, 
        "controlBehavior": 0,
        "clusterMode": false 
    }
]
```



json数据说明：

```
resource：资源名称；
limitApp：来源应用；
grade：阈值类型，0表示线程数，1表示QPS；
count：单机阈值；
strategy：流控模式，0表示直接，1表示关联，2表示链路；
controlBehavior：流控效果，0表示快速失败，1表示Warm Up，2表示排队等待；
clusterMode：是否集群。
```





流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/Sentinel%20%E8%A7%84%E5%88%99%E6%95%B0%E6%8D%AE%E6%8C%81%E4%B9%85%E5%8C%96%EF%BC%9A.png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Sentinel/Sentinel%20%E8%A7%84%E5%88%99%E6%95%B0%E6%8D%AE%E6%8C%81%E4%B9%85%E5%8C%962%EF%BC%9A.png)

注意：①Json数据里不能用注释，有注释的Sentinel规则不能显示。
          ②重启Java项目必须调用Controller层请求，Sentinel 规则数据才显示。





# 分布式事务(Steata):

​     产生原因：  
​      单体应用被拆分成微服务应用，原来的三个模块被拆分成三个独立的应用，分别使用三个独立的数据源，业务操作需要调用三个服务来完成。此时每个服务内部的数据一致性由本地事务来保证，但是全局的数据一致性问题没法保证。
​          ①一次业务操作需要跨多个数据源或需要跨多个系统进行远程调用，就会产生分布式事务问题。
​          ②feign由于用重试机制(3次请求)，所以账户余额还有可能被多次扣减。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/%E5%88%86%E5%B8%83%E5%BC%8F.png)



​      Seata是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单易用的分布式事务服务。

   官网：http://seata.io/zh-cn/

​       Seata优缺点 ：配置繁琐，使用降低。

### 传统分布式事务流程：

#### 原理：

分布式事务处理过程的 一 ID+三 组件模型

1. Transaction ID XID：全局唯一的事务ID
2. 3组件：
   -  事务协调器 (Transaction Coordinator,TC): 维护全局事务的运行状态，负责协调并驱动全局事务的提交或回滚。
   -    事务管理器(Transaction Manager ,TM)：控制全局事务的边界，负责开启一个全局事务，并最终发起全局提交或全局回滚的决议。
   -    资源管理器 (Resource ManagerRM)：控制分支事务，负责分支注册、状态汇报，并接收事务协调器的指令，驱动分支（本地）事务的提交和回滚。

#### 流程：

1. TM 向 TC 申请开启一个全局事务，全局事务创建成功并生成一个全局唯一的 XID；
2. XID 在微服务调用链路的上下文中传播；
3. RM 向 TC 注册分支事务，将其纳入 XID 对应全局事务的管辖；
4. TM 向 TC 发起针对 XID 的全局提交或回滚决议；
5. TC 调度 XID 下管辖的全部分支事务完成提交或回滚请求。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/%E4%BC%A0%E7%BB%9F%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A1%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

### Stata分布式事务流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/Stata%E5%88%86%E5%B8%83%E5%BC%8F%E6%B5%81%E7%A8%8B.bmp)



分布式事务流程:

1.   TM 开启分布式事务（TM 向 TC 注册全局事务记录）；
2.   按业务场景，编排数据库、服务等事务内资源（RM 向 TC 汇报资源准备状态 ）；
3.   TM 结束分布式事务，事务一阶段结束（TM 通知 TC 提交/回滚分布式事务）；
4.  TC 汇总事务信息，决定分布式事务是提交还是回滚；
5.  TC 通知所有 RM 提交/回滚 资源，事务二阶段结束。



### AT 模式（默认）:

 前提:

- 基于支持本地 ACID 事务的关系型数据库。

- Java 应用，通过 JDBC 访问数据库。

  AT 模式提供无侵入自动补偿的事务模式，目前已支持MySQL、Oracle。
  
- ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/AT%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

#### 一阶段加载:

在一阶段，Seata 会拦截“业务 SQL”，
1  解析 SQL 语义，找到“业务 SQL”要更新的业务数据，在业务数据被更新前，将其保存成“before image(前缀通知)”，
2  执行“业务 SQL”更新业务数据，在业务数据更新之后，
3  其保存成“after image(后缀通知)”，最后生成行锁。
以上操作全部在一个数据库事务内完成，这样保证了一阶段操作的原子性。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/%E4%B8%80%E9%98%B6%E6%AE%B5.png)

#### 二阶段提交：

​    二阶段顺利提交的话，“业务 SQL”在一阶段已经提交至数据库，所以Seata框架只需将一阶段保存的快照数据和行锁删掉，完成数据清理即可。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/%E4%BA%8C%E9%98%B6%E6%AE%B5-%E6%8F%90%E4%BA%A4.png)



#### 二阶段回滚：

​    二阶段如果是回滚的话，Seata 就需要回滚一阶段已经执行的“业务 SQL”，还原业务数据。
​       回滚方式便是用“before image(前缀通知)”还原业务数据；但在还原前要先校验脏写，对比“数据库当前业务数据”和 “after image(后缀通知)”，如果两份数据完全一致就说明没有脏写，可以还原业务数据，如果不一致就说明有脏写，出现脏写就需要转人工处理。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/%E4%BA%8C%E9%98%B6%E6%AE%B5-%E5%9B%9E%E6%BB%9A.png)







### Seata-Server安装：

#### 0.9版本：

##### conf/file.conf：

   目的: 修改自定义事务组名称 + 事务日志存储模式为db + 数据库连接信息。

service模块:

```sh
service {
 
  vgroup_mapping.my_test_tx_group = "fsp_tx_group"   # 事务分组：异地积分停电容错机制
 
  default.grouplist = "127.0.0.1:8091"
  enableDegrade = false
  disable = false
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
}
```



store模块:

```sh
 
## transaction log store
store {
  ## store mode: file、db
  mode = "db"
 
  ## file store
  file {
    dir = "sessionStore"
 
    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    max-branch-session-size = 16384
    # globe session size , if exceeded throws exceptions
    max-global-session-size = 512
    # file buffer size , if exceeded allocate new buffer
    file-write-buffer-cache-size = 16384
    # when recover batch read size
    session.reload.read_size = 100
    # async, sync
    flush-disk-mode = async
  }
 
  ## database store
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.cj.jdbc.Driver"
    url = "jdbc:mysql://127.0.0.1:3306/study_seata?serverTimezone=UTC"
    user = "root"
    password = "12345"
    min-conn = 1
    max-conn = 3
    global.table = "global_table"
    branch.table = "branch_table"
    lock-table = "lock_table"
    query-limit = 100
  }
}

```

lib: 新增mysql8.0jar包



#### conf/db_store.sql:

   新建数据库seata，并执行db_store.sql语句。



#### conf/registry.conf:

​     目的：指明注册中心为nacos，及修改nacos连接信息

```sh
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos"
 
  nacos {
    serverAddr = "localhost:8848"
    namespace = ""
    cluster = "default"
  }
```

#### 先启动Nacos,再启动seata-server

成功图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/Seata%E5%AE%89%E8%A3%852.png)







解压既安装：

- Seata1.5.X版本SeataServer的配置文件只需要配置application.yml 配置文件即可。
- 从1.5.2版本开始支持Mysql8。

conf/file.conf:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringCloud/Seata/Seata%E5%AE%89%E8%A3%85.png)



新增数据库并创建表：

​    SQl表在Seata官网部署指南、资源目录介绍哪里。

```sql

```

conf/registry.conf：

目的是：指明注册中心为nacos，及修改nacos连接信息

先启动Nacos，在启动Seata：bin/seata-server.bat



Seata的三种存储模式（store.mode）:

1. file:(默认)单机模式，全局事务会话信息内存中读写并持久化本地root.data文件，性能较高。

2. db:高可用模式，全局事务会话信息通过db共享，相应性能差些。

3. redis:Seata-Server1.3及以上版本支持，性能较高，存在事务信息丢失风险，请提前配置适合当前场景的redis持久化配置。

   

   目录说明：

   
   
### 订单/库存/账户业务数据库：















```sh
@Transactional # 提交本地事务

@GlobalTransactional  # 提交全局事务

https://search.maven.org/remote_content?g=io.zipkin.java&a=zipkin-server&v=LATEST&c=exec # zipkin

https://repo1.maven.org/maven2/io/zipkin/zipkin-server/

https://blog.csdn.net/weixin_44790046/article/details/106857369 # nginx

https://www.yuque.com/u12017535/vg8rkx/lg2h04 # 语雀笔记
```



   















<font color="red"></font>





| 注解                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
|                        |                                                              |
| @LoadBalanced          | RestTemplate开启负载均衡                                     |
| @EnableEurekaClient    | 开启Eureka服务发现,服务注册中心（专属）                      |
| @EnableDiscoveryClient | Zookeeper,Consul,Nacos服务发现,服务注册中心;注意没有Eureka。 |
|                        |                                                              |



```
// pay 52-100：爬虫   reptile
```








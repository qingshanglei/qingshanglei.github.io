# SpringBoot概念：

`SpringBoot` 是由Pivotal团队提供的全新**框架**，其设计目的是用来**`简化`**Spring应用的**`初始搭建`**以及**`开发过程`**。

`SpringBoot`主要作用就是简化 **`Spring` **的**`搭建过程`**和**`开发过程`**。

原始 `Spring` 环境搭建和开发存在以下问题：

* 配置繁琐
* 依赖设置繁琐

`SpringBoot` 程序优点恰巧就是针对 `Spring` 的缺点

* 起步依赖。简化`Spring` 程序依赖设置繁琐的问题
* 自动配置。简化 `Spring` 程序配置繁琐的问题（简化常用工程相关配置）
* 辅助功能（内置服务器,...）。我们在启动 `SpringBoot` 程序时既没有使用本地的 `tomcat` 也没有使用 `tomcat` 插件，而是使用 `SpringBoot` 内置的服务器。

### starter起步依赖：

注意：有**starter**的皆为起步依赖。

   结论：以后需要使用技术，只需要引入该技术对应的起步依赖即可

SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，以达到**减少依赖配置**的目的。

**starter**

* `SpringBoot` 中常见项目名称，定义了当前项目使用的所有项目坐标，以达到**`减少依赖配置`**的目的

### parent

* 所有 `SpringBoot` 项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到**`减少依赖冲突`**的目的
* `spring-boot-starter-parent`（2.5.0）与 `spring-boot-starter-parent`（2.4.6）共计57处坐标版本不
  1. 开发SpringBoot程序要继承spring_boot-starter-parent
  2. spring-boot-starter-parent中定义了若干个依赖管理
  3. 继承parent模块可以**避免**多个依赖使用相同技术是出现**依赖**版本**冲突**
  4. 使用**引入依赖**同样可实现继承parent的效果

**实际开发**

* 使用任意坐标时，仅书写GAV中的G和A，V由SpringBoot提供

  > G：groupid
  >
  > A：artifactId
  >
  > V：version

* 如发生坐标错误，再指定version（要小心版本冲突）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E8%25B5%25B7%25E6%25AD%25A5%25E4%25BE%259D%25E8%25B5%2596.png)



### 引导类：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E5%25BC%2595%25E5%25AF%25BC%25E7%25B1%25BB.png)

 SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目。

 SpringBoot工程运行化后初始化Spring容器，扫描引导类所在包加载bean

**静态页面-拦截器**：SpringBoot项目自带开启静态页面拦截器，不需要设置。

   Spring Boot2.4前默认支持JSP（JavaServer Pages）作为模板引擎。从Spring Boot 2.4起，Thymeleaf成为了Spring Boot的默认模板引擎。Thymeleaf是一种现代化的服务器端Java模板引擎，用于构建可展示HTML、XML、JavaScript、CSS和文本的应用程序。

# 创建SpringBoot项目

- 注意：SDK必须是1.8版本（JDK8）,其他的都不行。
- 必须联网才能创建SpringBoot项目
- SpringBoot项目Application目录必须在最外层。

### IDEA创建SpringBoot项目：

 注意：com.qsl.springboot_02后的.springboot_02去掉。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/IDEA%25E5%2588%259B%25E5%25BB%25BASpringBoot%25E9%25A1%25B9%25E7%259B%25AE.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/IDEA%25E5%2588%259B%25E5%25BB%25BASpringBoot%25E9%25A1%25B9%25E7%259B%25AE2.png)

### SpringBoot官网创建SpringBoot项目：

SpringBoot官网有问题，三个网站换着来。

https://start.springboot.io/

https://start.aliyun.com/  	==》阿里云的

https://start.spring.io/     ==》不推荐，经常出问题

1.进SpringBoot官网的创建SpringBoot项目

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E5%25AE%2598%25E7%25BD%2591%25E5%2588%259B%25E5%25BB%25BASprinBoot%25E9%25A1%25B9%25E7%259B%25AE.png)

2.创建的项目为zip包用IDEA打开pom.xml即可

### maven工程修改为SpringBoot项目：

不联网创建SpringBoot工程（以前创建过springBoot才行）

1. 创建maven工程
2. 继承依赖spring-boot-starter-web
3. 添加依赖spring-boot-starter-web
4. 制作引导类

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/maven%25E5%2588%259B%25E5%25BB%25BAspringboot%25E9%25A1%25B9%25E7%259B%25AE.png)



# SpringBoot整合:

### JUnit：

```java
@SpringBootTest(classes = SpringBoot07TestApplication.class)  一般省略为 @SpringBootTest
//  @SpringBootTest ：设置JUnit加重的SpringBoot启动类
//    classes:设置SpringBoot启动类 
 @RunWith  //设置运行器
```

注意：如果测试类在SpringBoot启动类的包或子包中，可以省略启动类（classes）的设置。

1. 导入测试对应的starter（maven形式创建SpringBoot项目没有）
2. 测试类使用@SpringBootTest修饰
3. 使用自动装配的形式添加要测试的对象

创建纯测试项目：

一般不使用，纯测试项目...

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E7%25BA%25AF%25E6%25B5%258B%25E8%25AF%2595%25E9%25A1%25B9%25E7%259B%25AE.png)

使用测试

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E6%2595%25B4%25E5%2590%2588%25E6%25B5%258B%25E8%25AF%2595.png)

引导类：把他所在的包 和子包都扫描一遍。

测试类会默认加载引导类.

### MyBaties:

基于SpringBoot实现SSM整合：

- `SpringBoot`整合Spring (不存在)
- `SpringBoot`整合SpringMVC（不存在）
- `SpringBoot`整合MyBatis（主要）

整合ssm步骤：

1. pom.xml

   配置起步依赖，必要的资源坐标(druid)

2. application.yml

   设置数据源、端口等

3. 配置类

   全部删除

4. dao

   设置@Mapper

5. 测试类

6. 页面

   放置在resources目录下的static目录中

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E6%2595%25B4%25E5%2590%2588mybatis.png)



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E6%2595%25B4%25E5%2590%2588mybatis2.png)



可替代④中的连接数据库

注意：`SpringBoot` 版本低于2.4.2，Mysql驱动版本大于8.0时，需要在url连接串中配置时区 `jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC`，或在MySQL数据库端配置时区解决此问题

**说明:**==serverTimezone是用来设置时区，UTC是标准时区，和咱们的时间差8小时，所以可以将其修改为`Asia/Shanghai`==

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
#    serverTimezone=UTC ： 时区，`SpringBoot` 版本低于2.4.2不写报错
    url: jdbc:mysql://localhost:3306/dp1?serverTimezone=UTC
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource #配置数据源
    
mybatis: # 扫描dao层
  type-aliases-package: com/qsl/domain/Book  
  type-aliases-package: com.qsl.domain.Book
```

配置数据源要加`德鲁伊`依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>    // TODO
```

#### 扫描实体类

```yml
mybatis:
  type-aliases-package: com.qsl.pojo
```



### MyBatis-Plus:

1.导依赖（springboot官网没有，阿里有）

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

由于SpringBoot中未收录MyBatis-Plus的坐标版本，需要指定对应的Version

这里还使用了个lombok

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E6%2595%25B4%25E5%2590%2588MyBatis-plus.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E6%2595%25B4%25E5%2590%2588MyBatis-plus2.png)



### Druid:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E6%2595%25B4%25E5%2590%2588druid0.png)

#### 1.添加依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.12</version> 
</dependency>
```

#### 2.配置德鲁伊

方式1：导入Druid对应的starter

```yaml
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/scholl_2022912?serverTimezone=UTC
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource
```

方式2： 推荐,变更Druid的配置方式(必须要status的才行)

```yaml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/scholl_2022912?serverTimezone=UTC
      username: root
      password: 123456
```

### SSMP整合



# 运维：



## jar包运行SpringBoot项目：

### jar包项目运行报错:

![jar包项目运行报错](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/jar%E5%8C%85%E9%A1%B9%E7%9B%AE%E8%BF%90%E8%A1%8C%E6%8A%A5%E9%94%99.png)

**注意：**  jar包运行必须有以下依赖,不然运行报错。

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

#### 能运行SpringBoot项目的jar详细目录：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/jar%25E5%258C%2585%25E5%2590%25AF%25E5%258A%25A8SpringBoot%25E9%25A1%25B9%25E7%259B%25AE%25E7%259B%25AE%25E5%25BD%2595.png)

### Windonws系统启动：

​       用maven的package打包命令打包，使用命令行（cmd）进入jar的目录，执行启动命令。

```shell
mvn package    #打包SpringBoot项目命令
java -jar jar包名称.jar     # 运行项目命令
nohup java -jar jar包名称.jar & # 后台启动java，&符号表示让运行命令立即返回，只能Linux启动。（jar包启动成功，nohup: ignoring input and appending output to 'nohup.out'）

  例：java -jar springboot_01_quickstart-0.0.1-SNAPSHOT.jar

```



SpringBoot项目运行成功图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E7%259A%2584jar%25E5%2590%25AF%25E5%258A%25A8%25E7%25A8%258B%25E5%25BA%258F.png)



### Linux系统启动：

部署java项目：

①安装JDK,且版本不低于打包时使用的JDK版本，

②Jar包项目放在/user/local/自定义目录中或$HOME（~）目录下。

```shell
java -jar jar包名称.jar     #运行项目命令
```

SpringBoot项目运行成功图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E7%259A%2584jar%25E5%2590%25AF%25E5%258A%25A8%25E7%25A8%258B%25E5%25BA%258F.png)



### 辅助功能之 切换web服务器：

  **`Jetty`**比Tomcat更轻量级，可扩展性更强，谷歌应用引擎（GAE）已全面切换为Jetty。

使用maven依赖管理变更起步依赖项：

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!-- 排除Tomcat服务器-->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- 添加Jetty-Web服务器 起步依赖，版本有SpringBoot的starter控制 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>
```

启动结果对比图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/Jetty%25E5%25AF%25B9%25E6%25AF%2594Tomcat%25E6%259C%258D%25E5%258A%25A1%25E5%2599%25A8.png)



# 基础配置：

### 临时属性：

①使用jar命令启动SpringBoot项目时可以使用临时属性替换配置文件中的属性。
②多个临时属性之间使用空格分割。
③临时属性必须是当前boot项目支持的属性，否则设置无效。

```shell
# 临时属性-单个：
   java-jar schoolTest-0.0.1-SNAPSHOT.jar --server.post=8081 #配置jar包项目临时端口为8081（原端口为8080）

# 临时属性-多个：
  java-jar schoolTest-0.0.1-SNAPSHOT.jar --server.post=8081 --spring.datasource.diruid.password=123  #配置jar包项目临时端口为8081,数据库密码为123（原端口为8080，原密码为123456）
```

### 自定义配置文件：

   ①单服务器项目：使用自定义配置文件需求较低。
   ②多服务器项目：使用自定义配置文件需求较高，将所有配置放置在一个目录中，统一管理。
   ③基于SpringCloud技术(nacos)，所有的服务器将不再设置配置文件，而是通过配置中心进行设定，动态加载配置信息。

#### 指定文件名：

默认端口8080 ，通过自定义配置文件配置端口81。

```shell
--spring.config.name=文件名        
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-%E6%8C%87%E5%AE%9A%E6%96%87%E4%BB%B6%E5%90%8D.png)



#### 指定文件路径:

```shell
 单个：
   --spring.config.location=classpath:/文件名.properties       
 
 多个： （配置多个只执行后面那个）
 --spring.config.location=
   classpath:/ebank.properties,classpath:/ebank-server.properties 
  注意：多配置文件常用于将配置进行分类，进行独立管理，或将可选配置单独制作便于上线更新维护
```

自定义配置文件-指定文件路径_多个图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-%E6%8C%87%E5%AE%9A%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84.png)





## 配置文件格式：

执行顺序：  properties  >  yml  > yaml 

application.properties原文件名称 

###  properties文件修改:

```properties
server.port=80  //修改端口号：
```

###  yml 文件:

```yaml
server:
   port: 81
```

###  yaml文件:

```yaml
server:
  port: 82
```



## yaml、yml:

YAML（YAML Ain't Markup Language），一种数据序列化格式。

**优点：**

* 容易阅读

  `yaml` 类型的配置文件比 `xml` 类型的配置文件更容易阅读，结构更加清晰

* 容易与脚本语言交互

* 以数据为核心，重数据轻格式

  `yaml` 更注重数据，而 `xml` 更注重格式

**YAML 文件扩展名：**

* `.yml` (主流)
* `.yaml`

XML 和 Properties 和 yaml格式区别：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/xml%25E5%2592%258Cproperties%25E5%2592%258Cyaml%25E6%25A0%25BC%25E5%25BC%258F%25E5%258C%25BA%25E5%2588%25AB.png)

### yaml语法规则:

* 大小写敏感

* 属性层级关系使用多行描述，每行结尾使用冒号结束

* 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）

  空格的个数并不重要，只要保证同层级的左侧对齐即可。

* 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）

* \# 表示注释

==核心规则：数据前面要加空格与冒号隔开==

### yaml 数据格式：

```yaml
enterprise:          # 正常格式
  name: itcast              
  age: 16
  tel: 4006184000   
  
  subject:           # 数组格式
    - Java
    - 前端
    - 大数据
  likes: [java,music,sleep]    # 数组格式简写
  
  users:   # 对象数组格式
    - name: tom
      age: 18
    - name: jerry
      age: 20
      
  users3: [{name: tom,age: 29},{name: jerry,age: 18}]  #对象数组简写
```



```yaml
boolean: TRUE  		#TRUE,true,True,FALSE,false，False均可
float: 3.14    			#6.8523015e+5  #支持科学计数法
int: 123       			#0b1010_0111_0100_1010_1110    #支持二进制、八进制、十六进制     
null: ~        			#使用~表示null
string: HelloWorld      		#字符串可以直接书写
string2: "Hello World"  		#可以使用双引号包裹特殊字符
date: 2018-02-17        		#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区

    八进制：  0(0-7)   例：“0127”转为“87”  
    十六进制：0x(0-9,a-f)   注意：密码等不要以0开头和结尾。o(╥﹏╥)o
```



### 读取yaml配置数据：

yaml读取数据三种格式：

####  @Value (直接读取)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E8%25AF%25BB%25E5%258F%2596%25E6%2595%25B0%25E6%258D%25AE.png)

####  Environment (封装后读取):

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E8%25AF%25BB%25E5%258F%2596%25E6%2595%25B0%25E6%258D%25AE2.png)

#### 实体类封装属性 （封装后读取）

自定义对象封装数据

set跟get略....

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E8%25AF%25BB%25E5%258F%2596%25E6%2595%25B0%25E6%258D%25AE3.png)

#### 第三方bean读取yml属性:

yaml文件：

```yaml
servers:
   ipAddress: 192.168.100.1
   port: 8888
   timeout: -1
```

实体类：

```java
@Component   //定义bean
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout; //服务器超时时间
}
```



```java
@Bean   //读取Dreuid
@ConfigurationProperties(prefix = "servers")
public DruidDataSource dataSource(){ 
    DruidDataSource ds = new DruidDataSource();  
    return ds;
}
```



​     @EnableConfigurationProperties注解可以将使用@ConfigurationProperties注解对应的类加入Spring容器。



宽松(松散)绑定：

​    注意：①1.@ConfigurationProperties绑定属性支持属性名宽松绑定。
​               ②@Value不支持宽松绑定。
​               ③绑定前缀名命名规范：仅能使用纯小写字母、数字、中划线作为合法的字符。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/@ConfigurationProperties-%E5%AE%BD%E6%9D%BE(%E6%9D%BE%E6%95%A3)%E7%BB%91%E5%AE%9A.png)



读取时间和存储大小：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/JDK8%E6%8F%90%E4%BE%9B%E7%9A%84%E6%97%B6%E9%97%B4%E5%92%8C%E5%AD%98%E5%82%A8%E5%8D%95%E4%BD%8D.png)

实体类：

```java
@Component   //定义bean
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout; //服务器超时时间
    @DurationUnit(ChronoUnit.HOURS)   //定义时间单位-小时
    private Duration serverTimeOut;  //服务器超时时间    Duration默认单位：s
    @DataSizeUnit(DataUnit.MEGABYTES)    //定义存储单位-MB
    private DataSize dataSize;//数据容量大小     DataSize默认单位:B

    private DataSize dataSizeSS;//数据容量大小     DataSize默认单位:B
}
```

yml:

```yaml
servers:
    ipAddress: 192.168.100.1        # 驼峰
    port: 8888
    timeout: -1
    serverTimeOut: 3
    dataSize: 10    # 默认B
    dataSizeSS: 10MB  # 默认B,修改为MB(注意：单位必须大写)
```

sss:

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(Application.class, args);
        ServerConfig bean = ctx.getBean(ServerConfig.class);
        System.out.println(bean);

    }
```



数据校验：

   开启数据校验有助于系统安全性，J2EE规范中JSR303规范定义了一组有关数据校验相关的API。

Hibernate框架：

**步骤①**：添加JSR303规范坐标与Hibernate校验框架对应坐标

```xml
<!--1.导入JSR303规范(接口)-->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
<!--使用hibernate框架提供的校验器做实现（实现类）-->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
</dependency>
```

**步骤②**：使用注解@Validated开启校验功能:

```java
@Component   //定义bean
@Data
@ConfigurationProperties(prefix = "servers")
@Validated   // 2使用注解@Validated开启校验功能
public class ServerConfig {
    private String ipAddress;
    // 3.设置具体的规则
    @Max(value = 8888,message = "最大值不能超过8888")
    @Min(value = 200,message = "最小值不能小于200")
    @NotEmpty
    private int port;
}
```

**步骤③**：yml

```yaml
servers:
    ipAddress: 192.168.100.1        # 驼峰
    port: 8888   #最大值不能超过8888,最小值不能低于202
    timeout: -1
    serverTimeOut: 3
    dataSize: 10    # 默认B
```



JSR303规范与Hibernate框架提供的接口:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/JSR303%E8%A7%84%E8%8C%83%E4%B8%8EHibernate%E6%A1%86%E6%9E%B6%E6%8F%90%E4%BE%9B%E7%9A%84%E6%8E%A5%E5%8F%A3.png)



#### 实体类封装属性警告:

实体类封装属性警告（自定义对象封装数据警告）...会在实体类上方弹出警告

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E8%25A7%25A3%25E5%2586%25B3%25E8%25AF%25BB%25E5%258F%2596%25E6%2595%25B0%25E6%258D%25AE3%25E8%25AD%25A6%25E5%2591%258A.png)

 在pom.xml中添加依赖...

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

### 配置yaml或 yml 文件提示：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E5%2585%25B3%25E4%25BA%258Eyaml%25E5%2592%258Cyml%25E6%258F%2590%25E7%25A4%25BA.png)

### 解决yml或yaml中文注释报错：

​    根本原因：因为我们在的yml的文件格式时GBK的 我们的中文注释在target文件中是乱码的。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%A7%A3%E5%86%B3yml%E6%88%96yaml%E4%B8%AD%E6%96%87%E6%B3%A8%E9%87%8A%E6%8A%A5%E9%94%99.png)



## 多环境配置：

​    多环境配置使用yml和properties任一种格式都行。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E5%25A4%259A%25E7%258E%25AF%25E5%25A2%2583%25E9%2585%258D%25E7%25BD%25AE.png)

#### 单个文件配置 *：

在 `application.yml` 中使用 `---` 来分割不同的配置

```yaml
#启动环境配置
spring:
  profiles:
    active: test # 使用指定环境
---  # 在 `application.yml` 中使用 `---` 来分割不同的配置

#开发环境
spring:
  profiles: dev # 定义开发环境起
server:
  port: 80

---
#生产环境
spring:
  profiles: pro # 定义生产环境起
server:
  port: 81

---
# 测试环境
spring:
  profiles: test # 定义测试环境
server:
  port: 82

```

推荐旧版，新版我在IDEA没提示

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/yaml%25E9%2585%258D%25E7%25BD%25AE%25E7%258E%25AF%25E5%25A2%2583-%25E6%2597%25A7%25E7%2589%2588VS%25E6%2596%25B0%25E7%2589%2588.png)

#### 多个文件配置：



properties文件的是多文件格式，根据**`文件名`**修改端口

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E5%25A4%259A%25E7%258E%25AF%25E5%25A2%2583%25E9%2585%258D%25E4%25BB%25B6-properties%25E6%2596%2587%25E4%25BB%25B6.png)



#### include属性:

注意：当主环境dev与其他环境有相同属性时，主环境属性生效；其他环境中有相同属性时，最后加载的环境属性生效。



#### group属性：

​     注意：Spring2.4后使用group属性替代include属性，降低了配置书写量。



#### jar包配置：

命令行运行SpringBoot项目的 **`jar`**配置环境...

注意：jar包使用前必须是UTF-8编码。

```shell
# 配置测试环境
  java -jar jar包名 --spring.profiles.active=环境名
  
# 修改端口
  java -jar jar包名 --server.port=要修改的端口
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%25E5%2591%25BD%25E4%25BB%25A4%25E8%25A1%258C%25E8%25BF%2590%25E8%25A1%258Cjar%25E5%258C%2585-%25E9%2585%258D%25E7%25BD%25AE%25E7%258E%25AF%25E5%25A2%2583.png)

SpringBoot参数优先级：https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E5%258F%2582%25E6%2595%25B0%25E4%25BC%2598%25E5%2585%2588%25E7%25BA%25A7.png)

#### Maven与SpringBoot环境开发兼用问题

Maven环境为主，SpringBoot环境为辅。 

##### 方式1-@：

1.Maven中设置多环境属性：

```xml
  <!-- SpringBoot与Maven多环境开发-->
    <profiles>
        <!-- 定义具体的环境-开发环境-->
        <profile>
            <id>maven_dev</id>
            <properties>
                <project.active>dev</project.active>
            </properties>
            <!-- 设置为默认启动环境-->
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>

        <!--生成环境-->
        <profile>
            <id>maven_pro</id>
            <properties>
                <project.active>pro</project.active>
            </properties>

        </profile>

        <!-- 测试环境-->
        <profile>
            <id>maven_test</id>
            <properties>
                <project.active>test</project.active>
            </properties>
        </profile>
    </profiles>
```

2.SpringBoot中引用Maven属性:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%E4%B8%8EMaven%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%85%BC%E7%94%A8-%E6%96%B9%E6%B3%951.png)





##### 方式2-插件：

方式2：需要插件。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/Maven%25E5%2592%258CSpringBoot%25E5%25A4%259A%25E7%258E%25AF%25E5%25A2%2583%25E5%2585%25BC%25E7%2594%25A8%25E9%2597%25AE%25E9%25A2%2598.png)



```xml
<build>
    <!-- 添加解析${}插件-->
    <resources>
        <resource>
            <!-- ${project.basedir} : 当前项目所在目录-->
 <directory>${project.basedir}/src/main/resources</directory>
            <!-- 开启解析${}-->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

java

```yaml
spring:
  profiles:
    active: ${project.active} #方式2
```





### 配置文件分类：

`SpringBoot` 中4级配置文件放置位置：           

* 1级：classpath：application.yml                   等级最低
* 2级：classpath：config/application.yml
* 3级：file ：application.yml
* 4级：file ：config/application.yml                  等级最高

> ==说明：==级别越高优先级越高

作用：

- 1级与2级用于系统**`开发阶段`**设置通用属性（IDEA里使用）
- 3级与4级留做系统**`打包后(上线后)`**设置通用属性 (jar包使用)

#### 1级 和2级配置：

1级被2级给覆盖了...

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E4%25B8%25AD1%25E7%25BA%25A7%25E5%2592%258C2%25E7%25BA%25A7%25E9%2585%258D%25E7%25BD%25AE%25E6%2596%2587%25E4%25BB%25B6%25E6%2594%25BE%25E7%25BD%25AE%25E4%25BD%258D%25E7%25BD%25AE%25EF%25BC%259A.png)

#### 3级 和4级配置：

3级被4级给覆盖了...

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBoot%25E4%25B8%25AD3%25E7%25BA%25A7%25E5%2592%258C4%25E7%25BA%25A7%25E9%2585%258D%25E7%25BD%25AE%25E6%2596%2587%25E4%25BB%25B6%25E6%2594%25BE%25E7%25BD%25AE%25E4%25BD%258D%25E7%25BD%25AE%25EF%25BC%259A.png)



公司一般用2.3.9或2.4.1版本

##### 2.5.0版本执行4级专有Bug

在config目录下新增任意目录就行了...

```xml
    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.5.0</spring-boot.version>
    </properties>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/2.5.0%25E7%2589%2588%25E6%259C%25ACBug.png)



## 日志：

### Controller等层日志：

Service，Controller等层日志：

依赖：

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.20</version>
</dependency>
```

#### Controller:

```java
@Slf4j
@RestController
@RequestMapping("/consumer/payment/")
public class PaymentController {

    @GetMapping("/discovery")
    public String discovery() {
        log.info("@@@ ==》控制台输出日志" );
        return  "控制台输出日志";
    }
}
```





### 包方式：

yaml文件：

```yaml
# 包方式 
  logging:
  level:
    root: info
    com.qsl.controller: debug
```



### 组方式：

注意：如报错，注释记得删除掉。yaml文件注释报错。

yaml文件：

```yaml
logging:
  level:	
    root: info
    ebank: debug  # 为对应组设置日志级别

  group: # 设置日志组
    ebank: com.qsl.controller # 自定义组名，设置当前组中所包含的包
```

### 日志输出详情：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E6%97%A5%E5%BF%97%E8%BE%93%E5%87%BA%E6%A0%BC%E5%BC%8F%E8%AF%A6%E6%83%85.png)

### 自定义日志：

#### yaml文件：

```yaml
logging: # 自定义日志
  pattern:
    console: "%d %clr(%5p) ---[%16t] %clr(%-40.40c){blue} : %m %n"
```

#### 自定义日志参数：

```yaml

    %d: 日期
     
    %p: 日志级别      #  %5p: 字体占5位，左对齐。（线程同理） 
    %clr: 颜色设置为彩色
    %clr{颜色名}: 指定颜色设置
     
    %t: 线程
     
    %c: 类名   #  %-40.40c: -40字体占40为，右对齐；字体截取40位。
     
    %m: 消息
    %n: 换行
```

### 以文件记录日志：

以文件记录日志：记录控制台的日志到文件。

#### 单个文件：

yaml文件：

```yaml
logging:
 file:
  name: server.log  #  日志记录 记录到文件(如服务器不停，缓存量到的时候才写进文件)
```



#### 多个文件：

yaml文件：

```yaml
logging:
  file: #  "日志信息"记录到文件
    name:  server.log
  logback:
    rollingpolicy:
      max-file-size: 3KB #  记录"日志信息"文件的内存大小大于3KB，重新创建一个文件
      file-name-pattern: server.%d{yyyy-MM-dd}.%i.log # 设置新创建文件的名称
```



# 测试：

## 测试业务层：



SpringBoot随机数：







## 测试表现层(Web环境)：



  测试表现层需要一个基础和一个功能。
    ①基础： 测试类中启动web测试。
    ②功能：在测试类中发送web请求



- MOCK：根据当前设置确认是否启动web环境，例如使用了Servlet的API就启动web环境，属于适配性的配置
- DEFINED_PORT：使用自定义的端口作为web服务器端口
- RANDOM_PORT：使用随机端口作为web服务器端口
- NONE：不启动web环境





# SpringBoot内置：

## 内置3数据源：

SpringBoot内置的数据源：HikariCP，DataSource，DBCP

### HikariCP：

 HikariCP：轻量级数据源。

配置

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/scholl_2022912?serverTimezone=UTC
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: 123456
      maximum-pool-size: 50 # 最大活动时间连接池的最大数量
```



### Tomcat提供DataSource：

​    HikariCP不可用的情况下，且在web环境中，将使用tomcat服务器配置的数据源对象。

### Commons DBCP：

​    Hikari不可用，tomcat数据源也不可用，将使用dbcp数据源。

## 内置1持久化技术：





## 内置3数据库：

内置3数据库优点：内存运行，运行速度快。

### H2:

```xml
H2浏览器访问路径  内存级数据库   启动数据库实例名称
```

## 内置服务器：

1.    tomcat(默认)：应用面广，负载了若干较重的组件。
2.    jetty:更轻量级，负载性能远不及tomcat
3.    undertow:undertow,负载性能勉强跑赢tomcat

# 缓存：

1. 缓存是一种介于数据永久存储介质与数据应用之间的数据临时存储介质。
2. 使用缓存可以有效的减少低速数据读取过程的次数（例如磁盘IO），提高系统性能。
3. 缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间。

## Simple（默认）：

1.导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

2.Application:

```java
@SpringBootApplication
@EnableCaching //开启缓存功能
public class Springboot19CacheApplication {
    public static void main(String[] args) {        SpringApplication.run(Springboot19CacheApplication.class, args);    }}

```

3.Service层：

```java
@Override
@Cacheable(value = "cacheSpace",key = "#Id") // 设置当前操作的结果进入缓存,查询相同取缓存值。
public Book getById(Integer Id) {
    return bookDao.getById(Id);
}
```

@Cacheable：





@CachePut：

```java
@CachePut(value = "smsCode", key = "#tele") // 设置当前操作的结果进入缓存，没有取功能（配合@Cacheable获取验证码）
public String sendCodeToSMS(String tele) {
    String code = codeUtils.generator(tele);
    System.out.println(code);
    return code;
}
```



## Encache缓存：

```xml
<!-- Ehcache缓存 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

# SpringBoot Admin(监控)：

​    Spring Boot Admin，开源社区项目，用于管理和监控SpringBoot应用程序。 客户端注册到服务端后，通过HTTP请求方式，服务端定期从客户端获取对应的信息，并通过UI界面展示对应信息。

监控的意义：

1. 监控服务状态是否宕机。
2. 监控服务运行指标（内存、虚拟机、线程、请求等）。
3. 监控日志。
4. 管理服务（服务下线）。

监控实式的方式：

1. 显示监控信息的服务器：用于获取服务信息，并显示对应的信息。

2. 运行的服务：启动时主动上报，告知监控服务器自己需要受到监控。

   

注意：①SpringBoot的版本要和SpringBootAdmin的版本一致。（但可能SpringBootAdmin的版本要低于SpringBoot的一个版本。）

​           ②一个SpringBootAdmin服务端对应一个或多个SpringBootClient客户端。

## server(服务端)：

1.导入依赖:

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

2.引导类上开启监控服务器：

```java
@SpringBootApplication
@EnableAdminServer //2.开启监控服务器
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

3.用浏览器访问当前端口(端口：8080)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/SpringBootAdmin%E7%9B%91%E6%8E%A7%E6%9C%8D%E5%8A%A1%E5%99%A8.png)





## client(客户端)：

1.导入依赖:

```xml
<!--     SpringBootAdmin客户端   -->
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

2.application.yml

   监控SpringBootAdmin服务端端口8080，并配置端点。

```yml
#
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080  # 监控SpringBootAdmin服务端端口8080
management:
  endpoint:
    health:    # 端点名称
      show-details: always
  endpoints: # 监控的数据
    web:
      exposure:
        include: "*"  # health（默认）：只监控SpringBootAdmin的健康值；      "*" : 监控所有信息
```



## 端点：

​      Actuator，可以称为端点，描述了一组监控信息。

​    查看当前SpringBootAdmin服务器或SpringBootClient客户端开启的端点

1.  访问当前应用所有端点信息：localhost:端口**/actuator**。
2.  访问端点详细信息：localhost:端口/actuator**/端点名称**。

```yml
端点获取方式： ①gmx获取，②http获取（web）
jconsole # 命令提示符打开Jconsole监控平台（gmx获取）  
```

### <font color="red" >**所有端点信息说明** </font>

| ID                                    | 描述                                                         | 默认启用 |
| ------------------------------------- | ------------------------------------------------------------ | -------- |
| auditevents                           | 暴露当前应用程序的审计事件信息。                             | 是       |
| beans                                 | 显示应用程序中所有 Spring bean 的完整列表。                  | 是       |
| caches                                | 暴露可用的缓存。                                             | 是       |
| conditions                            | 显示在配置和自动配置类上评估的条件以及它们匹配或不匹配的原因。 | 是       |
| configprops                           | 显示所有 @ConfigurationProperties 的校对清单。               | 是       |
| env                                   | 暴露 Spring ConfigurableEnvironment 中的属性。               | 是       |
| flyway                                | 显示已应用的 Flyway 数据库迁移。                             | 是       |
| <font color="red" >**health** </font> | 显示应用程序健康信息                                         | 是       |
| httptrace                             | 显示 HTTP 追踪信息（默认情况下，最后 100 个  HTTP 请求/响应交换）。 | 是       |
| <font color="red" >**info** </font>   | 显示应用程序信息。                                           | 是       |
| integrationgraph                      | 显示 Spring Integration 图。                                 | 是       |
| <font color="red" >**loggers**</font> | 显示和修改应用程序中日志记录器的配置。                       | 是       |
| liquibase                             | 显示已应用的 Liquibase 数据库迁移。                          | 是       |
| **<font color="red" >metrics</font>** | 显示当前应用程序的指标度量信息。                             | 是       |
| mappings                              | 显示所有 @RequestMapping 路径的整理清单。                    | 是       |
| scheduledtasks                        | 显示应用程序中的调度任务。                                   | 是       |
| sessions                              | 允许从 Spring Session 支持的会话存储中检索和删除用户会话。当使用 Spring Session 的响应式 Web 应用程序支持时不可用。 | 是       |
| shutdown                              | 正常关闭应用程序。                                           | 否       |
| threaddump                            | 执行线程 dump。                                              | 是       |



Web程序专用端点:

| ID         | 描述                                                         | 默认启用 |
| :--------- | :----------------------------------------------------------- | :------: |
| heapdump   | 返回一个 hprof 堆 dump 文件。                                |    是    |
| jolokia    | 通过 HTTP 暴露 JMX bean（当  Jolokia 在 classpath 上时，不适用于 WebFlux）。 |    是    |
| logfile    | 返回日志文件的内容（如果已设置 logging.file 或 logging.path 属性）。支持使用 HTTP Range 头来检索部分日志文件的内容。 |    是    |
| prometheus | 以可以由 Prometheus 服务器抓取的格式暴露指标。               |    是    |



### 启动端点：

#### 启用指定端点：

```yml
management:
  endpoint:
    health:  # 端点名称
      show-details: always
    info:  # 端点名称
      enabled: true  #  是否开放
  endpoints: # 监控的数据
      web:
        exposure:
          include: health,info # 只监控 health,info
```



#### 启用13个常用端点:

springboot admin设置了13个较为常用的端点作为默认开放的端点：

```yml
management:
  endpoints:
    enabled-by-default: true # 是否开启默认端点，默认true
    web:
      exposure:
        include: "*"  # 监控所有数据
```

#### 启用所有端点:

```yml
#
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080  # 监控SpringBootAdmin服务端端口8080
management:
  endpoint:
    health:    # 端点名称
      show-details: always
  endpoints: # 监控的数据
    web:
      exposure:
        include: "*"  # health（默认）：只监控SpringBootAdmin的健康值；      "*" : 监控所有信息
```



总结：

1. 被监控客户端通过添加actuator的坐标可以对外提供被访问的端点功能

2. 端点功能的开放与关闭可以通过配置进行控制

3. web端默认无法获取所有端点信息，通过配置开放端点功能



### 自定义端点信息:

#### info(信息)端点信息:

##### yml(静态)：

​     可以读取String,Maven数据；但不能执行计算。。。

```yml
management:
  endpoints:
    enabled-by-default: true # 是否开启默认端点，默认true
    web:
      exposure:
        include: "*"  # 监控所有数据
info:        # 自定义端点-info端点信息_静态数据
  author: "青衫泪"   # 作者
  appName: @project.artifactId@    # 软件名称(从maven中读取)
  version: @project.version@   # 版本
```

SpringBootAdmin服务端：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/info(%E4%BF%A1%E6%81%AF)%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF_%20yml(%E9%9D%99%E6%80%81).png)



##### InfoContributor接口(动态)：

actuator/InfoConfig类：

```java
/** 自定义端点-info端点信息_动态数据 */
@Component // bean
public class InfoConfig implements InfoContributor {

    @Override
    public void contribute(Info.Builder builder) {

        //  withDetail：添加单条信息
        builder.withDetail("runTime", System.currentTimeMillis()); // 运行时间
        builder.withDetail("arithmetic",3+5); // 算法

        // withDetails： 添加多条信息_数组
        HashMap infoMap = new HashMap();
        infoMap.put("buildTime","2023-2-15"); // 创建时间
        infoMap.put("author","青衫泪"); // 作者
        builder.withDetails(infoMap);
    }
}
```

SpringBootAdmin服务端：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF-info%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF_%E5%8A%A8%E6%80%81%E6%95%B0%E6%8D%AE.png)



#### Health(健康)端点信息:

actuator/AppHealthContributor类：

```java
@Component // bean
public class AppHealthContributor extends AbstractHealthIndicator {

    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {

        boolean condition = true; // 默认软件是否报错
        if (condition) {
            /**
             *   设置运行状态：
             *     up:   启动状态
             *     DOWN:  失败状态
             *     UNKNOWN:  未知状态(失败状态)
             *     OUT_OF_SERVICE： 不在服务状态(失败状态)
             */
  //        builder.up();  // 设置运行状态_启动状态——方式1
            builder.status(Status.UP); // 设置运行状态_启动状态——方式2(推荐)

            //  withDetail：添加单条信息
            builder.withDetail("runTime", System.currentTimeMillis()); // 运行时间

            // withDetails： 添加多条信息_数组
            HashMap infoMap = new HashMap();
            infoMap.put("buildTime", "2023-2-15"); // 创建时间
            builder.withDetails(infoMap);

        } else {
            builder.withDetail("软件上线了吗","你做梦");
            builder.status(Status.DOWN); // 设置运行状态_启动状态——方式2
        }
    }
}
```

SpringBootAdmin服务端：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF-Health%EF%BC%88%E5%81%A5%E5%BA%B7%EF%BC%89%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF-%E7%BB%A7%E6%89%BF.png)



#### Metrics(性能)端点:



注意：①  AppHealthContributor类名 ===》    端点实例名HealthContributor。 
​           ② 可继承多次类，和实现多次接口，以实现自定义端口功能。

##### SpringBootAdmin客户端：

​    Service层模拟付费操作

```java
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {

    @Autowired // 自动注入-按类型注入
    private BookDao book;

 
    private Counter counter;
    
    /** 自定义端点——Metrics(性能)端点 */
    public BookServiceImpl(MeterRegistry meterRegistry){
        counter = meterRegistry.counter("用户付费操作次数");
    }

    @Override
    public int deleById(Integer Id) {
        // 模拟付费操作： 每次执行删除业务等同于执行了付费业务。
        counter.increment();

        return book.deleteById(Id);
    }
}
```

##### SpringBootAdmin服务端操作：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AB%AF%E7%82%B9%E4%BF%A1%E6%81%AF%E2%80%94Metrics(%E6%80%A7%E8%83%BD)%E7%AB%AF%E7%82%B9.png)



### 自定义端点：

actuator/PayEndPoint类：

```java
/** 自定义端点 */
@Component // bean
@Endpoint(id = "pay",enableByDefault = true) // Id: 定义端点名称      enableByDefault: 是否默认开启(也可在yml里配置)
public class PayEndPoint {

    @ReadOperation  // 读取pay端点,就调用此方法。
    public Object getPay() {

        // 调用业务操作，获取支付相关信息结果，最终return
        HashMap payMap = new HashMap();
        payMap.put("青铜",111);  // 等级
        payMap.put("黄金",222);
        payMap.put("铂金",333);

        return  payMap;
    }
}
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AB%AF%E7%82%B9-%E6%9C%8D%E5%8A%A1%E7%AB%AF.png)















# 时间格式化：

yml文件：

```yaml
spring:
  jackson:  # 返回Json的全局时间格式
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```

使用前后对比图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringBoot/%E6%97%B6%E9%97%B4%E6%A0%BC%E5%BC%8F%E5%8C%96.png)



# 密码加密：

## 1.MD5加密：

创建MD5加密工具类：

```java
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;


/** MD5密码加密 */
public final class MD5 {

    public static String encrypt(String strSrc) {
        try {
            char hexChars[] = { '0', '1', '2', '3', '4', '5', '6', '7', '8',
                    '9', 'a', 'b', 'c', 'd', 'e', 'f' };
            byte[] bytes = strSrc.getBytes();
            MessageDigest md = MessageDigest.getInstance("MD5");
            md.update(bytes);
            bytes = md.digest();
            int j = bytes.length;
            char[] chars = new char[j * 2];
            int k = 0;
            for (int i = 0; i < bytes.length; i++) {
                byte b = bytes[i];
                chars[k++] = hexChars[b >>> 4 & 0xf];
                chars[k++] = hexChars[b & 0xf];
            }
            return new String(chars);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
            throw new RuntimeException("MD5加密出错！！+" + e);
        }
    }
}
```

Centroller层：

```java
@Api(tags = "用户管理接口")
@RestController
@RequestMapping("/admin/system/SysUser")
public class SysUserController {
    @ApiOperation("新增用户")
    @PutMapping("save")
    public Result saveUser(@RequestBody SysUser sysUser) {
        // MD5密码加密
        String encrypt = MD5.encrypt(sysUser.getPassword());
        sysUser.setPassword(encrypt); // 密码加密后

        boolean flag = sysUserService.save(sysUser);
        if (flag) {
            return Result.ok(flag);
        } else {
            return Result.fail(flag);
        }
    }  
}
```




















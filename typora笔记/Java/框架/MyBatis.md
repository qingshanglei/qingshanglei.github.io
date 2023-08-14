MyBatis框架

# 简介

   MyBatis 是一款优秀的==持久层框架==，用于简化（替代） JDBC 开发。

   JavaEE三层架构：表现层、业务层、持久层。

   持久层：负责将数据到保存到数据库的那一层代码。

## 各持久层框架使用比例：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%90%84%E6%8C%81%E4%B9%85%E5%B1%82%E6%A1%86%E6%9E%B6%E4%BD%BF%E7%94%A8%E6%AF%94%E4%BE%8B.png)



## 和其它持久化层技术区别：

### JDBC（全手动）：

- SQL 夹杂在Java代码中耦合度高，导致硬编码内伤
- 维护不易且实际开发需求中 SQL 有变化，频繁修改的情况多见
- 代码冗长，开发效率低

### Hibernate 和 JPA（全自动）：

- 操作简便，开发效率高
- 程序中的长难复杂 SQL 需要绕过框架
- 内部自动生产的 SQL，不容易做特殊优化
- 基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难。
- 反射操作太多，导致数据库性能下降

### MyBatis（半自动）：

- 轻量级，性能出色
- SQL 和 Java 编码分开，功能边界清晰。Java代码专注业务、SQL语句专注数据
- 开发效率稍逊于HIbernate，但是完全能够接受

## MyBatis特性

1. MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。
2. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
3. MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录
4. MyBatis 是一个 半自动的ORM（Object Relation Mapping）框架。

## **目标：**

1.  能够使用映射配置文件实现CRUD操作

2.  能够使用注解实现CRUD操作

## 配置MyBatisX插件：

​    MybatisX 是一款基于 IDEA 的快速开发插件，为效率而生。

主要功能：

1. XML映射配置文件 和 接口方法 间相互跳转
2. 根据接口方法生成 statement

## Maven加载机制：

maven加载xml流程：

![maven加载xml](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/maven%E5%8A%A0%E8%BD%BDxml%E6%B5%81%E7%A8%8B.png)

1.省略，2.看项目

3.配置方式自动加载：

依赖

```xml
<!--解决maven加载机制问题：src/main/java模块能加载yml、properties和xml文件 -->
<build>
     <!-- 打包插件   -->
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
    
    <resources>
        <resource>  
            <directory>src/main/java</directory>
            <includes><!--src/main/java模块能加载yml等文件 -->
                <include>**/*.yml</include>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes> <include>**/*.yml</include>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

application.properties:

```properties
mybatis-plus.mapper-locations=classpath:com/qsl/ggkt/vod/mapper/xml/*.xml
```





# 搭建、使用MyBatis

​     mapper接口相当于dao（一张表操作对应一个dao）,有几张表对应几个mapper接口和mapper映射文件。

前提：在Maven 项目中

## 1.在pom.xml中引入jar包：

~~~xml
1. 创建user表，添加数据
2. 创建模板，导入坐标（jar包）
3. 编写MyBatis核心配置文件 ==>  替换连接信息 解决硬编码问题
4. 编写SQL映射文件 ==> 统一管理sql语句，解决硬编码问题
5. 编码  
   ① 定义POJO类
   ②加载核心配置文件，获取SqlSessionFactory对象
   ③获取SqlSession对象，执行SQL语句
   ④释放资源
<dependency> <!-- mybatis jar包-->
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>
~~~

## 2. 在 main 下添加 java 和 resources目录:

## 3. 在 resources目录添加 mybatis-config.xml文件并配置:

## 4. 在java目录下添加 com.qsl.pojo

## 5. 创建mybatis-config.xml文档。

 注意：resources目录下创建包要使用/创建： 例:com/qsl/mapper

​       Mybatis核心配置文件中的标签顺序(有的标签可以不写，但顺序一定不能乱)：`properties、settings、typeAliases、typeHandlers、objectFactory、objectWrapperFactory、reflectorFactory、plugins、environments、databaseIdProvider、mappers`。

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <typeAliases> <!-- 方式2： 包扫描方式设置类型别名-->
           <package name="com.qsl.pojo"/>
       </typeAliases>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">     <!-- 连接数据库-->
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/javadp?useSSL=false"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
             <!-- 方法2（推荐）： 包扫描方式-引入映射文件(resources下:包扫描映射路径)
          必须满足两要求：
             ①：mapper接口所在的包和映射文件所在的包路径一致。
             ②：mapper接口要和映射文件的名字一致。 -->
           <mapper resource="com.qsl.mapper"/>
       </mappers>
   </configuration>

   ~~~

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/mybatis%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.png)

# 核心文件配置：*

## mybatis-config.xml（核心）文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration> <!-- 配置标签的时候要 遵循前后顺序-->
    <!-- Mybatis核心配置文件中的标签顺序(有的标签可以不写，但顺序一定不能乱)：`properties、settings、
typeAliases、typeHandlers、objectFactory、objectWrapperFactory
、reflectorFactory、plugins、environments、databaseIdProvider、mappers` -->

    <!-- 导入jdbc.properties文件-->
    <properties resource="jdbc.properties"></properties>

    <!-- 设置类型（实体类）别名，（类型别名不区分大小写,别名就是类名） -->
    <typeAliases>
        <!--方式1：  单个实体类设置别名
                    type: 设置需要设置别名的类型 alias="别名"（alias可写可不写，默认类名）-->
        <!--    <typeAlias type="com.qsl.pojo.user" alias="user"></typeAlias>
            <typeAlias type="com.qsl.pojo.brand" alias="brand"></typeAlias>-->

        <!--方式2（推荐）： 包扫描方式设置类型别名-->
        <package name="com.qsl.pojo"/>
    </typeAliases>

    <!-- environments:配置数据库连接环境信息。可以配置多个environments，
     通过default属性切换不同的环境（environments）-->
    <environments default="development">
        <!--  生产环境-上线数据库 -->
        <environment id="development"> <!-- environment：配置某个具体的环境 -->
            <!-- transactionManager：设置事务管理方式
                    属性： type="JDBC | MANAGED"
                      JDBC: 表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事务的提交或回滚需要手动的处理
                      MANAGED： 别管理。  例：spring -->
            <transactionManager type="JDBC"/>
            <!-- dataSource: 配置数据源
                    type: 设置数据源的类型
                    type:"POOLED | UNPOOLED | JNDJ
                      POOLED： 表示使用数据库连接池缓存数据库连接
                      UNPOOLED： 表示不使用数据库连接池
                      JNDJ： 表示使用上下文中的数据源
                      "-->
            <dataSource type="POOLED">
                <!-- 数据库连接方式-->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>

        <!-- 开发环境-测试数据库-->
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 数据库连接方式-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/javadp?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 引入映射文件-->
    <mappers>
        <!-- 方法1：映射（只能一个个地映射） -->
<!--                 <mapper resource="UserMapper.xml"/> &lt;!&ndash;映射1&ndash;&gt;-->
<!--                 <mapper resource="com/qsl/mapper/UserMapper.xml"/>&lt;!&ndash; 映射2：Mapper代理方式&ndash;&gt;-->

        <!-- 方法2（推荐）： 包扫描方式-引入映射文件(resources下:包扫描映射路径)
          必须满足两要求：
             ①：mapper接口所在的包和映射文件所在的包路径一致。
             ②：mapper接口要和映射文件的名字一致。 -->
        <package name="com.qsl.mapper"/>
    </mappers>
</configuration>

```



## jdbc.properties文件：

```properties
# 使用jdbc.前缀以区分不同的properties文件的数据。
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/study_mybatis?useSSL=false
jdbc.username=root
jdbc.password=123456

```

## 设置类型（实体类）别名：*

   注意：类型别名不区分大小写。

#### 单个实体类设置别名：

    单个实体类设置别名:
         type: 设置需要设置别名的类型
         alias:设置某个类型的别名，不设置则采用默认类名
         alias="别名"（alias可写可不写，默认类名）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E8%AE%BE%E7%BD%AE%E7%B1%BB%E5%9E%8B%EF%BC%88%E5%AE%9E%E4%BD%93%E7%B1%BB%EF%BC%89%E5%88%AB%E5%90%8D-%E5%8D%95%E4%B8%AA%E5%AE%9E%E4%BD%93%E7%B1%BB%E8%AE%BE%E7%BD%AE%E5%88%AB%E5%90%8D.png)



#### 包扫描方式设置别名：*

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E8%AE%BE%E7%BD%AE%E7%B1%BB%E5%9E%8B%EF%BC%88%E5%AE%9E%E4%BD%93%E7%B1%BB%EF%BC%89%E5%88%AB%E5%90%8D-%E5%8C%85%E6%89%AB%E6%8F%8F%E6%96%B9%E5%BC%8F%E8%AE%BE%E7%BD%AE%E5%AE%9E%E4%BD%93%E7%B1%BB%E5%88%AB%E5%90%8D.png)

## 引入映射文件：*

引入映射文件,resources文件下的文件。

   注意：引入映射文件才可以在Mapper层使用SQL语句，注解开发除外。



#### 单个引入：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%BC%95%E5%85%A5%E5%85%A5%E6%98%A0%E5%B0%84%E6%96%87%E4%BB%B6-%E5%8D%95%E4%B8%AA%E5%BC%95%E5%85%A5.png)



#### 包扫描方式引入： *

  包扫描方式引入,必须满足两要求 ：

​              ①：mapper接口所在的包和映射文件所在的包路径一致。

​               ②： mapper接口要和映射文件的名字一致。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%BC%95%E5%85%A5%E6%98%A0%E5%B0%84%E6%96%87%E4%BB%B6-%E5%8C%85%E6%89%AB%E6%8F%8F%E6%96%B9%E5%BC%8F.png)



# xml开发: *

查询功能的标签必须设置resultType或resultMap
 resultType（结果类型）：设置默认的映射关系。
 resultMap（结果映射）：设置自定义的映射关系（字段名不一致，处理一对多，多对一关系）。



### 接收参数值：*

   接收参数值的两种方法：参数占位符：
      ①： #{参数}：本质占位符赋值。
     执行SQL时，会将#{}占位符替换为？（参数传递的时候使用）
       ②：**`$`**{参数}: 本质字符串拼接。存在SQL注入问题,${}需要“”或''拼接。 （表名或者列名不固定的情况下使用）

​     接收参数值：  int ,对象（实体类），Map集合,List集合。



#### 单个参数：

```java
User selAll(String username);  //例：根据用户查询用户信息。
```

##### #{参数}：

```xml
<select id="getUserByUsername" resultType="User"> 
    select * from t_user where username = #{username} 
</select>
```



##### ${参数}：

注意：${}必须“”或‘’包括起来。

```xml
<select id="getUserByUsername" resultType="User"> 
    select * from t_user where username = '${username}'  
</select>
```



#### 多个参数：

   **注意**：mapper接口方法的参数为多个参数时，MyBatis会把参数封装到Map集合里[arg1,arg2,param1,param2]，必须使用它来定义参数。

1. ​      以arg0，arg1... 或@Param注解的值为键，以参数为值。
2. ​     以param1，param2...为键，以参数为值。

​     所以通过#{}或${}以键的方式访问值即可。

​    @Param("map键名称")： 修改Map集合中默认key名（org键名）。

```xml

```



#### 实体类（对象）参数：

   ②实体类封装参： 只需要保存SQL中的参数名和 实体类属性名对应上，即可设置成功。

#### map集合参数：

   ③map集合： 只需要保存SQL中的参数名和 map集合的键名称对应上，即可设置成功。



### 返回值：

​     返回值：  int ,对象（实体类），Map集合,List集合。



### 特殊SQL: *

#### 模糊查询 like：

 案例： 模糊查询品牌名称

##### '${参数}'：

```xml
   <select id="getUserByLike1" resultMap="brandResultMap">
        select *
        from tb_brand
        where brand_name like '%${PbrandName}%';
    </select>
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%AF%A2-%E6%96%B9%E6%B3%951.png)



##### concat拼接SQL:

```xml
 <select id="getUserByLike2" resultMap="brandResultMap">
        select *
        from tb_brand
        where brand_name like concat('%', #{brandName}, '%');
    </select>
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%AF%A2-%E6%96%B9%E6%B3%952.png)



##### '%' 拼接SQL: *

```xml
  <select id="getUserByLike3" resultMap="brandResultMap">
        select *
        from tb_brand
        where brand_name like '%' #{brandName} '%';
    </select>  
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%AF%A2-%E6%96%B9%E6%B3%953.png)



#### 特殊字符：

   SQL语句中特殊字符处理。  例： < 号。（SQL语句里的特殊字符必须转义才能用）

##### 转义字符：

```xml
   <select id="getUserByESC" resultType="com.qsl.pojo.Brand">
        select *
        from tb_brand
        where id &lt; #{Id};   <!-- 使用转义字符 -->
    </select>   
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%84%E7%90%86SQL%E8%AF%AD%E5%8F%A5%E4%B8%AD%E7%89%B9%E6%AE%8A%E5%AD%97%E7%AC%A6%E2%80%94%E6%96%B9%E6%B3%951.%E8%BD%AC%E4%B9%89%E5%AD%97%E7%AC%A6.png)

##### CDATA区：

```xml
   <select id="getUserByCDATA" resultType="com.qsl.pojo.Brand">
        select *
        from tb_brand
        where id <![CDATA[  <  ]]> #{Id};   快捷键：CD+回车
    </select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%84%E7%90%86SQL%E8%AF%AD%E5%8F%A5%E4%B8%AD%E7%89%B9%E6%AE%8A%E5%AD%97%E7%AC%A6%E2%80%94%E6%96%B9%E6%B3%952.DAATA%E5%8C%BA.png)



### 解决：数据库的字段名称和 实体类的属性名称不同 *

-  为字段起别名，保持和属性名的一致。
-  设置全局配置，将_自动映射为驼峰命名。 例：emp_name ==> empName



1.    **起别名：**在SQL语句中，对不一样的列名起别名，别名和实体类属性名一样（可以定义 SQL片段，提升复用性）。
2.    **resultMap**：定义完成不一致的属性名和列名的映射。

#### 起别名(给列起别名)：

~~~xml
  <select id="selectAll" resultType="com.qsl.pojo.Brand">
        select id, brand_name as brandName, company_name as companyName, ordered, description, status
        from tb_brand;
    </select>
~~~

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%AD%97%E6%AE%B5%E5%90%8D%E7%A7%B0%E5%92%8C%20%E5%AE%9E%E4%BD%93%E7%B1%BB%E7%9A%84%E5%B1%9E%E6%80%A7%E5%90%8D%E7%A7%B0%E4%B8%8D%E5%90%8C%20%E2%80%94%E2%80%94%E8%B5%B7%E5%88%AB%E5%90%8D.png)



####  sql片段(给列起别名)：

~~~xml
(这个字段Id会报红，解决方法：alt+inter ==>language injection setting ==>切换SQL为GenericSQL,但写查询表的时候会没提示)
   <sql id="自定义"> id, brand_name as brandName, company_name as companyName, ordered ...  </sql>  //定义sql片段  
   <include refid="sql片段id"  />  // 使用sql片段
~~~

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%AD%97%E6%AE%B5%E5%90%8D%E7%A7%B0%E5%92%8C%20%E5%AE%9E%E4%BD%93%E7%B1%BB%E7%9A%84%E5%B1%9E%E6%80%A7%E5%90%8D%E7%A7%B0%E4%B8%8D%E5%90%8C%20%E2%80%94%E2%80%94SQL%E7%89%87%E6%AE%B5.png)



#### resultMap（结果映射）： *

​     1.定义<resultMap>标签

​     2.在<select>标签中，使用resultMap属性替换 resultType属性

- 若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射，即使字段名和属性名一致的属性也要映射，也就是全部属性都要列出来

    parameterType: 用于设置参数类型，可省略（<select>  </select>标签的）

##### 字段不一致：

```xml
（property报红，解决方法，alt+inter ==》Inspection 'Mapper xml inspection'options）
<resultMap id="唯一标识" type="映射的类型，支持别名">
    <!-- 主键的映射 -->
     <id column="数据库表列名"  property="实体类的属性名"></id>
    <!-- 一般字段的映射 -->
    <result column="数据库表列名"  property="实体类的属性名"></result>
</resultMap>

<select id="接口方法名" resultMap="唯一标识">
     select * from 表名
</select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%AD%97%E6%AE%B5%E5%90%8D%E7%A7%B0%E5%92%8C%20%E5%AE%9E%E4%BD%93%E7%B1%BB%E7%9A%84%E5%B1%9E%E6%80%A7%E5%90%8D%E7%A7%B0%E4%B8%8D%E5%90%8C%20%E2%80%94%E2%80%94resultMap.png)



##### 一对多：

###### collection：

- association：处理数据库多对一的映射关系。
- property：需要处理多对的映射关系的属性名。
- javaType：该属性的类型。

实体类：

```java
@Data // getter、setter、toString、 equals、哈希值 ...
@AllArgsConstructor // 提供一个所有参数的构造器
@NoArgsConstructor // 无参构造器
public class Dept {
    private Integer deptId;
    private String deptName; // 部门名称
   
    List<Emp> emps; // 解决一对多问题（多对一是对象，一对多是集合。）
}
```



```xml
   <resultMap id="DeptMap" type="Dept">
        <id property="deptId" column="dept_id"></id>
        <result property="deptName" column="dept_name"></result>

        <collection property="emps" ofType="Emp">
            <id property="empId" column="emp_id"></id>
            <result property="empName" column="emp_name"></result>
            <result property="age" column="age"></result>
            <result property="sex" column="sex"></result>
            <result property="email" column="email"></result>
        </collection>
    </resultMap>

    <select id="getDeptAndEmp" resultMap="DeptMap">
        select *
        from tbl_dept d
                 left join tbl_emp e on d.dept_id = e.dept_id
        where d.dept_id = #{did}
    </select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%B8%80%E5%AF%B9%E5%A4%9A%E2%80%94%E2%80%94association.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%B8%80%E5%AF%B9%E5%A4%9A%E2%80%94%E2%80%94association2.png)



###### 分布式查询：

实体类：

```java
@Data // getter、setter、toString、 equals、哈希值 ...
@AllArgsConstructor // 提供一个所有参数的构造器
@NoArgsConstructor // 无参构造器
public class Dept {
    private Integer deptId;
    private String deptName; // 部门名称
   
    List<Emp> emps; // 解决一对多问题（多对一是对象，一对多是集合。）
}
```



 **①查询部门信息:**

```xml
  <resultMap id="deptAndEmpByStepResultMap" type="Dept">
        <id property="deptId" column="dept_id"></id>
        <result property="deptName" column="dept_name"></result>
        <collection property="emps"
                    select="com.qsl.mapper.EmpMapper.getDeptAndEmpByStepTwo"
                    column="dept_id">
            <id property="empId" column="emp_id"></id>
            <result property="empName" column="emp_name"></result>
            <result property="age" column="age"></result>
            <result property="sex" column="sex"></result>
            <result property="email" column="email"></result>
        </collection>
    </resultMap>
    <select id="getDeptAndEmpByStepOne" resultMap="deptAndEmpByStepResultMap">
        select *
        from tbl_dept
        where dept_id = #{did}
    </select>
```

**②查询员工信息:**

```xml
  <select id="getDeptAndEmpByStepTwo" resultType="Emp">
        select *
        from tbl_emp
        where dept_id = #{did}
    </select>
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%B8%80%E5%AF%B9%E5%A4%9A%E2%80%94%E2%80%94%E5%88%86%E5%B8%83%E6%9F%A5%E8%AF%A2.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%B8%80%E5%AF%B9%E5%A4%9A%E2%80%94%E2%80%94%E5%88%86%E5%B8%83%E6%9F%A5%E8%AF%A22.png)



##### 多对一：

①级联属性赋值：
②association：
③分布式查询： 分布查询的好处：延迟加载
 注意：  多对一是对象，一对多是集合。

###### 级联属性赋值:

**实体类：**

```java
@Data // getter、setter、toString、 equals、哈希值 ...
@AllArgsConstructor // 提供一个所有参数的构造器
@NoArgsConstructor // 无参构造器
public class Emp   {
    private Integer empId;
    private String empName; // 员工姓名
    private Integer age; // 年龄
    private String sex; // 性别
    private String email; // 邮箱
    // 解决多对一问题（多对一是对象，一对多是集合。）
    private Dept dept;
}
```



```xml
    <resultMap id="empAndDeptResultMapOne" type="Emp">
        <id property="empId" column="emp_id"></id>

        <result property="dept.deptId" column="dept_id"></result>
        <result property="dept.deptName" column="dept_name"></result>
    </resultMap>

    <select id="getEmpAndDept" resultMap="empAndDeptResultMapOne">
        SELECT *
        FROM tbl_emp e
                 LEFT JOIN tbl_dept d ON e.dept_id = d.dept_id
        WHERE e.emp_id = #{eid}
    </select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94%E7%BA%A7%E8%81%94%E5%B1%9E%E6%80%A7.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94%E7%BA%A7%E8%81%94%E5%B1%9E%E6%80%A72.png)



###### association:

- association：处理多对一的映射关系
- property：需要处理多对的映射关系的属性名
- javaType：该属性的类型

**实体类：**

```java
@Data // getter、setter、toString、 equals、哈希值 ...
@AllArgsConstructor // 提供一个所有参数的构造器
@NoArgsConstructor // 无参构造器
public class Emp   {
    private Integer empId;
    private String empName; // 员工姓名
    private Integer age; // 年龄
    private String sex; // 性别
    private String email; // 邮箱
    // 解决多对一问题（多对一是对象，一对多是集合。）
    private Dept dept;
}
```



```xml
<resultMap id="empAndDeptResultMapTwo" type="Emp">
        <id property="empId" column="emp_id"></id>
        <result property="empName" column="emp_name"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
        <result property="email" column="email"></result>
        <association property="dept" javaType="Dept">
            <id property="deptId" column="dept_id"></id>
            <result property="deptName" column="dept_name"></result>
        </association>
    </resultMap>

    <select id="getEmpAndDeptTwo" resultMap="empAndDeptResultMapTwo">
        SELECT *
        FROM tbl_emp e
                 LEFT JOIN tbl_dept d ON e.dept_id = d.dept_id
        WHERE e.emp_id = #{eid}
    </select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94association.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94association2.png)

###### 分布式查询:

- select：设置分布查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）。
- column：设置分步查询的条件。
- 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：

注意：分布式查询的每一步都可单独使用执行。

**实体类：**

```java
@Data // getter、setter、toString、 equals、哈希值 ...
@AllArgsConstructor // 提供一个所有参数的构造器
@NoArgsConstructor // 无参构造器
public class Emp   {
    private Integer empId;
    private String empName; // 员工姓名
    private Integer age; // 年龄
    private String sex; // 性别
    private String email; // 邮箱
    // 解决多对一问题（多对一是对象，一对多是集合。）
    private Dept dept;
}
```



1. ###### 查询员工信息：

   ```xml
    <resultMap id="empAndDeptByStepResultMap" type="Emp">
           <id property="empId" column="emp_id"></id>
           <result property="empName" column="emp_name"></result>
           <result property="age" column="age"></result>
           <result property="sex" column="sex"></result>
           <result property="email" column="email"></result>
   
           <association property="dept"
                        select="com.qsl.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                        column="dept_id"
                        fetchType="lazy"></association>
       </resultMap>
   
       <select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
           SELECT *
           FROM tbl_emp
           WHERE emp_id = #{eid}
       </select>
   ```

   

2. ###### 查询部门信息：

```xml
<!--此处的resultMap仅是处理字段和属性的映射关系-->
<resultMap id="EmpAndDeptByStepTwoResultMap" type="Dept">
	<id property="did" column="did"></id>
	<result property="deptName" column="dept_name"></result>
</resultMap>

<select id="getEmpAndDeptByStepTwo" resultMap="EmpAndDeptByStepTwoResultMap">
	select * from t_dept where did = #{did}
</select>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94%E5%88%86%E5%B8%83%E5%BC%8F%E6%9F%A5%E8%AF%A2.png)

注意：第七段设置MyBatis的全局配置：将_自动映射为驼峰命名。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%A4%9A%E5%AF%B9%E4%B8%80%20%E2%80%94%E2%80%94%E5%88%86%E5%B8%83%E5%BC%8F%E6%9F%A5%E8%AF%A22.png)



##### 延迟加载：

- 分步查询的优点：可以实现延迟加载，但是**必须在核心配置文件中设置全局配置信息**：
   - lazyLoadingEnabled：延迟加载的全局开关。当开启时， 所有关联对象都会延迟加载 。
   - aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载 。
- 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加载，默认) | eager(立即加载)"


```xml
<settings>   <!-- 使用分布查询后，在核心配置文件中开启延迟加载-->
	<!--开启延迟加载-->
	<setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

```java
  @Test
    public void getEmpAndDeptByStepOneLoad() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);

        //4. 执行方法
        Emp emp = mapper.getEmpAndDeptByStepOne(2);
        System.out.println(emp.getEmpName());
        //5. 释放资源
        sqlSession.close();
    }
```

###### 可在xml映射配置里改回立即加载：

```xml
    <resultMap id="empAndDeptByStepResultMap" type="Emp">
        <id property="empId" column="emp_id"></id>
        <result property="empName" column="emp_name"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
        <result property="email" column="email"></result>

        <association property="dept"
select="com.qsl.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                     column="dept_id"
                     fetchType="eager"></association>
    </resultMap>
```



###### 开不开延迟加载的区别：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%BC%80%E6%B2%A1%E5%BC%80%E9%AA%8C%E8%AF%81%E5%8A%A0%E8%BD%BD%E7%9A%84%E5%8C%BA%E5%88%AB.png)



#### 全局配置:

```xml
<settings>    <!-- 设置MyBatis的全局配置-->
    <!-- 将_自动映射为驼峰命名。 例：emp_name ==> empName -->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%AD%97%E6%AE%B5%E5%90%8D%E7%A7%B0%E5%92%8C%20%E5%AE%9E%E4%BD%93%E7%B1%BB%E7%9A%84%E5%B1%9E%E6%80%A7%E5%90%8D%E7%A7%B0%E4%B8%8D%E5%90%8C%20%E2%80%94%E2%80%94%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AEpng.png)



### 动态SQL:  *

##### 多条件：

###### if标签：

​    if：用于判断参数是否有值，使用test属性进行条件判断 ，test: 逻辑表达式

<!-- 判断状态、品牌名称、企业名称是否为空，不为空查询-->

```xml
 <select id="getByCondtionifOne" resultMap="brandResultMap">
        select *  
        from tb_brand
        where 1 = 1
        <if test="status !=null">
            and status = #{status}
        </if>
        <if test="brandName !=null and brandName !=''">
            and brand_name like '%' #{brandName} '%'
        </if>
        <if test="companyName !=null and companyName !=''">
            and company_name like '%' #{companyName} '%'
        </if>;
    </select>  
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL%E2%80%94if%E5%88%A4%E6%96%AD0.png)





###### where标签：

  1.当where标签中有内容时，会自动生成where关键字，并且将内容前多余的and或or去掉。
  2.当where标签中没有内容时，此时where标签没有任何效果。
       **注意**： where标签不能将其中内容后面多余的and或or去掉。

```xml
 <select id="getByCondtionifTwo" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <if test="status !=null">
                and status =#{status}
            </if>
            <if test="brandName !=null and brandName !=''">
                and brand_name like '%' #{brandName} '%'
            </if>
            <if test="companyName !=null and companyName !=''">
                and company_name like '%' #{companyName} '%'
            </if>;
        </where>
    </select>
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL%E2%80%94where-if%E5%88%A4%E6%96%AD.png)



###### trim标签：

​        若标签中有内容时：
* suffixOverrides | suffix: 将trim标签中内容前面或后面添加指定内容。
* prefix | prefixOverrides: 将trim标签中内容前面或后面去掉指定内容。
  若标签中没有内容时，trim标签没有任何效果。

```xml
 <select id="getByCondtionifThree" resultMap="brandResultMap">
        select *
        from tb_brand
        <trim prefix="where" suffixOverrides="and|or">
            <if test="status !=null and status !=''">
                status =#{status} and
            </if>
            <if test="brandName !=null and brandName !=''">
                brand_name like '%' #{brandName} '%' or
            </if>
            <if test="companyName !=null and companyName !=''">
                company_name like '%' #{companyName} '%'
            </if>;
        </trim>
    </select>
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL%E2%80%94trim-if%E5%88%A4%E6%96%AD.png)

##### 单条件：

###### choose、 when、otherwise:

   choose标签类似switch；  when标签类似case； otherwise标签类似default。

```xml
   <select id="selectByCondtionSingle" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <choose> <!-- 类似于switch-->
                <when test="status !=null">status = #{status}</when> <!-- 类似于case-->
                <when test="brandName !=null and brandName !=''">brand_name like '%' #{brandName} '%'</when> <!-- 类似于case-->
                <when test="companyName !=null and companyName !=''">company_name like '%' #{companyName} '%'</when>
                <otherwise>1 = 1</otherwise> <!-- 类似于default-->
            </choose>
        </where>
    </select>
```

流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL(%E5%8D%95%E6%9D%A1%E4%BB%B6)%E2%80%94where-choose(when%E3%80%81otherwise)%E5%88%A4%E6%96%AD.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL(%E5%8D%95%E6%9D%A1%E4%BB%B6)%E2%80%94where-choose(when%E3%80%81otherwise)2%E5%88%A4%E6%96%AD.png)



##### foreach标签：

​    foreach标签：用来迭代任何可迭代的对象（如数组，集合；类似增强for）。

- collection 属性：设置要循环的**`数组`**或**`集合`**。
- item 属性：表示集合或数组中的所有**`数据`** 。
- separator 属性：设置循环体之间的**`分隔符`**。最后一次迭代不会加分隔符。（分隔符前后默认有一个空格）
- open 属性：设置foreach标签中的内容的**`开始符`**。
- close 属性：设置foreach标签中的内容的**`结束符`**。
  - mybatis会将数组参数，封装为一个Map集合。
    -  默认：array = 数组
    -  使用@Param注解改变map集合的默认key的名称

```xml
<delete id="deleteByIds">   <!-- 通过数组id,实现批量删除 -->
    delete from tb_brand where id
    in
    <foreach collection="array" item="id" separator="," open="(" close=")"> #{id} </foreach> ;  
</delete>
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%8A%A8%E6%80%81SQL%E2%80%94foreach.png)





```java
   parameterType: 用于设置参数类型（可省略）    
```

### 新增

#### 新增单个：

```xml
<insert id="addBrandKey" useGeneratedKeys="true"
        keyProperty="id"> <!-- 返回添加数据主键（Id）：设置useGeneratedKeys="true" keyProperty="id" -->
    insert into tb_brand(brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%96%B0%E5%A2%9E%EF%BC%88%E5%8D%95%E4%B8%AA%E6%96%B0%E5%A2%9E%EF%BC%89%E2%80%94%E8%BF%94%E6%96%B0%E5%A2%9EId.png)



#### 批量新增：

```xml
  <insert id="insertMoreByList">
        insert into tb_brand
        values
        <foreach collection="brands" item="brand" separator=",">
            (null,#{brand.brandName},#{brand.companyName},#{brand.ordered},#{brand.description},#{brand.status})
        </foreach>
    </insert>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%96%B0%E5%A2%9E%EF%BC%88%E6%89%B9%E9%87%8F%E6%96%B0%E5%A2%9E%EF%BC%89.png)



### 删除：

#### 删除单个：

```xml
  <update id="delByIdDete">
        delete
        from tb_brand
        where id = #{id}  
    </update>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E5%88%A0%E9%99%A4%E2%80%94%E6%A0%B9%E6%8D%AEid%E5%88%A0%E9%99%A4.png)



#### 批量删除：

##### ${参数}:

```xml
<delete id="delByIdDates">
    delete
    from tb_brand
    where id in (${ids}); 
</delete>  <!--    注意：必须为${参数}才行，#{参数}报错-->
```

![批量删除—字符串](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%89%B9%E9%87%8F%E5%88%A0%E9%99%A4%E2%80%94%E5%AD%97%E7%AC%A6%E4%B8%B2.png)

##### foreach 方法1:

```xml
    <delete id="delByIdDates">
        delete
        from tb_brand
        where id in
        <foreach collection="array" item="id" separator="," open="(" close=")">#{id}</foreach>;
    </delete>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%89%B9%E9%87%8F%E5%88%A0%E9%99%A4%E2%80%94foreach%E6%96%B9%E6%B3%951.png)

##### foreach 方法2:

```xml
<delete id="delByIdDates2">
        delete
        from tb_brand
        where <foreach collection="array" item="id" separator="or">id=#{id}</foreach>;
    </delete>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E6%89%B9%E9%87%8F%E5%88%A0%E9%99%A4%E2%80%94foreach%E6%96%B9%E6%B3%952.png)



### 修改：

#### 修改全部：

```xml
  <update id="UpdateAll">
        update tb_brand
        set brand_name  =#{brandName},
            company_name=#{companyName},
            ordered=#{ordered},
            description=#{description},
            status=#{status}
        where id = #{id}
    </update>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BF%AE%E6%94%B9%E2%80%94%E4%BF%AE%E6%94%B9%E5%85%A8%E9%83%A8%E5%AD%97%E6%AE%B5.png)



#### 动态修改：

​     set标签可以为 SQL 语句动态的添加 set 关键字，剔除追加到条件末尾多余的逗号。

```xml
  <update id="UpdateBynamic">
        update tb_brand
        <set>
            <if test="brandName !=null and brandName !=''">brand_name =#{brandName},</if>
            <if test="companyName !=null and companyName !=''">company_name = #{companyName},</if>
            <if test="ordered !=null ">ordered =#{ordered},</if>
            <if test="description !=null and description !=''">description = #{description},</if>
            <if test="status  !=null ">status =#{status}</if>
        </set>
        where id = #{id}
    </update>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BF%AE%E6%94%B9%E2%80%94%E5%8A%A8%E6%80%81%E4%BF%AE%E6%94%B9.png)

### 默认的类型别名：

​         别名                                                 映射的类型

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E9%BB%98%E8%AE%A4%E7%9A%84%E7%B1%BB%E5%9E%8B%E5%88%AB%E5%90%8D1.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E9%BB%98%E8%AE%A4%E7%9A%84%E7%B1%BB%E5%9E%8B%E5%88%AB%E5%90%8D2.png)

### 查询：

1. 如果查询出的数据只有一条，可以通过。

   - 实体类对象接收。
   - List集合接收。
   - Map集合接收。结果:{password=123,sex=男，id=1,age=23,username=gl001}

2. 如果查询出的数据有多条，一定不能用实体类对象接收，会抛异常TooManyResultsException,可以通过。
   - 实体类类型的List集合接收。
- Map类型的List集合接收。
   - 在mapper接口的方法上添加@MapKey注解。
   

1. 查询的标签select必须设置属性resultType或resultMap，用于设置实体类和数据库表的映射关系  

	- resultType：自动映射，用于属性名和表中字段名一致的情况  
	- resultMap：自定义映射，用于一对多 或多对一 或字段名和属性名不一致的情况  
2. 当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值

#### 查询单条:

##### 实体类对象：





##### List集合：



##### Map集合：



#### 多数据：



##### 实体类类型的List集合：

##### Map类型的List集合：

##### 在Map类型的方法上添加@MapKey注解：





# 注解开发

1. @Select: 查询

2. @Insert: 新增

3. @Update: 修改

4. @Delete: 删除

5. @ResultMap()   :ResultMap片段

   注意：注解开发，只适合简单的开发，复杂的还是需要使用XML映射语句

~~~java
  /* 1.查询*/
    @Select("select * from mybatis_user where id=#{id};")   ---注解开发
    @ResultMap("id")  --- resultMap注解（类似sql片段）
    User selectId(int id);

~~~



### 分页查询：

```java
   @Select("select  * from  mybatis_brand limit #{begin},#{size}")
   @ResultMap("brandResultMap")
   List<Brand> selectByPage(@Param("begin") int begin, @Param("size") int size);
```



# 日志：*

注意： 使用**log4j日志**或 **slf4j日志**主要是来查看SQL执行语句的。

####  log4j日志：*

日志级别：

​     FATAL（致命）> ERROR(错误) > WARN（警告） > INFO（信息）>DEBUG(调试)，从左到右打印的内容越来越详细。

##### 1.添加日志依赖：

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```





#####   2.在项目里的log4j.xml文件加以下配置，**执行SQL即可**：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/xml/doc-files/log4j.dtd">

<log4j:configuration>
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m (%F:%L) \n"/>
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT"/>
    </root>
</log4j:configuration>
```



#### slf4j日志：

​    不推荐使用，输出太多日志了。

##### 1.在mavan里，添加依赖

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.20</version>
</dependency>
<!-- 添加logback-classic依赖 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
<!-- 添加logback-core依赖 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.2.3</version>
</dependency>
```

#####    2.在项目里的logback.xml文件加以下配置，**执行SQL即可**：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%level] %blue(%d{HH:mm:ss.SSS}) %cyan([%thread]) %boldGreen(%logger{15}) - %msg %n</pattern>
        </encoder>
    </appender>

    <logger name="com.itheima" level="DEBUG" additivity="false">
        <appender-ref ref="Console"/>
    </logger>


    <!--
      level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
     ， 默认debug
      <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
      -->
    <root level="DEBUG">
        <appender-ref ref="Console"/>
    </root>
</configuration>

```



​    

## MyBatis的缓存：

### 一级缓存：

- 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问  

  

   

#### 使一级缓存失效的四种情况：

```
1. 不同的SqlSession对应不同的一级缓存  
2. 同一个SqlSession但是查询条件不同
3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
4. 同一个SqlSession两次查询期间手动清空了缓存
```





### 二级缓存：

- 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取 。

#### 二级缓存开启的条件：

```
1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
2. 在映射文件中设置标签<cache />
3. 二级缓存必须在SqlSession关闭或提交之后有效
4. 查询的数据所转换的实体类类型必须实现序列化的接口
```



####   使二级缓存失效的情况：

​      两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效。

#### 二级缓存的相关配置:

- 在mapper配置文件中添加的cache标签可以设置一些属性。

- eviction属性：缓存回收策略。  
  - LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。  
  - FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。  
  - SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。  
  - WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
  - 默认的是 LRU。
  
- flushInterval属性：刷新间隔，单位毫秒。
  
   - 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句（增删改）时刷新。
   
- size属性：引用数目，正整数。
  
   - 代表缓存最多可以存储多少个对象，太大容易导致内存溢出
   
- readOnly属性：只读，true/false
   - true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象 不能被修改。这提供了很重要的性能优势。  
   - false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false。
   
   
   
   
   
###  MyBatis缓存查询的顺序：

   - 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用  。
   - 如果二级缓存没有命中，再查询一级缓存  。
   - 如果一级缓存也没有命中，则查询数据库  。
   - SqlSession关闭之后，一级缓存中的数据会写入二级缓存。



### 整合第三方缓存EHCache：

​    注意：存在SLF4J时，作为简易日志的log4j将失效，此时我们需要借助SLF4J的具体实现logback来打印日志。创建logback的配置文件`logback.xml`，名字固定，不可改变。



#### Ehcache各个jar包功能:

| jar包名称       | 作用                            |
| --------------- | ------------------------------- |
| mybatis-ehcache | Mybatis和EHCache的整合包        |
| ehcache         | EHCache核心包                   |
| slf4j-api       | SLF4J日志门面包                 |
| logback-classic | 支持SLF4J门面接口的一个具体实现 |



#### EHCache配置文件说明:

|             属性名              | 是否必须 | 作用                                                         |
| :-----------------------------: | -------- | :----------------------------------------------------------- |
|       maxElementsInMemory       | 是       | 在内存中缓存的element的最大数目                              |
|        maxElementsOnDisk        | 是       | 在磁盘上缓存的element的最大数目，若是0表示无穷大             |
|             eternal             | 是       | 设定缓存的elements是否永远不过期。 如果为true，则缓存的数据始终有效， 如果为false那么还要根据timeToIdleSeconds、timeToLiveSeconds判断 |
|         overflowToDisk          | 是       | 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上      |
|        timeToIdleSeconds        | 否       | 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时， 这些数据便会删除，默认值是0,也就是可闲置时间无穷大 |
|        timeToLiveSeconds        | 否       | 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大 |
|      diskSpoolBufferSizeMB      | 否       | DiskStore(磁盘缓存)的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区 |
|         diskPersistent          | 否       | 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false   |
| diskExpiryThreadIntervalSeconds | 否       | 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s， 相应的线程会进行一次EhCache中数据的清理工作 |
|    memoryStoreEvictionPolicy    | 否       | 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。 默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出 |



#### 1.添加依赖：

```xml
<!-- Mybatis EHCache整合包 -->
<dependency>
	<groupId>org.mybatis.caches</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 -->
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.2.3</version>
</dependency>
```

# 分页插件：*

### 分页插件常用数据：

- pageNum：当前页的页码  
- pageSize：每页显示的条数  
- size：当前页显示的真实条数  
- total：总记录数  
- pages：总页数  
- prePage：上一页的页码  
- nextPage：下一页的页码
- isFirstPage/isLastPage：是否为第一页/最后一页  
- hasPreviousPage/hasNextPage：是否存在上一页/下一页  
- navigatePages：导航分页的页码数  
- navigatepageNums：导航分页的页码，\[1,2,3,4,5]

### 分页插件的使用：

  **注意**：必须在查询，修改，删除功能之前执行PageHelper.startPage(int pageNum,int pageSize)开启分页。

#### 1.添加依赖：

```xml
<!-- mavan仓库路径： https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
	<groupId>com.github.pagehelper</groupId>
	<artifactId>pagehelper</artifactId>
	<version>5.2.0</version>
</dependency>
```

#### 2.配置分页插件：

  在MyBatis的核心配置文件（mybatis-config.xml）中配置插件,必须按核心配置文件的顺序配置。

```xml
<plugins>       
    <!--设置分页插件-->
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```





#### 3.分页插件的使用：

##### 方式1：

```java
   @Test
    public void selAllPage() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);

        // 分页插件
        Page<Object> page = PageHelper.startPage(1, 4);

        //添加判断
        EmpExample example = new EmpExample();
        example.createCriteria().andEmpNameEqualTo("不知火舞").andAgeEqualTo(23);
        //4. 执行sql
        List<Emp> emps = mapper.selectByExample(example);
        System.out.println("数据 ==> " + page);

        //5. 释放资源
        sqlSession.close();
    }
```

  过程截图：



##### 方式2：

```java
   @Test
    public void selAllPage2() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();

        //3. 获取Mapper接口的代理对象
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);

        // 分页插件
        PageHelper.startPage(1, 4);

        //添加判断
        EmpExample example = new EmpExample();
        example.createCriteria().andEmpNameEqualTo("不知火舞").andAgeEqualTo(23);

        //4. 执行sql
        List<Emp> emps = mapper.selectByExample(example);
        PageInfo<Emp> page = new PageInfo<>(emps, 5);
        System.out.println("数据 ==> " + page);

        //5. 释放资源
        sqlSession.close();
    }
```

  过程截图：



# MyBatis逆向工程：

- 正向工程：先创建Java实体类，由框架负责根据实体类生成数据库表。Hibernate是支持正向工程的

- 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：  

 - Java实体类  

   - Mapper接口  

   - Mapper映射文件

     


​     

### 简洁版 — MyBatis3Simple：

 MyBatis3Simple: 生成基本的CRUD（清新简洁版）。

#### 1.引入依赖和插件：

```xml
<dependencies>
	<!-- MyBatis核心依赖包 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.9</version>
	</dependency>
	<!-- junit测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13.2</version>
		<scope>test</scope>
	</dependency>
	<!-- MySQL驱动 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.27</version>
	</dependency>
	<!-- log4j日志 -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
	<!-- 构建过程中用到的插件 -->
	<plugins>
		<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.0</version>
			<!-- 插件的依赖 -->
			<dependencies>
				<!-- 逆向工程的核心依赖 -->
				<dependency>
					<groupId>org.mybatis.generator</groupId>
					<artifactId>mybatis-generator-core</artifactId>
					<version>1.3.2</version>
				</dependency>
				<!-- 数据库连接池 -->
				<dependency>
					<groupId>com.mchange</groupId>
					<artifactId>c3p0</artifactId>
					<version>0.9.2</version>
				</dependency>
				<!-- MySQL驱动 -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>8.0.27</version>
				</dependency>
			</dependencies>
		</plugin>
	</plugins>
</build>
```

#### 2.创建逆向工程的配置文件:

文件名必须是：`generatorConfig.xml`

注意：http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd爆红不用管。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean（实体类）的生成策略-->
        <javaModelGenerator targetPackage="com.qsl.pojo" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" /><!--是否使用子包（true：生成com.qsl.pojo多级包。  false: 生成com.qsl.pojo这个目录。）   -->
            <property name="trimStrings" value="true" /><!--去数据库字段空格  -->
        </javaModelGenerator>
        
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.atguigu.mybatis.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />     <!-- SQL映射文件的生成策略 -->
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.qsl.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```





### 奢华版 — MyBatis3：*

   MyBatis3: 生成带条件的CRUD（奢华尊享版）。

#### 1.引入依赖和插件：

```xml
<dependencies>
	<!-- MyBatis核心依赖包 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.9</version>
	</dependency>
	<!-- junit测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13.2</version>
		<scope>test</scope>
	</dependency>
	<!-- MySQL驱动 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.27</version>
	</dependency>
	<!-- log4j日志 -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
	<!-- 构建过程中用到的插件 -->
	<plugins>
		<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.0</version>
			<!-- 插件的依赖 -->
			<dependencies>
				<!-- 逆向工程的核心依赖 -->
				<dependency>
					<groupId>org.mybatis.generator</groupId>
					<artifactId>mybatis-generator-core</artifactId>
					<version>1.3.2</version>
				</dependency>
				<!-- 数据库连接池 -->
				<dependency>
					<groupId>com.mchange</groupId>
					<artifactId>c3p0</artifactId>
					<version>0.9.2</version>
				</dependency>
				<!-- MySQL驱动 -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>8.0.27</version>
				</dependency>
			</dependencies>
		</plugin>
	</plugins>
</build>
```

#### 2.创建逆向工程的配置文件:

文件名必须是：`generatorConfig.xml`，在resources目录下创建。

注意：http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd爆红不用管。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- javaBean（实体类）的生成策略-->
        <javaModelGenerator targetPackage="com.qsl.pojo" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" /><!--是否使用子包（true：生成com.qsl.pojo多级包。  false: 生成com.qsl.pojo这个目录。）   -->
            <property name="trimStrings" value="true" /><!--去数据库字段空格  -->
        </javaModelGenerator>
        
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.qsl.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />     <!-- SQL映射文件的生成策略 -->
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.qsl.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

# MyBatis的缓存：

## 一级缓存：

- 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问  

```java
 @Test
    public void OneCache() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);

        //4. 执行sql
        System.out.println(mapper.getEmpById(2));
        System.out.println("--------------------------------");
        System.out.println(mapper.getEmpById(2));
        //5. 释放资源
        sqlSession.close();
    }
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/MyBatis%E7%9A%84%E7%BC%93%E5%AD%98%E2%80%94%E2%80%941%E7%BA%A7%E7%BC%93%E5%AD%98.png)

### 使一级缓存失效的四种情况：

#### 1.不同的SqlSession对应不同的一级缓存。

```java
   @Test
    public void getBrandByIdLose() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
        //4. 执行sql
        System.out.println(mapper.getEmpById(2));

        System.out.println("--------------------------------");
        SqlSession sqlSession2 = SqlSessionUtils.getSqlSession();
        CacheMapper mapper2 = sqlSession2.getMapper(CacheMapper.class);
        System.out.println(mapper2.getEmpById(2));

        //5. 释放资源
        sqlSession.close();
    }

```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BD%BF%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98%E5%A4%B1%E6%95%88%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%83%85%E5%86%B5%EF%BC%9A%201.%E4%B8%8D%E5%90%8C%E7%9A%84SqlSession%E5%AF%B9%E5%BA%94%E4%B8%8D%E5%90%8C%E7%9A%84%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98.png)

#### 2. 同一个SqlSession但是查询条件不同。

```java
 @Test
    public void getBrandByIdLose2() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
        //4. 执行sql
        System.out.println(mapper.getEmpById(2));

        System.out.println("--------------------------------");
        System.out.println(mapper.getEmpById(1));

        //5. 释放资源
        sqlSession.close();
    }
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BD%BF%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98%E5%A4%B1%E6%95%88%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%83%85%E5%86%B5%EF%BC%9A2.%20%E5%90%8C%E4%B8%80%E4%B8%AASqlSession%E4%BD%86%E6%98%AF%E6%9F%A5%E8%AF%A2%E6%9D%A1%E4%BB%B6%E4%B8%8D%E5%90%8C.png)



#### 3. 同一个SqlSession两次查询期间执行了任何一次增删改操作。

```java
 @Test
    public void getBrandByIdLose3() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
        //4. 执行sql
        System.out.println(mapper.getEmpById(2));

        System.out.println("---------------------");
        mapper.delEmp(10);
        System.out.println("---------------------");
        System.out.println(mapper.getEmpById(2));
        //5. 释放资源
        sqlSession.close();
    }
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BD%BF%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98%E5%A4%B1%E6%95%88%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%83%85%E5%86%B5%EF%BC%9A3.%E5%90%8C%E4%B8%80%E4%B8%AASqlSession%E4%B8%A4%E6%AC%A1%E6%9F%A5%E8%AF%A2%E6%9C%9F%E9%97%B4%E6%89%A7%E8%A1%8C%E4%BA%86%E4%BB%BB%E4%BD%95%E4%B8%80%E6%AC%A1%E5%A2%9E%E5%88%A0%E6%94%B9%E6%93%8D%E4%BD%9C.png)



#### 4. 同一个SqlSession两次查询期间手动清空了缓存。

```java
 @Test
    public void getBrandByIdLose4() throws IOException {
        //1-2. 获取sqlSessionFactory
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        //3. 获取Mapper接口的代理对象
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
        //4. 执行sql
        System.out.println(mapper.getEmpById(2));

        // 手动清空缓存(只对一级缓存有效)
        sqlSession.clearCache();

        System.out.println("---------------------");
        System.out.println(mapper.getEmpById(2));
        //5. 释放资源
        sqlSession.close();
    }
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BD%BF%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98%E5%A4%B1%E6%95%88%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%83%85%E5%86%B5%EF%BC%9A%204.%20%E5%90%8C%E4%B8%80%E4%B8%AASqlSession%E4%B8%A4%E6%AC%A1%E6%9F%A5%E8%AF%A2%E6%9C%9F%E9%97%B4%E6%89%8B%E5%8A%A8%E6%B8%85%E7%A9%BA%E4%BA%86%E7%BC%93%E5%AD%98.png)



## 二级缓存：

- 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取  
- 二级缓存开启的条件: 

  1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置。
  2. 在映射文件中设置标签<cache />。
  3. 二级缓存必须在SqlSession关闭或提交之后有效。
  4. 查询的数据所转换的实体类类型必须实现序列化的接口。
- 使二级缓存失效的情况：两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/%E4%BA%8C%E7%BA%A7%E7%BC%93%E5%AD%98.png)



## MyBatis缓存查询的顺序

- 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。  
- 如果二级缓存没有，再查询一级缓存。  
- 如果一级缓存也没有，则查询数据库 。 
- SqlSession关闭之后，一级缓存中的数据会写入二级缓存。

## 整合第三方缓存EHCache：

添加依赖：

```xml
<!-- Mybatis EHCache整合包 -->
<dependency>
	<groupId>org.mybatis.caches</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 -->
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.2.3</version>
</dependency>
```

### 创建EHCache的配置文件ehcache.xml

- 注意： xsi:noNamespaceSchemaLocation的路径会报错，不用管他。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <!-- 磁盘保存路径 -->
    <diskStore path="D:\atguigu\ehcache"/>
    <defaultCache
            maxElementsInMemory="1000"   
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="true"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```

### 设置二级缓存的类型

- 在xxxMapper.xml文件中设置二级缓存类型

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

### 加入logback日志

- 存在SLF4J时，作为简易日志的log4j将失效，此时我们需要借助SLF4J的具体实现logback来打印日志。创建logback的配置文件`logback.xml`。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
      <!-- 指定日志输出的位置 -->
      <appender name="STDOUT"
                class="ch.qos.logback.core.ConsoleAppender">
          <encoder>
              <!-- 日志输出的格式 -->
              <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体内容、换行 -->
              <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
          </encoder>
      </appender>
      <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR -->
      <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
      <root level="DEBUG">
          <!-- 指定打印日志的appender，这里通过“STDOUT”引用了前面配置的appender -->
          <appender-ref ref="STDOUT" />
      </root>
      <!-- 根据特殊需求指定局部日志级别 -->
      <logger name="com.atguigu.crowd.mapper" level="DEBUG"/>
  </configuration>
  ```

  



### 各个jar包的功能：

| jar包名称       | 作用                            |
| --------------- | ------------------------------- |
| mybatis-ehcache | Mybatis和EHCache的整合包        |
| ehcache         | EHCache核心包                   |
| slf4j-api       | SLF4J日志门面包                 |
| logback-classic | 支持SLF4J门面接口的一个具体实现 |

### EHCache配置文件说明

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatis/EHCache%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E8%AF%B4%E6%98%8E.png)

# 多环境配置：

​     在mybatis-config.xml（核心配置文件）里通过environments标签的default属性配置。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration> <!-- 配置标签的时候要 遵循前后顺序-->

    <!-- 起类型别名 :包扫描方式-扫描实体类（别名就是类名）-->
    <typeAliases>
        <package name="com.qsl.pojo"/>
    </typeAliases>

    <!-- environments:配置数据库连接环境信息。可以配置多个environments，
     通过default属性切换不同的环境（environments）-->
    <environments default="development">
           <!--  生产环境-上线数据库 -->
        <environment id="development"> <!-- environment：配置某个具体的环境 -->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 数据库连接方式-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/tb_brand?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>

        <!-- 开发环境-测试数据库-->
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 数据库连接方式-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/javadp?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- resources下:包扫描映射路径-->
        <package name="com.qsl.mapper"/>
    </mappers>
</configuration>
```




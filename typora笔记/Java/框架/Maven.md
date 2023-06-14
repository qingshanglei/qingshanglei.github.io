# Maven

Maven是专门用于管理和构建Java项目的工具，它的主要功能有：

* 提供了一套标准化的项目结构

* 提供了一套标准化的构建流程（编译，测试，打包，发布……）

* 提供了一套依赖管理机制

  **使用Maven的软件：**

  

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%E4%BD%BF%E7%94%A8Maven%E7%9A%84%E5%B7%A5%E5%85%B7.png)

  **标准化的项目结构：**
  
  
  
  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Maven%E6%A0%87%E5%87%86%E7%9B%AE%E5%BD%95.png)

# Maven安装：

-  解压 apache-maven-3.6.1.rar 既安装完成

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/1.png)

 解压缩后的目录结构如下：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/2.png)

* bin目录 ： 存放的是可执行命令。mvn 命令重点关注。

* conf目录 ：存放Maven的配置文件。`settings.xml` 配置文件后期需要修改。

* lib目录 ：存放Maven依赖的jar包。Maven也是使用java开发的，所以它也依赖其他的jar包。

  ### 修改仓库路径(默认C盘)（mvn_resp）

  修改 conf/settings.xml 中的 <localRepository> 为一个指定目录作为本地仓库，用来存储jar包。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/3.png)

配置阿里云私服,修改 conf/settings.xml 中的 <mirrors>标签，为其添加如下子标签：

```xml
<!-- 仓库修改为阿里云仓库路径-->
<mirror>  
    <id>alimaven</id>  
    <name>aliyun maven</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>          
</mirror>

<!-- 指定Maven使用JDK1.8的版本-->
<profile>
    <id>jdk-1.8</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
    </activation>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
```





### 环境变量配置：

打开命令行输入 ：  mvn  -version  检查是否配置成功,以下为成功图

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven%25E7%259A%2584%25E7%258E%25AF%25E5%25A2%2583%25E5%258F%2598%25E9%2587%258F%25E9%2585%258D%25E7%25BD%25AE%25E5%25AE%258C%25E6%2588%2590%25E5%259B%25BE.png)






mvn_resp： 配置本地仓库

compile  -------------------- 编译该项目

test -------------------- 

package  -------------------- 把该项目打包成 jar包

## Maven基本使用

​     在IDEA中使用Maven。

1.在pom.xml文件配置下列文件。

Alt+Insert点击Dependency -------------------- 快速导入本地仓库jar包

~~~xml
<dependencies>  <!-- dependencies标签 --> 
    <dependency>   <!-- 引入坐标 -->
        <groupId>Maven项目隶属组织名称（一般是域名反写）</groupId>
        <artifactId>Maven项目名称（从com.到要使用的类名）</artifactId>
        <version>版本号</version>
        <scope>依赖范围（compile/test/provided/runtime/system）</scope>
        <optional>true</optional> <!--可选依赖 -->
        <exclusions>  <!-- 排除依赖（可多个）-->
            <exclusion>
                <groupId>com.itheima</groupId>
                <artifactId>maven_03_pojo</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
~~~



## Maven的基本命令

Maven的基本命令：（会下载相应的插件、jar包）

* compile ：编译

* clean：清理（删除 target目录）

* test：测试

* package：打包

* install：安装（将当前项目 安装到本地仓库中）
## 依赖范围

  ​       如图所示给 `junit` 依赖通过 `scope` 标签指定依赖的作用范围。 那么这个依赖就只能作用在测试环境，其他环境下不能使用。

| **依赖范围**        | 编译classpath |      测试classpath       | 运行classpath | 例子              |
| ------------------- | :-----------: | :----------------------: | ------------- | ----------------- |
| **compile（默认）** |       Y       |            Y             | Y             | logback           |
| **test**            |       -       |            Y             | -             | Junit             |
| **provided**        |       Y       |            Y             | -             | servlet-api       |
| **runtime**         |       -       |            Y             | Y             | jdbc驱动          |
| **system**          |       Y       |            Y             | -             | 存储在本地的jar包 |
| import              |               | 引入DependencyManagement |               |                   |
|                     |               |                          |               |                   |

  

  ## Maven 生命周期

  Maven 对项目构建的生命周期划分为3套：

  * clean ：清理工作。

  * default ：核心工作，例如编译，测试，打包，安装等。

  * site ： 产生报告，发布站点等。这套声明周期一般不会使用。

    同一套生命周期内，执行后边的命令，前面的所有命令会自动执行。

    
    
    ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Maven%2520%25E7%2594%259F%25E5%2591%25BD%25E5%2591%25A8%25E6%259C%259F.png)

默认的生命周期也有对应的很 多命令，其他的一般都不会使用，我们只关注常用的：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Maven%2520%25E7%2594%259F%25E5%2591%25BD%25E5%2591%25A8%25E6%259C%259F2.png)

## IDEA使用Maven:

### IDEA中配置Maven环境：

我们需要先在IDEA中配置Maven环境：

* 选择 IDEA中 File --> Settings

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E4%25BD%25BF%25E7%2594%25A8Maven.png)

- 设置 IDEA 使用本地安装的 Maven，并修改配置文件路径

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E4%25BD%25BF%25E7%2594%25A8Maven2.png)

###  IDEA 创建 Maven项目

* 创建模块，选择Maven，点击Next

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E5%2588%259B%25E5%25BB%25BAMaven%25E9%25A1%25B9%25E7%259B%25AE.png)

- 填写模块名称，坐标信息，点击finish，创建完成

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E5%2588%259B%25E5%25BB%25BAMaven%25E9%25A1%25B9%25E7%259B%25AE2.png)

- 创建好的项目目录结构如下：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E5%2588%259B%25E5%25BB%25BAMaven%25E9%25A1%25B9%25E7%259B%25AE3.png)

### IDEA 导入 Maven项目

- 选择右侧Maven面板，点击 + 号

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E5%25AF%25BC%25E5%2585%25A5Maven%25E9%25A1%25B9%25E7%259B%25AE.png)

- 选中对应项目的pom.xml文件，双击即可

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/IDEA%25E5%25AF%25BC%25E5%2585%25A5Maven%25E9%25A1%25B9%25E7%259B%25AE2.png)



强制更新依赖快照：

```sh
 mvn clean install -U #  清除之前生成的文件，并强制从远程仓库中下载最新版本的依赖。（清除没下完的jar包）
 
 mvn dependency:resolve -U  # 更新特定的依赖项而不是所有依赖项
 
 -U ：确保 Maven 下载最新版本的依赖项，而不是使用本地已缓存的旧版本。
```





# 项目`分模块开发:`

   一个项目有多个人开发，每人开发不同的模块；而每个模块又需要合在一起使用。

 将原始模块按照功能拆分成若干个子模块，方便模块间的相互调用，接口共享。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/maven%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591.png)

### 抽取domain层:



前提：有一个maven项目

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-0.png)



1.将一个项目的domain层进行拆分。

首先在原项目的基础上，再创建一个下项目。

   ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-1.png)



2:在`maven_03_pojo`项目中创建`com.itheima.domain`包，并将`maven_02_ssm`中Book类`剪切`到该包中

  注意:剪切完后，`maven_02_ssm`项目会报（找不到Book实体类）异常，在pom.xml中添加 **新项目的依赖**即可。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-2.png)

3:建立依赖关系

在`maven_02_ssm`项目的pom.xml添加`maven_03_pojo`的依赖

```xml
<dependency><!-- 注意`maven_03_pojo`模块为com.qsl.springCloud -->
    <groupId>com.qsl</groupId>  
    <artifactId>maven_03_pojo</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

6:将新项目安装本地仓库

   将需要被依赖的项目`maven_03_pojo`，使用maven的install命令，把其安装到Maven的本地仓库中。

  不安装会爆异常。。。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-3.png)

### 抽取Dao层：

1:创建名为`maven_04_dao`的新项目，并把`maven_02_ssm`项目中的 **`Dao层`**剪切到`maven_04_dao`新项目。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-dao%25E5%25B1%2582.png)

粘贴完`maven_04_dao`的项目dao层 和`maven_02_ssm`的项目**`service层`**分别会报以下错。。。

`maven_04_dao`的项目dao层错：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-dao%25E5%25B1%2582%25E6%258A%25A5%25E9%2594%2599.png)

`maven_02_ssm`的项目**`service层`**等错:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-dao%25E5%25B1%2582%25E6%258A%25A5%25E9%2594%25992.png)

2.在`maven_04_dao`的项目添加依赖

```xml
<dependencies>
     <!-- 安装maven_03_pojo项目的依赖(主要是想引用maven_03_pojo项目中的Book)-->
    <dependency>
        <groupId>com.qsl</groupId>
        <artifactId>maven_03_pojo</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    
    <!-- 加mybatis和mysql依赖，主要是因为@Select注解等。
       注意：这里maven_02_ssm项目也有mybatis和mysql依赖，但不能替代mybatis和mysql依赖-->
     <!-- mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <!-- mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.29</version>
        </dependency>
</dependencies>

```

3.在maven_02_ssm项目中添加依赖：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2588%2586%25E6%25A8%25A1%25E5%259D%2597%25E5%25BC%2580%25E5%258F%2591-dao%25E5%25B1%2582%25E6%258A%25A5%25E9%2594%25992.png)

```xml
<dependencies>
    <!-- 安装maven_04_dao项目的依赖-->
    <dependency>
        <groupId>com.qsl</groupId>
        <artifactId>maven_04_dao</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>

```

4.使用maven的install命令，将`maven_04_dao`项目安装到本地仓库，即可运行

# 依赖管理：

​    依赖指当前项目运行所需的jar，一个项目可以设置多个依赖。

### 依赖传递冲突问题：

- 路径优先：当依赖中出现相同资源是，层级越深，优先级越低；层级越浅，优先级越高。
- 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的。
- 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的。

依赖分为：

* 依赖传递

* 可选依赖

* 排除依赖

  依赖格式为：

  ```xml
  
  <dependencies> <!--设置当前项目所依赖的所有jar-->
      <dependency>  <!--设置具体的依赖-->
          <!--依赖所属群组id-->
          <groupId>org.springframework</groupId>
          <!--依赖所属项目id-->
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>    <!--依赖版本号-->
      </dependency>
  </dependencies>
  ```
  
  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/maven%25E9%25A1%25B9%25E7%259B%25AE%25E4%25BE%259D%25E8%25B5%2596.png)

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/maven%25E9%25A1%25B9%25E7%259B%25AE%25E4%25BE%259D%25E8%25B5%25962.png)

### 可选依赖 和排除依赖：

-   可选依赖指对外隐藏当前所依赖的资源---不想给
-   排除依赖指主动断开依赖的资源，被排除的资源无需指定版本---不需要

#### 可选依赖：

   可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递。

```xml
<dependency>
    <groupId>com.itheima</groupId> 
    <artifactId>maven_03_pojo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!-- 开启可选依赖-->
    <optional>true</optional>
</dependency>
```





#### 排除依赖：

  排除依赖是隐藏当前资源对应的依赖关系

```xml
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>maven_04_dao</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!-- 排除依赖（可多个）-->
    <exclusions>
        <exclusion>
            <groupId>com.itheima</groupId>
            <artifactId>maven_03_pojo</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



# 继承和聚合：

项目打包方式：

- jar(默认): 说明该项目为java项目。
- war:  该项目为Web项目。
- pom: 该项目为集合 或继承项目（总工程）。

```xml
<packaging>jar</packaging> <!-- 总工程 -->
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%2581%259A%25E5%2590%2588%25E4%25B8%258E%25E7%25BB%25A7%25E6%2589%25BF%25E7%259A%2584%25E5%258C%25BA%25E5%2588%25AB.png)



#### 聚合：

   **聚合工程主要是用来管理项目**。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%2581%259A%25E5%2590%2588%25E5%25B7%25A5%25E7%25A8%258B.png)

1.新建一个聚合项目：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%2581%259A%25E5%2590%2588%25E9%25A1%25B9%25E7%259B%25AE.png)



2.在新项目配置pom.xml：

```xml
    <!-- 2.将项目的打包方式改为pom：pom -->
<packaging>pom</packaging> 

    <!-- 3.设置管理项目（模块）名称：
          注意：聚合工程管理的项目在进行运行的时候，会按照项目与项目之间的依赖关系来自动决定执行的顺序和配置的顺序无关-->
    <modules>
        <module>../maven_02_ssm</module>
        <module>../maven_03_pojo</module>
        <module>../maven_04_dao</module>
    </modules>
```

3.使用聚合统一管理项目

看到项目右上角有 “ root ”即为根目录

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%2581%259A%25E5%2590%2588%25E9%25A1%25B9%25E7%259B%25AE2.png)

测试发现，当`maven_01_parent`的`compile`被点击后，所有被其管理的项目都会被执行编译操作。这就是聚合工程的作用。

#### 继承：

   继承:描述的是两个工程间的关系，子工程可以继承父工程中的配置信息，常见于依赖关系的继承（主要配公共样式）。

作用：

- 简化配置
- 减少版本冲突

##### 继承的基本使用：

案例说明：

-    模块 maven_01_parent 为：父类
-    模块 maven_02_ssm等为：子类

1:创建一个空的Maven项目并将其打包方式设置为pom

​    实际开发中，聚合和继承一般都放在同一个项目中，但是这两个的功能是不一样的。

```xml
    <packaging>pom</packaging> //设置打包为pom格式
```



2:在子项目中设置其父工程maven_01_parent模块

```xml
<!--配置当前工程的jar包继承自parent模块-->
<parent>
    <groupId>com.itheima</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <!--设置父项目pom.xml位置路径-->
    <relativePath>../maven_01_parent/pom.xml</relativePath>
</parent>
```

3:在 `父类`中导入jar包

  将子项目共同使用的jar包都剪贴出来，在父项目的pom.xml中维护

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <packaging>pom</packaging>

    <!--设置管理的模块名称-->
    <modules>
        <module>../maven_02_ssm</module>
        <module>../maven_03_pojo</module>
        <module>../maven_04_dao</module>
    </modules>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.16</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.0</version>
        </dependency>
    </dependencies>
</project>
```

4.在 `子类`中继承父类的jar包

```xml
   <!-- 配置当前工程的jar包继承自parent工程-->
    <parent>
        <groupId>com.qsl</groupId>
        <artifactId>maven_01_parent</artifactId>
        <version>1.0-SNAPSHOT</version>
        <!-- 设置父项目pom.xml位置路径-->
        <relativePath>../maven_01_parent/pom.xml</relativePath>
    </parent>
```

#####  继承之 可选依赖：

1.在父类定义可选依赖

```xml
  <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.12</version>
                <scope>test</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

2.在`子类`中使用继承可选依赖

注意：不需要版本号！！！

```xml
<dependencies> 
    <!-- 继承依赖之 可选依赖（不需要版本号！！！）-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

​    dependencyManagement的优点：

1. 如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改 ；另外如果某个子项目需要另外的一个版本，只需要声明version就可。
2. dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显示的声明需要用的依赖。
3. 如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;
4. 如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。



# 属性（变量）：

### 自定义属性：

#### 属性的基本使用：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%25B1%259E%25E6%2580%25A7.png)

1.定义属性

```xml
<properties>
    <!-- 1.定义属性-->
    <spring.version>5.2.10.RELEASE</spring.version>
    <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
</properties>
```





```xml
<dependencies>
    <!-- MyBatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>

    <!-- spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <!-- 2.使用属性-->
        <version>${spring.version}</version>
    </dependency>

    <!-- springMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <!-- 2.使用属性-->
        <version>${spring.version}</version>
    </dependency>

    <!-- spring操作数据库专用 jar包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>

<!-- properties标签下 1.定义属性（名称随意）-->
<properties>
    <spring.version>5.2.10.RELEASE</spring.version>
</properties>
```



#### 配置文件加载属性（加载properties文件）：

1.`maven_01_parent`父工程定义属性 

2:jdbc.properties文件中引用属性

修改maven_02_ssm项目中jdbc.properties文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=root
```

3:设置maven过滤文件范围

  在`maven_01_parent`父工程添加

```xml
<build>
    <resources>
        <!-- 设置文件资源目录-->
        <resource>
            <directory>../maven_02_ssm/src/main/resources</directory> 
            <!-- ${project.basedir}：  当前项目所在目录,子项目继承了父项目，相当于所有的子项目都添加了资源目录的过滤 -->
   <directory>${project.basedir}/src/main/resources</directory>
            <!-- 是否开启解析${} ，默认false-->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

修改完后，注意maven_02_ssm项目的resources目录就多了些东西，并且项目的顺序会改变，如图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%258A%25A0%25E8%25BD%25BDproperties%25E6%2596%2587%25E4%25BB%25B6.png)

4.打包

注意：打包的过程中如果报如下错误

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%258A%25A0%25E8%25BD%25BDproperties%25E4%25BF%259D%25E9%2594%2599.png)

原因就是Maven发现你的项目为web项目，就会去找web项目的入口web.xml[配置文件配置的方式]，发现没有找到，就会报错。

解决方案1：在maven_02_ssm项目的`src\main\webapp\WEB-INF\`添加一个web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
</web-app>
```

2: 配置maven插件

 配置maven打包war时，忽略web.xml检查

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.2.3</version>
            <configuration>
                <failOnMissingWebXml>false</failOnMissingWebXml>
            </configuration>
        </plugin>
    </plugins>
</build>
```



### 系统属性：



### 版本管理：

jar包的版本定义:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E7%2589%2588%25E6%259C%25AC%25E7%25AE%25A1%25E7%2590%2586.png)

# 多环境配置与应用：

  maven提供配置多种环境的设定，帮助开发者在使用过程中快速切换环境。

### 多环境开发基本使用：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%25A4%259A%25E7%258E%25AF%25E5%25A2%2583%25E5%25BC%2580%25E5%258F%2591.png)

1.在父项目中配置 `解析properties文件`

```xml
<build>
    <resources>
        <resource>
            <!-- ${project.basedir}：  当前项目所在目录,子项目继承了父项目，相当于所有的子项目都添加了资源目录的过滤 -->
            <directory>${project.basedir}/src/main/resources</directory>
            <!-- 是否开启解析${}，默认false-->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

2.修改properties文件

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=123456
```

3:在父工程配置多个环境,并指定默认激活环境

```xml
<profiles>
    <!-- 定义具体的环境：开发环境-->
    <profile>
        <id>env_dep</id>
        <properties>
            <jdbc.url>jdbc:mysql://localhost:3306/dp1?useSSL=false</jdbc.url>
        </properties>
        <!-- 设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>

    <!-- 定义具体的环境：生产环境-->
    <profile>
        <id>env_pro</id>
        <!-- 定义环境中专用的属性值-->
        <properties>
            <jdbc.url>jdbc:mysql://127.1.1.1/dp1?useSSL=false</jdbc.url>
        </properties>
    </profile>

    <!-- 定义具体的环境：测试环境-->
    <profile>
        <id>env_test</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.2.2.2/dp1?useSSL=false</jdbc.url>
        </properties>
    </profile>
</profiles>
```

4:执行maven安装命令

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%2581%259A%25E5%2590%2588%25E9%25A1%25B9%25E7%259B%25AE2.png)

5.查看env_dep环境是否生效

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%25AE%2589%25E8%25A3%2585%25E9%25A1%25B9%25E7%259B%25AE%25E5%2588%25B0jar%25E5%25BA%2593.png)

#### 代码切换环境：

 在生成、开发、测试环境 切换环境

```xml
<!-- 设定是否为默认启动环境-->
<activation>
    <activeByDefault>true</activeByDefault>
</activation>  
```

#### * 命令行切换环境：

执行maven指令 切换环境

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2591%25BD%25E4%25BB%25A4%25E8%25A1%258C%25E5%2588%2587%25E6%258D%25A2%25E7%258E%25AF%25E5%25A2%2583.png)

```xml
mvn 指令 -P 环境定义ID   例：mvn install -P env_dep
```

### 跳过测试：

#### 跳过所有测试：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E8%25B7%25B3%25E8%25BF%2587%25E6%2589%2580%25E6%259C%2589%25E6%25B5%258B%25E8%25AF%2595.png)

#### 配置插件实现跳过测试:

```xml
<build>
          <!-- 跳过测试插件-->
        <plugins>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.1</version>
                <configuration>
                    <!-- 设置是否跳过测试-->
                    <skipTests>false</skipTests>

                    <!-- 排除测试用例-->
                    <excludes>
                        <exclude>**/BookServiceTest.java</exclude>
                    </excludes>
                    <!-- 包含指定的测试用例-->
                    <includes>
                        <include>**/BookServiceTest.java</include>
                    </includes>
                </configuration>
            </plugin>
        </plugins>
</build>
```



#### 命令行跳过测试：

`mvn 指令 -D skipTests`

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/%25E5%2591%25BD%25E4%25BB%25A4%25E8%25A1%258C%25E8%25B7%25B3%25E8%25BF%2587%25E6%25B5%258B%25E8%25AF%2595.png)

注意事项:

* 执行的项目构建指令必须包含测试生命周期，否则无效果。例如执行compile生命周期，不经过test生命周期。
* 该命令可以不借助IDEA，直接使用cmd命令行进行跳过测试，需要注意的是cmd要在pom.xml所在目录下进行执行。

# 私服：

私服:公司内部搭建的用于存储Maven资源的服务器.

远程仓库:Maven开发团队维护的用于存储Maven资源的服务器.

目的:私服是一台独立的服务器，用于解决团队内部的资源共享与资源同步问题.

### Nexus

* Sonatype公司的一款maven私服产品
* 下载地址：https://help.sonatype.com/repomanager3/download
* 端口：8081

#### 安装&启动Nexus

1.解压`latest-win64.zip`既安装，解压得到以下两个目录

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%25AE%2589%25E8%25A3%2585Nexus.png)

2.使用cmd进入到解压目录下的`nexus-3.30.1-01\bin`,执行如下命令:

```
nexus.exe /run nexus   // 启动Nexus
```

启动成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%2590%25AF%25E5%258A%25A8Nexus.png)

3:浏览器访问

```
http://localhost:8081   //访问地址
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%25AE%2589%25E8%25A3%2585Nexus2.png)

4:首次登录重置密码

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%25AE%2589%25E8%25A3%2585Nexus3.png)

5.点击下一步，需要重新输入新密码

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%25AE%2589%25E8%25A3%2585Nexus4.png)

6.设置是否运行匿名访问,点击下一步就完成了

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E5%25AE%2589%25E8%25A3%2585Nexus5.png)

其他基础配置信息：

- 修改基础配置信息
  - 安装路径下etc目录中nexus-default.properties文件保存有nexus基础配置信息，例如默认访问端口。
- 修改服务器运行配置信息
  - 安装路径下bin目录中nexus.vmoptions文件保存有nexus服务器启动对应的配置信息，例如默认占用内存空间。

#### 私服仓库分类：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E4%25BB%2593%25E5%25BA%2593%25E7%25B1%25BB%25E5%2588%25AB.png)

宿主仓库hosted 

- 保存无法从中央仓库获取的资源
  - 自主研发
  - 第三方非开源项目,比如Oracle,因为是付费产品，所以中央仓库没有

代理仓库proxy 

- 代理远程仓库，通过nexus访问其他公共仓库，例如中央仓库

仓库组group 

- 将若干个仓库组成一个群组，简化配置
- 仓库组不能保存资源，属于设计型仓库



##### 本地仓库访问私服：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E6%259C%25AC%25E5%259C%25B0%25E4%25BB%2593%25E5%25BA%2593%25E8%25AE%25BF%25E9%2597%25AE%25E7%25A7%2581%25E6%259C%258D.png)

###### 1.私服创建仓库

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/%25E7%25A7%2581%25E6%259C%258D%25E5%2588%259B%25E5%25BB%25BA%25E4%25BB%2593%25E5%25BA%2593.png)

分别创建了itheima-snapshot仓库 和itheima-release仓库。

###### 2:配置本地Maven对私服的访问权限

在`maven/conf`下的settings.xml配置

```xml
<servers>
    <!-- 配置访问私服的权限-->
    <server>
        <!-- 私服中服务器id名称(ID对应Nexus的Name) -->
        <id>itheima-snapshot</id> 
        <username>admin</username>
        <password>admin</password>
    </server>
    <server>
        <id>itheima-release</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```

###### 3:配置私服的访问路径

```xml
<mirrors> <!-- 映射 -->
    <!-- 配置私服的访问路径 -->
    <mirror>
        <!--配置仓库组的ID (ID对应Nexus的Name)-->
        <id>maven-public</id>
        <!--*代表所有内容都从私服获取-->
        <mirrorOf>*</mirrorOf>
        <!--私服仓库组maven-public的访问路径-->
        <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
</mirrors>
```

注意：为了避免阿里云Maven私服地址的影响，建议先将之前配置的阿里云Maven私服镜像地址注释掉，等练习完后，再将其恢复。

#####  私服资源上传与下载：

###### 1:配置工程上传私服的具体位置:

在IDEA配置依赖

```xml
// 修改version就可在仓库切换（qsl-release，qsl-snapshot）
<version>1.0-RELEASE</version> <!-- 设置快照版本 或发布版本 -->

<!--配置当前工程保存在私服中的具体位置-->
    <distributionManagement>
        <repository>
            <id>qsl-release</id>    <!-- 发布版本 -->
            <!--release版本上传仓库的具体地址-->
            <url>http://localhost:8081/repository/qsl-release/</url>
        </repository>

        <snapshotRepository>
            <id>qsl-snapshot</id>   <!-- 快照版本-->
            <!--snapshot版本上传仓库的具体地址-->
            <url>http://localhost:8081/repository/qsl-snapshot/</url>
        </snapshotRepository>
    </distributionManagement>
```

###### 2.发布资源到私服

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/IDEA%25E4%25B8%258A%25E4%25BC%25A0%25E7%25A7%2581%25E6%259C%258D.png)

或执行Maven命令

```xml
mvn deploy
```

**注意:**

要发布的项目都需要配置`distributionManagement`标签，要么在自己的pom.xml中配置，要么在其父项目中配置，然后子项目中继承父项目即可。

发布成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/IDEA%25E4%25B8%258A%25E4%25BC%25A0%25E7%25A7%2581%25E6%259C%258D%25E6%2588%2590%25E5%258A%259F.png)

#### 配置中央仓库:

如果私服中没有对应的jar，会去中央仓库下载，速度很慢。可以配置让私服去阿里云中下载依赖。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Maven/Nexus/1630993028454.png)


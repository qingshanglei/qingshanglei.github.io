# javaWeb

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/java%20wed%E7%9B%AE%E5%BD%95.png)



B/S架构(Browser/Server): 浏览器/服务器架构模式。

   特点：客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端；浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可。



## Java Maven项目目录：

1. mapper  -------------

2. pojo   -------------

3. service  -------------

4. util   -------------

5. web   ------------- 

   

     表现层：servlet,SpringMVC

     业务层:

     数据层：JDBC,MyBatis/
   
     ----------,----------
   
   Dao层（mapper层，持久层，数据访问层）， 
   
   Service层（业务层，biz层） 
   
   Controller层（控制层，action层）
   
   Entity层（实体层，domain，pojo ,po）
   
   ## JavaWeb项目的**标准项目结构**:
   
   ```
   web项目/module
     --src
       -基本包名(比如com.gx)
          servlet/web/controller 放 servlet
          service/biz 放服务层 的接口
             impl 服务接口的实现类
          dao/mapper 数据库操作接口
             impl 数据库操作接口的实现类
          po/pojo/entity/entities/domain/javaBean 数据的实体类，一般和数据库的表对应
          vo/viewObject/valueObject  视图层，业务层之间的数据传递，多表联查（查询等数据封装）
          dto 前台传到后台的数据类
          common 放公共的类
          filter 过滤器
          listener 监听器
          util 放工具类
          api 放返回给前台服务调用的接口
     --web/webapp/webRoot
          WEB-INF   -- 受保护的目录（在浏览器中无法访问该目录的内容）
             web.xml  JavaWeb项目的核心配置文件
             lib       存放项目所需的jar包
          static/css/js/image  静态资源目录
          jsp  存放jsp文件
          *.jsp jsp文件
   ```
   
   
   
   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/JavaEE%E4%B8%89%E5%B1%82%E6%9E%B6%E6%9E%84.png)
   
   
   
    **JavaEE**: Java Enterprise Edition,Java企业版。指Java企业级开发的技术规范总和。包含13项技术规范:JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF。
   
   ## 导入驱动包
   
   将mysql的驱动包放在模块下的lib目录（随意命名）下，并将该jar包添加为库文件
   
   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/JDBC/%E9%A1%B9%E7%9B%AE%E5%AF%BC%E5%85%A5%E9%A9%B1%E5%8A%A8%E5%8C%85.png)
   
* 在添加为库文件的时候，有如下三个选项
     * Global Library  ： 全局有效
     * Project Library :   项目有效
     * Module Library ： 模块有效
     
      
   
   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/JDBC/jbr%E6%9C%89%E6%95%88%E8%8C%83%E5%9B%B4.png)



# Java Web三大组件

**Java Web三大组件：Servlet、Filter、Listener**

## Servlet

### Servlet基本使用:

   1.在 pox.xml引入Servlet依赖坐标（ Servlet jar包）, 或在lib里添加jar包

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope> 
</dependency>
```

####  ①注解配置（3.0+，推荐）

~~~java
@WebServlet("/demo10")
public class ServletDemo10 extends HttpServlet {
    @Override
    protect ed void doGet(ServletRequest req, ServletResponse res) {
        System.out.println("doGet... 注解配置");
    }
}
~~~

#### ②XML配置

~~~java
public class ServletDemo10 extends HttpServlet {
    @Override
    protect ed void doGet(ServletRequest req, ServletResponse res) {
        System.out.println("doGet... XML配置");
    }
}
~~~

~~~xml
<servlet>
    <servlet-name>demo10</servlet-name> <!-- 自命名-->
    <servlet-class>com.qsl.ServletDemo10</servlet-class><!--配置全类目（要使用的路径）-->
</servlet>

<servlet-mapping>
    <servlet-name>demo10</servlet-name> <!-- 填写刚刚的自命名-->
    <url-pattern>/demo10</url-pattern> <!-- 访问路径-->
</servlet-mapping>
~~~



### 继承HttpServlet类



~~~java
init() -------------------- 初始化方法
destroy() --------------------销毁方法    
doGet(HttpServletRequest req, HttpServletResponse resp) -------------------- get请求（默认）
doPost(HttpServletRequest req, HttpServletResponse resp) -------------------- Post请求
    
从路径中获取参数：
  req.getParameter(String str) --------------------根据String获取一个String参数值
  req.getParameterValues(String str) --------------------获取同一个参数名的多个值（数组）
  req.getParameterNames() -------------------- 返回一个包含请求消息中所有参数名的 Enumeration 对象，在此基础上，可以对请求消息中的所有参数进行遍历处理。
     hasMoreElements() --------------------判断是否有更多参数
     nextElement() --------------------获取参数
  req.getParameterMap() --------------------获取所有参数名和值的 Map 对象  
 
向页面返回数据：
  resp
       setContentType(String varl) --------------------修改页面编码格式
            (参数 1：text/plain;charset=UTF-8（修改为文本UTF-8格式）；
                  2：text/html;charset=UTF-8（修改为网页UTF-8格式）    )
       getOutputStream() --------------------获取一个 字节流
       getWriter() --------------------获取一个 字符流
    
~~~





### Request：使用request对象来**获取**请求数据

#### Request获取请求数据



1.  请求行 :        GET/request-demo/req1?username=zhangsan Http/1.1  

~~~java
    getMethod() --------------------获取请求方式：get
    getContextPath() --------------------获取虚拟目录（项目访问路径） /request-demo
    StringBuffer getRequestURL() --------------------获取URL（统一资源定位符） http://localhost:8080/request-demo/req1   
    getRequestURL() --------------------获取URL(统一资源标识符) /request-demo/req1
    getQueryString() --------------------获取请求参数（get方式） username=zhangsan&password=123  
~~~

2.  请求头：User-Agent: Mozilla/5.0Chrome/91.0.4472.106

~~~java
 getHeader(String name) --------------------根据请求头数据，获取值
~~~

3. 请求行：  username=superbaby&password=123

   ~~~java
     ServletInputStream getInputSTream()  --------------------获取字节输入流
     BuffereReader getReader() --------------------获取字符输入流
   ~~~

   #### Request通用方式获取请求参数

   

   ~~~java
   req
      Map<String,String[]> getParameterMap() --------------------获取所有参数Map集合
      getParameterValues(String name) --------------------根据名称（key）获取参数值（数组）
      getParameter() --------------------根据名称获取参数值（单个值,不能接受JSON数据）
   ~~~

   #### 请求转发（forward，跳转页面）：

~~~
 请求转发（forward）：一种在服务器内部的资源跳转方式
   ​     请求转发特点：
   ​             1.浏览器地址路径不发生变化
   ​             2.只能转发到当前服务器的内部资源
   ​             3.一次请求，可以在转发的资源间使用request共享数据
   
   
   // 跳转到请求转发页面
        request.getRequestDispatcher("/要添加页面的路径").forward(request,response);
   
   // 请求转发资源间共享数据：使用Request对象
        setAttribute(String name,Object o) --------------------根据键值对，存储数据到request域中    
        getAttribute(String name) --------------------根据key,获取存储的值
        removeAttribute(String name) --------------------根据key,删除存储该键的，键值对
   
~~~

### Response：使用response对象来**设置**响应数据

路径问题：

1. ​       浏览器使用：需要加虚拟目录（项目名）
2. ​       服务器使用：不需要加虚拟目录

####  Response设置响应数据功能介绍

~~~java
  1.响应行： HTTP/1.1 200 ok
    setStatus(int sc)  --------------------设置响应状态码
      
  2.响应头： Content-Type: text/html
    setHeader(String name String value) -------------------设置响应头键值对
      
  3.响应体： <html> <body> </body> </html>
      PrintWriter getWriter() --------------------获取字符输入流
      ServletOutputStream getOutputStream() --------------------获取字节输入流
      
~~~



#### Response 完成重定向

特点：

​      1.浏览器地址栏路径反生变化

​      2.可以重定向到任意位置的资源（服务器内部、外部都行）

​      3.两次请求，不能在多个资源使用request共享数据

~~~java
// ===== 简化前
        // 设置状态码 302
        resp.setStatus(302);
        // 设置响应头 Location
        resp.setHeader("location","/项目名/要跳转的路径");


// ===== 简化后  重定向(推荐)
        resp.sendRedirect("/项目名/要跳转的路径");

// ===== 动态获取重定向(推荐)
        String contextPath = req.getContextPath();
        resp.sendRedirect(contextPath + "/resp2");
~~~

#### Response 响应字符数据

~~~java
  // 设置他的响应格式及 字符集
  resp.setContentType("text/html;charset=utf-8"); //设置为html 和编码格式 (推荐)
  resp.setHeader("content-type","text/html"); // 设置为html
 
//1. 通过Response对象获取字符输出流
      PrintWriter respWriter=resp.getWriter();

//2. 写数据（基本数据 和html标签的都行）
     respWriter.writer("aaa");

注意：①该流自动关闭  ②response获取的字符输出流默认编码：ISO-8859-1
~~~

#### Response响应字节数据

~~~java
// 1. 读取文件
        FileInputStream fls = new FileInputStream("D:\\电脑常用链接\\图标\\壁\\zmx3yg.png");

        // 2. 通过Response对象获取字符输出流
        ServletOutputStream os = response.getOutputStream();
        // 3. 写数据
        /** 没使用IOUtils工具类 */
        /*byte[] bytes = new byte[1024];
        int len = 0;
        while ((len = fls.read(bytes)) != -1) {
            os.write(bytes, 0, len);
        }*/

        /** 使用IOUtils工具类*/
        // IOUtils.copy(输入流，输出流);
        IOUtils.copy(fls, os);

        // 释放资源
        fls.close();

~~~

## Filter(过滤器)

过滤器可以把对资源的请求拦截下来，从而实现**权限控制，统一编码处理，敏感字符处理**。。。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/1.png)

###  Filter基本使用：

1.定义类，实现javax.servlet包下的Filter接口，并重写其所有方法

~~~java
public class filterDemo implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    @Override
    public void destroy() {
    }
}

~~~

2.配置Filter拦截资源的路径：在类上定义@WebFilter注解

~~~java
@WebFilter("/*")
public class filterDemo implements Filter {  }
~~~

3.在doFilter方法中输出一句话，并放行

~~~java
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("1.doFilter...");

        //放行(让其访问本该的资源、路径)
        filterChain.doFilter(servletRequest, servletResponse)
          
       System.out.println("3.doFilter...");
    }
~~~

### Filter执行流程：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/10.png)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/11.png)

~~~java
   @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //1.放行前，对request数据进行处理
        System.out.println("1.doFilter...");

        //放行所拦截的路径
        filterChain.doFilter(servletRequest, servletResponse)
          
        //2.放行后，对response数据进行处理
        System.out.println("3.doFilter...");
    }
~~~

###  **Filter拦截路径配置**

使用 @WebFilter 注解进行配置。如： @WebFilter("拦截路径")

拦截路径有如下四种配置方式：

1. 拦截具体的资源： /index.jsp：  只有访问index.jsp时才会被拦截
2. 目录拦截                /user/*：        访问/user下的所有资源，都会被拦截
3. 后缀名拦截：       *.jsp：             访问后缀名为jsp的资源，都会被拦截
4. 拦截所有：            /*：                 访问所有资源，都会被拦截

###  过滤器链

过滤器链是指在一个Web应用，可以配置多个过滤器，这多个过滤器称为过滤器链。

过滤器的**优先级**是按照**过滤器类名(字符串)的自然排序**。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/15.png)



代码演示：

第一个过滤器：

~~~java
@WebFilter("/*") public class FilterDemo implements Filter {
    @Override public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException { 
        //1. 放行前，对 request数据进行处理    
           System.out.println("1.FilterDemo...");
        //放行
          chain.doFilter(request,response); 
        //2. 放行后，对Response 数据进行处理 
        System.out.println("3.FilterDemo..."); 
    }
    @Override public void init(FilterConfig filterConfig) throws ServletException { }
    @Override public void destroy() { } }
~~~

第二个过滤器：

~~~java
@WebFilter("/*") public class FilterDemo2 implements Filter { 
    @Override public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException { 
        //1. 放行前，对 request数据进行处理 
        System.out.println("2.FilterDemo..."); 
        //放行 
        chain.doFilter(request,response);
        //2. 放行后，对Response 数据进行处理 
        System.out.println("4.FilterDemo..."); 
    }
    @Override public void init(FilterConfig filterConfig) throws ServletException { }
    @Override public void destroy() { } }

~~~

修改 hello.jsp 页面中脚本的输出语句

~~~html
<%@ page contentType="text/html;charset=UTF-8" language="java" %> <html>
<head>
<title>Title</title> 
</head> 
<body>
    <h1>hello JSP~</h1>
      <% System.out.println("3.hello jsp"); %>
 </body>
 </html>
~~~

结果：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/16.png)

## Listener(监听器)

​     监听器可以监听就是在 application ， session ， request 三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件。

​    application 是 ServletContext 类型的对象。ServletContext 代表整个web应用，在服务器启动的时候，tomcat会自动创建该对象。在服务器关闭时会自动销毁该对象。

### 监听器分类（8个监听器）

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/20.png)

ServletContextListener 是用来监听ServletContext 对象的创建和销毁。

~~~java
 ServletContextListener 接口中有以下两个方法:
   void contextInitialized(ServletContextEvent sce) ：ServletContext 对象被创建了会自动执行的方法
   void contextDestroyed(ServletContextEvent sce) ： ServletContext 对象被销毁时会自动执行的方法
~~~

### **代码演示（ServletContextListener 监听器）**

1. 定义一个类，实现 ServletContextListener 接口
2. 重写所有的抽象方法
3. 使用 @WebListener 进行配置

~~~java
@WebListener 
public class ContextLoaderListener implements ServletContextListener {
    @Override public void contextInitialized(ServletContextEvent sce) {          //加载资源 
        System.out.println("ContextLoaderListener...");
    } 
    @Override public void contextDestroyed(ServletContextEvent sce) {        //释放资源 
    } 
}
~~~

启动服务器，就可以在启动的日志信息中看到 contextInitialized() 方法输出的内容，同时也说明了 ServletContext对象在服务器启动的时候被创建了。

# JDBC

​        JDBC   就是使用Java语言操作关系型数据库的一套API，全称：( Java DataBase Connectivity ) Java 数据库连接。

## JDBC本质

* 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口
* 各个数据库厂商去实现这套接口，提供数据库驱动jar包
* 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

## JDBC的基本使用

1.**在pom.xml引入jar**   或在lib中导入jar包，添加Add as Library...

~~~xml
<dependency> <!-- mysql驱动-->
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.29</version>
</dependency>
~~~

~~~java

1. 注册驱动,加载对应的Driver类 (mysql 5+的驱动包，可以不写)
    Class.forName("com.mysql.cj.jdbc.Driver")；
      /* 5.X驱动： com.mysql.jdbc.Driver*/    
      /* 8.x驱动 com.mysql.cj.jdbc.Driver*/
 2. 获取连接
     String url="jdbc:mysql://127.0.0.1/数据库名"; ---方法1
     String url = "jdbc:mysql://localhost:3306/数据库名?useSSL=false"; ---方法2
   Connection conn = DriverManager.getConnection(String url,String user,String passwerd)
 3.获取SQL语句
    String sql="sql语句"
 4.获取执行SQL对象
     Statement stmt =conn.createStatement();
 5.执行SQL
     executeQuery(sql) -------------查询     
     executeUpdate(sql) -------------修改
 6.处理返回结果
 7.释放资源
~~~

## JDBC APL详解

### DriverManager

~~~java
1.注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver")；(mysql 5+的驱动包，可以不写) 
2.获取数据库连接 (如果连接的是本机mysql并且端口是默认的3306，可以不写; useSSL=false (禁用安全连接方式，解决)  
    String url = "jdbc:mysql:///qsl?useSSL=false";
    DriverManager.getConnection(url, userName, passwerd);
~~~

### Connection接口

Connection（数据库连接对象）作用：

​     1.获取执行SQL对象

​     2.管理事务

~~~java
 1.获取执行SQL对象
   ①.获取执行SQL的对象: (普通执行SQL对象)
      Statement createStatement()
   ②.预编译SQL的执行对象：(防止SQL注入)
      PreparedStatement prepareStatement(sql)
   ③.执行存储过程的对象(不常用)
      CallableStatement prepareCall(sql)

2.JDBC事务管理：
    开启事务：setAutoCommit(boolean autoCommit) :true为自动提交事务；false为手动提交事务，既为开启事务
    提交事务：commit()
    回滚事务：rollback()
          
   ####   MySQL事务管理

* 开启事务 ： BEGIN; 或者 START TRANSACTION;
* 提交事务 ： COMMIT;
* 回滚事务 ： ROLLBACK;
~~~

### Statement

Statement对象的作用就是用来执行SQL语句。

~~~java
 Statement的作用:
   执行SQL语句：
       int executeUpdate(sql)： 执行DML、DDL语句
           返回值：①DML语句影响的行数 ②DDL语句执行后，执行成功也可能返回0
       
       ResultSet executeQuery(sql): 执行DQL语句
           返回值： ResultSet 结果集对象
~~~

### ResultSet

ResultSet（结果集对象）作用：  封装了SQL查询语句的结果。

~~~java
 ResultSet(结果集对象)作用：
     1.封装了DQL查询语句的结果
        ResultSet ececulteQuery():执行DQL语句，返回ResultSet对象
     
     next():判断当前行是否有数据
     getXxx():根据列数或列名，获取数据
       Xxx：数据类型；如 getInt(参数);   getString(参数);

~~~

### PreparedStatement接口

PreparedStatement作用：

* 预编译SQL语句并执行：预防SQL注入问题

~~~java
 useServerPrepStmts=true -------------开启预编译（默认关闭）

作用：
       1.预编译SQL语句并执行：预防SQL注入问题
          ① 获取PreparedStatement对象 
            SQL语句的参数值，使用？占位符替代
              String sql = "select * from user where username=? and password = ?";
             
            通过Connection对象获取，并传入对应的sql语句
                PrepareStatement pstemt=conn.prepareStatement(sql);
          ② 设置参数值
             setXxx(参数1，参数2)：------------- 给？赋值
                参数1：？的位置编号，从1开始
                参数2：？的值
          ③ 执行SQL
            executeUpdate(); / executeQuery();  // 不需要在传递sql

~~~

## 数据库连接池

* **数据库连接池**是个容器，负责分配、管理数据库连接(Connection)

* 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；

* 释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏
* 好处
  * 资源重用
  * 提升系统响应速度
  * 避免数据库连接遗漏

### 数据库实现连接池

标准接口：DataSource

​       官方（SUN）提供的数据库连接池标准接口，由第三方组织实现此接口

​      功能：获取连接

 ~~~java
Connection getConnection()
 ~~~

常见的数据库连接池： ①：DBCP     ②：C3P0      ③：Druid

### Druid(德鲁伊)

阿里巴巴开源的数据库连接池

使用步骤：

1.导入jar包druid-1.1.12jar
2.定义配置文件
3.加载配置文件

4.获取数据库连接池对象

5.获取连接

#### Driud的基本使用：

1.在pom.xml中导入    或导入jar包 druid-1.1.12.jar

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```



2.定义配置文件

~~~properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///db1?useSSL=false&useServerPrepStmts=true
username=root
password=1234
# 初始化连接数量
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
~~~

3.使用druid的代码如下：

~~~java
/**
 * Druid数据库连接池演示
 */
public class DruidDemo {

    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.定义配置文件
        //3. 加载配置文件
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
        //4. 获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        //5. 获取数据库连接 Connection
        Connection connection = dataSource.getConnection();
        System.out.println(connection); //获取到了连接后就可以继续做其他操作了

        //System.out.println(System.getProperty("user.dir"));
    }
}
~~~







# Tomcat

1. Web服务器的作用

> 封装HTTP协议操作，简化开发
>
> 可以将Web项目部署到服务器中，对外提供网上浏览服务

2. Tomcat是一个轻量级的Web服务器，支持Servlet/JSP少量JavaEE规范，也称为Web容器，Servlet容器。

Web服务器是安装在服务器端的一款软件，将来我们把自己写的Web项目部署到Web Tomcat服务器软件中，当Web服务器软件启动后，部署在Web服务器软件中的页面就可以直接通过浏览器来访问了。



shutdown.bat   -------------------- 强制关闭 

### 软件安装&卸载：

**安装：**

tomcat是绿色版软件,解压即安装。

​     bin:目录下有两类文件，一种是以`.bat`结尾的，是Windows系统的可执行文件，一种是以`.sh`结尾的，是Linux系统的可执行文件。

​     webapps:就是以后项目部署的目录。

1. 在要安装Tomcat的目录下解压 apache-tomcat-9.0.63
2. 在 conf目录下的 server.xml文件上，找到8080端口，并加上URIEncoding="UTF-8"
3. 在 bin目录下， 双击 startup.bat启动

**卸载**： 直接删除解压安装包的目录就好了。

## IDEA中使用Tomcat

### 1、集成本地Tomcat

项目配置(引用jar包)：

1.点击Add Configurations，点击+号，找到并点击Tomcat Server 下的 Local

2.点击Configure,指定安装Tomcat的具体路径

3.将开发项目部署项目到Tomcat中

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Tomcat/IDEA%E4%BD%BF%E7%94%A8Tomcat.png)

### Tomcat Maven插件

在pom.xml配置jar包（引入Tomcat插件），就完成了

~~~xml
 <!--  box.xml-->
<build>
        <plugins> <!-- Tomcat插件-->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
            </plugin>
        </plugins>
    </build>
~~~





# JSP

JSP:（JSP=HTML+Java）

概念：java Server Pager , java服务端页面   （JSP本质上就是一个Servlet）

一种动态的页面技术，其中既可以定义 HTML、JS、CSS等**静态**内容，还可以定义java代码的**动态**内容

## JSP的基本使用

前提： 创建一个java Maven项目,并把 Tomcat 装上

1. 在java Maven项目的 pox.xml文件配置 JSP

   ~~~xml
   <dependency> <!-- JSP jar包-->
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
        <scope>provided</scope>
   </dependency>
   ~~~

2. 在wep app目录下创建JSP文件，并编写代码

   ~~~jsp
   <body>
       <h1>hello javaweb  JSP</h1>
       <%
         System.out.println("hello JSP");
       %>
   </body>
   ~~~

3. 使用 Tomcat启动新创建的JSP文件

### JSP脚本

JSP脚本用于在JSP页面内定义Java 代码

JSP脚本分类：

 ~~~jsp
 <!-- 1.内容会直接放导 _jspService()方法之中 -->
  <% 代码块 %>
 <!-- 2.内容会放到 out.print()中，作为 out.print()的参数 -->
 <%= 代码块 %>
 <!-- 3.内容会放到 _jspService()方法之外，被类直接包含 -->
 <%! 代码块 %>
 ~~~

## EL表达式

Expression Language表达式语言，用于简化 JSP页面内的java代码

主要功能：获取数据

~~~jsp
<%@ page isELIgnored="false" %>  ----------------需要在JSP文件头上加上，不然jsp会忽略EL表达式

${ expression }  ----------------获取域中存储的key数据（expression：表达式）
${ cookie.key.value} ----------------根据cookie中的key键名称获取值
~~~

Java Web中四大域对象：（el表达式会依次获取数据）

1. page:   当前页面有效

2. request:  当前请求页面有效

3. session： 当前会话页面有效

4. application: 当前应用有效

   ##  JSTL标签

   JSP标准标签库（Jsp Standarded Tag Library）,使用标签取代JSP页面上的java代码
   
   #### JSTL标签库
   
   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/JSP/JSP%E4%B8%8B%E7%9A%84JSTL%E6%A0%87%E7%AD%BE.png)
   
   #### JSTL快速入门：

1.在 pox.xml文件下导入该jar包

注意：standard会报这个错：不用就好了（org.apache.catalina.startup.TaglibUriRule body
信息: TLD skipped. URI: http://java.sun.com/jstl/core_rt is already defined）

~~~xml
<dependencies>
    <dependency> <!-- jstl标签库 jar包 -->
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    
    <dependency>  <!-- jstl标签库 jar包 -->
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency> 
</dependencies>
~~~

2. 在JSP页面上引入JSTL标签库

~~~jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
~~~

3.在JSP页面的<body>标签下，使用

~~~jsp
// ============  <c:if test=" 表达式 ">  </c:if>
<%-- c:if  ==> test:来完成逻辑判断，替换java if else --%>
<c:if test=" 表达式 ">
   代码块
</c:if>

<c:if test="false/true">
    <h1>张灵玉</h1>
</c:if>

// ============ <c:forEach items="被遍历的容器（集合等）" var="自命名"></c:forEach>
<c:forEach items="${brands}" var="brand" varStatus="status">
    <!-- varStatus="status": 开启排序  index（从零开始排序）；  count（从1开始排序）  --> 
    <td>${status.index}</td> 
    <%-- ${brand.id}  到brands集合解析成 Id，再在前面拼接 get，然后调用此方法  --%>
    <td>${brand.id}</td>
</c:forEach>


普通forEach循环： （begin:开始数；  end:结束数； step:步长，每次增长1；  类似for循环）
<c:forEach begin="0" end="10" step="1" var="自命名" >
    ${自命名}
</c:forEach>
~~~

##### forEach标签

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/JSP/forEach%E6%A0%87%E7%AD%BE.png)







# IOUtils工具类使用

快速使用字节集

~~~xml

<!-- IOUtils工具类使用 -->
  // 1. 导入坐标
    <dependencies>
        <dependency>  <!-- IOUtils工具类： 快速使用字节流的工具-->
            <groupId>commons-io</groupId>  <!-- -->
            <artifactId>commons-io</artifactId>  <!-- -->
            <version>2.6</version>  <!-- 版本-->
        </dependency>
    </dependencies>
~~~

2.使用

~~~java

IOUtils.copy(输入流，输出流);
~~~

# 会话跟踪技术

​          会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含**多次**请求和响应

​         会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间**共享数据**

​          HTTP协议是无状态的。

##           Cookie (客户端会话跟踪技术)

Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问

## Cookie基本使用

### 发送Cookie

1.创建Cookie对象，设置数据

```java
Cookie cookie = new Cookie(String key,String value);
```

2. 发送Cookie到客户端：使用 response对象

```Servlet
response.addCookie(cookie);
```

### 获取Cookie

1. ​     使用request对象, 获取客户端携带的所有Cookie

   ```java
   Cookie[] cookies = request.getCookies();
   ```

2. 遍历数组，获取每一个Cookie对象：for

3. 使用Cookie对象方法获取数据

   ```
   cookie.getName();
   cookie.getValue();
   ```
   
   
   
   在jsp页面获取cookie值
   
   ~~~jsp
   ${ cookie.key.value} ----------------根据cookie中的key键名称获取值
   ~~~
   
   

## Cookie原理：

Cookie的实现是基于HTTP协议的

1. ​       响应头：set-cookie
2. ​       请求头：cookie

## Cookie使用细节：

### Cookie存活时间：

​         默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁

~~~java
         setMaxAge(int seconds): //设置Cookie存活时间
~~~
1.  正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
2.  负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
3.  零： 删除对应Cookie


### Cookie存储中文

默认不支持存储中文，如需存储则要进行转码：URL编码

~~~java
 
// URL编码 &解码       enc：要转换的编码格式（UTF-8,GBK等）
      URLEncoder.encode(String s, String enc); 
~~~



##           Session(服务端会话跟踪技术)

服务端会话跟踪技术:将数据保存到服务端

javaEE 提供HttpSession接口，来实现一次会话的多次请求间数据共享功能

注意：Session 是基于Cookie实现的 

#### Session钝化、活化

钝化：服务器在正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中 。  

活化：再次启动服务器后，从文件中加载数据到Session 中。

## Session的基本使用：

~~~java

// 1.获取Session对象
HttpSession session = request.getSession();

 session
   setAttribute(String name,Object o) --------------------  存储数据到Session7域中
   getAttribute(String name) -------------------- 根据key，获取值
   removeAttribute(String name) -------------------- 根据key,删除该键值对

   invalidate() --------------------Session销毁（方法2，退出键使用）
~~~

#### Session销毁 (默认30分钟自动销毁，方法1)

~~~xml
// 在web.xml配置

<session-config>
  <session-timeout>30</session-timeout>  <!-- 设置时间为分钟-->
</session-config>

~~~

Cookie和Session的区别：

| 存储位置：  | Cookie是将数据存储在客户端； | Session将数据存储在服务端 |
| :---------- | :--------------------------- | ------------------------- |
| 安全性：    | Cookie不安全；               | Session安全               |
| 数据大小:   | Cookie最大3KB；              | Session无限制             |
| 存储时间:   | Cookie可以长期存储；         | Session默认30分钟         |
| 服务器性能: | Cookie不占服务器资源；       | Session占用服务器资源     |

Translation ----IDEA翻译插件

# Ajax

​     AJAX **(Asynchronous JavaScript And XML)：异步的JavaScript和 XML。（ XML 是指以此进行数据交换 ）**

   

   作用：

1.   **与服务器进行数据交换**：通过AJAX可以给服务器发送请求，服务器将数据直接响应回给浏览器。

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/21.png)

2. **异步交互**：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术，如：搜索联想、用户名是否可用校验，等 

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/22.png)



**同步和异步：**

1. ​     同步发送：

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/25.png)

   ​     浏览器页面在发送请求给服务器，在服务器处理请求的过程中，浏览器页面不能做其他的操作。只能等到服务器响应结束后才能，浏览器页面才能继续做其他的操作。

2. ​     异步发送:

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Filter/26.png)

​    浏览器页面发送请求给服务器，在服务器处理请求的过程中，浏览器页面还可以做其他的操作。

### 基本使用：

1. 服务器代码：

​     在项目创建 com.itheima.web.servlet ，并在该包下创建名为 AjaxServlet 的servlet

 ~~~java
@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    @Override protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {        //1. 响应数据 
        response.getWriter().write("hello ajax~"); 
    }
    @Override protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 
        this.doGet(request, response);
    } 
}
 ~~~

1. 客户端代码：

​     在 webapp 下创建名为 01-ajax-demo1.html 的页面，在该页面书写 ajax 代码

~~~html
<!DOCTYPE html>
<html lang="en">
    <head><meta charset="UTF-8">
        <title>Title</title> 
    </head>
    <body> 
        <script>
            //1. 创建核心对象 
            var xhttp;
            if (window.XMLHttpRequest) 
            {
                xhttp = new XMLHttpRequest(); 
            } else { 
                // code for IE6, IE5 
                xhttp = new ActiveXObject("Microsoft.XMLHTTP");
            } 

            //2. 发送请求
            xhttp.open("GET", "http://localhost:8080/ajax-demo/ajaxServlet");
            xhttp.send(); 

            //3. 获取响应 
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) { 
                    alert(this.responseText); 
                } }; 
        </script> 
    </body> 
</html>
~~~

## axios

Axios 对原生的AJAX进行封装，简化书写。

### axios的基本使用：

1. 引入 axios 的 js 文件

   ~~~html
     <script src="js/axios-0.18.0.js"></script>
   ~~~

2. 使用axios 发送请求，并获取响应结果

   ​    **method** 属性用来设置请求方式的。取值为 get 或者 post 。 

   ​    **url **属性：用来书写请求的资源路径。如果是 get 请求，需要将请求参数拼接到路径的后面，格式为： url?参数名=参数值&参数名2=参数值2 。 

      **data** 属性：作为请求体被发送的数据。也就是说如果是 post 请求的话，数据需要作为 data 属性的值。

3. - 发送 get 请求

     ~~~html
     <script>
         axios({ 
             method:"get", 
             url:"http://localhost:8080/ajax-demo1/aJAXDemo1?username=zhangsan" }).then(function (resp){ 
             alert(resp.data); 
         })
     
         // 简写后：
         axios.get("http://localhost:8080/ajax-demo/axiosServlet?username=zhangsan").then(function (resp) {
             alert(resp.data); 
         });
     
            // 简写后2：  ES6
      axios.get("http://localhost:8080/brand-demo/selectAllServlet").then((res)=>{
             console.log(res)
         })
     </script>
     ~~~
   
   - 发送 post 请求

~~~html
<script>
    axios({ 
        method:"post", 
        url:"http://localhost:8080/ajax-demo/axiosServlet", 
        data:"username=zhangsan" }).then(function (resp) {
        alert(resp.data); 
    })

    // 简写后：
    axios.post("http://localhost:8080/ajax-demo/axiosServlet","username=zhangsan").then(function (resp) { 
        alert(resp.data); 
    })
</script>
~~~

​      then() 中传递的匿名函数称为 **回调函数**(该匿名函数在发送请求时不会被

调用，而是在成功响应后调用的函数)

### 其他请求：

1. get 请求 ： axios.get(url[,config]) 
2. delete 请求 ： axios.delete(url[,config]) 
3. head 请求 ： axios.head(url[,config]) 
4. options 请求 ： axios.option(url[,config]) 
5. post 请求： axios.post(url[,data[,config]) 
6. put 请求： axios.put(url[,data[,config]) 
7. patch 请求： axios.patch(url[,data[,config])

# JSON

​     JSON（JavaScript Object Notation ）：**JavaScript 对象表示法**

​    作用：由于其语法格式简单，层次结构鲜明，现多用于作为**数据载体**，在网络中进行数据传输。

- JavaScript 对象的定义格式：

~~~js
{ 
    name:"zhangsan",
    age:23,
    city:"北京" 
}
~~~

- JSON 对象的格式：

~~~js
{ 
    "name":"zhangsan",
    "age":23,
    "city":"北京" 
}
~~~

##  **JSON** **基本使用**

注意：**axios** 会 **自动对 js 对象和 JSON 串进行转换**。

~~~html
<html lang="en">
    <head>
        <meta charset="UTF-8"> 
        <title>Title</title> 
    </head> 
    <body> 
        <script> 
            //1. 定义JSON字符串
            var jsonStr = '{
                           "name":"zhangsan",
                           "age":23,
                           "addr":["北京","上海","西安"]
               }';
            alert(jsonStr); 

            // parse(str) ：将 JSON串转换为 js 对象。
            var jsObject = JSON.parse(jsonStr); 

            // stringify(obj) ：将 js对象转换为 JSON 串。
            var jsonStr = JSON.stringify(jsObject)
        </script>
    </body> 
</html>
~~~

配合ajxoi使用：

~~~html
</html>
    <body>
        <script>
            var jsObject = {name:"张三"}; 
            axios({ 
                method:"post", 
                url:"http://localhost:8080/ajax-demo/axiosServlet", 
                data: JSON.stringify(jsObject) 
                // stringify(obj) ：将 js对象转换为 JSON 串。
            }).then(function (resp) {
                alert(resp.data); 
            })
        </script>
    </body> 
</html>
~~~





## 定义格式:

​     JSON 串的键要求必须使用双引号括起来，而值根据要表示的类型确定。value 的数据类型分为如下：

1. 数字（整数或浮点数）
2. 字符串（使用双引号括起来）
3. 逻辑值（true或者false）
4. 数组（在方括号中）
5. 对象（在花括号中）
6. null

~~~js
var 变量名 = '{"key":"value","key":"value",...}'; 
~~~

# **Fastjson**

​         **Fastjson** 是阿里巴巴提供的一个Java语言编写的高性能功能完善的 **JSON 库**，是目前Java语言中最快的 JSON 库，可以实现 Java 对象和 JSON 字符串的相互转换。

 **序列化**： 将`集合`数据转换为 json 数据；

**反序列化**：将`  json `数据转换为 Java 对象。

​    java中如需返回JSON数据需要第三方jar包 ：例：  ①:jsonlib   ②: jackson  ③: fastjson(阿里巴巴)  ④:gson(谷歌)

## **Fastjson 的基本使用**

1. 在pox.xml文档中**导入坐标**

   ~~~xml
   <dependency> 
       <groupId>com.alibaba</groupId>
       <artifactId>fastjson</artifactId>
       <version>1.2.62</version>
   </dependency>
   ~~~

   2.在java类里进行 **Java 对象** 和 **JSON 串的相互转换**

```java
public class FastJsonDemo {
    public static void main(String[] args) {
        
        User user = new User(1,'张楚岚','123465');
        
        //1. 将Java对象转为JSON字符串
        String jsonString = JSON.toJSONString(user);                           System.out.println(jsonString);
        //{"id":1,"password":"123","username":"zhangsan"} 

        //2. 将JSON字符串转为Java对象
        User u = JSON.parseObject("{\"id\":1,\"password\":\"123\",\"username\":\"zhangsan\"}",User.class);
        System.out.println(u); 
    } 
}
```



# javaWeb依赖、jar:

```xml
<dependencies>
    <dependency> <!-- 测试单元-->
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
    </dependency>

    <dependency> <!-- mysql jar包-->
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.29</version>
    </dependency>

    <dependency> <!-- mybatis jar包-->
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>

    <dependency> <!-- servlet jar包-->
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>


    <!-- ==============环境===================-->
    <!-- 添加slf4j日志api -->
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
</dependencies>
```





# web项目初始化操作：





## 注解生效激活:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Java%E9%A1%B9%E7%9B%AE%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE-%E6%B3%A8%E8%A7%A3%E7%94%9F%E6%95%88%E6%BF%80%E6%B4%BB.png)



## java编译版本选8:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Java%E9%A1%B9%E7%9B%AE%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE-java%E7%BC%96%E8%AF%91%E7%89%88%E6%9C%AC%E9%80%898.png)



## **报不支持发行版本 5：**

**要在settings ==》bulid ==》java compiler把版本改成自己的版本**

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Tomcat/%E7%89%88%E6%9C%AC%E5%8F%B7%E4%BD%8E%E4%BA%8E5.png)



## 热部署：

​     重启：自定义开发代码，包含类、页面、配置文件等，加载文件restart类加载器。
​     重载：jar包，加载位置base类加载器。

 -  restart类加载器：用来加载开发者自己开发的类、配置文件、页面等信息，这一类文件受开发者影响。

 -  base类加载器：用来加载jar包中的类，jar包中的类和配置文件由于不会发生变化，因此不管加载多少次，加载的内容不会发生变化。

​    注意：①热部署仅仅加载当前开发者自定义开发的资源，不加载jar资源。

​               ②热部署对注解内的属性的修改不明显，建议重启微服务。



### 不触发热部署的目录：

```
/META-INF/maven
/META-INF/resources
/resources
/static
/public
/templates
```

### 启动热部署：

#### 手动启动热部署：

pom.xml设置热部署插件

```xml
<!-- 热部署插件-->  （注意：此部署需要手工编译一下，才热部署。）
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```



#### 自动启动热部署：

​    注意：Idea失去焦点5秒后才启动热部署。

①导入开发者工具对应的坐标：

```xml
<!-- 热部署-->  
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>2.5.4</version>
    <scope>runtime</scope>
</dependency>

<!-- 热部署插件-在父工程加，可不加--> 
<build>
    <finalName>你自己的工程名字</finalName>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
                <addResources>true</addResources>
            </configuration>
        </plugin>
    </plugins>
</build>
```

②.1配置自定热部署：

![](../../%25E7%25AC%2594%25E8%25AE%25B0%25E5%259B%25BE%25E7%2589%2587/Java/%25E6%25A1%2586%25E6%259E%25B6/SpringBoot/%25E7%2583%25AD%25E9%2583%25A8%25E7%25BD%25B2-%25E8%2587%25AA%25E5%258A%25A8%25E7%2583%25AD%25E9%2583%25A8%25E7%25BD%25B2.png)

③

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/java%20wed/Java%E9%A1%B9%E7%9B%AE%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE-%E7%83%AD%E9%83%A8%E7%BD%B2.bmp)

​     改为热部署，重启IDEA。

### 自定义热部署：

​    设置不参数热部署的文件&文件夹。

yml:

```yaml
spring:
  devtools:
    restart:   # 设置不参与热部署的文件或文件夹
     exclude: static/**,public/**,config/application.yml
```



### 关闭热部署：

注意：关闭热部署功能降低线上程序的资源消耗。

yml:

```yaml
spring:
  devtools:
    restart:
      enabled: false #关闭热部署
```





## Servlet

### 控制台中文乱码问题：

~~~java

request.setCharacterEncoding("UTF-8"); // 只对post请求有用

~~~



### 没有Servlet模板问题

1.点击Maven

2.点击上面的刷新键就行了

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Tomcat/%E8%A7%A3%E5%86%B3%E6%B2%A1%E6%9C%89Servlet%E6%A8%A1%E6%9D%BF%E9%97%AE%E9%A2%98.png)



![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Tomcat/%E6%B2%A1%E6%9C%89Servlet%E6%A8%A1%E6%9D%BF%E9%97%AE%E9%A2%98.png)

### 自定义Servlet模板

 点击File ==> settings ==> Editor ==> File and Code Templates ==> Other  ==>Web  ==> java code templates ==>Servlet Annotated Class.java

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Tomcat/%E8%87%AA%E5%AE%9A%E4%B9%89Servlet%E6%A8%A1%E6%9D%BF.png)

## mysql

## 写sql没有提示：

1. setting下搜索sql dialects,将Global SQL Dialect和Project SQL Dialect两个选项都修改为mysql

   ## 字段没提示：

2. 在sql语句上按alt+Enter，选择language injection setting ，选mysql



## 报ClassNotFoundException或NoClassDefoundError异常

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Jwt%E8%8E%B7%E5%8F%96id%E7%AD%89%E6%8A%A5%E9%94%99.png)

解决方法：把依赖范围从“运行时去除依赖”改为“不去除依赖”（默认）

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- <scope>provided</scope> --> 
    <!--  provided:运行时去除依赖    -->  
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
</dependency>
```


















































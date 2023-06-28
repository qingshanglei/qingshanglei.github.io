# SpringMVC框架

​    SpringMVC属于Spring框架的一部分，主要是用来进行Web开发，是对Servlet层进行了封装。

SpringMVC是处于Web层的框架，所以其主要的作用就是用来接收前端发过来的请求和数据然后经过处理并将处理的结果响应给前端。

SpringMVC是web层的框架，主要的作用是接收请求、接收数据、响应结果

   SpringMVC替换Servlet。



浏览器发送一个请求给后端服务器，后端服务器现在是使用Servlet来接收请求和数据

如果所有的处理都交给Servlet来处理的话，所有的东西都耦合在一起，对后期的维护和扩展极为不利

将后端服务器Servlet拆分成三层，分别是web、service和dao

web层主要由servlet来处理，负责页面请求和数据的收集以及响应结果给前端

service层主要负责业务逻辑的处理

dao层主要负责数据的增删改查操作

servlet处理请求和数据的时候，存在的问题是一个servlet只能处理一个请求

针对web层进行了优化，采用了MVC设计模式，将其设计为controller、view和Model

controller负责请求和数据的接收，接收后将其转发给service进行业务处理

service根据需要会调用dao对数据进行增删改查

dao把数据处理完后将结果交给service,service再交给controller

controller根据需求组装成Model和View,Model和View组合起来生成页面转发给前端浏览

器

这样做的好处就是controller可以处理多个请求，并

表现层：servlet,SpringMVC

业务层:

数据层：JDBC,MyBatis



## SpringMVC概念：

   SpringMVC是一种基于java实现MVC模型的轻量级web框架。

  SpirngMVC是一种表现层框架技术，专用于进行表现层功能开发。



优点

* 使用简单、开发便捷(相比于Servlet)

* 灵活性强

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/SpringMVC%25E6%2580%25BB%25E7%25BB%2593.png)
  
  

# SpringMVC的基本使用：

## 总结：

```java
@RequestMapping("/save") /* 设置当前模块的访问路径，value属性前面加不加/都可以*/
@ResponseBody /* 返回值序列化为JSON或XML格式*/
@RequestBody  // 接收json格式数据，把json格式数据封装到对象里面{...}（注意：不能使用get提交方式）
@PathVariable //解决接收路径参数不一致问题  
@RequestParam //绑定请求参与处理器方法形参间的关系（用于接收url地址传参 或表单传参）。 
@EnableWebMvc //开启json数据转换为对象
@DateTimeFormat //设置参数为日期时间数据格式   
@ComponentScan({"com.qsl.service","com.qsl.dao"}) // 默认bean加载
```

   Controller层：返回数据，不知道返回说明类型数据的时候，返回Map<String,Object>类型。
    特点： Map类型取值，放值都方便。

1.在pom.xml导入依赖

```xml
<!-- servlet jar包-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope><!-- servlet-api包和tomcat中的依赖发生冲突-->
</dependency>

<!-- springMVC jar包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>
```

2.初始化SpringMVC环境

```java
@Configuration //设置为配置类
@ComponentScan("com.qsl.controller") //包扫描
public class SpringMvcConfig {

}
```

3.定义Servlet类

```java
/* 2.1使用@Controller定义bean标签*/
@Controller
public class UserServlet {

    /* 2.2设置当前控制器方法请求访问路径(可在方法和类名上设置)*/
    @RequestMapping("/save")
    /* 2.3设置当前方法的返回值*/
    @ResponseBody
    public String sava() {
        System.out.println("user save...");
        return "{'module':'SpringMVC'}";
    }
}
```



4：初始化Servlet容器，加载SpringMVC环境，并设置SpringMVC技术处理的请求

```java
// 方法1：
/* 4.定义一个servlet容器启动的配置类，在里面加载spring的配置 */
public class ServletContainerslnitConfig extends AbstractDispatcherServletInitializer {
    /* 加载springMVC容器配置*/
    //加载springmvc配置类，产生springmvc容器（本质还是spring容器）
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        // 创建IOC容器
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        //注册配置
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    /* 设置那些请求归属springMVC处理*/
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    /* 加载spring容器配置*/
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}

// 方法2：推荐
public class ServletContainerslnitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    // spring配置
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    // springMVC配置
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    // 拦截路径配置
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    
       //get请求-乱码处理
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
}
```

### Controller加载控制 与bean加载控制：

​     bean加载控制：spring加载对应bean,springMVC加载对应bean。

​    1.SpringMVC相关bean（表现层bean：Servlet）

​    2.Spring控制的bean

​       业务bean（Service）

​       功能bean（DataSource等）

1.因为功能不同，如何避免Spring错误的加载到SpringMVC的bean——加载Spring控制的bean的时候排除掉SpringMVC控制的bean。

#### SpringMVC相关bean加载控制：

​      SpringMVC加载的bean对应的包均在com.itheima.controller包内。



####   Spring相关bean加载控制：

##### ***精准扫描包加载**

1.Spring加载的bean设定为：**精准扫描包加载

```java
@Configuration //设置为配置bean
@ComponentScan({"com.qsl.service","com.qsl.dao"}) 
//方法1：精准扫描包
public class SpringConfig {

}
```

##### *设置扫描范围

Spring加载的bean设定扫描范围为com.itheima,排除掉controller包中的bean

```java
@Configuration //设置为配置bean
@ComponentScan(value = "com.qsl"
        ,excludeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION,
                classes = Controller.class)
) // 方法2：设置扫描范围
public class SpringConfig {

}
```

* excludeFilters属性：设置扫描加载bean时，排除的过滤规则

* type属性：设置排除规则，当前使用按照bean定义时的注解类型进行排除

  * ANNOTATION：按照注解排除（重要）
  * ASSIGNABLE_TYPE:按照指定的类型过滤
  * ASPECTJ:按照Aspectj表达式排除，基本上不会用
  * REGEX:按照正则表达式排除
  * CUSTOM:按照自定义规则排除

  大家只需要知道第一种ANNOTATION即可

* classes属性：设置排除的具体注解类，当前设置排除@Controller定义的bean

  ==注意:测试的时候，需要把SpringMvcConfig配置类上的@ComponentScan注解注释掉，否则不会报错==

##### 不区分Spring与SpringMVC的环境，加载到同一个环境中

案例无。。。

# 参数：

​     请求与响应:参数

### 设置请求映射路径：

在类设置请求路径，

```java
@RequestMapping("/user")
public class UserServletImpl implements UserServlet {

    @RequestMapping("/save") // 设置当前控制器方法请求访问路径
    @ResponseBody //设置当前方法的返回值
    public String save() {
        System.out.println("user save ...");
        return "{'Spring':'user save ...'}";
    }
}

@RequestMapping("/book")
public class BookServletImpl implements UserServlet {

    @RequestMapping("/save") // 设置当前控制器方法请求访问路径
    @ResponseBody //设置当前方法的返回值
    public String save() {
        System.out.println("book save ...");
        return "{'Spring':'book save ...'}";
    }
}
```



### 请求参数：

 案例：  请求 &参数配合PostMan软件使用，不用写html代码。

​    请求：get请求，post请求 。。。。

​    参数：普通参数，POJO类型参数，嵌套POJO类型参数，数组类型参数，集合类型参数。

#### get请求：

```java
@Controller  //设置为bean标签
public class UserServeletImpl implements UserServelet {

    @RequestMapping("/save")// 设置当前控制器方法请求访问路径
    @ResponseBody //设置当前方法的返回值
    public String save(String @RequestParam("name")userName, String age) {
        System.out.println("user save ...");
        System.out.println("普通参数传递 ==>" + userName + "," + age);

        return "{'SpringMVC':'save'}";
    }
}
```

配合PostMan可不写html代码

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/API/PostMan/%25E8%25AE%25BE%25E7%25BD%25AE%25E5%258F%2582%25E6%2595%25B0.png)



get请求乱码:

Tomcat8.5以后的版本已经处理了中文乱码的问题，但是IDEA中的Tomcat插件目前只到Tomcat7，所以需要修改pom.xml来解决GET请求中文乱码问题

```xml
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port><!--tomcat端口号-->
          <path>/</path> <!--虚拟目录-->
          <uriEncoding>UTF-8</uriEncoding><!--访问路径编解码字符集-->
        </configuration>
      </plugin>
    </plugins>
  </build>
```



#### Post请求：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/API/PostMan/post%25E8%25AF%25B7%25E6%25B1%2582.png)

post请求跟get请求代码一样

```java
@Controller  //设置为bean标签
public class UserServelet {

    @RequestMapping("/save")// 设置当前控制器方法请求访问路径
    @ResponseBody //设置当前方法的返回值
    public String save(String @RequestParam("name")userName, String age) {
        System.out.println("user save ...");
        System.out.println("普通参数传递 ==>" + userName + "," + age);

        return "{'SpringMVC':'save'}";
    }
}
```

post请求中文乱码:

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //乱码处理
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
}
```



### 五种类型参数传递：

#### 普通参数：

请求路径：

```java
http://localhost:8080/SpringMVC_04_request_param/beg?name=&age=23
```

```java
@Controller  //设置为bean标签
public class UserServeletImpl implements UserServelet {

    /* get请求、post请求*/
    @RequestMapping("/beg") // 设置当前控制器方法请求访问路径
    @ResponseBody //设置当前方法的返回值
    public String beg(String name, int age) {
        System.out.println("user save ...");

        System.out.println("get/post请求 ==>" + name + "," + age);
        return "{'SpringMVC':'beg'}";
    }
}    
```

PostMan软件对应图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E6%2599%25AE%25E9%2580%259A%25E5%258F%2582%25E6%2595%25B0.png)



#### POJO参数：

​    POJO参数：请求参数名与形参对象属性名相同，定义POJO类型形参即可接收参数。

1.创建User类

```java
public class User {
    private String name;
    private int age;
    //setter...getter...略
}
```

2.编写程序

```java
    @RequestMapping("/pojoParam")
    @ResponseBody
    public String pojoParam(User user) {

        System.out.println("POJO参数 ==>" + user);
        return "{'SpringMVC':'pojoParam'}";
    }
```

3.

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/POJO%25E5%258F%2582%25E6%2595%25B0.png)



#### 嵌套POJO型参数：

​    嵌套POJO参数：请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套POJO属性参数。

1.创建Address和User类

```java
public class Address {
    private String province;
    private String city;
    //setter...getter...略
}
public class User {
    private String name;
    private int age;
    private Address address;
    //setter...getter...略
}
```

2.

```java
//POJO参数：请求参数与形参对象中的属性对应即可完成参数传递
    @RequestMapping("/pojoContainPojoParam")
    @ResponseBody
    public String pojoContainPojoParam(User user) {

        System.out.println("嵌套POJO参数 ==>" + user);
        return "{'SpringMVC':'pojoContainPojoParam'}";
    }
```

3.

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E5%25B5%258C%25E5%25A5%2597POJO%25E5%258F%2582%25E6%2595%25B0.png)

**注意:**请求参数key的名称要和POJO中属性的名称一致，否则无法封装。



#### 数组参数：

   数组参数：请求参数名与形参对象属性名相同且请求参数为多个，定义数组类型即可接收参数。

```java
    @RequestMapping("/arrayParam")
    @ResponseBody //返回当前方法的返回值
    @Override
    public String arrayParam(String[] arr) {

        System.out.println("数组参数 ==>" + Arrays.toString(arr));
        return "{'SpringMVC':'pojoContainPojoParam'}";
    }
```

2.

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E6%2595%25B0%25E7%25BB%2584%25E5%258F%2582%25E6%2595%25B0.png)



#### 集合参数：

   不加@RequestParam注解会报： 没有此构造方法java.lang.NoSuchMethodException: java.util.List.<init>()

  报错原因:SpringMVC将List看做是一个POJO对象来处理，将其创建一个对象并准备把前端的数据封装到对象中，但是List是一个接口无法创建对象，所以报错。

```java
  /* 集合参数:  */
    @RequestMapping("/listParam")
    @ResponseBody
    @Override
    public String listParam(@RequestParam List<String> gether) {

        System.out.println("集合参数 ==>" + gether);
        return "{'SpringMVC':'listParam'}";
    }
```

2

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E9%259B%2586%25E5%2590%2588%25E5%258F%2582%25E6%2595%25B0.png)



### *json参数：

常见的三种-JSON数据类型:

- json普通数组（["value1","value2","value3",...]）
- json对象
  - （{
  - ​      key1:value1,
  - ​      key2:value2,...}）
- json对象数组
  - （ [
  - ​    {key1:value1,...}
  - ​    ,{key2:value2,...}
  - ]）

#### @RequestBody与@RequestParam区别

* 区别

  * @RequestParam用于接收url地址传参，表单传参【application/x-www-form-urlencoded】。
  * @RequestBody用于接收json数据【application/json】。

* 应用

  * 后期开发中，发送json格式数据为主，@RequestBody应用较广。
  * 如果发送非json格式数据，选用@RequestParam接收请求参数。

  SpringMVC默认使用jackson来处理json的转换，所以json参数都用以下jar包。

- 接收参数：
  - 实体类数据：@RequestBody
  - 路径变量：@PathVariable

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```



#### JSON普通数组：

- json普通数组（["value1","value2","value3",...]）

1.

```java
@Configuration //设置为配置类
@ComponentScan("com.itheima.controller") // 包扫描
@EnableWebMvc //开启json数据类型自动转换
public class SpringMvcConfig {
}
```

2

```java
@Controller  //设置为bean标签
public class UserServele {
    
    @RequestMapping("/listParamForJson")
    @ResponseBody
    public String listParamForJson(@RequestBody List<String> gether) {

        System.out.println("json普通数组/集合参数(JSON) ==>" + gether);
        return "{'SpringMVC':'listParamForJson'}";
    }
}    
```

3.

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Postman-json%25E6%2599%25AE%25E9%2580%259A%25E6%2595%25B0%25E7%25BB%2584.png)

#### JSON对象：

- json对象（{key1:value1,key2:value2,...}）

1

```java
public class User {
    private String user;
    private Integer age;
    private  Adders adders;
    // get，set。。。。
}
```

2

```java
@Controller  //设置为bean标签
public class UserServelet {

    /* json对象（POJO参数：josn格式）*/
    @RequestMapping("/pojoParamForJson")
    @ResponseBody
    public String pojoParamForJson(@RequestBody  User user) {

        System.out.println("json对象/POJO参数(JSON) ==>" + user);
        return "{'SpringMVC':'pojoParamForJson'}";
    }
}

```

3

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Postman-json%25E5%25AF%25B9%25E8%25B1%25A1.png)



#### JSON对象数组:

- json对象数组（[{key1:value1,...},{key2:value2,...}]）

  1.

  ```java
  public class User {
      private String user;
      private Integer age;
      private  Adders adders;
  }
  
  public class Adders {
      private String province;
      private String city;
  }
  ```

  2

```java
@Controller  //设置为bean标签
public class UserServelet {
    @RequestMapping("/listPojoParamForJson")
    @ResponseBody
    public String listPojoParamForJson(@RequestBody List<User> user) {

        System.out.println("Json对象数组/集合参数(JSON) ==>" + user);
        return "{'SpringMVC':'listPojoParamForJson'}";
    }
}    
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Postman-json%25E5%25AF%25B9%25E8%25B1%25A1%25E6%2595%25B0%25E7%25BB%2584.png)



### 日期参数传递：

 日期的格式有N多中输入方式，比如:

* 2088/08/18 （Date,默认）

* 2088-08-18

* 08/18/2088

* ......

  
```java

//设置参数为日期时间数据格式   pattern:日期时间格式字符串
@DateTimeFormat(pattern = "yyyy-HH-dd") 
```

  

```java
@Controller  //设置为bean标签
public class UserServet {    
    
    @RequestMapping("/dataParam")
    @ResponseBody
    public String dataParam(Date date
         , @DateTimeFormat(pattern = "yyyy-HH-dd") Date date2
         , @DateTimeFormat(pattern = "HH/dd/yyyy") Date date3) {

        System.out.println("dataParam ==>");
        System.out.println("yyyy/HH/dd: " + date);
        System.out.println("yyyy-HH-dd: " + date2);
        System.out.println("HH/dd/yyyy: " + date3);
        return "{'SpringMVC':'dataParam'}";
    }
}    
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Postman-%25E6%2597%25A5%25E6%259C%259F%25E5%258F%2582%25E6%2595%25B0.png)



### 响应：

#### 响应页面：

```java
@Controller //设置为bean标签
public class UserServeletImpl implements UserServelet {
    @RequestMapping("/toJumpPage") 
    @Override
    public String toJumpPage() {

        System.out.println("user toJumpPage ...");
        return "index.jsp";
    }
}    
```

#### 返回文本数据：

```java
@Controller //设置为bean标签
public class UserServelet {
   @RequestMapping("/goText")
    @ResponseBody  //如不写包No mapping for GET /SpringMVC_05_response/response text
    public String goText() {

        System.out.println("user goText ...");
        return "response text";
    }
}    
```



#### 响应数据JSON数据：

##### 响应POJO对象：

```java
@Controller //设置为bean标签
public class UserServelet {

    @RequestMapping("/goJsonPOJO")
    @ResponseBody
    public User toJsonPOJO() {

        System.out.println("user goJsonPOJO ...");
        User user = new User("张灵玉", 21);
        return user;
    }
}    
```

直接访问：http://localhost:8080/SpringMVC_05_response/goJsonPOJO

##### 响应POJO集合对象：

```java
@Controller //设置为bean标签
public class UserServelet {
    
    @RequestMapping("/goJsonList")
    @ResponseBody
    public List<User> goJsonList() {

        System.out.println("user goJsonList ...");
        User user = new User("张灵玉", 23);
        User user1 = new User("张楚岚", 21);

        List<User> list = new ArrayList<>();
        list.add(user);
        list.add(user1);
        System.out.println(list);
        return list;
    }
}    
```

直接访问：http://localhost:8080/SpringMVC_05_response/goJsonList

# Rest风格：

   REST是一种软件架构风格，可以降低开发的复杂性，提高系统的可伸缩性，后期的应用也是非常广泛。

### Rest风格概念：

==REST==（Representational State Transfer），表现形式状态转换,它是一种软件架构==风格==

* 传统风格资源描述形式
  * `http://localhost/user/getById?id=1` 查询id为1的用户信息
  * `http://localhost/user/saveUser` 保存用户信息
* REST风格描述形式
  * `http://localhost/user/1` 
  * `http://localhost/user`

REST的优点：

- 隐藏资源的访问行为，无法通过地址得知对资源是何种操作
- 书写简化

### 注解总结：

```java
@RestController //设置当前控制器类为RESTful风格，@RestController = @Controller + ResponseBody
```

Rest风格图解：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Rest%25E9%25A3%258E%25E6%25A0%25BC%25E6%25B3%25A8%25E8%25A7%25A31.png)

### RESTful风格：

根据REST风格对资源进行访问称为**`RESTful`**。

按照不同的请求方式代表不同的操作类型。

* 发送**`GET请求`**是用来做`查询`: `http://localhost/users`
* 发送**`POST请求`**是用来做`新增/保存` :`http://localhost/users`
* 发送**`PUT请求`**是用来做`修改/更新` : `http://localhost/users`
* 发送**`DELETE请求`**是用来做`删除` : `http://localhost/users/1`

注意：上述行为是约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范

* REST提供了对应的架构方式，按照这种架构设计项目可以降低开发的复杂性，提高系统的可伸缩性
* REST中规定GET/POST/PUT/DELETE针对的是查询/新增/修改/删除，但是我们如果非要用GET请求做删除，这点在程序上运行是可以实现的。 

​     描述模块的名称通常使用复数，也就是加s的格式描述，表示此类资源，而非单个资源，例如:users、books、accounts......

#### Restful参数问题：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Restful%25E5%258F%2582%25E6%2595%25B0%25E9%2597%25AE%25E9%25A2%2598.png)

#### RESTful基本使用：

```java
 新增请求：  @RequestMapping(value = "/users", method = RequestMethod.POST)
删除请求：   @RequestMapping(value = "/users/{Id}", method = RequestMethod.DELETE)
查询请求：  @RequestMapping(value = "/users",method = RequestMethod.GET)
修改请求：  @RequestMapping(value = "/users", method = RequestMethod.PUT)
```



1.在pom.xml中添加依赖

```xml
      <!-- servlet jar包-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>

        <!-- springMVC jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>

        <!-- jackson jar-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.0</version>
        </dependency>
    </dependencies>
```



##### 新增：

   设置当前请求方法为POST，表示REST风格中的添加操作

```java
public class UserSrvletImpl implements UserServlet {

    //  新增
    @RequestMapping(value = "/users", method = RequestMethod.POST)
    @ResponseBody
    public String save() {
        System.out.println("user save ...");
        return "{'SpringMVC':' user save ...'}";
    }
}    
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/RESTful%25E9%25A3%258E%25E6%25A0%25BC-%25E6%2596%25B0%25E5%25A2%259E.png)

注意：使用method属性限定该方法的访问方式为`POST`

* 如果发送的不是POST请求，比如发送GET请求，则会报错

  ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/Restful%25E8%25AF%25B7%25E6%25B1%2582%25E4%25BF%259D%25E9%2594%2599.png)



##### 删除：

```java
public class UserSrvletImpl implements UserServlet {  
    
    @RequestMapping(value = "/users/{Id}", method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable  Integer Id) {

        System.out.println("user delete ...");
        return "{'SpringMVC':' user delete ...'}";
    }  
}
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/RESTful%25E9%25A3%258E%25E6%25A0%25BC-%25E5%2588%25A0%25E9%2599%25A4.png)



##### 查询：

###### 查询所有：

```java
public class UserSrvletImpl implements UserServlet {  
    @RequestMapping(value = "/users",method = RequestMethod.GET)
    @ResponseBody
    public String selectAll() {

        System.out.println("user selectAll ...");
        return "{'SpringMVC':' user selectAll ...'}";
    }
}
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/RESTful%25E9%25A3%258E%25E6%25A0%25BC-%25E6%259F%25A5%25E8%25AF%25A2%25E6%2589%2580%25E6%259C%2589.png)

###### 查询单个：

```java
public class UserSrvletImpl implements UserServlet {  
    
    @RequestMapping(value = "/users/{Id}",method = RequestMethod.GET)
    @ResponseBody
    public String selById(@PathVariable  Integer Id) {

        System.out.println("user selById ...");
        return "{'SpringMVC':' user selById ...'}";
    }
}
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/RESTful%25E9%25A3%258E%25E6%25A0%25BC-%25E6%259F%25A5%25E8%25AF%25A2%25E5%258D%2595%25E4%25B8%25AA.png)

##### 修改：

1.将json数据转换为对象

   必须开启，不然@RequestBody转不了JSON

```java
@Configuration //设置为配置类
@ComponentScan("com.qsl.servlet")
@EnableWebMvc //将json数据转换为对象
public class SpringMvcConfig {

}
```

2.

```java
public class UserSrvletImpl implements UserServlet {  

    @RequestMapping(value = "/users", method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User user) {

        System.out.println(user);
        System.out.println("user update ...");
        return "{'SpringMVC':' user update ...'}";
    }  
}
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/RESTful%25E9%25A3%258E%25E6%25A0%25BC-%25E4%25BF%25AE%25E6%2594%25B9.png)

#### `*`RESTful快速开发：

```java
   @RestController:  （设置当前控制器类为RESTful风格，等同于@Controller与@ResponseBody两个注解组合功能）
   @PostMapping : 新增请求
   @DeleteMapping : 删除请求
   @PutMapping : 修改请求
   @GetMapping("/{形参}") : 查询请求
```

 **@RequestBody , @RequestParam  ,   @PathVariable**的区别：

1. @RequestBody用于接收json数据。
2. @RequestParam用于接收url地址传参 或表单传参。
3. @PathVariable用于接收路径参数，使用{参数名称}描述路径参数。

应用：

- 后期开发中，发送请求参数超过1个小时，以json格式为主，@RequestBody应用较广。
- 如果发送非json格式数据，选用@RequestParam接收请求参数。
- 采用RESTful进行开发，当参数较少时，例1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值。

##### 1.将json数据转换为对象

```java
@Configuration //设置为配置类
@ComponentScan("com.qsl.servlet")
@EnableWebMvc //将json数据转换为对象
public class SpringMvcConfig {

}
```

##### 2.新增、删除、查询、修改

```java
@RestController // @RestController = @Controller + ReponseBody
@RequestMapping("/books") //访问路径
public class BookServletImpl implements BookServlet {

    //新增
    @Override
    @PostMapping //   @PostMapping 替代 @RequestMapping(method = RequestMethod.POST)
    public String save() {

        System.out.println("book seve ...");
        return "{'SpringMVC':' books save ...'}";

    }

    // 删除 (设置当前请求方法为DELETE，表示REST风格中的删除操作)
    @Override
    @DeleteMapping("/{Id}") //  @DeleteMapping 替代 @RequestMapping(value = "/{id}",method = RequestMethod.DELETE)
    public String delete(Integer Id) {

        System.out.println("book delete ...");
        return "{'SpringMVC':' books delete ...'}";
    }

    // 修改 (设置当前请求方法为DELETE，表示REST风格中的删除操作)
    @Override
    @PutMapping // @PutMapping 替代  @RequestMapping(method = RequestMethod.PUT)
    public String update(@RequestBody Book book) {

        System.out.println("book update ..." + book);
        return "{'SpringMVC':' books update ...'}";
    }

    // 查询所有
    @Override
    @GetMapping // @GetMapping 替代 @RequestMapping(method = RequestMethod.GET)
    public String selectAll() {

        System.out.println("book selectAll ...");
        return "{'SpringMVC':' books selectAll ...'}";
    }

    // 查询单个
    @Override
    @GetMapping("/{Id}") // @GetMapping 替代 @RequestMapping(value = "/{id}",method = RequestMethod.GET)
    public String selById(Integer Id) {

        System.out.println("book selById ...");
        return "{'SpringMVC':' books selById ...'}";
    }
}
```



#### 访问静态页面：

​     SpringMVC拦截了静态资源，根据/pages/books.html去controller找对应的方法，找不到所以会报404的错误。

1.在conifg文件夹，让SpringMVC将静态资源进行放行。

```java
@Configuration // 设置为标签类
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    //设置静态资源访问过滤，当前类需要设置为配置类，并被扫描加载
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //当访问/pages/????时候，从/pages目录下查找内容
                                             registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}

```

2.导入文件

```java
@Configuration
@ComponentScan({"com.qsl.controller","com.qsl.config"})
@EnableWebMvc
public class SpringMvcConfig {
}
```





# SSM整合：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/ssm%25E6%2595%25B4%25E5%2590%2588%25E6%25B5%2581%25E7%25A8%258B.png)



设置统一数据返回结果类：

   注意：Result类中的字符并不是固定的，可以根据需要自行增减，提供若干个构造方法，方便操作。

```java
public class Result{
    private Integer code; //状态码
    private String message; //返回状态信息（成功 失败）
    private T data; //返回数据
}
```

 成功：1结尾，   失败：0结尾。

   20011: 新增成功 ，20021：删除成功，20031：修改成功,  20041： 查询成功.

   20010: 新增失败 ，20020：删除失败，20030：修改失败,  20040： 查询失败.

# 异常处理器：

   异常处理器： 集中的、统一的处理项目中出现的异常。

异常流程:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%E5%BC%82%E5%B8%B8%E6%B5%81%E7%A8%8B.png)

项目异常分类 &异常处理：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E9%25A1%25B9%25E7%259B%25AE%25E5%25A4%2584%25E7%2590%2586%25E6%2596%25B9%25E6%25B3%2595.png)



## 全局异常：

### exception/ProjectExceptionAdvice:

```java
@RestControllerAdvice //2. 开启异常处理器
public class ProjectExceptionAdvice {

    //1、在BookServicempl类的getById方法里自定义里异常。

    //3.全局异常(其他异常)
    @ExceptionHandler(Exception.class)
    @ResponseBody
    public Result doException(Exception e) {
        e.printStackTrace(); //打印异常
        // 记录日志
        // 发送邮件给运维人员，提醒维护
        // 发送消息给开发人员，ex对象发送给开发人员
        return new Result(Code.SYSTEM_UNKNOW_ERR, null, "系统繁忙，请稍后重试！");
    }
}
```



## 特定异常：

### exception/ProjectExceptionAdvice:

```java
@RestControllerAdvice //2. 开启异常处理器
public class ProjectExceptionAdvice {

    /* （特定异常处理）系统异常*/
    @ExceptionHandler(SystemException.class)
    public Result deSystemException(SystemException ex) {
        // 记录日志
        // 发送邮件给运维人员，提醒维护
        // 发送消息给开发人员，ex对象发送给开发人员
        return new Result(ex.getCode(), null, ex.getMessage());
    }
}
```





## 自定义异常：

### exception/BusinessException:

```java
/* 自定义异常 */
@Data // getters、setter 。。。
public class BusinessException extends  RuntimeException{
    private Integer code;  //状态码
    private String message; // 返回消息

    /**
     * 通过状态码和错误消息创建异常对象
     * @param code
     * @param message
     */
    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
        this.message = message;
    }

    /**
     * 接收枚举类型对象
     * @param resultCodeEnum
     */
    public BusinessException(ResultCodeEnum resultCodeEnum) {
        super(resultCodeEnum.getMessage());
        this.code = resultCodeEnum.getCode();
        this.message = resultCodeEnum.getMessage();
    }

    @Override
    public String toString() {
        return "BusinessException{" +
                "code=" + code +
                ", message=" + this.getMessage() +
                '}';
    }
}
```



### exception/ProjectExceptionAdvice:

```java
@RestControllerAdvice //2. 开启异常处理器
public class ProjectExceptionAdvice {
    //1、在BookServicempl类的getById方法里自定义里异常。
    
    /* 自定义异常 */
    @ExceptionHandler(BusinessException.class)
    public Result doBusinessException(BusinessException ex) {

        //发送对应消息传递给用户，提现规范操作
        return new Result(ex.getCode(), null, ex.getMessage());
    }
}
```



## 测试异常：

```java
 @Override
    public Book getById(Integer Id) {

        // 将可能出现的异常进行包装，转换成自定义异常。
        /* 模拟：业务异常*/
        if (Id == 1) {
            // 自定义异常
            throw new BusinessException(Code.BBSINESS_ERR, "请不要使用你的技术挑战我的耐性！");
        }

        /* 模拟：系统异常，特定异常*/
        try {
            int i = 1 % 0;
        } catch (Exception e) {
            throw new SystemException(Code.SYSTEM_ERR, "服务器连接超时，请重试！");
        }
        return bookDao.getById(Id);
    }
```





![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E9%25A1%25B9%25E7%259B%25AE%25E5%25BC%2582%25E5%25B8%25B8%25E5%25A4%2584%25E7%2590%2586.png)





# 拦截器(Interceptor)：

## 概念：

​     拦截器（Interceptor）是一种动态拦截方法调用的机制，在SpringMVC中动态拦截控制器方法的执行。


![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E6%258B%25A6%25E6%2588%25AA%25E5%2599%25A8%25E5%259B%25BE%25E8%25A7%25A3.png)

* 作用:
  * 在指定的方法调用前后执行预先设定的代码。
  * 阻止原始方法的执行。
  * 总结：拦截器就是用来做增强。

拦截器和过滤器之间的区别是什么?

- 归属不同：Filter（过滤器）属于Servlet技术，Interceptor(拦截器)属于SpringMVC技术。

- 拦截内容不同：Filter对所有访问进行增强，Interceptor仅针对SpringMVC层的访问进行增强。（即请求Controller接口前先调用拦截器）

#### 参数：

* request:请求对象。
* response:响应对象。
* handler:被调用的处理器对象，本质上是一个方法对象，对反射中的Method**对象进行了再包装。**
* modelAndView:如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行调整。
* ex:如果处理器执行过程中出现异常对象，可以针对异常情况进行单独处理  

#### 拦截器链：

配置多个拦截器，按先进后出的原则。

* 当配置多个拦截器时，形成拦截器链
* 拦截器链的运行顺序参照拦截器添加顺序为准
* 当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行
* 当拦截器运行中断，仅运行配置在前面的拦截器的afterCompletion操作

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/SpringMVC/%25E6%258B%25A6%25E6%2588%25AA%25E9%2593%25BE.png)



##    Spring整合:

​     注意：使用拦截器必须把spirng和SpringMvc配置分开，否则只拦截，不放行（即请求接口）。

1.在servlet =》interceptor 创建类ProjectInterceptor

```java
@Controller //定义为bean标签
public class ProjectInterceptor implements HandlerInterceptor {

    //原始方法调用前执行的内容
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
        return true; //是否终止原始操作
    }

    //原始方法调用后执行的内容
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle");
        return true; // 是否终止请求方法：  false:终止，  true：继续执行
    }

    //原始方法调用完成后执行的内容
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("postHandle");
    }
}
```

#### 2 :在config包下，配置拦截器类

```java
@Configuration //设置为配置类
public class SpringMvcSupport extends WebMvcConfigurationSupport {

    // 按类型注入
    @Autowired
    private ProjectInterceptor projectInterceptor;

    // 静态页面(静态资源)放行，当前类需要设置为配置类，并被扫描加载
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        System.out.println(3453);
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }

    // 配置拦截器
    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");

    }
}
```



#### 2.配置拦截器类可替换为以下操作：

```java
@Configuration //设置为配置类
@ComponentScan({"com.itheima.controller"}) // 包扫描
@EnableWebMvc //开启json数据转换为对象
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig  implements WebMvcConfigurer {
    @Autowired //按类型注入
    private ProjectInterceptor projectInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        
    //配置多个拦截器  addPathPatterns(路径,路径2);
 registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*"); 
    }
}
```







写项目的顺序：

1. dao
2. domain
3. service ==> impl
4. servlet





# 浏览器跨域问题：

   跨域本质是浏览器对于ajax请求的一种安全限制：一个页面发起的ajax请求，只能是与当前页域名相同的路径，这能有效的阻止跨站攻击。

解决方法：

1. 在后端接口Controller添加@CrossOrigin注解
2. 使用httpclient
3. 通过gateway网关

## @CrossOrigin注解：

```java
@Api(tags = "教师接口")
@RestController
@RequestMapping(value = "/admin/vod/teacher")
@CrossOrigin // 当前接口支持跨域
public class TeacherController {
    @Autowired
    private TeacherService teacherService;

    @ApiOperation("查询所有")
    @GetMapping("findAll")
    public List<Teacher> findAllTeacher() {
        List<Teacher> teacherList = teacherService.list();
        System.out.println(teacherList);
        return teacherList;
    }
    
    。。。。
}
```


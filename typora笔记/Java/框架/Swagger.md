# 简介：

- Swagger号称世界上最流行的API框架
- Restful Api 文档在线自动生成器 => **API 文档 与API 定义同步更新**
- 直接运行，在线测试API
- 支持多种语言 （如：Java，PHP等）
- 官网：https://swagger.io/
- 注意：正式环境要记得关闭Swagger，一来出于安全考虑二来也可以节省运行时内存。

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。

## Swagger优点：

1、及时性 (接口变更后，能够及时准确地通知相关前后端开发人员)

2、规范性 (并且保证接口的规范性，如接口的地址，请求方式，参数及响应格式和错误信息)

3、一致性 (接口信息一致，不会出现因开发人员拿到的文档版本不一致，而出现分歧)

4、可测性 (直接在接口文档上进行测试，以方便理解业务)

# Swagger与Postman等区别

​      ①相较于传统的Postman或Curl方式测试接口，使用swagger简直就是傻瓜式操作，不需要额外说明文档(写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击Execute，如果再配合自动化框架，可以说基本就不需要人为操作了。

   ② 相较于传统的要先出Word接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。

# SpringBoot集成Swagger

​    注意：①Jdk必须在1.8+，否则swagger2无法运行。

​              ②swagger2和SpringBoot、SpringCloud等有版本关系，必须按照版本关系才能使用，版本关系不对的等swagger2页面一直弹Swagger路径。 （本次SpringBoot版本2.5.6）

1.添加依赖：

```xml
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger2</artifactId>
   <version>2.9.2</version>
</dependency>

<dependency>  <!-- 访问 http://localhost:8080/doc.html -->
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.8</version>
</dependency>

 <!-- 浏览器页面(Swagger默认)，依赖使用上面那个，这个页面太丑了-->
<dependency>  <!-- 访问 http://localhost:8080/swagger-ui.html -->
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

​    注意：SpringCloud的主启动必须使用@EnableSwagger2注解开启Swagger2，SpringBoot不需要。

2.添加SwaggerConfig配置类

​    Config/SwaggerConfig:

```java
@Configuration //配置类
@EnableSwagger2// 开启Swagger2的自动配置
public class SwaggerConfig {  

    // 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(Environment environment) {
        // 设置要显示的Swagger环境
        Profiles profiles = Profiles.of("dev","test");
        // 判断是否处于指定的环境中
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2)
            .select() // basePackage:  根据包路径扫描接口
            .apis(RequestHandlerSelectors.basePackage("com.qsl.controller"))
            // paths:过滤controller层接口的路径   any() ：任何请求都扫描
            .paths(PathSelectors.any()) 
            .build()
    }

    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
            .title("网站-API文档")
            .description("本文档描述了网站微服务接口定义")
            .version("1.0")
            .contact(new Contact("qsl", "http://atguigu.com", "atguigu.com"))
            .build();
    }
}
```

3.添加HelloController测试：

```java
@RestController
@RequestMapping("hellos")
public class HelloController {

    @GetMapping
    public String getById() {
        return "hello-swagger";
    }
}
```

4.打开测试路径：

测试访问：http://localhost:8080/swagger-ui.html

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Swagger%E4%B8%BB%E9%A1%B5%E9%9D%A2.png)

# Swagger开关配置:

## yml文件配置多环境：

```yaml
#启动环境配置
spring:
  profiles:
    active: dev
---  # 在 `application.yml` 中使用 `---` 来分割不同的配置

#开发环境
spring:
  profiles: dev #定义开发环境起
server:
  port: 8080

---
#生产环境
spring:
  profiles: pro # 定义生产环境起
server:
  port: 8081

---
# 定义测试环境
spring:
  profiles: test
server:
  port: 8082
```



## SwaggerConfig：

Config/SwaggerConfig：

```java
@Configuration // 配置类
@EnableSwagger2 // 开启Swagger2的自动配置
public class SwaggerConfig {

    // 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(Environment environment) {
    // 设置要显示的Swagger环境
        Profiles profiles = Profiles.of("dev","test");
        // 判断是否处于指定的环境中
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2)
                .select()
.apis(RequestHandlerSelectors.basePackage("com.qsl.controller"))
                .paths(PathSelectors.any())
                .build()
                .enable(flag); //是否启用Swagger，默认true
    }
```

关闭Swagger样式：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Swagger-enable%EF%BC%88%E5%85%B3%E9%97%AD%E6%A0%B7%E5%BC%8F%EF%BC%89.png)



#  Swagger信息配置：

​     添加Config/SwaggerConfig:

```java
@Configuration // 配置类
@EnableSwagger2 // 开启Swagger2的自动配置
public class SwaggerConfig {

    // 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return  new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
    }

    // 配置Swagger信息
    private ApiInfo apiInfo(){

        Contact contact = new Contact("青衫", "", "2274916510@qq.com");
        return  new ApiInfo("青衫泪的Swagger Api文档", //标题
                "Api 文件介绍", //描述
                "1.0",
                "urn:tos", // 组织链接
                contact, // 联系人信息
                "Apache 2.0", //许可
                "http://qingshanglei..org", // 许可链接
                new ArrayList()); // 扩展
    }

}
```



# 配置扫描接口：









## select,paths参数:

#### select.RequestHandlerSelectors参数:

  通过.select()方法，去配置扫描接口，RequestHandlerSelectors配置如何扫描。

| 配置                                      | 说明                                 |
| ----------------------------------------- | ------------------------------------ |
| basePackage                               | 根据包路径扫描接口                   |
| any                                       | 扫描所有接口                         |
| none                                      | 不扫描接口                           |
| withMethodAnnotation(GetMapping.class)    | 只扫描方法上get请求注解的接口        |
| withClassAnnotation(RequestMapping.class) | 只扫描类上有RequestMapping注解的接口 |
|                                           |                                      |
| eft                                       |                                      |



#### paths.PathSelectors参数：

| 配置                          | 说明                                    |
| ----------------------------- | --------------------------------------- |
| any                           | 任何请求都扫描                          |
| none                          | 任何请求都不扫描                        |
| regex(final String pathRegex) | 通过正则表达式控制                      |
| ant("/hellos/**")             | 只扫描ant()指定的路径(controller层接口) |



##  配置扫描接口:

   添加Config/SwaggerConfig:

```java
// 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return  new Docket(DocumentationType.SWAGGER_2)
                .select() // 通过.select()方法，去配置扫描接口，RequestHandlerSelectors配置如何扫描 
.apis(RequestHandlerSelectors.basePackage("com.qsl.controller"))
 // basePackage:  根据包路径扫描接口
                .build();
    }
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/%E9%85%8D%E7%BD%AE%E6%89%AB%E6%8F%8F%E6%8E%A5%E5%8F%A3-basePackage.png)



## 过滤controller层接口的路径:

Config/SwaggerConfig:

```java
@Configuration // 配置类
@EnableSwagger2 // 开启Swagger2的自动配置
public class SwaggerConfig {

    // 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return  new Docket(DocumentationType.SWAGGER_2)
                .select() 
.apis(RequestHandlerSelectors.basePackage("com.qsl.controller"))
                .paths(PathSelectors.ant("/hellos/**"))  //
                .build();
    }

}
```



# API分组配置：

注意：配置多个分组只需要配置多个docket即可。

```java
@Configuration // 配置类
@EnableSwagger2 // 开启Swagger2的自动配置
public class SwaggerConfig {

    // 配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(Environment environment) {

        return new Docket(DocumentationType.SWAGGER_2)
                .select()
.apis(RequestHandlerSelectors.basePackage("com.qsl.controller"))
                .paths(PathSelectors.any())
                .build()
                .enable(true) //是否启用Swagger，默认true
                .groupName("视频组"); // 配置分组，默认分组为default
    }

    @Bean
    public  Docket docketA(){
        return  new Docket(DocumentationType.SWAGGER_2).groupName("用户组"); // 配置分组
    }
    
    @Bean
    public  Docket docketB(){
        return  new Docket(DocumentationType.SWAGGER_2).groupName("运营组");
    }
    
    @Bean
    public  Docket docketC(){
        return  new Docket(DocumentationType.SWAGGER_2).groupName("构建师组");
    }
}

```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/%E9%85%8D%E7%BD%AEAPI%E5%88%86%E7%BB%84-group.png)

# 实体类配置：

## pojo:

```java
/** 实体类*/
//@Api("用户试题类")   // @Api同ApiModel注解
@ApiModel("用户试题类") // 实体类注释
@Data // getter、setter
public class User {

    @ApiModelProperty("用户名") // 字段注释
    public String userName;  // private的话需要getter/setter，public不需要
    @ApiModelProperty("密码")
    private String password;

}
```

## Controller层：

```java
@RestController
@RequestMapping("/User")
public class UserController {

   // 只要实体类在请求接口的返回值上（即使是泛型），都能映射到实体项中。
    @GetMapping("/user")
    public User getUser() {
        return new User();
    }
}
```

实体类注释：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/%E5%AE%9E%E4%BD%93%E7%B1%BB%E9%85%8D%E7%BD%AE.png)

# Conntroller层配置：

## pojo:

```java
/* 实体类 */
@ApiModel("用户试题类") // 为实体类添加注释 （注意：此处不能使用/分割）
@Data // getter、setter
public class User {

    @ApiModelProperty("用户名") // 为类属性添加注释（字段）
    public String userName;  // private的话需要getter/setter，public不需要
    @ApiModelProperty("密码")
    private String password;

}
```

## Controller层:

```java
@Api(tags = "角色管理")   // controller层注释
@RestController
@RequestMapping("/hellos")
public class HelloController {

    /**
     * 
     * @param userName  用户名
     * @param password  密码
     * @return @ApiOperation：Controller层方法注释
     * @return  @ApiParam：参数注解
       参数名称:name，参数说明:value,required:是否必须（false：可选参数）        注意：①name和value参数的顺序写错时，接口不可用。
                 ②写name时接口可能不可用。
     */
    @ApiOperation("获取用户名") // ：Controller层方法注释
    @GetMapping("/User/userName/password")
    public String getUsers(
        @ApiParam(name = "userName", value = "用户名"
                  ,required=true) String userName,
        @ApiParam("密码")  String password) {
        return "hello" + userName;
    }
}
```

效果图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Conntroller%E5%B1%82%E9%85%8D%E7%BD%AE.png)



# Swagger其他皮肤：

1、默认的  **访问 http://localhost:8080/swagger-ui.html**

2、knife4j

 文档地址：https://doc.xiaominfo.com/

​    knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案。

​     knife4j属于service模块公共资源，因此我们集成到service-uitl模块

  bootstrap-ui  **访问 http://localhost:8080/doc.html**

```xml
<dependency> <!-- 白皮 -->
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>swagger-bootstrap-ui</artifactId>
   <version>1.9.1</version>
</dependency>  

<!-- 黑皮      两个依赖任选一个 -->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.8</version>
</dependency>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Swagger%E7%9A%AE%E8%82%A42.png)

3、Layui-ui  **访问 http://localhost:8080/docs.html**

```xml
<dependency> <!-- 页面打不开。。。-->
   <groupId>com.github.caspar-chen</groupId>
   <artifactId>swagger-ui-layer</artifactId>
   <version>1.1.3</version>
</dependency>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Swagger%E7%9A%AE%E8%82%A43.png)

4、mg-ui  **访问 http://localhost:8080/document.html**

```xml
<dependency>
    <groupId>com.zyplayer</groupId>
    <artifactId>swagger-mg-ui</artifactId>
    <version>1.0.6</version>
</dependency>
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Swagger/Swagger%E7%9A%AE%E8%82%A44.png)





# **前后端分离**

- 前端 -> 前端控制层、视图层
- 后端 -> 后端控制层、服务层、数据访问层
- 前后端通过API进行交互
- 前后端相对独立且松耦合

**产生的问题**

- 前后端集成，前端或者后端无法做到“及时协商，尽早解决”，最终导致问题集中爆发

**解决方案**

- 首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险
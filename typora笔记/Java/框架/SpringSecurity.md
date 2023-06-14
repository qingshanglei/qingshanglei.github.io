权限：
基本技术：Fillter加Aop权限
开源框架:Spring Security、Shiro等



SpringSecurity 重要核心功能，关于安全方面的两个核心功能是“**认证**”和“**授权**”，一般来说，Web 应用的安全性包括**用户认证（Authentication）和用户授权（Authorization）**两个部分。

（1）用户认证指的是：验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码，系统通过校验用户名和密码来完成认证过程。

**系统判断用户是否登录**

（2）用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

**系统判断用户是否有权限去做某些事情。**



Acegi 安全 2007 年底正式更名为"Spring Security"

**SpringSecurity 特点：**

⚫ 和 Spring 无缝整合。

⚫ 全面的权限控制。

⚫ 专门为 Web 开发而设计。

​	◼旧版本不能脱离 Web 环境使用。

​	◼新版本对整个框架进行了分层抽取，分成了核心模块和 Web 模块。单独引入核心模块就可以脱离 Web 环境。

⚫ 重量级。

####  Shiro

Apache 旗下的轻量级权限控制框架。

**特点：**

⚫ 轻量级。Shiro 主张的理念是把复杂的事情变简单。针对对性能有更高要求

的互联网应用有更好表现。

⚫ 通用性。

​	◼好处：不局限于 Web 环境，可以脱离 Web 环境使用。

​	◼缺陷：在 Web 环境下一些特定的需求需要手动编写代码定制。

ssm里整合Spring Security配置过于繁琐。
常见的安全管理技术栈的组合是这样的：
• SSM + Shiro
• Spring Boot/Spring Cloud + Spring Security



Spring Security中三个核心组件：**
​	1、`Authentication`：存储了认证信息，代表当前登录用户
​	2、`SeucirtyContext`：上下文对象，用来获取`Authentication`
​	3、`SecurityContextHolder`：上下文管理对象，用来在程序任何地方获取`SecurityContext`

**`Authentication`中是什么信息呢：**
​	1、`Principal`：用户信息，没有认证时一般是用户名，认证后一般是用户对象
​	2、`Credentials`：用户凭证，一般是密码
​	3、`Authorities`：用户权限

## Spring Security权限：

要对Web资源进行保护，最好的办法莫过于Filter
要想对方法调用进行保护，最好的办法莫过于[AOP](https://so.csdn.net/so/search?q=AOP&spm=1001.2101.3001.7020)。

Spring Security进行认证和鉴权的时候,就是利用的一系列的Filter来进行拦截的。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring%20Security/SpringSecurity%E8%BF%87%E6%BB%A4%E5%99%A8%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

​     一个请求想要访问到API就会从左到右经过蓝线框里的过滤器，其中**绿色部分是负责认证的过滤器，蓝色部分是负责异常处理，橙色部分则是负责授权**。进过一系列拦截最终访问到我们的API。

​    这里面我们只需要重点关注两个过滤器即可：`UsernamePasswordAuthenticationFilter`负责登录认证，`FilterSecurityInterceptor`负责权限授权。

说明：**Spring Security的核心逻辑全在这一套过滤器中，过滤器里会调用各种组件完成功能，掌握了这些过滤器和组件你就掌握了Spring Security**！这个框架的使用方式就是对这些过滤器和组件进行扩展。



## 整合SpringSecurity：

1.添加依赖:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

说明：依赖包（spring-boot-starter-security）导入后，Spring Security就默认提供了许多功能将整个应用给保护了起来：

​	1、要求经过身份验证的用户才能与应用程序进行交互
​	2、创建好了默认登录表单
​	3、生成用户名为`user`的随机密码并打印在控制台上
​	4、`CSRF`攻击防护、`Session Fixation`攻击防护
​	5、等等等等......

添加配置类：

```java
package com.qsl.system.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity // 开启SpringSecurity权限框架
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {


}
```



#### 启动项目测试

在浏览器访问任意接口   例：http://localhost:8080/admin/system/sysRole/findAll

自动跳转到了登录页面

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring%20Security/%E5%90%AF%E5%8A%A8SpringSecurity%E9%BB%98%E8%AE%A4%E9%A1%B5%E9%9D%A2.png)

默认的用户名：user

密码在项目启动的时候在控制台会打印，**注意每次启动的时候密码都会发生变化！**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring%20Security/SpringSecurity%E9%BB%98%E8%AE%A4%E5%AF%86%E7%A0%81.png)

输入用户名，密码，成功访问到controller方法并返回数据，说明Spring Security默认安全保护生效。

SpringSecurity执行流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring%20Security/%E8%AE%A4%E8%AF%81%E6%B5%81%E7%A8%8B%E5%9B%BE.png)



# 配置权限：

​    在SpringSecurity中，会使用默认的FilterSecurityInterceptor来进行权限校验。在FilterSecurityInterceptor中会从SecurityContextHolder获取其中的**Authentication**，然后获取其中的权限信息。判断当前用户是否拥有访问当前资源所需的权限。

Config/WebSecurityconfig:

```java

@Configuration //配置类
@EnableWebSecurity // 开启SpringSecurity权限框架
@EnableGlobalMethodSecurity(prePostEnabled = true) // 开启SpringSecurity权限注解,默认false
public class WebSecurityconfig extends WebSecurityConfigurerAdapter {
    
}
```

Controller层：

```java
@Api(tags = "角色管理接口")
@RestController
@RequestMapping("/admin/system/sysRole")
public class SysRoleController {

    @Autowired private SysRolService sysRolService;

    @ApiOperation("根据Id删除")
    @DeleteMapping("/remove/{Id}")
    @PreAuthorize("hasAnyAuthority('bnt.sysRole.remove')") //根据数据库判断是否有该权限
    public Result removoRole(@PathVariable String Id) { ... }

    @ApiOperation("添加角色")
    @PostMapping("/save")
    @PreAuthorize("hasAuthority('bnt.sysRole.add')") //根据数据库判断是否有该权限
    public Result saveRole(@RequestBody SysRole sysRole) { ... }

    // 修改-根据Id查询
    @ApiOperation("根据Id查询")
    @GetMapping("findRoleById/{Id}")
    @PreAuthorize("hasAuthority('bnt.sysRole.update')") //根据数据库判断是否有该权限
    public Result findRoleById(@PathVariable Long Id) { ... }

    // 修改
    @ApiOperation("修改")
    @PostMapping("update")
    @PreAuthorize("hasAuthority('bnt.sysRole.update')") //根据数据库判断是否有该权限
    public Result updateRole(@RequestBody SysRole sysRole) { ... }
    
    。。。
}
```



​    1、有权限的能够正常返回接口数据。
​	2、没有权限的会抛出异常：org.springframework.security.access.AccessDeniedException: 不允许访问



##### 解决SpringSecurity不允许访问异常

​    解决SpringSecurity不允许访问异常,主要是不返回提示框给前端。。。

###### 1.扩展Spring Security异常处理类:

   1.扩展Spring Security异常处理类：AccessDeniedHandler、AuthenticationEntryPoint。

​    方案1说明：如果系统实现了全局异常处理，那么全局异常首先会获取AccessDeniedException异常，要想Spring Security扩展异常生效，必须在全局异常再次抛出该异常。

######   2、在spring boot全局异常统一处理

exception/ProjectExceptionHandler:

注意: 必须有SpringSecurity的依赖。

```java
// 异常处理:     注意： 特定异常处理 >  全局异常处理
@ControllerAdvice  // 开启异常处理器
public class ProjectExceptionHandler {

    .... 
        
    /** 解决SpringSecurity框架前端无提示异常  */
    @ExceptionHandler(AccessDeniedException.class)
    @ResponseBody
    public Result error(AccessDeniedException e) throws AccessDeniedException {
        return Result.fail().message("没有对应权限")
            .code(ResultCodeEnum.PERMISSION.getCode());
    }
    
    ....
}
```


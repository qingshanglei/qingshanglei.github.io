# Lombok:

Lombok，一个Java类库，提供了一组注解，简化POJO实体类开发。

总结：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Lombok/%E6%80%BB%E7%BB%93.png)

Lombok常见的注解有:

* @Setter:为模型类（实体类）的属性提供setter方法
* @Getter:为模型类的属性提供getter方法
* @ToString:为模型类的属性提供toString方法
* @EqualsAndHashCode:为模型类的属性提供equals和hashcode方法
* **@Data**:是个组合注解，包含上面所有注解的功能  (重点)
* **@NoArgsConstructor**:提供一个无参构造函数
* **@AllArgsConstructor**:提供一个包含所有参数的构造函数

添加依赖:

```xml
<dependency>  // lombok依赖
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
     <scope>provided</scope>
</dependency>
```



```java
// get
public String getName() {
    return name;
}

// set
public void setName(String name) {
    this.name = name;
}

// toString
@Override
public String toString() {
    return "user{" +
        "name='" + name + '\'' +
        '}';
}

// equals
@Override
public boolean equals(Object o) {
    user user = (user) o;
    return name.equals(user.name);
}

// hashCode
@Override
public int hashCode() {
    return Objects.hash(name);
}
```

# 日志：

​    基于lombok提供的@Slf4j注解为类快速添加日志对象。

Controller层：

```java
@Slf4j //使用lombok的4Slf4j的日志
@RestController
@RequestMapping("/books")
public class BookController {

    @GetMapping
    public String getById(){
        System.out.println("SpringBoot is running...");

        log.debug("debug...");
        log.info("info...");
        log.warn("warn...");
        log.error("error...");

        return "SpringBoot is running...";
    }
}
```

yaml文件：

```yaml
logging:
  level:
    root: info
    ebank: debug

  group:
    ebank: com.qsl.controller
```










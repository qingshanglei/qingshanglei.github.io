# Spring概念：

 Spring Framework简称 ： Spring框架

Spring技术是JavaEE开发必备技能，企业开发技术选型命中率 >80%

Spring可以**框架整合**，高效整合其他技术，提高企业级应用开发与运行效率。

Spring框架中提供了两个大的**核心技术**：IOC/DI，AOP（包含**事务处理**）。

​    事务处理属于Spring中AOP的具体应用，可以简化项目中的事务管理。

Spring框架主要的优势是在简化开发和框架整合上。

主流框架：**MyBatis**，MyBatis-plus，Struts，Struts2，Hibernate ......

Spring四内容:

(1)IOC/DI,  

(2)整合Mybatis(IOC的具体应用)，

(3)AOP,

(4)声明式事务(AOP的具体应用)



  Spring Framework:Spring框架，是Spring中最早最核心的技术，也是所有其他技术的基础。

​      SpringBoot:简化开发，而SpringBoot是来帮助Spring在简化的基础上能更快速进行开发。

​     SpringCloud:这个是用来做**分布式之微服务架构**的相关开发。

###  **系统架构图**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

(1)核心层:

* Core Container:Spring的核心容器，其他的模块都需要依赖该模块。

(2)AOP层:

* AOP:面向切面编程，它依赖核心层容器，目的是==**在不改变原有代码的前提下对其功能进行增强**==
* Aspects:AOP是思想,Aspects是对AOP思想的具体实现

(3)数据层：

* Data Access:数据访问，Spring全家桶中有对数 据访问的具体实现技术
* Data Integration:数据集成，Spring支持整合其他的数据层解决方案。(例：Mybatis)
* Transactions:事务，Spring中事务管理是Spring AOP的一个具体实现。 *重点

(4)Web层： SpringMVC框架使用。

(5)Test层： Spring主要整合了Junit来完成单元测试和集成测试



### **Spring学习路线：**

Spring的学习主线是IOC、AOP、声明式事务和整合MyBais。

* ==Spring的IOC/DI==
* ==Spring的AOP==
* ==AOP的具体应用,事务管理==
* ==IOC/DI的具体应用,整合Mybatis==

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/0.png)![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/0.png)

Spring报错：一开始看最后一个错，不行再看上一层的错，不行再上一层。



Spring核心概念:主要包含IOC/DI、IOC容器和Bean。

**IOC（Inversion of Control）控制反转：**使用对象时，由主动new产生对象转换为由**外部**提供对象，此过程中对象创建控制权由程序转移到外部，此思想称为控制反转。

**IOC容器**：Spring创建了一个容器用来存放所创建的对象，这个容器就叫IOC容器。

​    IOC容器的作用以及内部存放：

1.   IOC容器负责对象的创建、初始化等一系列工作，其中包含了数据层和业务层的类对象被创建或被管理的对象在IOC容器中统称为**Bean**。

2. IOC容器中放的就是一个个的Bean对象。

     **DI（Dependency Injection）依赖注入**：在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入。

    **Bean**:容器中所存放的一个个对象就叫Bean或Bean对象。(bean本质上是一个对象)

# IOC/DI:

### Spring的基本使用：

**1:创建Maven项目**

**2:在pom.xml中添加Spring的依赖  jar包**

```xml
<dependencies>
   <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.22</version>
        </dependency>
</dependencies>
```

**3:添加案例中需要的类**

在**域名反写**包下，创建BookService,BookServiceImpl，BookDao和BookDaoImpl四个类

```java
public interface BookDao { 
    public void save(); 
}
public class BookDaoImpl implements BookDao {
    public void save() { 
        System.out.println("book dao save ..."); 
    }
}

public interface BookService {
    public void save();
}
public class BookServiceImpl implements BookService {
    //删除业务层中使用new的方式创建的dao对象
    private BookDao bookDao;
    
    public void save() {
        System.out.println("book service save ..."); 
        bookDao.save(); 
    }
    
     //提供对应的set方法
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
} 
```

3：**添加spring配置文件**

resources下添加名为applicationContext.xml配置文件，并完成bean的配置

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E6%B7%BB%E5%8A%A0spring%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.png)



5:在**applicationContext.xml**配置文件中配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--bean标签标示配置bean
    	id属性标示给bean起名字
        class="bean的类全名"  
        name="起别名（可多个，多个别名可用 <,>逗号，< >空格，<;>分号分割）"         scope="bean作用范围"  singleton(默认):单个       prototype：多个    (设置是单个对象/单例，还是多个对象/非单例)  （isSingleton方法和scope同时使用的时候，会以isSingleton的方法为主）
	-->
	<bean id="bookDaoId" name="bookDaoId2"    
          class="com.itheima.dao.impl.BookDaoImpl"/>
    
    <bean id="bookServiceId" 
          class="com.itheima.service.impl.BookServiceImpl"/>
    
         <!-- property标签表示配置当前bean的属性，
         name属性表示要new那个对象，ref属性表示根据哪一个bean的Id
         value属性填值-->
            <property name="bookDao" ref="bookDaoId"  value='值' ></property>
</beans>
```

6:获取IOC容器

```java
public class App {
    public static void main(String[] args) {
        //6.获取IOC容器
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); //根据配置文件applicationContext.xml获取IOC容器
        
        //7:根据bean标签的id属性和name属性的任意一个值来获取bean对象
     //   BookDao bookDao = (BookDao) ctx.getBean("bookDaoId");
//        bookDao.save();
        BookService bookService = (BookService) ctx.getBean("bookServiceId");
        bookService.save();//调用bookService接口的方法
    }
}

```



### XML开发:

#### IOC相关内容:

##### Bean的配置:

1.设置 bean

```xml
<!-- bean标签表示配置bean
     id属性标示给bean起名字
     class="bean的类全名"  
     name="起别名（别名可多个，多个别名可用 <,>逗号，< >空格，<;>分号分割）" 
     scope="bean作用范围"     singleton(默认):单个   prototype：多个 (设置是new单个对象/单例，还是new多个对象/非单例)
     factory-mehod:具体工厂类中创建对象的方法名
     factory-bean:工厂的实例对象
     factory-method:工厂对象中的具体创建对象的方法名,-->

   <bean id="daoId"  class="com.qsl.dao.impl.BookDaoImpl"
         name="service ,service2" scope="prototype"
         factory-mehod:""> （isSingleton方法和scope同时使用的时候，会以isSingleton的方法为主）

    <!-- property标签表示配置当前bean的属性，
         name属性表示要new那个对象，
         ref属性表示根据哪一个bean的Id或name获取-->
    <property name="bookDao" ref="daoId"  ></property>
</bean>

```

2.获取 Bean

```java
public class AppForName {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        
        //根据bean标签的id属性和name属性的任意一个值来获取bean对象
        BookService bookService = (BookService) ctx.getBean("service2");
        //获取不到bean包NoSuchBeanDefinitionException异常。
        bookService.save();
    }
}
```



**Ebi：全称Enterprise Business Interface，翻译为企业业务接口**

**property图解：**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/bean.png)

 scope使用后续思考：

* 为什么bean默认为单例?
  * bean为单例的意思是在Spring的IOC容器中只会有该类的一个对象
  * bean对象只有一个就避免了对象的频繁创建与销毁，达到了bean对象的复用，性能高
* bean在容器中是单例的，会不会产生线程安全问题?
  * 如果对象是有状态对象，即该对象有成员变量可以用来存储数据的，因为所有请求线程共用一个bean对象，所以会存在线程安全问题。
  * 如果对象是无状态对象，即该对象没有成员变量没有进行数据存储的，因方法中的局部变量在方法调用完成后会被销毁，所以不会存在线程安全问题。
* 哪些bean对象适合交给容器进行管理?
  * 表现层对象
  * 业务层对象
  * 数据层对象
  * 工具对象
* 哪些bean对象不适合交给容器进行管理?
  * 封装实例的域对象，因为会引发线程安全问题，所以不适合。



##### bean的实例化：

Spring的IOC实例化对象的三种方式分别是:

* 构造方法(常用)

* 静态工厂(了解)

* 实例工厂(了解)

  * FactoryBean(实用)

###### 构造方法(常用)：

   Spring底层使用的是类的无参构造方法,构造器底层用的是**暴力反射**。

```java
public class BookDaoImpl implements BookDao {
    
    //1.提供构造器方法
    public BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    
      /* 私有构造方法*/
  //  private BookDaoImpl() {
  //  System.out.println("book dao constructor is running ....");
  //}
    
    public void save() {
        System.out.println("book dao save ...");
    }
}
```



```java

```





###### 静态工厂：

​    静态工厂：一般是用来兼容早期的一些老系统。

  1.准备一个OrderDao和OrderDaoImpl类

```java
public interface OrderDao {
    public void save();
}

public class OrderDaoImpl implements OrderDao {
    public void save() {
        System.out.println("order dao save ...");
    }
}
```

  2.创建一个工厂类OrderDaoFactory并提供一个==静态方法==

```java
//静态工厂创建对象
public class OrderDaoFactory {
    public static OrderDao getOrderDao(){
        System.out.println("factory setup....");//模拟必要的业务操作
        return new OrderDaoImpl();
    }
}
```

  3.编写AppForInstanceOrder运行类，在类中通过工厂获取对象

```java
public class AppForInstanceOrder {
    public static void main(String[] args) {
        //通过静态工厂创建对象
        OrderDao orderDao = OrderDaoFactory.getOrderDao();
        orderDao.save();
    }
}
```

  4.在spring的配置文件application.properties中添加

```xml
 <bean id="orderDaoId" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>
```

对应关系如下图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/bean2.png)

   5.

```java
public class AppForInstanceOrder {
    public static void main(String[] args) {
        // 1.创建IOC容器
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 2.获取bean
        OrderDao orderDao = (OrderDao) ctx.getBean("orderDaoId");
        orderDao.save();

    }
}
```



###### 实例工厂：



###### FactoryBean(实用):

```java
//FactoryBean创建对象
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }

    //返回所创建类的Class对象
    public Class<?> getObjectType() {
        return UserDao.class;
    }

    //设置是否为单例：  true为单例（默认）  false为非单例
    // 跟bean作用范围效果一样，不过 isSingleton > bean作用范围  
    public boolean isSingleton() {
        return false;
    }
}
```





##### bean的生命周期：

Spring的IOC容器是运行在JVM中。

生命周期两个阶段：

* bean创建之后，想要添加内容，比如用来初始化需要用到资源
* bean销毁之前，想要添加内容，比如用来释放用到的资源

###### bean的生命周期设置：

1.基于**spring基本使用**的修改一下这个两个地方

```java
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    //表示bean初始化对应的操作
    public void init(){
        System.out.println("init...");
    }
    //表示bean销毁前对应的操作
    public void destory(){
        System.out.println("destory...");
    }
}
```

2.bean标签

```xml
 <!-- bean标签  init-method="方法名"： 定义初始化方法
               destroy-method="方法名"：定义销毁方法
        -->
<bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl" 
      init-method="init" destroy-method="destory"/>
```

######           关闭IOC容器:

​     close()是在调用的时候关闭，registerShutdownHook()是在JVM退出前调用关闭。

```java

public class Demo {
    public static void main(String[] args) {
        //1.获取IOC容器(必须要ClassPathXmlApplicationContext，ApplicationContext中没有close方法)
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        /* 根据bean标签的id属性和name属性的任意一个值来获取bean对象 */
        BookService bookService = (BookService) ctx.getBean("bookServiceId");
        bookService.save();

      //方法1： 关闭IOC容器（暴力关闭）
        ctx.close();

      //方法2： 注册钩子关闭容器(推荐) :在虚拟机退出前先关闭容器在退出虚拟机
        ctx.registerShutdownHook();
    }
}
```



#### DI相关内容:

DI:依赖注入

向类传递数据的两种注入方式：

* setter注入
  * 简单类型(基本数据类型与String)
  * ==引用类型==
* 构造器注入
  * 简单类型(基本数据类型与String)
  * 引用类型



##### set注入：

环境：基于Spring的基本使用：

* 引用数据类型使用的是`<property name="" ref=""/>`
* 简单数据类型使用的是`<property name="" value=""/>`

###### 注入引用数据类型：

​       set注入关系图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/bean.png)

1:声明属性并提供setter方法

```java
public class BookServiceImpl implements BookService{
    // 1.
    private BookDao bookDao;
    private UserDao userDao;
    
    // 2.设置set
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
        userDao.save();
    }
}
```

2.配置 bean设置 

​    在applicationContext.xml配置文件中使用property标签注入

- 在resources下提供spring的配置 bean

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl"/>
      <bean id="userDaoId" class="com.itheima.dao.impl.UserDaoImpl"/>
      
      <bean id="bookServiceId" class="com.itheima.service.impl.BookServiceImpl">
          
        <!-- property标签表示配置当前bean的属性，
           name属性表示要new那个对象，ref属性表示根据哪一个bean的Id
           value属性填值-->
       <property name="bookDaoId" ref="bookDao" >
       <property name="userDaoId" ref="userDao" >
      </bean>
  </beans>
  ```

- 加载IOC容器，根据获取获取 bean，就好了

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E7%BB%93%E6%9E%9C.png)



###### 注入简单数据类型：

1:声明属性并提供setter方法

```java
public class BookDaoImpl implements BookDao {

    // 注入简单数据类型
    private String databaseName;
    private int connectionNum;

    public void setConnectionNum(int connectionNum) {
        this.connectionNum = connectionNum;
    }

    public void setDatabaseName(String databaseName) {
        this.databaseName = databaseName;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

2:从配置文件中进行注入配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl">
        // property标签   value:注入值
        <property name="databaseName" value="mysql"/>
     	<property name="connectionNum" value="10"/>
    </bean>
    <bean id="userDaoId" class="com.itheima.dao.impl.UserDaoImpl"/>
    
    <bean id="bookServiceId" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDaoId" ref="bookDao"/>
        <property name="userDaoId" ref="userDao"/>
    </bean>
</beans>
```

##### 构造器注入:

###### 注入引用数据类型:

   如要引入多个数据类型，请在构造器中提供对应的参数，如忘记看下面这个 **注入多个简单数据类型**

1:提供构造方法

```java
public class BookServiceImpl implements BookService{
    private BookDao bookDao;

    public BookServiceImpl(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

2:配置文件中进行配置构造方式注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl"/>
    
    <bean id="bookServiceId" class="com.itheima.service.impl.BookServiceImpl">
        
        // constructor-arg标签  name：对应构造函数中方法形参的参数名
        <constructor-arg name="bookDao" ref="bookDaoId"/>
    </bean>
</beans>
```

###### 注入多个简单数据类型：

 1:提供构造方法

```java
public class BookDaoImpl implements BookDao {
    //1. 创建简单类型：
    private String databaseName;
    private int connectionNum;

    //2. 创建构造器：
    public BookDaoImpl(String databaseName, int connectionNum) {
        this.databaseName = databaseName;
        this.connectionNum = connectionNum;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

2.配置完成多个属性构造器注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl">
        
        <constructor-arg name="databaseName" value="mysql"/>
        <constructor-arg name="connectionNum" value="666"/>
    </bean>
    
    <bean id="userDaoId" class="com.itheima.dao.impl.UserDaoImpl"/>
    
    <bean id="bookServiceId" class="com.itheima.service.impl.BookServiceImpl">
        
        // constructor-arg标签  name：对应构造函数中方法形参的参数名
        <constructor-arg name="bookDao" ref="bookDaoId"/>
        <constructor-arg name="userDao" ref="userDaoId"/>
    </bean>
</beans>
```

##### 自动配置：

​     自动配置：IOC容器根据bean所依赖的资源在容器中自动查找并注入到bean中的过程称为自动装配。

​    自动配置：    autowire：按名称装配（byName），按类型装配（byType）。

​    自动配置的方式：

* ==按类型（常用）==
* 按名称
* 按构造方法
* 不启用自动装配

 1.自动装配只需要修改applicationContext.xml配置文件即可:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    
    <!--bean标签
        autowire属性：  按类型装配：byType    按名称装配时：byName-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byType"/>

</beans>
```

注意：

1. 自动装配用于引用类型依赖注入，不能对简单类型进行操作

2. 使用按类型装配时（byType）必须保障容器中相同类型的bean唯一，推荐使用

3. 使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因变量名与配置耦合，不推荐使用

4. 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

   

##### 集合注入:

集合类型分类:

* 数组
* List
* Set
* Map
* Properties

注意：

* property标签表示setter方式注入，构造方式注入constructor-arg标签内部也可以写`<array>`、`<list>`、`<set>`、`<map>`、`<props>`标签
* List的底层也是通过数组实现的，所以`<list>`和`<array>`标签是可以混用
* 集合中要添加引用类型，只需要把`<value>`标签改成`<ref>`标签，这种方式用的比较少

```java
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {

    private int[] array; // 数组

    private List<String> list; // List集合

    private Set<String> set; // Set集合

    private Map<String,String> map; // Map

    private Properties properties; // Properties

     public void save() {
        System.out.println("book dao save ...");

        System.out.println("遍历数组:" + Arrays.toString(array));

        System.out.println("遍历List" + list);

        System.out.println("遍历Set" + set);

        System.out.println("遍历Map" + map);

        System.out.println("遍历Properties" + properties);
    }
	//setter....方法省略，自己使用工具生成
}
```

2. 在resources下applicationContext.xml文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 集合注入： -->
    <bean id="bookDaoId" class="com.qsl.dao.impl.BookDaoImpl">

        <!-- 数组-->
        <property name="arrayName">
            <array>
                <value>111</value>
                <value>222</value>
                <value>666</value>
                <!-- 引用类型： ref-->
<!--                <ref bean=""></ref>-->
            </array>
        </property>

        <!-- list集合-->
        <property name="listName">
            <list>
                <value>张灵玉</value>
                <value>张楚岚</value>
                <value>老天师</value>
            </list>
        </property>

        <!-- set集合-->
        <property name="setName">
            <set>
                <value>孙悟空</value>
                <value>猪八戒</value>
                <value>白龙马</value>
            </set>
        </property>

        <!-- map集合-->
        <property name="mapName">
            <map>
                <entry key="孙悟空" value="西游记"></entry>
                <entry key="猪八戒" value="西游记"></entry>
                <entry key="白龙马" value="西游记"></entry>
            </map>
        </property>

        <!-- properties集合-->
        <property name="properties">
            <props>
                <prop key="老天师">一人之下</prop>
                <prop key="张灵玉">一人之下</prop>
                <prop key="张楚岚">一人之下</prop>
            </props>
        </property>
    </bean>

</beans>
```



#### IOC/DI配置管理第三方bean:

   主要管理第三方的jar 包。

##### 数据源对象管理（德鲁伊 和C0P0）：

   数据源对象管理：实现德鲁伊 和C0P0的管理。

1.在pom.xml中添加依赖

​    使用德鲁伊不需要 mysql jar包，而使用 C3P0要使用 mysql的jar包， Druid程序运行虽然没有报错，但是当调用DruidDataSource的getConnection()方法获取连接的时候，也会报找不到驱动类的错误。

​    C0P0缺少mysql的驱动包:包的错ClassNotFoundException。

```xml
  <!-- spring jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.22</version>
        </dependency>

        <!-- 德鲁伊 jar包-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.11</version>
        </dependency>

        <!-- C3P0 jar包-->
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <!-- mysql jar包-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.29</version>
        </dependency>
```

2.在applicationContext.xml配置文件中添加`DruidDataSource`的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
    
	<!-- 管理DataSource对象-->
    <!-- 德鲁伊-->
    <bean id="druidId" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/test20220804"></property>
        <property name="username" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>

    <!-- C3P0 ：要导jdbc的 jar包才能使用-->
    <bean id="c3p0Id" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test20220804"></property>
        <property name="user" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>
</beans>
```

3.从IOC容器中获取bean对象

```java
public class Demo {
    public static void main(String[] args) {
        /* 3.获取 IOC容器*/
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        /* 德鲁伊*/
        DataSource druidId = (DataSource) ctx.getBean("druidId");
        System.out.println("德鲁伊: " + druidId);

        /* C3P0 */
        DataSource dataSource = (DataSource) ctx.getBean("c3p0Id");
        System.out.println("C3P0: " + dataSource);

    }
}
```

##### 加载properties文件：

1:准备properties配置文件

​    在resources下创建一个jdbc.properties文件,并添加对应的属性键值对：

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/spring_db
jdbc.username=root
jdbc.password=root
```

2:开启`context`命名空间

​    在applicationContext.xml中开`context`命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
    
    // 1. 复制上方的xmlns行 和下方的两行路径，并把beans修改为context
    // 2.使用`context`命名空间下的标签来加载properties配置文件
    
    <!-- 方式4： 加载properties文件 标准格式-->
    <context:property-placeholder location="classpath:*.properties" system-properties-mode="NEVER"/>
    
<!--  方式1：   system-properties-mode="NEVER":  不加载系统属性-->
    <context:property-placeholder location="jdbc.properties" system-properties-mode="NEVER"/>

    <!-- 方式2： 加载多个properties文件-->
    <context:property-placeholder location="jdbc.properties,jdbc2.properties" system-properties-mode="NEVER"/>

    <!-- 方式3： 加载所有properties文件-->
    <context:property-placeholder location="*.properties" system-properties-mode="NEVER"/>

    <!-- 方式5： 从类路径或jar包中搜索并加载 properties文件-->
    <context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER"/>

    
    // 4:完成属性注入
     <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    
    // 5.此代码相当于 控制台输出。
      <bean id="bookDaoId" class="com.itheima.dao.impl.BookDaoImpl">
          
<!-- property标签：system-properties-mode="NEVER"：不加载系统属性 -->
        <property name="name" value="${jdbc.driver}" system-properties-mode="NEVER"/>
    </bean>
</beans>
```

**查看系统的环境变量：**

```java
public static void main(String[] args) throws Exception{
    Map<String, String> env = System.getenv();
    System.out.println(env);
}
```



```xml
    
    <!-- 方式4： 加载properties文件 标准格式-->
    <context:property-placeholder location="classpath:*.properties" system-properties-mode="NEVER"/>


<!--  方式1：   system-properties-mode="NEVER":  不加载系统属性-->
    <context:property-placeholder location="jdbc.properties" system-properties-mode="NEVER"/>

    <!-- 方式2： 加载多个properties文件-->
    <context:property-placeholder location="jdbc.properties,jdbc2.properties" system-properties-mode="NEVER"/>

    <!-- 方式3： 加载所有properties文件-->
    <context:property-placeholder location="*.properties" system-properties-mode="NEVER"/>

    <!-- 方式5： 从类路径或jar包中搜索并加载 properties文件-->
    <context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER"/>

```

#### 核心容器：

   这个核心容器主要是new `ApplicationContext`。

-    BeanFactory是IoC容器的顶层接口，初始化BeanFactory对象时，加载的bean延迟加载。

- ApplicationContext接口是Spring容器的核心接口，初始化时bean立即加载。

- ApplicationContext接口提供基础的bean操作相关方法，通过其他接口扩展其功能

- ApplicationContext接口常用初始化类

     **==ClassPathXmlApplicationContext(常用)==**

  ​    FileSystemXmlApplicationContext

##### 核心容器：

```java
public class Demo {
    public static void main(String[] args) {
        /** 1.创建IOC容器 的两种方式：  */
        /* 方式1：获取 IOC容器 （类路径下的XML配置文件,推荐）*/
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        
        /* 方式2：获取 IOC容器 （文件系统下的XML配置文件）*/
//        ApplicationContext ctx = new FileSystemXmlApplicationContext("G:\\练习\\SSM\\spring_10_di_container\\quickstart\\src\\main\\resources\\applicationContext.xml");


        /** 2.获取 Bean的三种方式：*/
        /* 方式1： 推荐 */
        BookDao bookDao = (BookDao) ctx.getBean("BookDaoId");
        /* 方式2：*/
//        BookDao bookDao=ctx.getBean("BookDaoId",BookDao.class);
        /* 方式3：*/
//        BookDao bookDao = ctx.getBean(BookDao.class);

        bookDao.save();
    }
}
```

##### bean相关：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/bean%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B92.png)

##### 依赖注入相关：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%E7%9B%B8%E5%85%B32.png)



### IOC/DI注解开发(推荐)：

纯注解开发：Spring 3.0+

注解开发总结：



![](../../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E6%B3%A8%E8%A7%A3%E5%BC%80%E5%8F%91%E6%80%BB%E7%BB%931.png)

bean标签：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/bean%E6%A0%87%E7%AD%BE%E6%B3%A8%E8%A7%A3.png)

bean标签细分：

![](../../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E6%B3%A8%E8%A7%A3%E5%BC%80%E5%8F%91%E5%AE%9A%E4%B9%89bean.png)

```java
bean标签： @Component > @Controller; @Service; @Repository
配置类: @Configuration替换`applicationContext.xml`文件

包扫描: @ComponentScan替换`<context:component-scan base-package="com.itheima"/>`
    
设置bean的作用范围：@Scope("prototype")
    singleton(默认):单例       prototype：非单例
    
 @PropertySource: 加载properties文件
 @Bean： 设置该方法的返回值为spring管理的bean
 @Import： 导入   
```





#### < bean >标签:

@Component替代<bean>标签：

1.注解配置bean标签

```java
// 使用@Component注解替代`<bean>`标签,以下三个为分类
//       @Controller:表示 表现层 bean定义
//       @Service： 表示 业务层 bean定义
//       @Repository: 表示 数据层 bean定义(持久层)
// 注意：四个用那个都一样，只不过为了好分辨。
@Component("bookDaoId")
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ..." );
    }
}
```

2.在applicationContext.xml文件，添加包扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 包扫描(为了让Spring框架能够扫描到写在类上的注解，) -->
    <context:component-scan base-package="com.itheima"/>
</beans>
```



#### @Configuration配置类

- ​     @Configuration注解将其标识为一个 **配置类**,替换 applicationContext.xml。

-   @ComponentScan注解为**包扫描**，替换`<context:component-scan base-package=""/>`

```java
@Configuration // 配置类替代'applicationContext.xml'文件
@ComponentScan("com.itheima") // 包扫描
public class SpringConfig {
}
```

2.注解配置bean 

```java

public interface BookDao {
    public void save();
    
}

@Component("BookDaoId")
public class BookDaoImpl implements BookDao {

    @Override
    public void save() {
        System.out.println("book dao save ...");
    }
}
```



#### bean作用范围与生命周期管理：



##### Bean的作用范围：

```java
@Repository //定义数据层bean标签
@Scope("prototype")//设置bean的作用范围    singleton(默认):单例       prototype：非单例
public class BookDaoImpl implements BookDao {

    public void save() {
        System.out.println("book dao save ...");
    }
}
```

##### Bean的生命周期：

1.在pom.xml中导入依赖（JDK9以后jdk中的javax.annotation包被移除了）。

```xml
<dependency>
  <groupId>javax.annotation</groupId>
  <artifactId>javax.annotation-api</artifactId>
  <version>1.3.2</version>
</dependency>
```



```java

@Repository //定义数据层bean标签
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    
    <!--设置该方法为初始化方法 -->
    @PostConstruct //在构造方法之后执行，替换 init-method
    public void init() {
        System.out.println("init ...");
    }
    
    <!--设置该方法为销毁方法 -->
    @PreDestroy //在销毁方法之前执行,替换 destroy-method
    public void destroy() {
        System.out.println("destroy ...");
    }
}

```



```java
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao1 = ctx.getBean(BookDao.class);
        BookDao bookDao2 = ctx.getBean(BookDao.class);
        System.out.println(bookDao1);
        System.out.println(bookDao2);
        ctx.close(); //关闭容器
    }
}
```



#### 依赖注入：

##### @Resource ：

```
 @Resource ：自动注入2-默认按byName
```

```java
@Service
public class BookServiceImpl implements BookService {
    
    @Resource //默认按byName
    private BookDao bookDao;
    
//	  public void setBookDao(BookDao bookDao) {
//        this.bookDao = bookDao;
//    }
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```





##### @Autowired: 按类型注入

```
@Autowired // 自动注入-按byType（按类型注入）
```

```java
@Service
public class BookServiceImpl implements BookService {
    
    @Autowired //按类型注入
    private BookDao bookDao;
    
//	  public void setBookDao(BookDao bookDao) {
//        this.bookDao = bookDao;
//    }
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

```java
@Repository // 
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...2");
    }
}
```





#####  @Qualifier：按照名称注入

​    @Autowired：  按照类型注入 
​    @Qualifier(名称) ：按照名称注入（必须和@Autowired《类型注入》 一起使用）

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired //按类型注入
    @Qualifier("bookDaoId") //按名称注入
    private BookDao bookDao;
    
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

##### @Value: 简单数据类型注入

   简单类型注入的是**基本数据类型**或者**字符串类型**。

@Value("itheima"):   **一般配合 注解读取properties配置文件使用**

```java
@Repository("bookDaoId")
public class BookDaoImpl implements BookDao {
    // @Value("青衫泪") : 简单数据类型 或值类型注入（注入的基本数据类型 或者字符串类型）
    @Value("青衫泪")
    private String name;
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

#####       @PropertySource: 加载properties文件中的属性值

1：resource下准备jdbc.properties文件

```properties
name=itheima888
```

2: 使用注解加载properties配置文件

​        @PropertySource：加载properties文件中的属性值

​     注意：@PropertySource注解不支持通配符 *

```java
@Configuration
@ComponentScan("com.itheima")
@PropertySource("jdbc.properties")
public class SpringConfig {
    
}

```

3：使用@Value读取配置文件中的内容

```java
@Repository("bookDaoId")
public class BookDaoImpl implements BookDao {
    
    @Value("${name}")
    private String name;
    
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```



#### IOC/DI注解开发管理第三方bean：

注解开发管理第三方bean jar包（例：德鲁伊，C3P0等jar包）

```java
 
 @Configuration :   //配置 bean   
 @ComponentScan("com.qsl.Dao")  //包扫描 
 @Import(jdbcConfig.class)：    //把 jdbcConfig导入到bean配置
 @Bean： //添加@Bean，表示当前方法的返回值是一个bean

```

##### 注解开发管理第三方bean：

​    @Bean注解: 设置该方法的返回值为spring管理的bean对象。

 管理`Druid`jar包。

1.导入pom.xml依赖

​    管理德鲁伊jar包

```xml
 <!-- spring jar包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.22</version>
    </dependency>

    <!-- 德鲁伊 jar-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.11</version>
    </dependency>
```

2.在配置类中添加一个方法

```java
@Configuration
public class SpringConfig {
    
    //@Bean注解:将方法的返回值制作为Spring管理的一个bean对象
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**注意:不能使用`DataSource ds = new DruidDataSource()`**

3.从IOC容器中获取对象并打印

```java
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        DataSource dataSource = ctx.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}
```

###### 引入外部配置类:

1:去除JdbcConfig类上的注解

```java
public class JdbcConfig {
	@Bean // 设置该方法的返回值为spring管理的bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

2:在Spring配置类中引入

```java
@Configuration
//@ComponentScan("com.itheima.config")
@Import({JdbcConfig.class})
public class SpringConfig {
	
}
```

**注意:**

* 扫描注解可以移除

* @Import参数需要的是一个数组，可以引入多个配置类。

* @Import注解在配置类中只能写一次

###### 注解开发实现为第三方bean注入资源:

 1.注入 **简单数据类型**:

①:类中提供四个属性

```java
public class JdbcConfig {
    private String driver;
    private String url;
    private String userName;
    private String password;
    
	@Bean // 设置该方法的返回值为spring管理的bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

②:使用`@Value`注解引入值

```java
public class JdbcConfig {
    @Value("com.mysql.jdbc.Driver")
    private String driver;
    @Value("jdbc:mysql://localhost:3306/spring_db")
    private String url;
    @Value("root")
    private String userName;
    @Value("password")
    private String password;
    
	@Bean // 设置该方法的返回值为spring管理的bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```



2.注入 **引用数据类型**:

​     ==引用类型注入只需要为bean定义方法设置形参即可，容器会根据类型自动装配对象。==

①:在SpringConfig中扫描BookDao

```java
@Configuration//配置类:@Configuration替换`applicationContext.xml`文件
@ComponentScan("com.itheima.dao") //包扫描
@Import({JdbcConfig.class}) //导入
public class SpringConfig {
}
```

②:在JdbcConfig类的方法上添加参数

```java
@Bean
public DataSource dataSource(BookDao bookDao){
    System.out.println(bookDao);
    DruidDataSource ds = new DruidDataSource();
    ds.setDriverClassName(driver);
    ds.setUrl(url);
    ds.setUsername(userName);
    ds.setPassword(password);
    return ds;
}
```



#### Spring整合:

##### Spring整合Mybatis:

###### 1.在pom.xml添加依赖

```xml
<dependencies>
    <dependency>    <!-- spring jar-->
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>  <!-- 德鲁伊 jar-->
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>

    <dependency> <!-- mybatis jar-->
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>

    <dependency> <!-- mysql jar-->
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>

    <!-- spring操作数据库专用 jar包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.22</version>
    </dependency>

    <!--MyBatis与Spring整合专用 jar包
        这个jar包mybatis在前面，是Mybatis提供的-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.0</version>
    </dependency>
    <!--注意：上面的mybatis 和mybatis-spring jar包有对应的关系的，不要乱写。 -->
</dependencies>
```

###### 2:创建Spring的主配置类

```java
@Configuration //设置为配置类
@ComponentScan("com.itheima")//包扫描，，主要扫描的是项目中的AccountServiceImpl类
@PropertySource("classpath:jdbc.properties") // 加载properties文件
@Import(JdbcConfig.class) //导入
public class SpringConfig {
    
}

```

###### 3:创建数据源的配置类

```java
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

###### 4:加载properties文件

```properties
// 主配置类中读properties并引入数据源配置类
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/dp1
jdbc.username=root
jdbc.password=123456
```



##### Spring整合Junit:

###### 1.在pom.xml导入依赖 

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>
```

###### 2.在test\java下创建一个AccountServiceTest类

```java
//设置类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//设置Spring环境对应的配置类
@ContextConfiguration(classes = {SpringConfiguration.class}) //加载配置类
//@ContextConfiguration(locations={"classpath:applicationContext.xml"})//加载配置文件
public class AccountServiceTest {
    //支持自动装配注入bean
    @Autowired
    private AccountService accountService;
    
    @Test
    public void testFindById(){
        System.out.println(accountService.findById(1));
    }
    
    @Test
    public void testFindAll(){
        System.out.println(accountService.findAll());
    }
}
```

**注意:**

* 单元测试，如果测试的是注解配置类，则使用`@ContextConfiguration(classes = 配置类.class)`
* 单元测试，如果测试的是配置文件，则使用`@ContextConfiguration(locations={配置文件名,...})`
* Junit运行后是基于Spring环境运行的，所以Spring提供了一个专用的类运行器，这个务必要设置，这个类运行器就在Spring的测试专用包中提供的，导入的坐标就是这个东西`SpringJUnit4ClassRunner`



### 自定义注解：

#### 1.定义业务操作类型和操作人类别泛型：

```java
package com.qsl.system.enums;

/*** 泛型-业务操作类型 */
public enum BusinessType {

    /**
     * 其它
     */
    OTHER,

    /**
     * 新增
     */
    INSERT,

    /**
     * 修改
     */
    UPDATE,

    /**
     * 删除
     */
    DELETE,

    /**
     * 授权
     */
    ASSGIN,

    /**
     * 导出
     */
    EXPORT,

    /**
     * 导入
     */
    IMPORT,

    /**
     * 强退
     */
    FORCE,

    /**
     * 更新状态
     */
    STATUS,

    /**
     * 清空数据
     */
    CLEAN,

}
```



```java
package com.qsl.system.enums;

/**
 * 泛型-操作人类别
 */
public enum OperatorType {
    /**
     * 其它
     */
    OTHER,

    /**
     * 后台用户
     */
    MANAGE,

    /**
     * 手机端用户
     */
    MOBILE

}
```

#### 2.定义注解：

```java
package com.qsl.system.enums;
/**
 * 自定义注释-自定义操作日志记录注解
 */
@Target({ElementType.PARAMETER,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface LogAnnotation {

    /**
     * 模块
     */
    public String title() default "";

    /**
     * 功能
     */
    public BusinessType businessType() default BusinessType.OTHER;

    /**
     * 操作人类别
     */
    public OperatorType operatorType() default OperatorType.MANAGE;

    /**
     * 是否保存请求的参数
     */
    public boolean isSaveRequestData() default true;

    /**
     * 是否保存响应的参数
     */
    public boolean isSaveResponseData() default true;
}
```

#### 使用注解：

```java
@Api(tags = "角色管理接口")
@RestController
@RequestMapping("/admin/system/sysRole")
public class SysRoleController {

    @ApiOperation("根据Id删除")
    @DeleteMapping("/remove/{Id}")
    @PreAuthorize("hasAnyAuthority('bnt.sysRole.remove')") //根据数据库判断是否有该权限
    @LogAnnotation(title = "角色管理",businessType = BusinessType.DELETE) // 自定义注解-操作日志—新增
    public Result removoRole(@PathVariable String Id) {
        System.out.println(Id);
        boolean flag = sysRolService.removeById(Id);

        if (flag) {
            return Result.ok();
        } else {
            return Result.fail();
        }
    }
}
```



#### 注解参数说明：

```java
@Target
@Documented // 修饰的注解类会被JavaDoc工具提取成文档。
@Inherited  // 指定该注解可以被继承。
```

ElementType枚举类说明：

| 名称            | 说明                         |
| --------------- | ---------------------------- |
| TYPE            | 用于类、接口及枚举           |
| FIELD           | 用于成员变量（包含枚举常量） |
| METHOD          | 用于方法                     |
| PARAMETER       | 用于形式参数                 |
| CONSTRUCTOR     | 用于构造函数                 |
| LOCAL_VARIABLE  | 用于局部变量                 |
| ANNOTATION_TYPE | 用于注解类型                 |
| PACKAGE         | 用于包                       |
| TYPE_PARAMETER  | 用于类型参数（JDK1.8+）      |
| TYPE_USE        | 用于类型使用（JDK1.8+）      |
| MODULE          | 用于模块（JKD 9+）           |



# AOP:

ApringAOP本质：代理模式。

### AOP概念：

#### 1.什么是AOP

- ​    AOP(Aspect Oriented Programming)面向切面编程，一种编程范式，指导开发者如何组织程序结构（AOP编程思想）。

* ​    OOP(Object Oriented Programming)面向对象编程（OOP编程思想）。

#### 2.AOP作用：

​        AOP是**在不惊动(改动)原有设计(代码)**的前提下对其功能进行增强。

#### 3.AOP核心概念：

两个核心概念：

* 目标对象(Target)：原始功能去掉共性功能对应的类产生的对象，这种对象是无法直接完成最终工作的。
* 代理(Proxy)：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/AOP%E6%A6%82%E5%BF%B5.png)

​      Spring的理念：无入侵式/无侵入式。

- ​      代理（Proxy）：SpringAOP的核心本质是采用代理模式实现的。

- ​      连接点(JoinPoint)：程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等。   （在SpringAOP中，理解为任意方法的执行）
- ​     切入点(Pointcut):匹配连接点的式子，也是具有共性功能的方法描述。
- ​     通知(Advice):在切入点处执行的操作，也就是共性功能。 （在SpringAOP中，功能最终以方法的形式呈现）
- ​    切面(Aspect):描述通知与切入点的对应关系。
- ​     目标对象(Target): 被代理的原始对象成为目标对象。

### AOP基本使用：

```java
 @EnableAspectJAutoProxy //开启Spring对AOP注解驱动支持
 @Aspect //定义为AOP
 @Pointcut("execution(void com.qsl.dao.impl.BookDaoImpl.update())")   // 定义切点
 @Before("pt()")   // 定义切面
```



#### 说明：对BookDaoImpl的update添加AOP增强

```java
@Component  //定义为Bean标签
public class BookDaoImpl implements BookDao {

    @Override
    public void save() {
        System.out.println("启动秒数："+System.currentTimeMillis());
        System.out.println("book dao save");
    }

    @Override
    public void update() {
        System.out.println("book dao update");
    }
}
```

#### 1.添加依赖

```xml
  <!-- Aspectj jar包-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.9</version>
        </dependency>

        <!-- AOP jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>5.3.22</version>
        </dependency>
```

#### 2.在SpringConfig中`开启Spring对AOP注解驱动支持`

```java
@Component //配置Bean
@ComponentScan("com.qsl")  //包扫描
@EnableAspectJAutoProxy //开启Spring对AOP注解驱动支持
public class SpringConfig {
}
```

#### 3.增强BookDaoImpl的update

```java
@Component //定义为Bean
@Aspect //定义为AOP
public class MyAdvice {

    // 1.定义切点 (接口 和实现类都行)
    @Pointcut("execution(void com.qsl.dao.BookDao.update())")
//    @Pointcut("execution(void com.qsl.dao.impl.BookDaoImpl.update())")
    private void pt() {
    }

    // 2.定义切面
    @Before("pt()")
    public void method() {
        System.out.println("启动秒数：" + System.currentTimeMillis());
    }
}
```

#### 4.测试

```java
public class Demo {
    public static void main(String[] args) {
        //1. 创建IOC容器
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        //2. 获取Bean
        BookDao bean = ctx.getBean(BookDao.class);
        bean.update();
    }
}
```





### AOP切入点表达式：

​    切入点：要进行增强的方法。

​    切入点表达式：要进行增强的方法的描述方式。

​    切入点表达式作用：用于快速描述，范围描述。

1.语法格式：

​    切入点表达式标准格式：动作关键字 （访问修饰符  返回值  包名./类/接口名.方法名（参数）异常名）

​     例： execution (private User com.qsl.service.UserService.selectById(int ))

2.通配符：

```java

1.  < * > :单个独立的任意符号，可独立出现，也可作为前缀或者后缀的匹配符出现。
2.  < .. > :多个连续的任意符号，可独立出现，常用于 简化包名与参数的书写。
3.  < + >  :专用于匹配子类类型。
    execution (public * com.qsl.*.UserService.find*(*))
    execution (public User com..UserService.findById(..))
    execution (* *..*Service+.*(..))
    
    最终效果：execution (void com..*Dao+.u*(..) //取BookDaoImpl实现类的update()
  // @Pointcut ：切点     execution():切点表达式
  @Pointcut("execution(void com.qsl.dao.impl.BookDaoImpl.update())")
```

描述方式：**接口** 和**实现类**都行

```java
<!-- 方式1： 执行 域名反写下的BookDao接口中无参数update方法-->
     execution（void com.qsl.dao.BookDao.update()）
    
<!-- 方式2：执行 域名反写下的BookDaoImpl实现类中无参数update方法-->
     execution（void com.qsl.dao.impl.BookDaoImpl.update()）
```

3.技巧：

- 所有代码按照标准规范开发，否则以下技巧全部失效

- 描述切入点通**==常描述接口==**，而不描述实现类,如果描述到实现类，就出现紧耦合了

- 访问控制修饰符针对接口开发均采用public描述（**==可省略访问控制修饰符描述==**）

- 返回值类型对于增删改类使用精准类型加速匹配，对于查询类使用\*通配快速描述

- 减少使用 ..的形式描述包

- **对接口进行描述**，使用*表示模块名，例如UserService书写成\*Service，绑定业务层接口名

- **==方法名==**书写以**==动词==**进行**==精准匹配==**，名词采用*匹配，例如getById书写成getBy*,selectAll书写成selectAll

- 参数根据业务方法灵活调整

- 通常**==不使用异常==**作为**==匹配==**规则

  

### AOP通知类型：

AOP通知共为5种：

1. 前置通知 ：  @Before
2. 后置通知 :  @After
3. 环绕通知(重点) ： @Around   （没返回值报异常：AopInvocationException）
4. 返回(对象)后通知（了解） ：@AfterReturning   (在没异常的时候才能运行)
5. 异常后（运行）通知（了解） ：@AfterThrowing   (原始方法异常后通知) 

   注意： **后置通知**是不管原始方法有没有抛出异常都会被执行。

```java
@Component //定义为Bean标签
@Aspect   //定义为AOP
public class AspectJ {

    /* 定义切点 */
    @Pointcut("execution (void  com.qsl.doa.BookDao.save())")
    public void savePt() {
    }

    @Pointcut("execution (int  com.qsl.doa.BookDao.selectAll())")
    public void selectPt() {
    }

    /**
     * 定义切面
     * 1.前置通知： @Before
     * 2.后置通知： @After
     * 3.环绕通知(重点) ： @Around 
     * 4.返回(对象)后通知（了解） ：@AfterReturning   
     * 5.异常后通知（了解） ：@AfterThrowing 
     */

    /* 1.前置通知： @Before */
    @Before("savePt()")
//    @Before("AspectJ.pt()")  //也行,但不建议
    public void before() {
        System.out.println("前置通知(Before) advice...");
    }

    /*  2.后置通知:  @After (注意： 后置通知是不管原始方法有没有抛出异常都会被执行。) */
    @After("selectPt()")
    public void after() {
        System.out.println("后置通知(After)  advice...");
    }

    /*  3.环绕通知: @Around */
    @Around("selectPt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕通知（Around）前 ");
        //表示对原始操作的调用
        Integer proceed = (Integer) pjp.proceed();
        System.out.println("环绕通知（Around）后");
        
        Signature signature = pjp.getSignature();
        String Typename = signature.getDeclaringTypeName(); //获取类名
        String name = signature.getName();//获取方法
        
        return proceed+566;
        //没返回值报异常：AopInvocationException
    }

    /* 4.返回后通知：@AfterReturning () */
    @AfterReturning("selectPt()")
    public int afterReturning() {
        System.out.println("（返回后通知）AfterReturning ... ");
        return 300;
    }

    /* 5.异常后通知 (原始方法报错才出现) */
    @AfterThrowing("selectPt()")
    public  void AfterThrowing(){
        System.out.println("异常后通知(AfterThrowing)   ...");
    }
}

```

==**环绕通知注意事项**==

1. 环绕通知必须依赖形参ProceedingJoinPoint才能实现对原始方法的调用，进而实现原始方法调用前后同时添加通知
2. 通知中如果未使用ProceedingJoinPoint对原始方法进行调用将跳过原始方法的执行
3. 对原始方法的调用可以不接收返回值，通知方法设置成void即可，如果**接收返回值**，必须设为Object类型
4. 原始方法的返回值如果是void类型，通知方法的返回值类型可以设置成void,也可以设置成Object
5. 由于无法预知原始方法运行后是否会抛出异常，因此环绕通知方法**必须要处理Throwable异常**



### AOP获取参数：



###### 获取参数：

1.前置通知：@Before

2.环绕通知: @Around 

- JoinPoint: 适用于前置、后置、返回后、抛出异常后通知，设置为方法的第一个形参。
- ProceedJointPoint:适用于环绕通知。

   使用JoinPoint的方式获取参数适用于`前置`、`后置`、`返回后`、`抛出异常后`通知。

```java
@Component //定义为Bean标签
@Aspect   //定义为AOP
public class AspectJ { 

  /* 定义切点 */
    @Pointcut("execution ( * com.qsl.doa.BookDao.save(..))")
    public void pt() {}
    
    // 方法1：前置通知获取参数
    /* 定义切面 ---> 前置通知： @Before */
    @Before("pt()")
    public String before(JoinPoint jp) {
        Object[] args = jp.getArgs(); //获取参数
        System.out.println(Arrays.toString(args));

        System.out.println("前置通知(Before) advice...");
        return null;
    }

    //方法2：环绕通知获取参数
      /* 定义切面 ---> 环绕通知: @Around */
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        Object[] args = pjp.getArgs();//获取参数
        System.out.println("获取参数:" + Arrays.toString(args));

        System.out.println("环绕通知（Around）前 ");
        //表示对原始操作的调用
        pjp.proceed();
        System.out.println("环绕通知（Around）后");
        return null;
    }
}
```

###### 获取返回值：

1. 环绕 Around
2. AfterReturing



```java
  @Component //定义为Bean标签
@Aspect   //定义为AOP
public class AspectJ { 

  /* 定义切点 */
    @Pointcut("execution ( * com.qsl.doa.BookDao.save(..))")
    public void pt() {}
  
    //方法1：环绕通知获取返回值
    /* 定义切面 --->  环绕通知: @Around */
    @Around("pt()")
    public Object around2(ProceedingJoinPoint pjp) throws Throwable {

        System.out.println("环绕通知（Around）前 ");
        //表示对原始操作的调用
        Object proceed = pjp.proceed();
        return proceed;
    }
    
       // 方法2：
    /* 返回后通知:  @After (注意： 后置通知是不管原始方法有没有抛出异常都会被执行。) */
    @AfterReturning(value = "pt()", returning = "ret")
    public void after(Object ret) {
        System.out.println("后置通知(After)  advice..." + ret);
    }
    
}
```



###### 获取异常(了解)：

```java
@Component //定义为Bean标签
@Aspect   //定义为AOP
public class AspectJ { 

  /* 定义切点 */
    @Pointcut("execution ( * com.qsl.doa.BookDao.save(..))")
    public void pt() {}
  
      /* 方法1： 环绕通知: @Around */
    @Around("pt()")
    public Object Aound2(ProceedingJoinPoint pjp) {
        Object ret = null;

        try {
            ret = pjp.proceed();
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println(throwable);
        }
        System.out.println("（环绕通知）后 ... ");

        return ret;
    }

    /* 方法2： 异常后通知(原始方法报错才出现)*/
    @AfterThrowing(value = "pt()", throwing = "t")
    public void AfterThrowing(Throwable t) {
        System.out.println("异常后通知(AfterThrowing)   ..." + t);
    }
}
```



### 整合AspectJ

​      XML 和 注解开发，XML的没有自己找。

​    AspectJ是AOP思想的一个具体实现，Spring有自己的AOP实现，但是相比于AspectJ来说比较麻烦，所以我们直接采用Spring整合ApsectJ的方式进行AOP开发。

```java
@EnableAspectJAutoProxy //开启Spring对AOP注解驱动支持
@Aspect   //设置当前类为AOP切面类
@Pointcut("execution(* com.qsl.dao.BookDao.*d*(..))") //定义切入点表达式

 // 定义切面    
@Before()  //1.前置通知
@After     //2.后置通知
@Around(切入点方法名)  //3.环绕通知(重点) 
@AfterReturning       //4.返回(对象)后通知（了解）
@AfterThrowing        //5.异常后通知（了解）

```

​     原始对象就是最开始的对象，目标对象是你类里面有别AOP的方法，去掉AOP的方法后的对象。

#### AspectJ的基本使用：

1.在pom.xml中导入依赖

```xml
   <!-- spring jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.22</version>
        </dependency>

        <!-- Aspectj jar包-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.9</version>
        </dependency>

        <!-- AOP jar包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>5.3.22</version>
        </dependency>
```

2.开始Spring对AOP注解的支持

```java
@Configuration //设置配置类
@ComponentScan("com.qsl")  //包扫描
@EnableAspectJAutoProxy //开启Spring对AOP注解驱动支持
public class SpringConfig {

}
```

2.定义BookDao和BookDaoImpl实现类

```java
public interface BookDao {
    
    public void save();
    public  void update();
}

@Component  //定义为Bean标签
public class BookDaoImpl implements BookDao {

    @Override
    public void save() {
        System.out.println("启动秒数："+System.currentTimeMillis());
        System.out.println("book dao save");
    }

    @Override
    public void update() {
        System.out.println("book dao update");
    }
}
```

4.创建MyAdvice类，并把他定义为AOP

```java
@Component //定义为Bean标签
@Aspect //定义为AOP
public class MyAdvice {

    // 定义切点 (接口 和实现类都行)
    @Pointcut("execution(void com.qsl.dao.BookDao.update())")
  //@Pointcut("execution(void com.qsl.dao.impl.BookDaoImpl.update())")
    private void pt() { }

    // 定义切面
    @Around("pt()")
    public void method(ProceedingJoinPoint pjp) {
        System.out.println("启动秒数：" + System.currentTimeMillis());
        //表示对原始操作的调用
         Object ret = pjp.proceed();
    }
}

```

5.创建IOC容器并使用AOP

```java
public class Demo {
    public static void main(String[] args) {
        //1. 创建IOC容器
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        //2. 获取Bean
        BookDao bean = ctx.getBean(BookDao.class);
        bean.update();

        /*System.out.println(bean);
        System.out.println(bean.getClass());*/
    }
}
```



# Spring事务：

事务作用：在数据层保障一系列的数据库操作同成功同失败。

Spring事务作用：在数据层或**==业务层==**保障一系列的数据库操作同成功同失败。



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E5%91%98%E4%B8%8E%E4%BA%8B%E5%8A%A1%E5%8D%8F%E8%B0%83%E5%91%98%E7%9A%84%E5%8C%BA%E5%88%AB.png)

- 事务管理员：发起事务方，在Spring中通常指代业务层开启事务的方法
- 事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法

==注意:==目前的事务管理是基于`DataSourceTransactionManager`和`SqlSessionFactoryBean`使用的是同一个数据源。

```java

@Transactional   :开启事务
@EnableTransactionManagement: 开启Spring对事务的驱动
```

   Spring注解事务通常添加在**业务层接口中**而不会添加到业务层实现类中，降低耦合，注解式事务可以添加到业务方法上表示当前方法开启事务，也可以添加到接口上表示当前接口所有方法开启事务。

### Spring事务的基本使用：



1.开启Spring对事务驱动

```java
@Configuration
@ComponentScan("com.qsl")
@PropertySource("jdbc.properties") //加载properties
@EnableTransactionManagement //开启Spring对事务驱动
@Import({jdbcConfig.class, MyBatisConfig.class}) //导入
public class SpringConfig {

}
```

2.在JdbcConfig类中配置事务管理器

​    **注意：**事务管理器要根据使用技术进行选择，Mybatis框架使用的是JDBC事务，可以直接使用DataSourceTransactionManager。

```java
public class jdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String root;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource() {
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(root);
        ds.setPassword(password);

        return ds;
    }

    //配置事务管理器，mybatis使用的是jdbc事务
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource) {
        DataSourceTransactionManager dtm = new DataSourceTransactionManager();
        dtm.setDataSource(dataSource);
        return dtm;
    }
}
```

3.开启事务

​     **注意**:@Transactional可以写在接口上（推荐）、接口类上、接口方法上、实现类上和实现类方法上。

- 写在接口类上，该接口的所有实现类的所有方法都会有事务。
- 写在接口方法上，该接口的所有实现类的该方法都会有事务。
- 写在实现类上，该类中的所有方法都会有事务。
- 写在实现类方法上，该方法上有事务。

```java
@Service("AccountServiceId")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;

    /**
     * 转账操作
     * @param out   传出方
     * @param in    转入方
     * @param money 金额
     */
    @Override
    @Transactional //开启事务
    public void transfer(String out, String in, Double money) {
        //收账
        accountDao.inMoney(out, money);

        if (true) {
            throw new NullPointerException("自定义异常");
        }

        // 转账
        accountDao.outMoney(in, money);
    }
}
```

### Spring事务属性：

#### 事务配置：

 1.开启事务:   @Transactional

```java
    @Transactional(rollbackFor = {IOException.class})
 public void shiwu(){};
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/Spring%E4%BA%8B%E5%8A%A1%E9%85%8D%E7%BD%AE.png)

以上异常都可在@Transactional注解的参数上进行设置：

* readOnly：true只读事务，false读写事务；增删改要设为false,查询为true。

* timeout:设置超时时间单位秒，在多长时间之内事务没有提交成功就自动回滚，-1表示不设置超时时间。

* rollbackFor:当**出现指定异常**进行事务回滚。

* noRollbackFor:当出现指定异常不进行事务回滚。

* isolation设置事务的隔离级别

  * DEFAULT   :默认隔离级别, 会采用数据库的隔离级别
  * READ_UNCOMMITTED : 读未提交
  * READ_COMMITTED : 读已提交
  * REPEATABLE_READ : 重复读取
  * SERIALIZABLE: 串行化  

   Spring的事务只会对`Error异常`和`RuntimeException异常`及其子类进行事务回顾，其他的异常类型是不会回滚的，例IOException不符合上述条件所以不回滚。
  
  

##### **事务传播行为的可选值：**

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/Spring%E4%BA%8B%E5%8A%A1%E4%BC%A0%E6%92%AD%E8%A1%8C%E4%B8%BA%E7%9A%84%E5%8F%AF%E9%80%89%E5%80%BC.png)

SUPPORTS:支持事务。

NOT-SUPPORTED：不支持事务。

MANDATORY：必须携带事务，否则包ERROR错。

NEVER: 不能携带事务，否则包ERROR错。

**事务传播行为图解**：REQUIRES_NEW,其他类似。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring/%E4%BA%8B%E5%8A%A1%E4%BC%A0%E6%92%AD%E8%A1%8C%E4%B8%BA.png)







加密：  

1.    对称加密： AES
2.   非对象加密：  RAS

建网站：

1.    向CA申请证书
2.    网站配置CA给的私钥证书
3.    浏览器需要从CA获取网站对应

# crm的技术架构：

- 视图层（view）:展示数据，跟用户交互。

  ​    使用技术： html,css,js,jquery,bootstrap(ext | easyUI),jsp 。。。

- 控制层（Controller）:控制业务处理流程（接收请求，接收参数，封装参数；根据不同的请求调用业务层）

  ​     技术：servlet,springMVC（，webwork,struts1, struts2）

- 业务层（Service）:处理业务逻辑。（处理业务的步骤以及操作的原子性）

  ​        1.添加学生   2.记录操作日志

     技术：JAVASE（工作流：activiti | JBPM）

    **注意**：业务层一定要制作测试类测试功能。

- 持久层/数据层（Dao/Mapper）: 操作数据库

     技术：jdbc，Mybatis（，hibernate,ibatis）
    
- 整合层：维护类资源，维护数据库资源

  spring(IOC,AOP), ejb,corba
  
- 

    
























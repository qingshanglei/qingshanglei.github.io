# 概念：

MyBatis-Plus框架:概念框架基于MyBatis框架基础上开发的**`增强型工具`**，旨在简化开发、提高效率。(简化dao层和service层)

开发模式：

- 基于MyBatis使用MyBatisPlus
- 基于Spring使用MyBatisPlus
- 基于SpringBoot使用MyBatisPlus *

mp流程图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/mp%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

# MP的dao层基本使用：

1.添加依赖

```xml
<dependency>          <!-- MyBatisPlus-->
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>	
</dependency>

<dependency>        <!-- 德鲁伊-->
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

2.编写程序

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/0.png)



```yml
spring: # 配置数据源信息 
  datasource:  # 配置数据源类型
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/dp1?characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource  # 配置连接数据库信息 
```

# Service层基本使用：

​    使用MP提供的业务层通用接口ISerivce<T> ，与业务层通用实现类ServiceImpl<M,T>

   使用的查询是Mybatis-plus的自动生成,连接数据库省略...

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/MP%25E7%259A%2584Service%25E5%25B1%2582%25E5%259F%25BA%25E6%259C%25AC%25E4%25BD%25BF%25E7%2594%25A8.png)



# 表名映射与字段映射(数据库与实体类)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E6%2595%25B0%25E6%258D%25AE%25E5%25BA%2593%25E8%25A1%25A8%25E4%25B8%258E%25E5%25AE%259E%25E4%25BD%2593%25E7%25B1%25BB%25E8%25A1%25A8%25E5%2585%25B3%25E7%25B3%25BB.png)

```java
// 实体类
@Data // 自动生成 setter、getter、toString、equals、hashCode等方法
@TableName("tb_brand")  //实体类名对应数据库的表名
public class brand {
    private Integer id;
    @TableField(value = "brand_name")
    private String brandName;
    /** value： 设置数据库表字段名称， 为实体类名称        select：是否查询此字段 */
    @TableField(value = "company_name", select = false)
    private String companyName;
    private Integer ordered;
    private String description;
    private Integer status;
    /** 设置该属性在数据库表字段中是否存在 */
    @TableField(exist = false)
    private Integer online; // 是否在线
     /** 逻辑删除（0:不可用 1:可用）*/
    @TableField("is_deleted")
    private Integer isDeleted;
}
```





# 总结：

|             |                              |
| ----------- | ---------------------------- |
| @TableName  | 表名注解，标识实体类对应的表 |
| @TableId    | 主键注解                     |
| @TableField | 字段注解（非主键）           |
| @TableLogic | 逻辑删除                     |
|             |                              |
|             |                              |

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/MyBatisPlus%25E6%2580%25BB%25E7%25BB%2593.png)

yml:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/MyBatisPlus%25E6%2580%25BB%25E7%25BB%25932.png)

在Dao层代理：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/MyBatisPlus.png)

批量查询和 批量删除，其他类型

```java
@SpringBootTest
class MyBatisPlus01QuickstartApplicationTests {

    @Autowired //按类型注入
    private UserDao userDao;
    
    // 批量删除
    @Test
    void delAll() {
        ArrayList<Long> ids = new ArrayList<>();
        ids.add(10L);
        ids.add(11L);
        userDao.deleteBatchIds(ids);
    }

    // 批量查询
    @Test
    void selAll() {
        ArrayList<Long> ids = new ArrayList<>();
        ids.add(1L);
        ids.add(2L);
        userDao.selectBatchIds(ids);
    }
}
```

- Wrapper：用来构建条件查询的条件，目前我们没有可直接传为Null
- List<T>:因为查询的是所有，所以返回的数据是一个集合



```java
@TableName  //设置当前类对应于数据库表关系
```





# DQL查询表数据：

## 新增：

```java
@SpringBootTest
public class SysRoleMapperTest {

    @Autowired
    private SysRoleMapper sysRoleMapper;
    
    @Test
    public void testInsert(){
        SysRole sysRole = new SysRole();
        sysRole.setRoleName("角色管理员");
        sysRole.setRoleCode("role");
        sysRole.setDescription("角色管理员");

        int result = sysRoleMapper.insert(sysRole);
        System.out.println(result); //影响的行数
        System.out.println(sysRole.getId()); //id自动回填
    }
}    
```





## 删除：

单个：

```java
@SpringBootTest
public class SysRoleTest {

    @Autowired
    private SysRoleMapper sysRoleMapper;
    
    /* 根据Id删除*/
    @Test
    void delAll() {
        sysRoleMapper.deleteById(2);
    }   
}
```



多个：

```java
@SpringBootTest
public class SysRoleTest {

    @Autowired
    private SysRoleMapper sysRoleMapper;
    /* 根据Id批量删除*/
    @Test
    void delAll() {
        sysRoleMapper.deleteBatchIds(Arrays.asList(1,2,3));
    }   
}
```



## 修改：

```java
@Test
public void testUpdateById(){
    SysRole sysRole = new SysRole();
    sysRole.setId(1L);
    sysRole.setRoleName("角色管理员1");

    int result = sysRoleMapper.updateById(sysRole);
    System.out.println(result);

}
```





## 查询：

```java
@SpringBootTest
public class SysRoleMapperTest {

    @Autowired
    private SysRoleMapper sysRoleMapper;

    @Test
    public void testSelectList() {
        System.out.println(("----- selectAll method test ------"));
        //UserMapper 中的 selectList() 方法的参数为 MP 内置的条件封装器 Wrapper
        List<SysRole> users = sysRoleMapper.selectList(null);
       	for (SysRole sysRole : sysRoles) {
           System.out.println("sysRole = " + sysRole);
        }
    }
}
```



## Wrapper(条件判断):

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/QueryWrapper.png)

```java
  QueryWrapper qw = new QueryWrapper(); // 方法1
  LambdaQueryWrapper<user> lqw = new LambdaQueryWrapper(); // 方法2：根据Lambod表达式查询
     ql.like(String column,Object val)  //模糊查询 
       .like(boolean condition, R column, Object val)//模糊查询
       .gt(String column,Object val) // 数据大于多少
       .ge(String column,Object val) //大于等于
       .le(String column,Object val)  // 数据小于等于
       .between(String column,Object val2,val2) // 数据在什么之间
     排序  
       .eq(Stringcolumn, Object val) //等于
       .orderByDesc(String column)    // 降序
       .orderByAsc(String column)    // 升序
     并且，或
       .and()  //并且(默认)
       .and(Consumer<QueryWrapper<user>> consumer)  //并且(里面放的是lombad表达式，而表达式的条件优先执行)
       .or()  //或
       .or(Consumer<QueryWrapper<user>> consumer)  //或(里面放的是lombad表达式，而表达式的条件优先执行) 
       .isNotNull(String column)  // 查询数据不为空
       .isNull(String column)  // 查询数据为空
      子查询：   
       .inSql(Stirng column,Stirng inValue)   //子查询： column数据库字段  inValue:要执行的代码
     
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%AD.png)

#### 条件查询：

查询年龄小于18的。

##### 1.按条件查询

```java
@Test
void getById() {
    QueryWrapper qw = new QueryWrapper();
    qw.lt("age", 18); // 年龄小于18
    List<user> users = userDao.selectList(qw);
    System.out.println(users);
}

@Test
public void testQueryWrapper() {
    QueryWrapper<SysRole> queryWrapper = new QueryWrapper<>();
    queryWrapper.ge("role_code", "role");
    List<SysRole> users = sysRoleMapper.selectList(queryWrapper);
    System.out.println(users);
}
```

##### 2.lombok表达式-按条件查询 *

```java
// 方法2 *
@Test
void getById() {
    QueryWrapper<user> QW = new QueryWrapper<>();
    QW.lambda().lt(user::getAge,14); // 小于14
    List<user> userList = userDao.selectList(QW);
    System.out.println(userList);
}

// 方法3 *
@Test
void getById() {
    LambdaQueryWrapper<user> lqw = new LambdaQueryWrapper();
    lqw.lt(user::getAge, 14);        // 年龄小于14
    List<user> LQUser = userDao.selectList(lqw);
    System.out.println(LQUser);
} 
```

#### 聚合查询：

count:总记录数

max:最大值，     min:最小值

avg:平均值，       sum:求和

```java
@SpringBootTest
class Mybatisplus02DqlApplicationTests {

    @Autowired //按类型注入
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        //lqw.select("count(*) as count");
        //SELECT count(*) as count FROM user
        //lqw.select("max(age) as maxAge");
        //SELECT max(age) as maxAge FROM user
        //lqw.select("min(age) as minAge");
        //SELECT min(age) as minAge FROM user
        //lqw.select("sum(age) as sumAge");
        //SELECT sum(age) as sumAge FROM user
        lqw.select("avg(age) as avgAge");
        //SELECT avg(age) as avgAge FROM user
        List<Map<String, Object>> userList = userDao.selectMaps(lqw);
        System.out.println(userList);
    }
}
```

#### 分组查询：

```java
@SpringBootTest
class Mybatisplus02DqlApplicationTests {

    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("count(*) as count,tel");
        lqw.groupBy("tel");
        List<Map<String, Object>> list = userDao.selectMaps(lqw);
        System.out.println(list);
    }
}
```



#### or()和and()优先级

```java
@SpringBootTest
class Mybatisplus02DqlApplicationTests {

    @Autowired
    private UserDao userDao;
    
    // 将用户名中包含有a并且（年龄大于20或邮箱为null）的用户信息修改
    @Test
    void testGetAll(){
        QueryWrapper<User> qw = new QueryWrapper<User>();
          qw.like("user_name","a")
            .and(i->i.gt("age",20).or().isNull("email"));
          // and里的lombad表达式优先执行
          User user=new User();
             user.setName("张三");
            int up=userDao.update(user,qw);
          System.out.println("up:"+up);  
    }
}
```



**注意:**

* 聚合与分组查询，无法使用lambda表达式来完成
* MyBatisPlus只是对MyBatis的增强，如果MyBatisPlus实现不了，我们可以直接在**`dao接口`**中使用MyBatis的方式实现

#### 子查询：

子查询的区别：

1. inSql：同等于${}，存在SQL注入隐患。
2. in：同等于#{}，不存在SQL注入隐患。



##### insql:

```java
inSql(Stirng column,Stirng inValue)   //子查询： column数据库字段  
```

```java
@SpringBootTest
class Mybatisplus02DqlApplicationTests {

    @Autowired
    private UserDao userDao;

    // 查询id小于等于100的用户信息
    @Test
    void testGetAll(){
        QueryWrapper qw=  new QueryWrapper<>();
        qw.inSql("id","select id form bl_user where id <= 100");
        List<user> list= userDao.selectList<qw>;
        list.forEach(System.out::println);
    }
}
```

###### 模拟Controller层

```java
@Test()
void test10（）{
    Stirng username="a"; //模拟参数
    Integer ageBegin = null;
    Integer ageEnd=30;
    QueryWrapper<user> qw=new QueryWrapper<>();
     qw.like(StringUtils.isNotBlank(username),"user_name",username)
        .ge(ageBegin!=null,"age",ageBegin) // 大于等于
        .le(ageEnd！=null,"age",ageEnd) //小于等于
      List<user> list=userMapper.selectList(qw);
      List.forEach(System.out::println);
}
```

##### in:

```java
@Service
public class ActivityRuleServiceImpl extends ServiceImpl<ActivityRuleMapper, ActivityRule> implements ActivityRuleService {

    // 根据优惠卷ID子查询activity_rule表,并根据优惠卷金额和数量分组。
    @Override
    public List<ActivityRule> getActivityRuleBy(Set<Long> activityIdSet) {
        LambdaQueryWrapper<ActivityRule> lqw = new LambdaQueryWrapper<>();
        lqw.in(ActivityRule::getActivityId, activityIdSet);
        lqw.orderByDesc(ActivityRule::getConditionAmount, ActivityRule::getConditionNum);
       return  baseMapper.selectList(lqw);
        
//        SELECT * FROM activity_rule
//        WHERE activity_id IN (SELECT activity_id FROM activity_sku)
//        ORDER BY
//        condition_amount DESC,
//        condition_num DESC
    }
}
```



# DML：操作表数据

不同的表应用不同的id生成策略

* 日志：自增（1,2,3,4，……）

* 购物订单：特殊规则（FQ23948AK3843）

* 外卖单：关联地区日期等信息（10 04 20200314 34 91）

* 关系表：可省略id

* ……

  

id生成策略控制：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/id%25E7%2594%259F%25E6%2588%2590%25E7%25AD%2596%25E7%2595%25A5.png)

- AUTO（0）： 使用数据库Id自增策略控制id生成（默认）
- NONE (1) : 不设置id生成策略
- INPUT（2）: 用户手工输入id
- ASSIGN_ID（3）： 雪花算法生成id（可兼容数值型与字符串型）
- ASSIGN_UUID（4）： 以UUID生成算法作为id生成策略。

雪花算法：

雪花算法(SnowFlake),是Twitter官方给出的算法实现 是用Scala写的。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E9%259B%25AA%25E8%258A%25B1%25E7%25AE%2597%25E6%25B3%2595Id.png)

### Id策略生成器(Insert)：

#### Id策略生成器`全局配置`

在 yml文件配置

```yml
 #  开启MyBatisPlus日志（控制台日志,打印SQL日志到控制台）
mybatis-plus:  
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:  # 去除mybatis-plus 图标
    banner: false
    db-config:  #  ID策略生成器
      id-type: assign_uuid  # auto、none、input、assign_id、assign_uuid 替代@TableId(type = IdType.ASSIGN_UUID)注解
      table-prefix: tb_   #表名前缀 替代 @TableName注解  
```



#### AUTO策略（默认）：

步骤1:设置生成策略为AUTO

  在使用该策略的时候`数据库表`设置为ID主键自增，否则无效。

```java
// 实体类
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;  // 
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```



#### INPUT策略：

**注意:**这种ID生成策略，需要将表的`自增`策略删除掉

##### 步骤1:设置生成策略为INPUT

```java
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.INPUT)
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
}
```

##### 步骤2:添加数据手动设置ID

```java
@SpringBootTest
class Mybatisplus03DqlApplicationTests {

    @Autowired 
    private UserDao userDao;
	
    @Test
    void testSave(){
        User user = new User();
        //设置主键ID的值
        user.setId(666L);
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");
        userDao.insert(user);
    }
}
```



#### ASSIGN_ID策略

注意:  雪花算法生成id（可兼容数值型(bigint)与字符串型）; 如收到设置Id,则会覆盖算法Id。

##### 步骤1:设置生成策略为ASSIGN_ID

```java
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
}
```

#### ASSIGN_UUID策略：

注意：主键的类型不能是Long，而应该改成String类型。

##### 步骤1:设置生成策略为ASSIGN_UUID

```java
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
}
```

主键类型设置为varchar，长度要大于32，因为UUID生成的主键为32位，如果长度小的话就会导致插入失败。



#### 打开策略生成器源码：

这就需要我们点击右上角的`Download Sources`,会自动帮你把这个类的java文件下载下来，我们就能看到具体的注释内容。因为这个技术是国人制作的，所以他代码中的注释还是比较容易看懂的。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/id%25E7%25AD%2596%25E7%2595%25A5%25E7%2594%259F%25E6%2588%2590%25E5%2599%25A8%25E6%25BA%2590%25E7%25A0%2581.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/id%25E7%25AD%2596%25E7%2595%25A5%25E7%2594%259F%25E6%2588%2590%25E5%2599%25A8%25E6%25BA%2590%25E7%25A0%25812.png)

### 分布式ID?

* 当数据量足够大的时候，一台数据库服务器存储不下，这个时候就需要多台数据库服务器进行存储
* 比如订单表就有可能被存储在不同的服务器上
* 如果用数据库表的自增主键，因为在两台服务器上所以会出现冲突
* 这个时候就需要一个全局唯一ID,这个ID就是分布式ID。



### 逻辑删除(Delete/Update)：

* 物理删除:业务数据从数据库中丢弃，执行的是delete操作

* 逻辑删除:为数据设置是否可用状态字段，删除时设置状态字段为不可用状态，数据保留在数据库中，执行的是update操作

* **逻辑删除的本质其实是修改操作。如果加了逻辑删除字段，查询数据时也会自动带上逻辑删除字段。**

* 执行的SQL语句为:

  UPDATE tbl_user SET **`deleted `** 1 where id = ? AND **`deleted`** 0

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E9%2580%25BB%25E8%25BE%2591%25E5%2588%25A0%25E9%2599%25A4%25E5%2585%25B3%25E7%25B3%25BB%25E8%25A1%25A8.png)

张三离职，但是工资表 和业绩不能删，删了金额就对不上了。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E9%2580%25BB%25E8%25BE%2591%25E5%2588%25A0%25E9%2599%25A42.png)

​    区分的方式：就是在员工表中添加一列数据`deleted`，0为在职员工，1为离职。

```java
@Data
@TableName("tbl_user")  // 可以不写是因为配置了全局配置
public class User {
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;
    private String name;
    private String password;
    private Integer age;
//  @TableLogic：逻辑删除，标记当前记录是否被删除， value为正常数据的值，delval为删除数据的值
    @TableLogic(value="0",delval="1")
    private Integer deleted;
}
```

```java
@SpringBootTest
class Mybatisplus03DqlApplicationTests {

    @Autowired
    private UserDao userDao;
	
    // 逻辑删除
    @Test
    void testDelete(){
       userDao.deleteById(1L);
    }
}
```



#### 修改逻辑删除配置：

注解：

```java
//value为正常数据的值，delval为删除数据的值
@TableLogic(value="0",delval="1")
```

yml配置：（全局配置）

```yml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: deleted   # 逻辑删除字段名
      logic-not-delete-value: 0    # 逻辑删除字面值：未删除为0
      logic-delete-value: 1  # 逻辑删除字面值：删除为1
```



### 乐观锁(Update):

乐观锁主要解决的问题是当要更新一条记录的时候，希望这条记录没有被别人更新。

业务

以下方案：专门针对于小型企业的解决方案（2000请求以下）

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E4%25B9%2590%25E8%25A7%2582%25E9%2594%25810.png)

```java
// ==实体类
@Data // 自动生成getter、setter、toString、等方法
@NoArgsConstructor // 无参构造器
@AllArgsConstructor // 提供一个所有字段的构造器
public class user {
    private long id;
    private String name;
    private String password;
    private String age;
    private String tel;
    @Version // TODO 1.乐观锁： @Version
    private Integer version;
}
```

2.添加拦截器

```java
@Configuration //
public class MybatisPlusConfig {

    // TODO 2.乐观锁
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        //1.定义MybatisPlus拦截器
        MybatisPlusInterceptor myInterceptor = new MybatisPlusInterceptor();
        // 2.添加乐观锁拦截器
        myInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return  myInterceptor;
    }
}
```

3.

方法1：

```java
@SpringBootTest
class Mybatisplus03DqlApplicationTests {

    @Autowired //按类型注入
    private UserDao userDao; 

    // TODO 3.乐观锁
    @Test
    void Version() {
        /* 方法1： */
        user user = new user();
        user.setId(3l);
        user.setName("Jock aaa");
        user.setVersion(1); // 乐观锁: update set abc = 1, version = version + 1 where version = 1
        userDao.updateById(user)
    }} 
```

方法2：

```java
@Test   // TODO 乐观锁
void Version() {

    user user = userDao.selectById(3L); // version=3
    user user2 = userDao.selectById(3L); // version=4

    user.setName("Jock aaa");
// 乐观锁:update set abc = 1,version = version + 1 where version = 1
    userDao.updateById(user); //  version => 4 

    user2.setName("Jock bbb");
    userDao.updateById(user2); // version =3,不成立，故不执行
}  
```

## 枚举：

数据库中固定的字段：男、女，使用枚举来实现。

```java
@EnumValue //设置为枚举项，会将注解所标识的属性的值存储到数据库中（配合扫描枚举使用）
type-enums-package:枚举路径  //扫描枚举
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E6%259E%259A%25E4%25B8%25BE1.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E6%259E%259A%25E4%25B8%25BE2.png)

注意：若yml文件不扫描，报错。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%E6%9E%9A%E4%B8%BE%E6%8A%A5%E9%94%99.png)





# 分页插件：

方式1：

```java
@Configuration //配置类
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页拦截器
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

}
```

方式2：

```java
@Configuration // 配置类    
@MapperScan("com.qsl.ggktparent.vod.mapper") //  @MapperScan注解替代@Mapper或@Resource注解  ==》com.qsl.ggktparent.*.mapper也行
public class ServiceVodConfig {

    /*MP分页插件*/
    @Bean
    public PaginationInterceptor paginationInterceptor(){
        return  new PaginationInterceptor();
    }
}
```





# 代码生成器（模板加配置）：

何模块的开发，对于这段代码，基本上都是对红色部分的调整，所以我们把去掉红色内容的东西称之为==模板==，红色部分称之为==参数==，以后只需要传入不同的参数，就可以根据模板创建出不同模块的dao代码。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E4%25BB%25A3%25E7%25A0%2581%25E7%2594%259F%25E6%2588%2590%25E5%2599%25A8.png)

* ① 可以根据数据库表的表名来填充
* ② 可以根据用户的配置来生成ID生成策略
* ③到⑨可以根 据数据库表字段名称来填充

### 代码生成器基本使用：

```xml
<!-- Mybatis-plus代码生成器-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>

<!--velocity模板引擎-Mybatis-plus代码生成器 -->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

2.同Application路径，创建类执行

  Application路径或测试（推荐）路径都行。。。

```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

public class CodeGenerator {
    public static void main(String[] args) {
        //1.获取代码生成器的对象
        AutoGenerator autoGenerator = new AutoGenerator();

        //设置数据库相关配置
        DataSourceConfig dataSource = new DataSourceConfig();
        dataSource.setDriverName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        autoGenerator.setDataSource(dataSource);

        //设置全局配置
        GlobalConfig globalConfig = new GlobalConfig();

        // 这个好像才行 ，下面的路径配置不行globalConfig.setOutputDir(System.getProperty("user.dir")+"/src/main/java"); 
        globalConfig.setOutputDir(System.getProperty("user.dir")+"/mybatisplus_04_generator/src/main/java");    //设置代码生成位置
        globalConfig.setOpen(false);    //设置生成完毕后是否打开生成代码所在的目录
        globalConfig.setAuthor("青衫泪");    //设置作者
        globalConfig.setFileOverride(true);     //设置是否覆盖原始生成的文件
        globalConfig.setMapperName("%sDao");    // 设置数据层接口名，%s为占位符，指代模块名称
        globalConfig.setServiceName("%sService"); //  去掉Service接口的首字母I, 例:IUserService

        globalConfig.setIdType(IdType.ASSIGN_ID);   //设置Id生成策略
        autoGenerator.setGlobalConfig(globalConfig);

        // 4、包配置     com.qsl.system...
        PackageConfig packageInfo = new PackageConfig();
        packageInfo.setParent("com.qsl.ggkt");   //设置生成的包名，与代码所在位置不冲突，二者叠加组成完整路径
        packageInfo.setModuleName("vod"); // 模块名  最终生成：com.qsl.ggktparent.vod
        packageInfo.setController("controller");// controller层
        packageInfo.setService("service");//service层
        packageInfo.setMapper("mapper");//mapper层
        packageInfo.setEntity("pojo");//设置实体类包名
        autoGenerator.setPackageInfo(packageInfo);

        // 5、策略设置（根据数据库生成表）
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setInclude("tbl_book","user"); // 指定要参与生成的表名(可多个)
        strategyConfig.setNaming(NamingStrategy.underline_to_camel);//数据库表映射到实体的命名策略  注意：使用这个可能不需要setTablePrefix。
        strategyConfig.setTablePrefix("tb_","springboot_"); // 生成的类，去除数据库表的前缀（可多个）
        // 名称，模块名 = 数据库表名 - 前缀名 例如：User = tb_user - tbl
        strategyConfig.setRestControllerStyle(true); //是否启用Rest风格
        strategyConfig.setVersionFieldName("version"); //根据乐观锁名-开启乐观锁
        strategyConfig.setLogicDeleteFieldName("del"); //根据逻辑删除名-开启逻辑删除(值要自己设置)
        strategyConfig.setEntityLombokModel(true); //是否要使用lombok
        autoGenerator.setStrategy(strategyConfig);
        //2.执行生成操作
        autoGenerator.execute();
    }
}
```

代码生成器生成的代码：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/MyBatisPlus/%25E4%25BB%25A3%25E7%25A0%2581%25E7%2594%259F%25E6%2588%2590%25E5%2599%25A82.png)


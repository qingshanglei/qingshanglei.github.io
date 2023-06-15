# SQL

SQL(Structured Query Language)：，结构化查询语言,操作关系型数据库的编程语言，定义操作所有关系型数据库的统一标准。

### 数据库分类：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/Sql%E5%88%86%E7%B1%BB.jpg)





![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/%E6%95%B0%E6%8D%AE%E5%BA%93.png)

* Oracle：收费的大型数据库，Oracle 公司的产品
* ==MySQL==： 开源免费的中小型数据库。后来 Sun公司收购了 MySQL，而 Sun 公司又被 Oracle 收购
* SQL Server：MicroSoft 公司收费的中型的数据库。C#、.net 等语言常使用
* PostgreSQL：开源免费中小型的数据库
* DB2：IBM 公司的大型收费数据库产品
* SQLite：嵌入式的微型数据库。如：作为 Android 内置数据库
* MariaDB：开源免费中小型的数据库

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/image-20210721185303106.png)



### DDL:操作数据库:

```mysql
  USE 数据库名称;       // 使用数据库
  SELECT DATABASE();   //查看当前使用的数据库
  
  SHOW DATABASES;      // 查询所有的数据库
  
  CREATE DATABASE 数据库名称;  //创建数据库
  CREATE DATABASE IF NOT EXISTS 数据库名称;  //创建数据库(先判断，如果不存在则创建)
  
  DROP DATABASE 数据库名称;  //删除数据库
  DROP DATABASE IF EXISTS 数据库名称;  //删除数据库(先判断，如果存在则删除)
  
```

### DDL:操作表

```mysql
 SHOW TABLES;       // 查询当前数据库下所有表名称
 DESC 表名称;       //查询表结构

 CREATE TABLE 表名 (      // 创建表
	字段名1  数据类型1,
	字段名2  数据类型2,
	…
	字段名n  数据类型n
); 

DROP TABLE 表名;            //删除表
DROP TABLE IF EXISTS 表名;  //删除表时判断表是否存在

/** =============================== **/
ALTER TABLE 表名 RENAME TO 新的表名;      //修改表名
ALTER TABLE 表名 ADD 列名 数据类型;       //添加一列
ALTER TABLE 表名 MODIFY 列名 新数据类型;  //修改数据类型
ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型;  //修改列名和数据类型
ALTER TABLE 表名 DROP 列名;            //删除列

```

### DML:操作表数据：

​      DML主要是对 **表数据**进行增（insert）删（delete）改（update）操作。

```mysql
   INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…)，(值1,值2,…)...;  //给指定列添加数据(可批量添加数据)
   INSERT INTO 表名 VALUES(值1,值2,…)，(值1,值2,…)...;  //给全部列添加数据(可批量添加数据)


  UPDATE 表名 SET 列名1=值1,列名2=值2,… [WHERE 条件] ; //修改表数据(不加条件，所有数据都修改)

  DELETE FROM 表名 [WHERE 条件] ;  //删除数据(不加条件，所有数据都删除)

```



### DQL：查询表数据

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/DQL-%E6%9F%A5%E8%AF%A2%E8%AF%AD%E6%B3%95.png)

注意：
​     mySQL数据库分页查询使用limit。
​     Oracle分页查询使用rownumber。
​     SQL Server分页查询使用top。

```sql
SELECT 字段列表 FROM 表名;
SELECT * FROM 表名; -- 查询所有数据    //查询多个字段

SELECT DISTINCT 字段列表 FROM 表名;   --去除重复记录
 
AS 别名   --起别名,为表和字段起别名（也可以省略）

SELECT 字段列表 FROM 表名 WHERE 条件列表; 
 
--===============模糊查询======================
like关键字:          --模糊查询
_ : 代表单个任意字符     % : 代表任意个数字符    --通配符进行占位

--===============排序查询======================
  SELECT 字段列表 FROM 表名
ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;    -- ASC ： 升序排列（默认值）      DESC ： 降序

--===============聚合函数======================


--===============分组查询======================
   SELECT 字段列表 FROM 表名
[WHERE 分组前条件限定] 
GROUP BY 分组字段名
[HAVING 分组后条件过滤];

--===============分页查询======================
SELECT 字段列表 FROM 表名 LIMIT  起始索引 , 查询条目数;
```

#### 条件查询:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/%E6%9D%A1%E4%BB%B6%E6%9F%A5%E8%AF%A2.png)



## 约束：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/%E7%BA%A6%E6%9D%9F.png)



## 事务:

​    数据库的事务（Transaction）是一种机制、一个操作序列，包含了==一组数据库操作命令==。

​    事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令==要么同时成功，要么同时失败==。

​     事务是一个不可分割的工作逻辑单元。

 事务的四大特征：

* 原子性（Atomicity）: 事务是不可分割的最小操作单位，要么同时成功，要么同时失败

* 一致性（Consistency） :事务完成时，必须使所有的数据都保持一致状态

* 隔离性（Isolation） :多个事务之间，操作的可见性

* 持久性（Durability） :事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

```sql

START TRANSACTION; 或者  BEGIN;  --开启事务

commit;   --提交事务

rollback;  --回滚事务

SELECT @@autocommit;  --查询默认事务提交方式：1为自动提交，0表示手动提交。
set @@autocommit = 0;  --修改事务的提交方式 
```









# SQL Server

## SQL Server数据库位置：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/SQL%20Server/SQL%20Server%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BD%8D%E7%BD%AE.png)

devtoys

```sql
select 字段（*） from (库名)表名
group by  字段 ------分组	
having  聚合函数 --判断式          [GROUP BY]子句
ORDER BY   字段 （ASC升序|DESC降序） 
offset  要跳过的行数 ROWS            ----分页
fetch next 要显示的行数 ROWS ONLY   ----分页
 
```

## 基本



```sql
order by   字段（ASC升序|DESC降序）      

Top--------返回前n条数据    percent(百分比)--------百分之多少和Top搭配使用

Top 行数 with ties----------先返回行数，再拿行数中的最后一行的字段匹配和他相同的字段

OFFSET  要跳的行数 ROWS     ----分页（写这个一定要写ORDER BY）---
FETCH NEXT 要显示的条数 ROWS ONLY  ----分页(可不写)

as（可不写，取表和字段）  名字-------------- 取别名
-------------------where----------
or（或，可多个，查的字段可为空）   语句--------------（逻辑运算符）查找满足两个条件中的任何一个的行

and（可多个）  语句------- 是一个逻辑运算符 (要配合where才能使用)

like（可配合not使用） 包含什么-------配合通配符使用：  %  任意多个字符，包含0个字符，    _ 任意单个字符  
[list of characters]:指定集合中任何单个字符       ,[character-character]:指定范围内的任何单个字符 
[^]:不在列表或范围内的任何单个字段（必须配合[character-character]使用）

between 字段  and 字段------在什么之间              

 is null----------没有。。。东西     
 not is null----------有。。。东西
  
 <=> ----------安全等于       
 distinct-------检索指定列列表中的唯一不同值（去重复，null视为相同值，实在想要加order by）
```

## 函数

~~~java
--------------------------------函数------------------------------- 
    len()--------查询字符串长度
    In()---------判断某字段的值是否属于in**列表中的某一项
    UPPER()----------大写              LOWER()------------小写
    concat()-------------拼接字符
多行函数（聚合函数）配合 having使用---
    sum()---------求和           avg()--------------平均值
    max()---------最大值         min()---------------最小值
    count()--------计算个数、行数
单行函数
日期函数:
    curdate() : 返回当前日期(MySQL数据库专属)
    getdate()-------获取当前日期（SQL Server数据库专属）
    date() -----获取当前年月日
    year()--------年   month()-------月   day()------日
    dateadd( ,在当前的日期加的天数,getdate()) ;  -------在当前的时间增加天数
    str_to_date()-----------将日期格式的字符装换成指定格式的日期           date_format()-------------将日期装换成字符
    from_unixtime()-------------将Uxin时间戳转为日期
转换函数：
    convert(要转换的类型,‘要转换的值’)  ----类型转换
    cast(‘要转换的值’， as 要转换的类型) --类型转换
数字函数：    
    isnull(字段,要转换的值) ----------把null值替换指定的值
    round(X,D)： 返回参数X的四舍五入的有 D 位小数的一个数字。如果D为0，结果将没有小数点或小数部分。

    datepart(dw,getdate()) --返回指定的日期(返回国外的星期)。
    abs() ----------返回数值表达式的绝对值
    floor() ------正数取最小整数，负数取最大(最大整数)
    ceiling()  ------正数取最大，负数取最小(最小整数)
    rand()  ------随机返回0~1之间的数
字符串函数：
    ltrim() ------删除前面空格的字符表达式
    rtrim() -------删除后面空格的字符表达式
    reverse() --------反转字符表达式
    right() -----从右边取值
系统函数：
    substring('字符串',开启位置，结束位置) -----截取字符串
    coalesce(参数,参数,参数) -----返回第一个非null表达式
    datalength() -------返回任何数据类型的实际长度
    nullif(expr1,expr2) Expr1与Expr2相等时,返回Null,否则返回第一个表达式
    row_number -----分页
    rank() ----相同的分数会使用同一排名

~~~



## 多表查询

### 内连接

  内连接（可多个表连）：两表合并为1表

~~~sql
     
         ---隐式内连接                
            select * from 表1，表2  
                 where 连接条件
      -- 例：查询在 SC 表存在成绩的学生信息
          select a.* from Student a,SC b
             where a.Sid=b.Sid 
                
         ---显式内连接 （inner可省略）
              select * from 表1
                 inner join  表2 on 连接条件， 
                 Where 判断式 
~~~

### 左、右外连接

~~~sql
    左外连接：两表合并，显示左表，右表没数据null表示
          select * from   表1
             left join 表2 on 连接条件

    右外连接：两表合并，显示右表，左表没数据null表示
          select * from   表1
             right join 表2 on 连接条件 
~~~

## 子查询

~~~sql

 ---单行单列：  (作为条件值，使用 = != > <等进行条件判断)
     select * from 表 
        where 字段名 = (子查询)
        
 ---多行单列： (作为条件值，使用in等关键字进行条件判断)
     select * from 表 
        where 字段名 in ()
        
 ---多行多列:  (作为虚拟表)
     select * from  (子查询)...
         where 条件；



 子查询：在查询里面在查询一个表格
select * from  表1 where  字段 
   in( select * from  表2 where  字段)
         或
select * from 
     ( select * from  表1 where  字段),
     ( select * from  表2 where  字段),
   where 连接条件

      嵌套子查询：最多32层。
      相关子查询（重复子查询）：
      Exists运算符：in适用于外表大而内表小的情况；Exists适合与外表小而内表大的情况；一样大，随便。
      any :任意一个
select * from  表1 
   where  字段   >(>,<,=,in,Exists)
     (select * from  表2  
       where  连接条件 )


~~~



```sql

       交叉连接：两表相乘，表1的每一行 对表2中的每一行连接 
 例：表1和表2都只有3行，连接后就有9行。
 
      自连接：自己连接自己，使用 内连接 或左连接字句。
      
      全外连接：两表合并，没数据null表示。
     select * from 表1 
       FULL OUTER JOIN  表2 on 连接条件
    
     
      
      union (不包括重复行)顶级并集:
      union all (包括重复行)
      
                  集合运算：
      Union(并集，本身有去重复)：
      表1   Union  表2 
      
 1.两个查询中列的数量必须相同。
 2.相应列的数据类型必须相同或兼容。
      Union all(并集，查询所有)：   
 
      Intersect(交集)：去两表相同的地方
 
      Except(差积)：2表的字段减1表的字段；若两表字段不同 减不了，显示1表
      
```

## 增删查改

~~~java
                    增删查改：
      新建表格：
      create table 表名(
          字段名 int primary key identity (1,1)(主键),
          字段名 varchar (字段长度) not null(不允许为空),
          字段名  numeric (3,小数的长度) default 0(默认0),
          字段名 date(时间格式),
          FOREIGN KEY(外键（可多个）)  (字段ID)  REFERENCES 要指向的表名 (字段ID)
      )
      
      创建多个主键约束：
      create table 表名(
     字段1  int,
     字段2  int
       PRIMARY KEY (主键字段1, 主键字段2 );
      )
      
      
          添加新行数据：
      insert into 表名(列名...)  values(字段值 ...)
      
          添加多行数据：
      insert into 表名(列名...)  values(字段值)，(字段值)...
      
      主键开关——开：set IDENTITY_INSERT 表名 on;
      主键开关——关：set IDENTITY_INSERT 表名 off;
       
      清空表数据（主键不会清除）：
      delete from 表名    [ where 条件 ]；
      
      清空表数据（主键从零开启，彻底删除）
      truncate table 表名;
      
      删除表：
      drop table 表名；
      删除失败也不报错：
      drop table if exists 表名;
      
      修改数据：(不加where则全改)
      update 表名 set 列名 =‘值’(可多个)，‘值’(可多个)...   [ where 条件 ]
      
  非unicode 一个中文占2个字节
  unicode 一个中文占1个字节
  
  复制表（要有新表）：
  insert into 新表名 [column] select * from 原表;
  
  复制表（没有表，就新建表格并复制原表）：
  select 字段 into  新表名  from 原表 
  
~~~



## merge（更新表数据）

~~~java
  --------------------merge（更新表数据）--------------------------
  merge 目标表 using 原表
  on 连接条件
  when matched  --匹配到的数据，按照新表的数据进行更新
       then update set  连接条件
  when not matched  by target --目标表中匹配不到的数据，在这插入
        then insert (字段) --在这不可以写表的别名
             values (原表的字段)
  when not matched by source  --原表中找不到的数据，在这删除
        then delete;
  
  
  1.select @@IDENTITY; --要紧跟着错误的语句查询
  2.select max(id) from pm.sasa
   DBCC CHECKIDENT('表名') --
  3.DBCC CHECKIDENT('表名',reseed,@@IDENTITY) 
  
  -----添加列
  alter table 表名 add 列名  数据类型 是否为空；
   -----添加主键
  alter table 表名 add primary key；
  -----修改列
  alter table 表名 alter column 列名  数据类型；
  -----删除列
  alter table 表名 drop column 列名;
  
      ---------------约束
  --添加约束：
   alter table 表名 add constraint 字段 check(字段 > 1);
  --添加新列约束：
   alter table 表名 add 字段 dec(10,2)   check(字段>0)
  --删除约束：
   alter table 表名 drop constraint  字段
     
  exec sp_help '表名';  ------检查约束 
  创建试图：
  create view 试图名 as select 字段 from 表名 
  更改表名或试图名：
  exec sp_rename '旧表名（旧试图名）','新表名（新试图名）'
   查询试图名1：
  select * from sys.views --当前数据库的试图
  查询试图名3：
  select * from sys.all_views  --整个系统的试图
  查询试图名4：
  select * from sys.sql_modules
  where OBJECT_ID=OBJECT_ID('视图名')
  查询试图名5：
  select OBJECT_SCHEMA_NAME(object_id),* from sys.views

  数据库备份：
  Backup Database 数据库名 To disk='存储位置\备份名称.bak'
  
  新建账号：
  exec sp_addlogin'账号名','密码'
  删除账号：
  exec sp_droplogin'登录账号'
  
  建立数据库用户：
  exec sp_grantbaccess '登录帐户名','数据库用户名'
  删除数据库用户：
  exec sp_dropuser '数据库用户名'
  
~~~



## 事务（ACID）

1. 原子性(Atomicity):  事务是不可分割的最小操作单位，要么同时成功，要么同时失败。
2.  一致性(Consistency)： 事务完成是，必须是所有的数据都保持一致状态
3.  隔离行(Isolation):   多个事务之间，操作的可见性
4.  持久性(Durability):   事务一旦提交或回滚，它对数据库的数据的改变就是永久的

~~~sql
 
  start transaction;  或 begin; --------------- 开启事务
  rollback; --------------- 事务回滚
  commit; --------------- 提交事务
      
  select @@autocommit ;  ---------------查询事务的默认提交方式
  set @@autocommit = 0; ---------------修改事务的提交方式 手动提交

~~~







# MySQL

​      MySQL数据库表格名-默认全小写（使用“_”分割），不区分大小写。



![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/mysql4%E5%A4%A7%E7%89%88%E6%9C%AC.png)

## MySQL的安装和 卸载

mysql --version //查询mysql环境

### MySQL安装：

#### 安装(解压)

​      MySQL因为是轻量级数据库，所以还要安装一个使用MySQL的软件（例：MySQL，Navicat）

MySQL官网： https://downloads.mysql.com/archives/community/

点开上面的链接就能看到如下界面：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/1.png)

   选择选择和自己**系统位数**相对应的版本点击右边的`Download`，此时会进到另一个页面，同样在接近页面底部的地方找到如下图所示的位置：

<img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/2.png" style="zoom:50%;" />

   不用理会上面的登录和注册按钮，直接点击 `No thanks, just start my download.` 就可以下载。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/3.png)

下载完成后我们得到的是一个压缩包，将其解压，我们就可以得到MySQL 5.7.24的软件本体了(就是一个文件夹)，我们可以把它放在你想安装的位置。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/4.png)



配置环境变量：



#### 初始化MySQL

​     以管理员方式运行 **命令行提示符**,进入mysql的bin目录

在**命令行提示符**敲入`mysqld --initialize-insecure`，回车，稍微等待一会，如果出现没有出现报错信息(如下图)则证明data目录初始化没有问题，此时再查看MySQL目录下已经有data目录生成。

```sql
mysqld --initialize-insecure
```

 ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/5.png)

   由于权限不足的报错

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/6.png)

#### 注册MySQL服务

在黑框里敲入  mysqld -install 服务名，回车。

```sql
mysqld -install 服务名（MySQL80）
```

#### 启动MySQL服务

在黑框里敲入 net start 服务名，回车。

```java
net start 服务名（MySQL80）   // 启动mysql服务
net stop  服务名（MySQL80）  // 停止mysql服务
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/7.png)

#### 修改默认账户密码

   在黑框里敲入`mysqladmin -u root password 1234`，这里的`1234`就是指默认管理员(即root账户)的密码，可以自行修改成你喜欢的。

```mysql
mysqladmin -u root password 密码
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/8.png)

####  登陆MySQL

登录 mysql -u root -p 密码，出现下图就安装MySQL成功了

```sql
mysql -u root -p 密码
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%AE%89%E8%A3%85/9.png)

#### 退出mysql：

```sql
exit
quit
```

注意：

-    如果提示`Can't connect to MySQL server on 'localhost'`则证明添加成功；
- ​    如果提示`mysql不是内部或外部命令，也不是可运行的程序或批处理文件`则表示添加添加失败，请重新检查步骤并重试。

### MySQL卸载：

​      打开`命令提示符(管理员)`，进入MySQL的bin目录

1. 敲入`net stop 服务名（例：MySQL80），回车。

   ```sql
   net stop 服务名（MySQL80）   --停止服务
   ```

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%8D%B8%E8%BD%BD.png)

2. 再敲入mysqld -remove 服务名，回车。

   ```mysql
   mysqld -remove 服务名    // 删除服务
   ```

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/MySQL/MySQL%E5%8D%B8%E8%BD%BD2.png)

3.  最后删除MySQL目录及相关的环境变量。**MySQL就卸载完成！**



## 查询：

```sql
select * from 表格 limit 1,10;  --分页查询
```



# PowerDesigner：

学习目标：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E5%AD%A6%E4%B9%A0%E7%9B%AE%E6%A0%87.png)

## 概要：

Rational Rose和Sy Base PowerDesigner是目前流行的两大计算机建模工具。

Rose出道是时走的是UML面向对象建模，而后再向数据库建模发展，而PowerDesigner则反其道而行之，它先是一个纯粹的数据库建模工具，后来才向面向对象建模，业务逻辑建模及需求分析建模进军。

Rose和PowerDesigner都既可以进行数据库建模，也可以进行面向对象建模，只是存在支持上的偏重而已。

Rational被IBM收购。 SDP被Powersoft公司收购，同年Sybase这只大黄雀又吃下了Powersoft这只螳螂。



## 数据库设计规则

**以下规则只针对本模块，更全面的文档参考《阿里巴巴Java开发手册》：**

1、库名与应用名称尽量一致

2、表名、字段名必须使用小写字母或数字，禁止出现数字开头，

3、表名不使用复数名词

4、表的命名最好是加上“业务名称_表的作用”。如，edu_teacher

5、表必备三字段：id, gmt_create, gmt_modified

说明：

其中 id 必为主键，类型为 bigint unsigned、单表时自增、步长为 1。

（如果使用分库分表集群部署，则id类型为verchar，非自增，业务中使用分布式id生成器）

gmt_create, gmt_modified 的类型均为 datetime 类型，前者现在时表示主动创建，后者过去分词表示被 动更新。

6、单表行数超过 500 万行或者单表容量超过 2GB，才推荐进行分库分表。 说明：如果预计三年后的数据量根本达不到这个级别，请不要在创建表时就分库分表。 

7、表达是与否概念的字段，必须使用 is_xxx 的方式命名，数据类型是 unsigned tinyint （1 表示是，0 表示否）。 

说明：任何字段如果为非负数，必须是 unsigned。 

注意：POJO 类中的任何布尔类型的变量，都不要加 is 前缀。数据库表示是与否的值，使用 tinyint 类型，坚持 is_xxx 的 命名方式是为了明确其取值含义与取值范围。 

正例：表达逻辑删除的字段名 is_deleted，1 表示删除，0 表示未删除。 

8、小数类型为 decimal，禁止使用 float 和 double。 说明：float 和 double 在存储的时候，存在精度损失的问题，很可能在值的比较时，得到不 正确的结果。如果存储的数据范围超过 decimal 的范围，建议将数据拆成整数和小数分开存储。

9、如果存储的字符串长度几乎相等，使用 char 定长字符串类型。 

10、varchar 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长度大于此值，定义字段类型为 text，独立出来一张表，用主键来对应，避免影响其它字段索 引效率。

11、唯一索引名为 uk_字段名；普通索引名则为 idx_字段名。

说明：uk_ 即 unique key；idx_ 即 index 的简称

12、不得使用外键与级联，一切外键概念必须在应用层解决。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险；外键影响数据库的插入速度。



## 三大范式：

什么是合理数据库（范式的优点）：

1.   结构合理。
2.   冗余较小（重复数据少）。
3.   尽量避免新增、修改、删除异常。

**范式的缺点**：

1.    性能降低。
2.    多表查询比单表查询数据慢。

一般达到第三范式即可。

### **总结**：

1.   范式是指导数据设计的规范化理论，可以保证数据库设计质量。
2.   第一范式：字段不能再分。
3.   第二范式： 不存在局部依赖。
4.   第三范式：  不含传递依赖（间接依赖）。
5.   使用范式可以减少冗余，但是会降低性能。
6.   特定表的设计可以违反第三范式，增加冗余提供性能。

### 第一范式：

### 第二范式：

- ​    第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。

- ​    既在一个数据库表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%AC%AC%E4%BA%8C%E8%8C%83%E5%BC%8F.png)



### 第三范式：

数据库的设计应该根据当前情况和需求做出灵活处理。

1.  在实际设计中，要整体遵循范式理论。
2.  如果在某些特定的情况下，可**适当增加冗余而提高性能**。







## 数据库建模:

数据库建模:

-    利用试图-关系图创建“概念数据模型”-CDM。
-    根据CDM产生基于特定数据库的“物理数据模型”-PDM。
-    根据PDM产生SQL语句并可以文件形式存储。
-    由已存在的数据库或SQL语句反向生产PDM,CDM。

### 数据表关系：

#### 一对一：

​    一对一：有**外键关联**和**主键关联**两种方式，本质上都是外键关联。

##### 外键关联：

   外键关联：在从的一端加入外键，并添加唯一约束。

​     ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E4%B8%80%E5%AF%B9%E4%B8%80-%E5%A4%96%E9%94%AE%E5%85%B3%E8%81%94.png)



##### 主键关联：

​    主键关联：使用主表的主键同时做子（从）表的主键和外键。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E4%B8%80%E5%AF%B9%E4%B8%80-%E4%B8%BB%E9%94%AE%E5%85%B3%E8%81%94.png)



#### 一对多：

​    在多的一段增加一个外键列。外键表示的就是一种一对多的关联。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E4%B8%80%E5%AF%B9%E5%A4%9A.png)



#### 多对多：

​     增加一个中间表，将一个多对多转换为两个一对多。中间表有外键。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E5%A4%9A%E5%AF%B9%E5%A4%9A0.png)



## 面向对象建模（UML建模）：

  制作UML图工具：IBM Rational Rose、StarUML、MS Visio。

​    Unified Modeling Language (UML) 统一建模语言或标准建模语言。
​    它是一个支持模型化和软件系统开发的**图形化语言**，为**软件开发的所有阶段**提供模型化和可视化支持。
​     它不仅统一了Booch、Rumbaugh和Jacobson的表示方法，而且对其作了进一步的发展，并最终统一为大众所接受的标准建模语言.

  UML定义了10种模型图，对应软件开发的不同阶段。

​     ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E5%BB%BA%E6%A8%A1-UML%E5%BB%BA%E6%A8%A1.png)



###  类和类之间的6种关系：

- 泛化/继承关系（is a）:      

   例：Cat is a Animal (类或接口的继承)

  ​          猫继承之动物。

  ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E6%B3%9B%E5%8D%8E%E5%85%B3%E7%B3%BB%EF%BC%88%E7%BB%A7%E6%89%BF%EF%BC%89.png)

- 实现关系（like a）： 

   例：Cooker like a FoodMenu。  厨师  ==》菜单。

  ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E5%AE%9E%E7%8E%B0%E5%85%B3%E7%B3%BB.png)

- 关联关系（has a）:   例：Programmer has a Computer。

  单双关联：

    例：朋友（friend）之间关联。

  ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E5%85%B3%E8%81%94%E5%85%B3%E7%B3%BB%EF%BC%88%E5%8D%95%E9%80%89%E5%85%B3%E8%81%94%EF%BC%8C%E8%87%AA%E5%85%B3%E8%81%94%EF%BC%89.png)

   双向关联：

     例：丈夫(husband)和妻子（wife）关联。

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E5%85%B3%E8%81%94%E5%85%B3%E7%B3%BB%EF%BC%88%E5%8F%8C%E5%90%91%EF%BC%89.png)

- 聚合关系（）： 聚合关系描述的是整体和部分的关系，聚合关系是比较特殊的关联关系。例：一个教室当中有多个学生，教室和学生之间的关系就是整体和部分关系。在聚合关系中，整体的生命周期不会决定部分的生命周期。例 ： 教室没了，学生还在；或学生走了，教室还在。

  ​     例：一个教室对多个学生。

  ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E8%81%9A%E5%90%88%E5%85%B3%E7%B3%BB%EF%BC%88%E4%B8%80%E5%AF%B9%E5%A4%9A%EF%BC%89.png)

- 组合关系： 可以看做是一种特殊的聚合关系，整体的生命周期决定部分的生命周期，部分是依附在整体上面的，部分离开了整体是无法“存活的”。

      例：人和四肢的关系。

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E7%BB%84%E5%90%88%E5%85%B3%E7%B3%BB.png)

- 依赖关系: 依赖关系是所有关系中最弱的一种，这种关系通常体现在类和局部变量之间的关系。

  ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E7%B1%BB%E4%B8%8E%E7%B1%BB%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB-%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.png)

### 类图：

​       类图（Class Diagram）：描述类的信息（包括属性、方法），以及类和类之间的关系信息。

### 用例图：

​       用例图(Use Case Diagram)：站在系统用户（系统角色）的角度分析系统存在哪些功能。

### 时序图：

​        时序图（Sequence Diagram）：描述了方法的调用过程，程序的执行流程，以及方法执行结束的返回值等信息。

### 业务流程图（BPM）:





## 数据库-导出完整图片:

Ctrl+A：选中所有的模型，点击Export Image...另存为图片格式。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E5%AF%BC%E5%87%BA%E6%B8%85%E6%99%B0%E5%9B%BE%E7%89%87.png)



## 数据库建模-逆向工程：

注意：数据库建模-默认是正向工程。

案例：使用的是SQLLog软件

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/PowerDesigner/%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B.png)
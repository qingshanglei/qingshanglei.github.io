mySQL端口：3306

#### 三、模糊查询 where

##### like 包含什么

特点：① 一般和通配符搭配使用

​            通配符：  %  任意多个字符，包含0个字符

​                              _    任意单个字符   

案例1、查询员工中包含 字符a 的员工信息

```mysql
select  *
from  employess(表名)
where name link '%a%';
```

案例2：查询员工中第三字符为 e, 第五个字符为 a 的   

员工名和工资

```mysql
select name 
from  employess(表名)
where name like '_ _e_a';
```



案例三：查询员工名中第二个字符为 ”_“ 的员工

注意：要通过转义查询

```mysql
 select name
 from  employess(表名)
 whrer  name like '_\_%';
 
```



或者

escape:申明是一个转义字符

```mysql
 select name
 from  employess(表名)
 whrer  name like '_$_%' escape '$';

```

##### between and    在什么之间

特点：①：可以提高语句的简洁值

​           ②： 包含临界值

​            ③：两个临界值不能调换顺序

案例1：查询员工编号在100到120之间的员工信息

```mysql
正常的：
     select *
     from employess(表名)
     where ISBN >=100 and ISBN <=120;
--------------------------分割线---------------------
between and:   
     select *
     from employess(表名)
     where between 100 and 120;
```

##### In   判断某字段的值是否属于in**列表中的某一项**

含义：判断某字段的值是否属于in列表中的某一项

特点：①：使用in提高语句简洁度

​            ②：in列表的值类型必须一致或兼用

案例：查询员工的Commodity 是 花花公子，附件，华为-fs



```mysql
正常的：
  select *
  from employess(表名)
  where Commodity  ='花花公子' or Commodity  ='神龟十多个'or Commodity='华为-fs';
  --------------------------分割线---------------------
In:
  select *
  from employess(表名)
  where Commodity in('花花公子','神龟十多个','华为-fs');
```

##### is null   没有。。。东西

特点： ①： = 或 <>不能用于判断null值

​            ②：is null 或is not null都可以判断null值

案例1：查询 没有奖金的员工名和奖金税

```mysql
 select name ,ISBN
 from employess(表名)
 where ISBN is null;
```

##### is not null   有。。。东西

案例1：查询 有奖金的员工名和奖金税

```mysql
 select name,ISBN
 from employess(表名)
 where ISBN  is not null;

```

##### 安全等于 ：     <=>

is null  和 <=> 比较：

is null : 仅仅可以判断null 值，可读性高，建议使用

<=>     : 既可以判断null值，又可以判断普通的数值，可读性低

案例1：查询没有奖金的员工名和奖金税

```mysql

select name , ISBN
from employess(表名)
where ISBN <=> null;

```

案例2：查询工资为1200的员工信息

```mysql

select name , ISBN
from employess(表名)
where ISBN <=> 1200;

```

## 进阶3：排序查询  order by  |   asc（默认）:从小到大      desc：从大到小

```mysql
语法： select 查询列表

     from 表

     [where 筛选条件]

     order by 排序列表 [asc|desc]   asc:从小到大      desc：从大到小

```

特点：②：order by 字句中可以支持单个字段、多个字段、表达式、函数、别名

​            ③：order  by 字句一般是放在查询语句的罪后面，limit字句除除

### 案例1：查询员工信息，要求工资 从高 到底排序

```mysql
 
  select * 
  from  employess(表名)
  order by   ISBN  asc; 
```

### 案例②：查询部门编号 >=90员工信息，按入职时间的先后进行排序

```mysql

select ISBN,foundTime
from S_User
where ISBN >= 90
  order by foundTime asc;

```

### 案例三：按 年薪 的高低显示员工的信息 和年薪 【按表达式排序】

```mysql
 select loanID
 ,Interest+2*(1+nullif(Interest,0)) as 年薪
 from J_Loan
 order by Interest+2*(1+nullif(Interest,0)) asc;
```

### 案例四：按年薪的高低显示员工的信息和年薪（按别名排序）

```mysql

 select loanID
 ,Interest+2*(1+nullif(Interest,0)) as 年薪
 from J_Loan
 order by 年薪 asc;
```

### 案例五：按姓名的长度 显示员工的姓名和工资（按函数排序）

```mysql

 select loanID,length(Name) as 年薪
 from J_Loan
 order by length(Name) asc;

```

### 案例六：查询员工信息，要求先按工资升序，再按员工编号降序（按多个字段排序）

```mysql
select LetyID,Commodity
from S_Commodity
order by LetyID asc , Commodity desc;

```



## 进阶4：常用函数

概念：类似java的方法，将一组逻辑语句封装在方法体中，对外暴露方法名

好处：1、隐藏了实现节        2、提高代码的重用性

调用：select 函数名（实参列表） [from  表]；

特点：       ①叫什么（函数名）

​                   ②干什么（函数功能）

分类：

​              ①、单行函数：concat 、length、ifnull等

​              ②、分组函数

功能：做统计使用，又称为统计函数、聚合函数、组函数

```mysql
模糊查询：where     
   link----------包含什么     AND----------用来链接link查询   
   escape----------转义字符                   between and----------在什么之间
   in()-----------判断某字段的值是否属于in列表中的某一项
   IS NULL----------没有...东西                 IS NOT NULL----------有...东西 
   <=>----------安全等于
排序查询：order by
    order by 排序列表 [asc|desc]    asc----------从小到大      desc----------从大到小
一：字符函数   
    UPPER()----------大写              LOWER()------------小写
    substr()/substring()-----------截取指定字符（索引从1开始）
    concat()-------------拼接字符      instr()------------返回子串第一次出现的索引，没有返回0               
    trim()----------去空格             lpad()-----------用指定的字符左填充，实现指定长度                            rpad()-----------用指定的字符右填充，实现指定长度                            replace()--------替换

二：数学函数
    round（）----------四舍五入         truncate()--------截断
    ceil()---------向上取整（返回>=该参数的最小整数）      floor()--------向下取整（返回<=该参数的最大整数）
    mod()----------取余
    
三：日期函数
     now()---------返回当前日期+时间    
     curdate()-----返回当前日期            curtime()--------返回当前时间
     year()--------年                     month()-------月 或 monthname()------月（英文月份）
     str_to_date()-----------将日期格式的字符装换成指定格式的日期             date_format()-------------将日期装换成字符
     
四：其它函数
   version()------------查询版本号            database()------------查询当前数据库
   user()-------------查询当前用户
   
五：流程控制函数
   if()------------跟if else一样         switch-----------跟switch case一样
六：分组函数
  特点：2、以上分组函数都忽略null值
       3、可以和distinct搭配实现去重的运算
       4、一般使用count(*)用作统计行数
       5、和分组函数一同查询的字段要求是group by后的字段
   sum()---------求和                     avg()--------------平均值
   max()---------最大值                   min()---------------最小值
   count()----------计算个/行数           DIFFRENCE ---------------差距 


```

### 一：字符函数

#### 案例：将性变大写，名变小写，然后拼接

```mysql
select concat(UPPER('b'),LOWER('jj')) as 姓名;
```

#### 案例一：截取陆展元三个字

截取从指定索引处后面所有字符

```mysql
 select substr(‘李莫愁爱上了陆展元’,7)  as 表头；
```

#### 案例二：截取李莫愁三个字

截取从指定索引处指定字符长度的字符

```mysql
select substr('李莫愁爱上了陆展元',1,3) as 表头；
```

案例：姓名中首字母大写，其它字符小写然后用—拼接，显示出来

```mysql
select CONCAT(UPPER(SUBSTR(ISBN,1,1)),'-',LOWER(SUBSTR(ISBN,2)))  as 姓名
from S_Commodity;
```

#### 案例：instr查询张三第一次出现的索引

```mysql
select INSTR('杨不盈爱上张三','张三') as 表格；
```

#### 案例：利用trim去指定的数字a(只能去前后)

```mysql
select TRIM('a' FROM  'aaa我啊啊aaaaa省公司aaa') as 表格;
```

#### 案例：lpad用指定的字符左填充，实现指定长度

```mysql
select LPAD('张三',10,'*') as 表格;
```

#### 案例：rpad用指定的字符右填充，实现指定长度

```mysql
select LPAD('张三',10,'a') as 表格;
```

#### 案例：replace替换

```mysql
select  REPLACE('张三置顶','张三','李四') as 表格;
```

### 二：数学函数

#### 案例：round（） 四舍五入

```mysql
 案例一： 四舍五入
select round(23.6);

案例一：保留几位小数的四舍五入
select round(23.2 , 2);
```

#### 案例： ceil()  向上取整（返回>=该参数的最小整数）

```mysql
select CEIL(-2.5);
```

#### 案例：  floor()  向下取整（返回<=该参数的最大整数）

```mysql
select floor(-2.5);
```

#### 案例：  truncate()   截断（从小数点后的一位开始）

```mysql
select truncate(1.6999,1);          结果：1.6
```

#### 案例：   mod()   取余

```mysql
select mod(10,-3);          结果：1
```

### 三：日期函数：

#### 案例：   now ()   返回当前日期 和时间

```mysql
select now ();          
```

#### 案例：   curdate ()   返回当前日期

```mysql
select curdate ();          
```

#### 案例：   curtime ()   返回当前时间

```mysql
select curtime  ();           
```

#### 案例：     str_to_date()   将日期格式的字符装换成指定格式的日期

```mysql
select   *  from  J_Loan  
where  borrowDate =str_to_date('5-1 19999','%c-%d %Y');        结果：1999-5-1
```

![](F:\Typora\笔记图片\mysqy\日期装换格式.png)

#### 案例： date_format()    查询有奖金的员工名和入职日期（x月 / x日 /x年）

```mysql
基础：
select date_format(now(),'%y年%m月%d日') as 日期； 

查询有奖金的员工名和入职日期：
select name,date_format(borrowDate,'%m月 /%d日 %y年') as 入职日期
form J_Loan
where borrowDate is not null;
```

### 四、其它函数

### 五、流程控制函数

#### if()   跟 if  else一样

##### 案例1： if()    判断10 是否大于5

```mysql
select if(10>5,'大','小');                  结果：大
```

##### 案例2： if()    判断员是否有奖金

```mysql
select name ,ISBN , 
if(ISBN,  is null ,'没奖金，呵呵','有奖金，嘻嘻') as 备注
from  J_Loan;
```

#### case  使用1 ： 跟 switch case一样

mysql中

```mysql
case 要判断的函数或表达式
   when 常量1 then 要显示的值1或语句1
   when 常量2 then 要显示的值2或语句2
   ...
else 要显示的值n或语句n
end
```

java中

```java
switch (变量或表达式){
   case 常量1 ： 语句1；break;
   ...
   default: 语句n:break;
else 要显示的值n或语句n
end
```

#### 案例1:查询员的工资，要求loanID=1，显示的工资+1，loanID=2，显示的工资+2，loanID=3，显示的工资+3，其它为原工资

```mysql
select loanID,Interest 原始工资,
case  loanID
when 1  then Interest+1
when 2  then Interest+2
when 3  then Interest+3
else Interest
end as 新工资
```

#### case  使用2 ： 跟多重 if一样

mysql中

```mysql
case 
   when 条件1 then 要显示的值1或语句1
   when 条件2 then 要显示的值2或语句2
   ...
else 要显示的值n或语句n
end
```

java中

```java
if(条件1){
       语句1；      
} else if(条件2){
       语句2；      
}
...
else(条件n){
       语句n；      
}
```

#### 案例2:查询员的工资，判断loanID=1，显示的工资+1，loanID=2，显示的工资+2，loanID=3，显示的工资+3，其它为原工资

```mysql
select Interest as 原工资,
case  
when loanID=1  then Interest+1
when loanID=2  then Interest+2
when loanID=3  then Interest+3
else Interest
end as 新工资
from J_Loan;
```

#### 案例：count函数的详细介绍

```mysql
统计有数据的有几行：
select count(Price) from S_Commodity;

统计表里的数据一共有几行：
select count(*) from S_Commodity;
select count(1) from S_Commodity;

效率：
 MYISAM存储引擎下，count(*)的效率高
 INNODB存储引擎下，count(*)和count(1)的效率差不多，比count(字段)高

```

#### 案例：和distinct搭配    （去重复）

```mysql
select sum(distinct ISBN) ,sum(ISBN)  from  S_Commodity;
```

## 进阶5：分组查询   group by

语法：  select 分组函数，列（要求出现在group by的后面）

​              from 表

​              【where  筛选条件】

​               group by 分组的列表

​              [order by 子句]

注意：查询列表必须特殊，要求是分组函数和group by后出现的字段

特点：1、分组查询中的筛选条件分为两类

​                                       数据源                     位置                                   关键字

​        分组前筛选           原始表                      group by字句的前面       where

​        分组后筛选           分组后的结果集       group by字句的后面      having

​        ①：分组函数做条件肯定是放在having字句中

​        ②：能分组前筛选的，就优先考虑使用分组前筛选    

​            2、group by字句支持 单个/多个字段分组（多个字段之间用逗号隔开没有顺序要求），表达式和函数（用得较少）        

​            3、也可以添加排序（排序放在整个分组查询的最后）

### 添加简单的筛选条件

##### 案例1：查询邮箱中包含a字符的,每个部门的平均工资

```mysql
select AVG(Price),Commodity
from S_Commodity 
where UnitID like '%a%'
group by Commodity;
```

##### 案例2： 查询有奖金的每个领导手下员工的最高工资

```mysql
select MAX(salary),manager_id
from employees 
where commission_pct  is not null
group by manager_id;
```

### 添加复杂的筛选条件

##### 案例1： 查询哪个部门的员工个数 > 2

```mysql
         ①：查询每个部门的员工个数
select count(*),department_id
from employees 
group by manager_id;
         ②：根据①的结果进行筛选，查询哪个部门的员工个数 > 2
 having count(*)>2;        
```

##### 案例2： 查询每个工种有奖金的员工的最高工资>1200的工种编号和最高工资

```mysql
         ①：查询每个工种有奖金的员工的最高工资
select MAX(salary),job_id
from employees 
where commission_pct IS NOT NULL
group by job_id
         ②：根据①的结果进行筛选，最高工工资>12000
 having MAX(salary) > 12000;        
```

##### 案例3： 查询领导吧编号>120的每个领导手下的最低工资>5000的领导编号是哪个，以及其最低工资

```mysql
         ①：查询每个领导手下的员工最低工资
 select MIN(salary),manager_id
 from employees 
         ②：添加筛选条件：编号>120
 where manager_id >120
         ③：添加筛选条件：最低工资 >5000
 group by manager_id
 having MIN(salary) >5000;        
```

### 按表达式或函数分组

#### 案例：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5的有哪些

```mysql
         ①：查询每个长度的员工个数
 select COUNT(*)，LENGTH(last_name)  as len_name
 from employees 
         ②：添加筛选条件：
 group by LENGTH(last_name)
 having COUNT(*) >5;        
```

#### 案例：(同上)支持别名

```mysql
         ①：查询每个长度的员工个数
 select COUNT(*) as c，LENGTH(last_name)  as len_name
 from employees 
         ②：添加筛选条件：
 group by len_name
 having c >5;        
```

### 按多个字段分组(分组字段顺序可换)

#### 案例：查询每个部门每个工种的员工的平均工资

```mysql
 select AVG(salary) ，department_id, //(部门)  //job-id（员工）
 from employees 
 group by department_id,  job-id；  
```

### 添加排序

#### 案例：查询每个部门每个工种的员工的平均工资，并且按平均工资的高低显示

```mysql
 select AVG(salary) ，department_id, //(部门)  //job-id（员工）
 from employees 
 group by department_id,  job-id
 
   ②：按工资高低排序
 ORDER BY AVG(salary) DESC；  
 
```

### 	总结案例：

#### 1、查询个job_id的员工工资的最大值，最小值，平均值，总和，并按job_id升序

```mysql
select MAX(salary),MIN(salary),AVG(salary),SUM(salary)
from employees
GROUP BY job-id
ORDER BY job-id；
```

#### 2、查询员工最高工资和最低工资的差距(DIFFERNCE)

```mysql
select MAX(salary) - MIN(salary) DIFFRENCE
from employees
GROUP BY job-id
ORDER BY job-id；
```

#### 3、查询各个管理者手下员工的最低工资，其中最低工资不能低于6000， 没有管理者的员工不计算在内

```mysql
select MAX(salary) ,manager_id
from employees
where manager_id  IS NOT NULL
GROUP BY job-id
HAVING MIN(salary) >= 6000;
```

#### 4、查询所有部门的编号，员工数量和工资平均值，并按平均工资升序

```mysql
select department_id,COUNT(*),AVG(salary) as a
from employees
GROUP BY department_id
ORDER BY a DESC;
```

#### 5、选择具有各个job_id 的员工人数

```
select COUNT(*) as 个数,job_id 
from employees
GROUP BY job_id;
```

## 进阶5：多表查询

含义：又称多表查询，当查询的字段来于多个表时，就会用到连接查询笛卡尔乘积现象：表1  有m行，表2 有n行，结果=m*n行

发生原因:没有有效的连接条件

如何避免：添加有效的连接条件

```mysql
两表相连
select NAME,boyName FROM boys, beauty
WHERE beauty.boyfriend_id = boys.id;
```

按年代分类：

  sq192标准：仅仅支持内连接

  sq199标准[推荐]：支持内连接+外连接(左外和右外)+交叉连接

   按功能分类：

​                       内连接：等值连接

​                                      非等值连接

​                                      自连接

​                        外连接：左外连接

​                                       右外链接

​                                       全外链接

​                         交叉连接

### 笛卡尔集的错误情况：

```mysql
select COUNT(*)  from beauty;
假设输出12行
select COUNT(*)  from boys;
假设输出4行

最终结果：12*4 = 48行
```

### 1、等值连接

#### 案例1：查询女神名和对应的男神名

```mysql
select NAME,boyName
from boys,beauty
where beauty.boyfriend_id = boys.id;
```

#### 案例2：查询员工名和对应的部门名

```mysql
select last_name,department_name
from employees,departments
where employees.`department_id` = departments.`department_id`;
```

### 2、为表起别名

①提高语句的简介度

②区分多个重名的字段

注意：如果为表起了别名，则查询的字段就不能使用原来的表名去限定

注意：如果为表起了别名，则查询的字段就不能使用原来的表名起限定

#### 查询员工名、工种号、工种名

```mysql
select employees.last_name,employees.job_id,job_title
from employees  as e,jobs as j
where e.`job_id`=j.`job_id`;
```

#### 案例1：查询有奖金的员工名、部门名

```mysql
select last_name,department_name
from employees as e,departments as d
where e.`department_id`=d.`department_id`
AND e.`commission_pct` Is NOT NULL;
```

#### 案例2：查询城市名中第二个字符为o的部门名和城市名

```mysql
select department_name,city
from departments as d ,locations as l
where d.`location_id` = l.`location_id`
AND city like '_o';
```

5.可以加分组

案例1：查询每个城市的部门个数

```mysql
select COUNT(*)  as 个数,city
from departments as d,loations as l;
GROUP BY city;
```

72














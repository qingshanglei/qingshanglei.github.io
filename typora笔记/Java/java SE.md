#   运行java

javac 文件名.java ----------运行

java 文件名   ----------编译，根据原文件生成java（class）文件

File -- Settings... -- Editor --File Encodings :更换编码格式（UTF-8等）

**注意：文件名要和里面的类名相同；**

package 包名；（多级包用 .分开）  --------------------在代码下手动建包      例：package com.itheima;

javac -d.  文件名.java   --------------------在命令窗下自动建包

309

71 ---https://www.bilibili.com/video/BV1Cv411372m?p=71&share_source=copy_web



修饰符：abstract(抽象类)、sealed(密封类)、static（静态类）

成员的修饰符 : readonly(只读)、static（静态）、const（常量）:名称全部大写  

访问修饰符 enum 变量名 ：数据类型{    }； ------------------ 枚举类型

​      枚举：只能在固定(自定义)范围内取值。


 int[,] inst = new int[3, 2] { { 4, 2 }, { 5, 3 }, { 5, 2 } } -------------- 二维数组

 int[,] afs = new int[3, 1] { { 5 }, { 5 },{ 5 } };//[3,1]指的是{}的长度，1指的是{}里面个数的长度。 -------------- 多维数组

 ~类名（）{  }  -------------- 析构函数

java中的8种基本数据类型：byte、boolean、int、long、char、double、float、short

# 基本

```java
public class 类名 {   {   }--------初始化     }  ------------------类的定义
publi static void main(String[] ages){    } ------------------(快捷键：psvm回车)
System.out.println(" ");   ------------------输出语句（默认换行,快捷键：sout回车）
System.out.print(" ");  ------------------输入语句（默认不换行） 
    
    修饰符 数据类型 变量名=初始化值；------------------定义成员变量
    
   类名 对象名=new 类名();  ------------------创建对象（使用它要先创建类）  
     对象名.成员变量 ------------------使用成员变量       
     对象名.成员方法() ------------------使用成员方法    
    { } ------------------代码块（创建对象一次调用一次）
    static { } ------------------静态代码块（只调用一次）
    
    StringBuilder sb=new  StringBuilder(要转换的String的参数); ----------------String转StringBuilder
     reverse()---------- 反转字符串（好像是StringBuilder的new才行）   
     append() ----------  
    
    equalsIgnoreCase()  ------------------判断字符串是否相等，不在大小写
    

    ======================泛型======================
修饰符 class 类名 <类型> {  public 类型 t； }------------------定义泛型类      类型：T、E、K、V ...
类名<类型> =new 类名<>(); ------------------使用泛型类
    
修饰符 <类型> 返回值类型 方法名（类型 变量名）{  } ------------------泛型方法
    
修饰符 interface 接口名<类型>{  } ------------------泛型接口
public class 类名<泛型> implements 接口名<泛型> {  重写接口... }  ------------类实现（继承）接口 
      
 <?> ------------------类型通配符
 <?extends 类型> ------------------类型通配符上限 (子类型)
 <? super 类型> ------------------类型通配符下限 （父类型） 
    
    ======================可变参数======================
 修饰符 返回值类型 方法名(数据类型... 变量名){  } ------------------定义可变参数
   ①这里的变量其实是一个数组
   ②如果一个方法有多个参数，包含可变参数，可变参数要放在最后
   
    Arrays（类）:
       asList(T... a) ------------------返回由指定数组支持的固定大小的列表（只能set()修改）
    
    List（接口）:
       List.of(E... elements) ------------------返回包含任意数量元素的不可变列表
    
    Set（接口）:
         Set.of(E... elements) ------------------返回一个包含任意数量元素的不可变集合
                          
       
    ======================异常======================
 Throwable:
   getMessage() ------------------返回此throwable的详细消息字符串
   toString() ------------------返回可抛出的简短描述
   printStackTrace() ------------------把异常的错误信息输出在控制台
   
 throws 异常类名； ------------------异常处理throws（这个格式是跟在方法的括号后面的）     
   自定义异常--------throw new NullPointerException("自定义异常")；  
       
HashCode() ------------------ 返回对象的哈希码值
对象哈希值特点：
        ①同一个对象多次调用hashCode()方法返回的哈希值是相同的
        ②不同对象的哈希值是不同的（默认），重写hashCode()方法可相同

    ======================Collections(类)==================
     Collections.sort(参数);
     sort() ------------------将指定的列表按升序排序
     reverse() ------------------反转指定列表中元素的顺序
     shuffle() ------------------ 使用默认的随机原随机排序指定的列表
       
       
       
    //导包：
            import java.util.Scanner;
      //创建对象：
            Scanner sc =new Scanner(System.in);
      //接收数据(键盘录入事件)：
            int x =sc.nextInt();（快捷键：sc.接收类型()+Alt+回车） （字符串类型：nextLine()）
    
//--------------随机数
           import java.util.Random;
             //创建对象：
           Random 自命名=new Random();
           int c=自命名.nextInt(整数)+整数;
 
public static void 方法名(参数){   } -----------定义方法    方法名()； -------方法的调用（在main方法里调用，程式里的入口）
public static 返回值类型 方法名(){  return  类型;  } ------定义方法     数据类型 变量名=方法名()；-------方法的调用
public class 类名 {  public static void ZZ(int a);    public static int ZZ(double a);        } -------重载 （多个方法在同一个类中；多个方法具用相同的方法名；多个方法的参数不相同，类型不同或者数量不同）
    
    关键字:
    private  ----------设置权限...  
    final ----------最终态 （修饰方法：不能被重写；修饰变量：不能再次被赋值；修饰类：不能被继承；修饰对象可以）   
    static ---------- 静态类（静态成员方法只能访问静态成员）
    abstract ----------抽象类（new 的话也要使用多态的方式）
      //快捷键：alt+insert(alt+fn+insert)
get变量名(){ return name;} ------------使用方法       set变量名（参数）{ this.name=name; this.age=age;  简化：this(name，age); }------------设置值                      对象名.set变量名() --------使用方法（main中调用）
    this ----------用于指代成员变量（子类，本类对象引用） （必须跟类名参数相同的才行）       
    super ----------用于指代成员变量（父类，父类对象引用）
public class 类名(){   修饰符  类名（参数）{     }}   --------定义构建方法（默认无参，可多个类似重载）     类名  字命名=new 类名(参数) --------使用构建方法
... %  -----------------换行符
    APl:应用程序编程接口
         
 Line.length() -------获取字符串的长度       Line.charAt(索引) -----根据索引获取字符串
 System.exit(0); -----------------JVM退出（java虚拟机退出）
 public class 子类名 extends 父类名{   }; ------------继承。     父类（基类，超类）    子类（派生类）
 @Overrider -----------------验证方法重写的正确性
 方法重写注意事项：
     ①私有方法不能被重写（父类私有成员子类不能继承）
     ②子类方法访问权限不能更低（public > 默认 >私有）
     
   多态： 猫 cat =new 猫；    动物 animal =new 猫(); ------------（默认向上转型）
     特点：编译看左边，执行看左边（成员方法看右边）
     前提和提现：
         ①有继承/实现关系
         ②有方法重写
         ③有父类引用指向子类对象
     Cat c=(Cat) animal; ------------（向下转型） 或者        ((Cat) a).jump();
    public interface 接口名 { }  ------------创建接口
    public class 类名 implements 接口名 { }  ------------类实现（继承）接口 (使用接口，也用多态)
   //cat 继承 Animal 实现 inter 接口，接口 ...（1个类可以实现多个接口） 
   接口跟接口是继承关系（可多继承）      
   方法的形参（返回值）是类名， 需要（返回）的是该类的对象。
        public class 类名 {  修饰符 class 类名 { } } ------------（内部类）
      内部类访问特点：
            ①内部类可以直接访问外部类的成员，包括私有
            ②外部类要直接访问内部类的成员，必须创建对象
   外部类名.内部类名 对象名= 外部类名.内部类名；     例：Outer.lnner ol=new Outer().new lnner();  ------------（使用内部）
   new 类名或接口名(){    重写方法;  }；------------匿名内部类定义
   new 类名或接口名(){    重写方法;  }.方法（）； ------------匿名内部类定义加单次调用
   类名或接口名 自命名 = new 类名或接口名(){    重写方法;  }；------------匿名内部类定义   自命名.方法名(); ------------可多次调用  
            

            
        for(int 变量名:数组或Collection集合){ } ------------Java增强型for循环,主要用于增强（简化）数组、集合循环,是只读的，不能对数组进行赋值。
    System.getProperty("user.dir") ------------------获取当前项目路径
        
  
```

## 字符串：

```java

Strings.isNotEmpty() //字符串不等于空
    
StringUtils.isNotBlank() //判断某个字符串是否不为空，不为null,不为空白符（whitespace）。
```



## 时间：

```java
    ======================Date对象  要导包======================
    setTime() -------------设置时间，给的是毫秒值
    
    =================SimpleDateFormat对象 要导包
    format() -------------将日期格式转化 字符串日期格式
    parse() -------------从给定字符串的开始解释文本以生成日期（要抛出ParseException异常）
    
    Calendar cr = Calendar.getInstance()  ------------------提供一个类方法getInstance用于获取Calendar对象，其日历字段已使用当前日期和时间初始化； 
    get ------------------返回给定日历字段的值
    add ------------------根据日历的规则，将指定的时间量添加或减去给定的日期字段
    set ------------------设置当前日期的年月日 
      set(int year, int month, int date)  
```

## 集合：

```java
// 判断给定的集合对象是否为空或者null
CollectionUtils
    .isEmpty(@Nullable Collection<?> collection)
    .isEmpty(@Nullable Map<?, ?> map)
    
```



```java
==========================object============================-
    .toString()  ------------返回对象的字符串表示形式     
    .equals()  ------------判断是否对象相等（默认比较地址，重写比较内容）  
==========================Arrays============================-
    .toString() ------------返回指定数组的内容的字符串表示形式
    .sort() ------------按照数字顺序排序指定的数组
    ==========================Integer============================-
       .valueOf(参数,要转换的进制) ------------字符串进制转换（默认十进制）
       .parseInt(参数,要转换的进制) ------------字符串进制转换（默认十进制）
            
       .toString(,) ------------转为[2,36]进制数
       .toBinaryString() ------------把int转为二进制字符串
       .toOctalString() ------------把int转为八进制字符串
       .toHexString()   ------------把int转为十六进制字符串
            
                   --Double--
        .parseDouble()  ------------  String转Double 
        .valueOf() ------------  String转Double 
        .toHexString()  ------------把Double转为十六进制字符串
```



# 集合

集合流程图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/Java SE/%E9%9B%86%E5%90%88%E7%BB%93%E6%9E%84%E4%BD%93%E7%B3%BB.png)

## Collection

~~~java
===========Collection接口
    Collection集合的常用方法 
     add() ------------------添加元素 
     remove() ------------------从集合中移除指定的元素 
     clear() ------------------清除集合中的元素
     contains() ------------------判断集合中是否存在指定的元素
     isEmpty() ------------------判断集合是否为空 
     size() ------------------集合的长度（集合中元素的个数）

    Iterator<E> ------------------迭代器，集合的专用遍历方式
      Iterator<String> it = 集合名.iterator();
        next ------------------ 返回迭代中的下一个元素 
        hasNext ------------------如果迭代具有更多元素，则返回true
            
   ======================list集合(可重复)====================== 
      list集合(有序集合，序列集合)特有方法： 
            app(E element) -----将指定的元素追加到此集合的末尾 
            remove(int index) -----删除指定索引的元素，返回被删除的元素
            set(int index,E element) -----修改指定索引的元素，返回被修改的元素 
            get(int index) -----返回指定索引的元素
            
      Listlterator（列表迭代器，List集合特有迭代器）
            next() ------------------返回迭代中的下一个元素
            hasNext() ------------------如果迭代具有更多元素，则返回true 
            previous() ------------------返回列表中的上一个元素
            hasPrevious() ----------------如果此列表迭代器在相反方向遍历列表时 具有更多元素，则返回true 
            add(E e) ------------------将指定的元素插入列表
             
      ArrayList集合 （底层是数组，查询快，增删慢） 
            app(E element) -----将指定的元素追加到此集合的末尾 
            app(int index，E element) -----在集合的指定位置插入指定的元素 
            remove(E element) -----删除指定的元素（索引和String都行） 
            remove(int index) -----删除指定索引的元素，返回被删除的元素 
            set(int index,E element) -----修改指定索引的元素，返回被修改的元 素 
            get(int index) -----返回指定索引的元素 
            size() -----返回集合中的元素个数 
            
      LinkedList集合（底层数据结构是链表，查询慢，增删快） 
            addFirst(E e) ------------------在该列表开头插入指定的元素 
            addLast(E e) ------------------将指定的元素追加到此列表的末尾 
            getFirst() ------------------返回此列表中的第一个元素 
            getLast() ------------------ 返回此列表中的最后一个元素 
            removeFirst() ------------------从此列表中删除并返回第一个元素 
            removeLast() ------------------ 从此列表中删除并返回最后一个元素      
            
   ==================set集合:(HashSet、LinkedHashSet、TreeSet,不可重 复)==================
       set集合特点：①不包含重复元素的集合 ②没有带索引的方法，所以不能使用普通for循环遍历 
       
       HashSet:对集合的迭代循环不做任何保证（要保证元素唯一性，需要重写 hasCede()和equals()）                 set<String> 变量名=new HashSet<>(); 
            TreeSet集合：(只能使用包装类创对象) ①元素排序按照一定的规则（构造方法）进行排序
            TreeSet():根据其元素的自然排序进行排序 
                TreeSet(Comparator comparator):根据指导的比较器进行排序 ②没有带索引的方法，所以不能使用普通for循环遍历
                
       Comparable:
          ①：用TreeSet集合存储自定义对象，无参构造方法使用的是 自然排序对元 素进行排序 
          ②：自然排序，就是 让元素所属的类实现Comparable接口，重写 compareTo(To)方法 
          ③：重写方法是，一定要注意排序规则必须按照要求的主要条件和次要条件 来写重写：
              返0，不添加； 返正数，升序； 
              返负数，降序 返回： 升序（this放前面） 降序(this放后面)
~~~

## Map

~~~java
======================Map<K,V>集合(多态-HashMap() )====================== 
    K:键的类型 V:值的类型 
        put(k ,v) ------------------ 添加元素 
        remove(Object key) ------------------根据键删除键值对元素 
        clear() ------------------移除所有的键值对元素 
        containsKey(Object key) ------------------判断集合是否包含指定的健 
        containsValue(Object value) ------------------判断集合是否包含指定的 值 
        isEmpty() ------------------判断集合是否为空 
        size() ------------------集合的长度（集合中键值对的个数） 
        
     获取：
        get(Okject key) ------------------ 根据键获取值
        keySet() ------------------ 获取所有键的集合 
        values() ------------------获取所有值的集合 
        entrySet() ------------------获取所有键值对对象的集合
        
        TreeMap（排序）
        ArrayList 嵌套 HashMap 
        HashMap （不排序） 嵌套 ArrayList
~~~

## Collections

~~~java
======================Collections(类)==================
    Collections.sort(参数); 
         sort() ------------------将指定的列表按升序排序 
         reverse() ------------------反转指定列表中元素的顺序 
         shuffle() ------------------ 使用默认的随机原随机排序指定的列表

~~~







# IO流



```java
-=====================File(类\构造方法)===================-
 File(String pathname) ------------通过将给定的路径名 将字符串转换为抽象路径来创建新的File实例
    项目名\\文件名  ------------  在本项目中创建文件 (相对路径)                  
 File(String parent,String child) ------------从父路径名 字符串和子路径名 字符串创建新的File实例
 File(File parent,String child) ------------从父抽象路径名和子路径名 字符串创建新的File实例

      File创建功能
 createNewFile() ------------文件不存在时，创建一个新文件（文件已有，返false，要抛异常） 
 mkdir() ------------创建目录 (抽象路径) 
 mkdirs() ------------创建目录，包括任何必需但不存在的父目录 (抽象路径) 
                
      File类判断和获取功能
 isDirectory() ------------ (抽象路径名)  是否为目录
 isFile() ------------(抽象路径名) 是否为文件
 exists() ------------路径 是否存在
                
 getAbsolutePath() ------------返回此抽象路径名的绝对路径名 字符串（绝对路径）
 getPath() ------------将此抽象路径名转换为路径名 字符串               
 getName() ------------返回（抽象路径名）文件或目录的名称
                
 list() ------------返回此抽象路径表示的目录中的文件和目录的名称字符串数组
 listFiles() ------------返回此抽象路径名表示的目录中的文件 和目录的File对象数组（对象）
                
      File类删除功能 
delete() ------------删除由此抽象路径名表示的文件或目录
      new File("名称").delete(); ---这样快     
            
      ------------递归          
      递归：方法定义中调用方法本身的现象
     n! = n * (n-1)! -------------- 阶乘公式
      解决递归问题：
                递归出口：否则会出现内存溢出
                递归规则：与原问题相似的规模较小的问题
                
      ------------IO          
      InputStream --------------这个抽象类是表示 字节输入流的所有类的超类
      OutputStream --------------这个抽象类是表示 字节输出流的所有类的超类
      子类名称都是以其父类名作为子类名的后缀 
     
-==========================字节流============================-
                            输出流--- 写数据
    FileOutputStream:文件输出流用于将数据写入Fle
        FileOutputStream(String name) --------------创建文件输出流以指定的名称写入文件(要抛异常)
        FileOutputStream(String name,boolean append) --------------创建文件输出流以指定的名称写入文件末尾(数据追加，要抛异常)
        
        write()：将指定的字节（字符）写入此文件输出流   
          write(int b) --------------将指定的字节写入此文件输出流，1次写1个字节数据
          write(byte[] b) --------------将b.length字节从指定的字节数组写入此文件输入流，，1此写1个字节数组数据
          write(byte[] b,int off,int len) --------------将len字节从指定的字节数组开始，从偏移量off开始写入此文件输出流，1此写1个字节数组的部分数据
        
       getBytes() -------------- 返回字节符对应的字节数组（字符串） 
       close() --------------关闭此文件输出流 并释放与此流相关的任何系统资源     
        
        
        ------------ 字节流写数据加载异常处理
         finally ------------在异常处理是提供finally块来执行所有清除操作          例：IO释放资源
  try {  可能出现的异常代码;  } catch(异常类名 变量名) { 异常的处理代码; } finally{ 执行所有清除操作}
------------try...catch...finally的做法
    
      try(定义流对象){ 可能出现异常的代码；}catch( 异常类名 变量名){ 异常的处理代码；} ----------     --JDK7改进方案：  （自动释放资源）
      定义输入流（输出流）对象；  try(输入流对象; 输出流对象){  可能出现异常的代码；}catch(异常类名 变量名){  异常的处理代码} ------------JDK9改进方案  （要抛异常，自动释放资源）  

                        输入流---读数据
        FileInputStream：
           read() ------------1次读取1个字节
           read(byte[] b) ------------1次读取1个字节数组
                            
      字节缓冲流（构造方法）：
         BufferedOutputStream(OutputStream out) ------------字节缓冲输出流
         BufferedInputStream(InputStream in) ------------字节缓冲输入流
                                          
     GBK编码(存汉字，2字节)                   UTF-8编码（存汉字，3字节）    
                            
     编码：
       getBytes()------------使用平台的默认字符集将改String编码为一系列字节，将结果存储到新的字节数组中（默认UTF-8）
       getBytes(String charsetName)------------使用指定的字符集将改String编码为一系列字节，将结果存储到新的字节数组中
       Arrays.toString(变量名) ------------控制台输出编码          
                            
     解码：
       String(byte[] bytes)------------通过使用平台的默认字符集解码指定的字节数组来构造新的String
       String(byte[] bytes,String charsetName) ------------通过指定的字符集解码指定的字符数组来构造新的String
                                     
-==========================字符流============================-
       字符流抽象基类：
          Reader:字符输入流的抽象类 (读数据)
          Writer:字符输出流的抽象类 (写数据)
       
      编码，解码(默认UFT-8)
         InputStreamReader
           InputStreamReader(InputStream in)------------创建一个使用默认字符集的InputStreamReader
           InputStreamReader(InputStream in,String charseName)------------创建一个使用命名字符集的InputStreamReader
         OutputStreamWriter     
           OutputStreamWriter(OutputStream out)------------创建一个使用默认字符编码的OutputStreamWriter
           OutputStreamWriter(OutputStream out,String charsetName))------------创建一个使用命名字符集的OUtputStreamWriter
           FileReader(String fileName) ------------用于读取字符文件的便捷类 (InputStreamReader简写) 
           FileWriter(String fileName)------------用于写入字符文件的便捷类（OutputStreamWriter简写）
              
         字符流写数据的5种方式：
            write(int c)------------写入一个字符
            write(char[] cbuf)------------写入一个字符数组
            write(char[] cbuf,int off, int len)------------写入字符数组中的一部分
            write(String str)------------写一个字符串
            write(String str,int off,int len)------------写一个字符串的一部分
              
          flush()------------刷新流
          close()------------关闭流，先刷新
     
       字符流读取数据的2中方式：
          read()------------1次读一个字符数据
          read(char[] cbuf)-----------一次读一个字符数组数据
          
      
       字符缓冲流（构造方法）：
          BufferedReader(Reader in) -----------  字符输入流(读数据)
          BufferedWriter(Writer out) ----------- 字符输出流
               
      字符缓冲流特有功能：
             newLine() -----------写一行行分隔符，行分隔符字符串由系统属性定义（BufferedWriter）
             readLine()-----------读一行文字。结果包含行的内容的字符串，不包括任何行终止字符，如果结尾已经到达，则为null (BufferedReader)
-==========================特性操作流============================-
        标准输入\输出流：
          InputStream 
            in -----------标准输入流。通常该流对应于 键盘输入或由主机环境或用户指定的另一个输入源
          PrintStream 
            out -----------标准输出流。通常该流对应于 显示输出或由主机环境或用户指定的另一个输出目标
        自己实现键盘录入数据：BufferedReader br=new BufferedReader(new InputStreamReader(System.in));

        打印流分类：(只负责输出数据，不负责读取数据)
            字节打印流：PrintStream
               PrintStream(String fileName) -----------使用指定的文件名创建新的打印流（创建新文档）
            字符打印流：PrintWriter
               PrintWriter(String fileName)-----------创建一个新文档（要刷新）
               PrintWriter(Writer out,booleran autoFlush)-----------创建一个新文档（不要刷新）    
            写数据：
               write()-----------把数据写入到文档中（不换行）
               print()-----------把数据写入到文档中（不换行）
               println()-----------把数据写入到文档中（换行）
            
        对象系列化流：
        ObjectOutputStream(InputStream in)----------- 对象系列化流 （创建文档,必须要实现Serializable接口，标记接口）
               writeObject(object obj)-----------将指定的对象写入ObjectOutputStream中
        ObjectInputStream(InputStream in)-----------对象反系列化流 （读取文档,要抛IOException，ClassNotFoundException异常）
               readObject()-----------从ObjectInputStream读取一个对象
               private static final long serialVersionUID = 42L;-----------  //修改原对象报InvalidClassException错加这个

            关键字：Transient -----------对象的某个成员变量不被序列化
                   
       Properties: (Map类型，但不能写泛型)   
         setProperty(String key,String value) -----------设置集合的键和值，都是String类型，底层调用Hashtable方法put
             getProperty(String key) -----------根据指定的健搜索值
             stringPropertyNames() -----------返回一个不可修改的键集，其中键及其对应的值是字符串
             
      Properties和IO流结合的方法：
          load(InputStream inStream) -----------从输入字节流读取属性列表（键和元素对） （读取文档）
          load(Reader reader) -----------从输入字符流读取属性列表    （读取文档）
          store(OutputStream out,String comments) -----------将此属性列表（键和元素对）写入此Properties表中，以适合与使用load(InputStream)方法的格式昔日输出字符流   （创建文档）
          store(Writer writer,String comments)-----------将此属性列表（键和元素对）写入此Properties表中，以适合使用load(Reader)方法的格式写入输出字符流   （创建文档）
               
```



# 多线程:

线程流程图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87//Java/Java%20SE/%E7%BA%BF%E7%A8%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

```java

       ========================实现多线程========================
     进程：是正在运行的程序
           ①是系统进行资源分配和调用的独立单位
           ②每一个进程都有它自己的内存空间和系统资源
     线程：是进程中的单个顺序控制流，是一条执行路径
           ①单线程：一个进程只有一个执行路径，则称为单线程程序
           ②多线程：一个进程有多条执行路径，则称为多线程程序
           
           
          多线程的实现方式：
             方式1：继承Thread类重写， 重写run()方法
                    start()----------- 线程开始执行；Java虚拟机调用此线程的run方法
                   为什么要重写run方法：  因为run()是用来封装被线程执行的代码
                   run()方法和start()方法的区别：
                       run(): 封装线程执行的代码，直接调用，相当与普通方法的调用
                       start(): 启动线程；然后由JVM调用此线程的run()方法
             设置和获取线程名称：
                setName()-----------更改此线程名称
                getName()-----------返回此线程的名称
                Thread.currentThread()-----------返回对当前正在执行的线程对象的引用（设置为主线程）
           
          线程有两种调度模型： （java使用的是抢占式调度模型）
              分时调度模型：所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间片
              抢占式调度模型：优先让优先级高的线程使用CPU；优先级相同，随机选择一个；优先级高的线程获取CPU的数据片相对多一些
          设置和获取 线程优先级的方法： （默认5，最小1，最大10）
              getPriority()-----------返回此线程的优先级
              setPriority(int newPriority)-----------更改此线程的优先级
           
           线程控制：
             sleep(long millis)-----------使当前正在执行的线程 停止指定和毫秒数
             join()-----------等待这个线程死亡
             setDaemon(true)-----------将此线程标记为守护线程，当运行的线程都是守护线程时，java虚拟机退出
           方式2: 实现Runnable接口，重写run()方法
              Thread(Runnable target)-----------分配一个新的Thread对象
              Thread(Runnable target,String name)-----------分配一个新的Thread对象
               
            相比继承Thread类，实现Runnable接口的好处
               ①避免了Java单继承的局限性
               ②适合多个相同程序的代码去处理同一个资源的情况，把线程和程序的代码、数据有效分离，较好的体现了面向对象的设计思想
       ========================线程同步========================
           同步代码块：
             synchronized(任意对象){  多条语句操作共享数据的代码} -----------锁多条语句操作共享数据，可以使用同步代码块实现
             好处：解决了多线程的数据安全问题
             弊端：当线程多的时候，每个线程都会上锁，很耗费资源，降低程序的运行效率
               
           修饰符 synchronized 返回值类型 方法名(方法参数){    }-----------同步方法
              this -----------同步方法锁对象
           修饰符 static synchronized 返回值类型 方法名(方法参数){   }-----------同步静态方法
              类名.class -----------同步静态方法锁对象（字节码文件对象）
                  
           线程安全类：
              StringBuffer （线程安全，可变的字符系列）
                  StringBuilder-----------(版本JDK5替代StringBuffer,并支持所有相同操作，更快但不执行同步)
              Vector（java2平台v1.2开始，实现了List接口、可扩展的对象数组）
                  ArrayList -----------不需要实现线程安全
              Hashtable (实现了Map接口)
                  HashMap -----------不需要实现线程安全
              Collections.synchronizedList(List<T> list) -----------返回由指定列表支持的同步（线程安全）列表    
           Lock锁：（接口，采用ReentrantLock来实例化）
             ReentrantLock() ----------- 创建一个ReentrantLock的实例   
           加锁，解锁
               lock()-----------获取锁，加锁
               unlock()-----------释放锁，解锁
                  
       ========================生产者消费者========================
        wait()-----------让当前线程等待，直到另一个线程调用改线程的notify()方法或notifyAll()方法
        notify()-----------唤醒正在等待对象监视器的单个线程
        notifyAll()-----------唤醒正在等待对象监视器的所有线程
```

# 网络编程

```java
 ========================网络编程入门========================
     网络编程三要素：IP、端口、协议
       IP:
         InetAddress:(此类表示Internet协议（IP）地址)
           getByName(String host)-----------确定主机名称的IP地址。主机名称可以是 机器名称/IP地址(要抛异常UnknownHostException)
           getHostAddress()-----------返回文本显示中的IP地址字符串
           getHostName() -----------获取此IP地址的主机名
      端口：（取整范围：0 ~ 65535，知名网络服务和应用：0 ~1023，设备上应用程序的唯一标识） 
      协议：
          TCP协议：(三次握手)
               ①传输控制协议
               ②面向连接的通信协议，有可靠无差错的数据传输
               ③应用于上传文件、下载文件、浏览网页
          UDP协议： 
               ①用户数据报协议
               ②无连接通信协议，既在数据传输时，数据的发送端和接收端不建立逻辑连接
               ③应用于音频、视频和普通数据等不太重要数据的传输   例：视频会议
 ========================UDP通信程序（先运行接数据，在运行发数据）========================
     UDP协议是一种不可靠的网络协议，他在通信的两端各建立一个Socket对象，但两个Socket只是发送，
             发送数据：  
       DatagramSocket -----------创建发送端的Socket对象（基于UDP协议的Socket）
       DatagramPacket(byte[] buf,int length,InetAddress address,int port) -----------创建数据，并把数据打包 
        send(DatagramPacKet p) ----------- 调用DatagramPacket对象的方法发送数据(要抛IOException异常)        
           
           接收数据：    
       DatagramSoket(int port)-----------根据指定的端口创建接收端的Socket对象         
       DatagramPacket(byte[] buf,int length) -----------创建一个数据包，用于接收数据       
         receive() -----------调用DatagramSocket对象的方法接收数据  
         getDate() -----------返回数据缓冲区
         getLength()-----------返回要发送的数据的长度或接收到的数据的长度
         close() -----------关闭 发送\接收端    
               
 ========================TCP通信程序（客户端：先写后读， 服务端：先读后写）========================
       TCP通信协议是一种可靠的网络协议，它在通信的两端各建立一个Socket对象，从而在通信的两端形成网络虚拟链路，一旦建立了虚拟的网络链路，两端的程序就可以通过虚拟链路进行通信（通过IO类来进行网络通信）    	
       java为客户端提供了Socket类，为服务器提供了ServerSocket类        
      发送数据：            
        Socket:实现 客户端套接字（也称“套接字”），套接字是两台机器之间通讯的端点  
          Socket(InetAddress address,int port) -----------创建流套接字并将器连接到指定IP地址的指定端口号
          Socket(String host,int port) -----------创建流套接字并将器连接到指定主机上的指定端口号（要抛UnknownHostException异常）
               
           getOutputStream() -----------获取输出流，发送数据 （要抛IOException异常）      
      
      接收数据：  
        ServerSocket: 实现了 服务器套接字，服务器套接字等待通过网络进入的请求
          ServerSocket(int port) -----------创建绑定到指定端口的服务器套接字（要抛IOException异常）
            accept() -----------侦听要连接到此套接字并接受它
              getInputStream() -----------获取输入流，读数据，并把数据显示在控制台
              getOutputStream() -----------获取输出流，发送数据
          shutdownOutput() -----------结束标记
```

# Lambda表达式

~~~java
(形式参数) -> { 代码块 } -----------Lambda表达式(必须有一个接口，而且 接口中只有一个抽象方法,配合函数式接口使用) 
    Lambda表达式三要素：形式参数，箭头，代码块
    Lambda表达式简写： 
        useFlyable(s->{ System.out.println(s); }) -----------参数只有一个，小括号可以省略 
        useFlyable(s-> System.out.println(s)) -----------代码只有一条， 大括号和分号可以省略 
        useAddable((x,y)-> x + y)-----------代码只有一条，大括号和分号可以省略,且return也要省略 可推导的就是可省略的
    
    -=========================方法引用符-========================= 
    方法引用符 ::   例：usePrintable(System.out::println);
      对象 :: 成员方法 -----------引用对象的实例方法      例：new Student :: toUpperCase 
      类名 :: 成员方法 -----------引用类的实例方法        例： Student :: toUpperCase 
      类名 :: new -----------引用构造器（引用构造器方法，形式参数全部传递给构 造器作为参数）
    
    -=========================函数式接口-=========================
          useStartThread(() -> System.out.println(Thread.currentThread().getName() + " :线程启动了"));-- --------- 函数式接口作为方法的参数 
          private static Comparator<String> getComparator(){ return s1.length() - s2.length(); } -----------函数式接口作为方法的返回值
          
          Supplier接口（生产型接口）
            Supplier<T>：无参方法 
               get()-----------获得结果
          
          Consumer接口（表示接受单个输入参数且不返回结果的操作。消费型接口，消费的 数据类型有泛型指定） 
             方法名(String name,Consumer<T> con ...) ----------- 使用Consumer接口 （可实现多个） 
             accept(T t) -----------对给定的参数执行此操作 
             andThen(Consumer affter) -----------依次执行此操作，然后执行after操 作（可多个）
          
          Predicate接口（判断参数是否满足指定的条件）
             test(T t) -----------对给定的参数进行判断
             negate() -----------返回一个逻辑的否定（逻辑非）
             and(Predicate other) -----------返回一个组合判断（短路与）
             or(Predicate other) -----------返回一个组合判断（短路或）
          
          Function<T,R>接口（用于对参数进行处理，转换，然后返回一个新的值） 
          T:函数输入的类型 R:函数结果的类型 
              apply<T t> -----------将此函数应用于给定的参数 
              andThen(Consumer affter) -----------依次执行此操作，然后执行after 操作（可多个）
              
          -=========================Stream流-========================= 
       startsWith(String name) -----------查询以name开头的字符串
         Stream流中间方法：                    
           filter(Predicate predicate) ----------- 用于对流中的数据进行过滤 (依次执行此操作)    
              
           Predicate接口
             test(T t):对给定的参数进判断，返回一个布尔值
             forEach() -----------终结流 
             limit(long maxSize) -----------截取指定参数个数的数据
             skip(long n) -----------跳过指定参数的个数 
             concat(Stream a,Stream b) -----------合并a和b两个流 (静态方法) distinct() -----------去重复 
             sorted() -----------按英文排序
             sorted(Comparator comparator) -----------根据提供的Comparator进行 排序(自定义排序)              map(Function mapper) -----------返回由给定函数应用于此流的元素的结 果组成的流  
          
           Function接口 
             apply(T t)-----------将此函数应用于给定的参数
             mapToInt(ToIntFunction mapper)-----------返回一个IntStream其中包含将给定函数应用于此流的元素的结果 (InStream:原始int流) 
           
           ToIntFunction接口
             applyAsInt(T value) sum()-----------返回此流中的元素总和    
              
      Stream流终结方法：
         forEach(Consumer action) -----------对流的每个元素执行操作
           Consumer接口 
              accept(T t):对给定的参数执行此操作
         count()-----------返回此流中的元素个数    
              
      Stream流的收集方法：
        collect(Collectors collector)
          Collectors:是个接口 
            toList() -----------把元素收集到List集合中 
            toSet() -----------把元素收集到Set集合中
            toMap(Function keyMapper,Function valueMapper)----------- 把元素收集到Map集合中        
    -=·-=·-=·-=·-=· Collection集合 
       stream()-----------生成流 
              
    -=·-=·-=·-=·-=· Map集合（间接生成流）
              
    -=·-=·-=·-=·-=· 数组 
       Stream.of(T...values) -----------（通过Stream接口的静态方法of生成 流）          
~~~

# 反射

~~~java
-=========================类加载器-========================= 
    内置类加载器： 
      Bootstrap:(虚拟机的内置类加载器，null表示，没有父null) 
      Platform:(平台类加载器) 
      System:(应用程序类加载器)
      类加载器的继承关系：System -->Platform -->Bootstrap 
          
    ClassLoader：（负责加载类的对象）
      getSystemClassLoader() -----------返回用于委派的系统类加载器
      getParent()-----------返回父类加载器进行委派
          
-=========================反射-========================= 
     Java反射机制:是指在运行时去获取一个类的变量和方法信息。然后通过获取 到的信息来创建对象，调用方法的 一种机制。由于这种动态性，可以极大的增强程序的灵活性，程序不用在编译器就完成 确定，在运行期依然可以扩展
         
 -============获取Class类型的对象：
         class -----------使用类的class属性来获取该类对应的Class对象 （基本数据类型都能通过.class得到对应的Class类型）
          getClass()-----------返回该对象所属类对应的Class对象 
          Class.forName(String className)-----------返回该路径所属类对应 的Class对象（传完整包名的路径，要抛ClassNotFoundException异常）
             
-============获取构造方法并使用：（Constructor类）
          数组：
             getConstructors() -----------返回所有公共构造方法 对象的数 组（公共的，要抛NoSuchMethodException异常） 
             getDeclaradConstructors() -----------返回所有构造方法 对象 的数组（包含私有的）
             
          单个：
             getConstructor(Class<?>... parameterTypes)-----------返回 单个公共构造方法对象（公共的） 
             getDeclaredConstructor(Class<?>... parameterTypes)------- ----返回单个构造方法对象（包含私有的）
          Constructor类中创建对象的方法：
             setAccessible(boolean flag)-----------值为true，取消访问检 查(暴力反射,检查私有)                newInstance(Object... initargs)-----------报已过时？(要抛 异常)  
             
-============获取成员变量并使用：（Field类）
             数组：
                   getFields() -----------返回所有 公共成员变量对象的数组
                   getDeclaredFields() -----------返回所有成员变量对象的数组 
             单个：
                  getField(String name) -----------返回单个公共成员变量对 象（要抛NoSuchFieldException异常） 
                  getDeclaredField(String name) -----------返回单个成员变 量对象
             Field类用于给成员变量赋值的方法： 
                  set(Object obj,Object value) -----------给obj对象的成员 变量赋值为value 
             
-============ 获取成员方法并使用：（Method类）
             数组:
                getMethods() -----------返回所有公共成员方法对象的数组，包 括继承的 
                getDeclaredMethods() -----------返回所有成员方法对象的数 组，不包括继承的  
                    
             单个：
                getMethod(String name,Class<?>... parameterTypes)------- ----返回单个公共成员方法对象 
                getDeclaredMethod(String name,Class<?>... parameterTypes)-----------返回单个成员方法对象 
             Method类中用于调用成员方法的方法：
                invoke(Object obj,Object... args)-----------调用obj对 象的成员方法，参数是args,返回值是Object类型   
                    
             =========================模块化-========================= 
        模块的使用： 
           导出/使用模块前，必须在模块的src目录下新建一个名为module-info.java 的描述性文件 ，专门定义模块名，访问权限，模块依赖的信息
               exports 包名; ----------- 模块导出 
                requires 模块名; -----------使用模块   
                    
        模块服务的使用：（java9+） 
             provides 接口名 with 实现类; -----------服务提供(服务接口指定实 现类) 
             uses 接口名; -----------声明服务接口
             ServiceLoader.load(接口名.class); -----------加载无法            
~~~

# 单元测试

  @Test -----------------单元模块（单元测试，要导入org, junit.Test ）(Assert. sddrtyEquals( 期望的值，运算的结果))
    @Before ----------------- 初始化方法（用于资源申请，在执行测试方法 之前会先执行该方法）
    @After -----------------释放资源方法 （修饰的方法会 在测试方法执行之后执行）

~~~java

public class Demo{

    @Before /* 初始化方法（用于资源申请，在执行测试方法 之前会先执行该方法）*/
    public void  init(){
        System.out.prinln("init...");
    }

    @Test /* 单元测试*/
    public void test(){
        System.out.prinln("test...");
    }

    @After /* 释放资源方法*/
    public void close(){
        System.out.prinln("close...");
    }
}
~~~



# @&关键字

~~~java
    //TODO是一种注释标记，它用于指示在代码中需要完成某些任务或实现某些功能。
    @Overrider -----------------表示方法重写 
    @FunctionalInterface -----------------表示函数式接口（函数式接口：有 且只有一个抽象方法的接口，配合Lambda表达式使用） 
    @Deprecated -----------------表示代码已过时
    @SuppressWarnings -----------------压制警告
        
    @Test -----------------单元模块（单元测试，要导入org, junit.Test ）(Assert. sddrtyEquals( 期望的值，运算的结果))
    @Before ----------------- 初始化方法（用于资源申请，所有测试方法在执行之前都会先执行该方法）
    @After -----------------释放资源方法 
        
    @author  (作者注解)
    @version (目前版本)
    @since ( 只支持 1.5之后的版本 )
        
        关键字：
        private ----------私有类，设置权限...
        final ----------最终态 （修饰方法：不能被重写；修饰变量：不能再次被赋值；修饰类：不能被继承；修饰对象可以） 
        static ---------- 静态类（静态成员方法只能访问静态成员） 
        abstract ----------抽象类（new 的话也要使用多态的方式）
        protected ----------子类可继承这个方法
        synchronized ----------------- 同步方法 
        volatile -----------------内存可见 
        instanceof -----------------判断它左边的对象是否是它右边的类 或该子类 的实例 
            1.先有继承关系，再有instanceof的使用。 
            2.当该测试对象创建时右边的声明类型和左边的类其中的任意一个跟测试类必 须得是继承树的同一分支或存在继承关系，否则编译器会报错
~~~









整个表达式的类型自动提升到表达式中最高等级操作数同样的类型：

等级顺序：（byte ,short ,char）===> int ===> long ===> float ===>double

i ++; ------------------先拿变量参与操作，后拿变量做++运算；

++ i;  ------------------先++，后拿变量参与操作；

逻辑运算符：

| 符号                                 | 作用 | 解析                              |
| :----------------------------------: | :--: | --------------------------------- |
| &                     |     逻辑与          | 有false则false   （代码全部执行） |
| \|                    |        逻辑或     | 有true 则true（代码全部执行）     |
| ^                   |      逻辑异或 | 相同为false,不同为true            |
|                      | 短路逻辑运算符： |                                   |

| 符号 |  作用  | 解析                                        |
| :--: | :----: | ------------------------------------------- |
|  &&  | 短路与 | 有false则false（有false就剩下的就不执行了） |
| \|\| | 短路或 | 有true 则true  （有true就剩下的就不执行了） |

关键字（字母全小写）

强制类型转换：目标数据类型 变量名 = (目标数据类型)值或者变量;

字符在计算的数值：     

| 字符                                          数值：         |      |      |
| ------------------------------------------------------------ | ---- | ---- |
| ' A '                                           65                     A-Z是连续的（每次加1） |      |      |
| ' a '                                           97                      a-z是连续的 |      |      |
| ' 0 '                                           48                      0-9是连续的 |      |      |
|                                                              |      |      |



# throws 和 throw  异常的区别

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/Java%20SE/throws%20%E5%92%8C%20throw%20%20%E5%BC%82%E5%B8%B8%E7%9A%84%E5%8C%BA%E5%88%AB.png)



| throws                                         | throw                              |
| :--------------------------------------------- | ---------------------------------- |
| 用在方法声明后面，跟的是异常类名               | 用在方法体内，跟的是异常对象名     |
| 表示抛出异常，由该方法的调用者来处理           | 表示抛出异常，由方法体内的语句处理 |
| 表示出异常的一种可能性，并不一定会发生这些异常 | 执行throw一定抛出了某种异常        |





# IDEA插件：

补全Maven项目骨架缺失问题：Maven Archetype Cotalogs
















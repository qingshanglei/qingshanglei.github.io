# Linux：

管理

运行、维护、管理

 Linux是一个操作系统（OS）

另外的系统：Unix、Minix(Unix变种)、Linux系统。

for tran

new B语言（C语言）

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/linux.png)

Linux的发行版本：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/Linux%E7%9A%84%E5%8F%91%E8%A1%8C%E7%89%88%E6%9C%AC.png)

国内常用：CentOS(ypm、yum)、ubuntu(apt)

插槽：UPU

内核：当前CPU的

主机或物理机：自己的电脑
创建的电脑：虚拟机
G->T->P->E  
Super键就是win10键。
Super+上：窗口最大化                       Super+下：窗口最小化
Super+左：窗口在左边（Super+下：恢复到原始窗口）
Super+右：窗口在右边
Ctrl+Alt+F2: 打开命令提示窗（F2~F6都行），Ctrl+Alt+F1: 返回Linux页面

## Linux安装：

超级管理员：root:  071800                          普通用户：qsl: 123456

Ip地址:192.168.139.131           路由：192.168.139.2         

DNS: 192.168.139.2

linux2:

Ip地址:192.168.139.132       路由：192.168.139.2         

DNS: 192.168.139.2

## Linux目录：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/linux%E7%9B%AE%E5%BD%95.png)

## 网络配置：*

### IP地址：

#### 查看：

```java
ifconfig  //查看所有网络接口的配置信息
```



#### 修改：

​    iP地址：/etc/sysconfig/network-scripts/ifcfg-ens33

```
图中的域名解析器写错了改为：192.168.139.2
ip的前三个不变，只改后面那个：  192.168.139.140 （第三位时虚拟机专用的）
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E4%BF%AE%E6%94%B9IP.png)

#### **修改** **IP** **地址后可能会遇到的问题**:

1. 物理机能 ping 通虚拟机，但是虚拟机 ping 不通物理机,一般都是因为物理机的防火墙问题,把防火墙关闭就行。
2. 虚拟机能 Ping 通物理机,但是虚拟机 Ping 不通外网,一般都是因为 DNS 的设置有问题。
3. 虚拟机 Ping www.baidu.com 显示域名未知等信息,一般查看 GATEWAY 和 DNS 设置是否正确。
4. 如果以上全部设置完还是不行，需要关闭 NetworkManager 服务。

    - systemctl  stop NetworkManager 关闭 
    - systemctl disable NetworkManager 禁用

5. 如果检查发现 systemctl status network 有问题 需要检查 ifcfg-ens33。



### 主机名：

#### 查看：

```java
  spark ...
  hostname      // 查看当前服务器的主机名称(查看主机名称)
  hostnamectl   // 详细查看主机名..
```

#### 修改:

##### 修改主机名路径:

​    主机名路径： /etc/hostname

```java
  vim /etc/hostname  // 打开hostname文件，并修改主机名称为hadoop100(修改完重启Linux即可)
```

##### 修改hosts映射文件：

​      克隆linux虚拟机配置linux集群时，虚拟机会比较多，配置时通常会采用主机名的方式配置。不用刻意记Ip地址。

######   Linux映射文件： 

​      映射路径：/etc/hosts

```java
 vim /etc/hosts   //打开hosts文件，并配置映射路径
     
   映射路径：
      192.168.139.100  hadoop100
      192.168.139.101  hadoop101
      192.168.139.102  hadoop103
      ...
```

###### win10映射文件：

1.  进入C:\Windows\System32\drivers\etc路径,并打开hosts文件
2. 在hosts文件添加hddoop集群

```java
 192.168.139.100  hadoop100
 192.168.139.101  hadoop101
 192.168.139.102  hadoop103
 ...
```



## 系统管理：

 ls /usr/sbin/ | grep service  --管道查询service  	

  一个正在执行的程序或命令，被叫做“进程”（process）。 

  启动之后一只存在、常驻内存的进程，一般被称作“服务”（service）。

   .service结尾的为服务     .target结尾的为服务集群

   有d结尾的为守护进程

   服务有绿色表示开启。。。

### 服务：

  启动 | 停止 | 重启 | 查看服务。。。

#### service(CentOS 6):

   查看存放所有服务地方： cd  /etc/init.d/ （默认只开启两个服务）

```java
   service 服务名 start | stop | restart |status   // 启动 | 停止 | 重启 | 查看
   service  firewalld  status //  例：查看防火墙服务状态

```



#### systemctl(CentOS 7): *

 查看存放所有服务地方： cd /usr/lib/systemd/system（兼用CentOS6）

```java
 systemctl   start | stop | restart | status  服务名    // 启动 | 停止 | 重启 | 查看
	
 systemctl status firewalld    // 例： 查看防火墙服务状态
```

### 开机自启动服务：

  服务是否启动，开机自启动服务...

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%9C%8D%E5%8A%A1.png)

#### 使用图形界面改：

```java
setup   //进入开机自启动服务界面
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1.png)

有星号的为开启...（空格选择）

System5   ==》SysV     (5的罗马字母为V)

#### 命令行改：

#####    chkconfig(CentOS 6)：

```java
chkconfig    //查看所有服务器自启配置（开机自动启动）
chkconfig  服务名 --list  //查看服务开机启动状态
chkconfig  服务名 off //关闭指定服务的自动启动
chkconfig  服务名 on  //开启指定服务的自动启动

例：chkconfig network on  //开启网络服务的自动启动

chkconfig --level 指定级别  服务名  off/on  //设置启动级别服务自启动   
例: chkconfig --leve 3 network  on //只开启第3级网络
    
```



#####    systemctl(CentOS 7)：

  ```java
systemctl list-unit-files  //查看服务开机启动状态
systemctl disable 服务名   //关掉指定服务的开机自启动
systemctl enable  服务名   //开启指定服务的自动启动
    
例：   systemctl disable firewalld.service //开启自启动防火墙
  ```



#### 系统运行级别 ：

##### CentOS 6:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/linux%E8%BF%90%E8%A1%8C%E7%BA%A7%E5%88%AB.png)



##### CentOS7 :

CentOS7的运行级别简化为:

multi-user.target 等价于原运行级别 3（多用户有网，无图形界面）

graphical.target 等 价于原运行级别 5（多用户有网，有图形界面）



### 防火墙：

iptwbles:   CentOS 6 防火墙

firewalld：CentOS 7 防火墙

```julia
 1.临时操作(CentOS 7)
   systemctl status firewalld  #查看防火墙状态
   systemctl stop firewalld    #关闭防火墙
   systemctl start firewalld   #开启防火墙
2.开机启动时操作
   systemctl enable firewalld.service #查看防火墙开机启动状态

#  例Tomcat：TCP 8080端口对应Http协议， TCP 8443端口对应HttpS协议。
# 配置防火墙规则，放行端口   
# 放行8080端口的外部访问 
firewall-cmd --add-port=8080/tcp --permanent		# --add-port=8080/tcp表示放行8080端口的tcp访问，--permanent表示永久生效
firewall-cmd --reload	# 重新载入防火墙规则使其生效
```

### Linux关机:

​    Linux系统中为了提高磁盘的读写效率，磁盘采取了“预持读写”的操作，缓冲区满了才保存到磁盘中。

```java
sync            //将数据由内存同步到硬盘中
halt           //停机，关闭系统，但不断电
poweroff        //关机，断电
reboot          //立即重启，类似shutdown -r now

shutdown [选项] [参数]   //等1分钟在关机（默认） 
shutdown -c     //停止关机
shoutdown now   //立刻关机
shoutdown n  //计划关机-n分钟后在关机   例：shoutdown 3：三分钟后在关机 
shoutdown 00:00 //计划关机-n小时后关机  例：shoutdown 12:00:  12点关机   
```

| 选项 | 功能           |
| :--: | -------------- |
|  -H  | 停机（--halt） |
|  -r  | 重启(reboot)   |

|       参数       | 功能             |
| :--------------: | ---------------- |
|       now        | 立刻关机         |
| 时间(分钟、小时) | 在指定的时间关机 |



## Linux远程连接工具：

   Linux远程连接：Xshell、FinalShell、SSH Secure Shell 、SecureCRT 、electerm、putty、mobaxterm...VNC(有图形界面):

Xshell: linux的终端模拟软件。

##    文件远程传输：

工具：Xftp、winScp、filelillq ...

命令：

#### win传Linux: rz

```sh
rz+回车，选择要传送的文件  #  文件远程传输（只能传输文件）

alt+p //上传    ,put(上传)    h:(盘) # 好像要FinalShell软件才行
    Sftp > put h:/jdk-7u71-linux-i586.tar.gz 
   
```



#### 多台服务器之间传输数据：scp

   功能：只要知晓服务器的账户和密码（密钥），即可在不同的Linux服务器之间，通过`SSH`协议互相文件。
  scp(ssh cp):通过SSH协议完成文件的复制。

```shell
scp [-r] 参数1 参数2
- -r：复制文件夹
- 参数1：本机路径 或 远程目标路径
- 参数2：远程目标路径 或 本机路径
# scp命令的高级用法
cd /export/server
scp -r jdk node2:`pwd`/    # 将本机当前路径的jdk文件夹，复制到node2服务器的同名路径下
scp -r jdk node2:$PWD      # 将本机当前路径的jdk文件夹，复制到node2服务器的同名路径下

如： 
scp -r /export/server/jdk root@node2:/export/server/
# 将本机上的jdk文件夹， 以root的身份复制到node2的/export/server/内
# 同SSH登陆一样，账户名可以省略（使用本机当前的同名账户登陆）

如：scp -r  /opt/tar.gz/  root@192.168.139.141:$PWD （本地linux传另一台linux）
scp -r node2:/export/server/jdk /export/server/
# 将远程node2的jdk文件夹，复制到本机的/export/server/内
```











## 帮助命令：

  内置命令（built-in）： 命令直接内置在shell中的，系统加载启动后会随着shell一起加载，常驻系统内存中的。

```java
  man 命令名或配置文件   // 查看命令或配置（按q退出）
      例： man ls  
  type  命令名          //查看是否为内置命令或外置命令...（命令名 is c shell builtin : ...命令为内置命令 ）
  halp  命令名        //查看shell内置命令的帮助信息
  命令名  --help      //查看外部命令的帮助信息
  history  [行数]         //查看使用过的所有命令   
  history -c        //删除所有使用过命令记录    
  alias            //查看别名命令   
  ！id           //根据id查看使用过的命令
  which  命令 //查询命令所在目录(主要查找系统*PATH目录下*的可执行文件,就是查找安装好可以直接执行的命令。)
  whereis 命令 //查询命令所在目录（可以用来查找二进制（命令）、源文件、man文件，可以通过文件索引数据库而非PATH来查找）   
```

  ctrl+l （clear）： 清屏

  reset:  彻底清屏

  man命令信息说明：

|    信息     |           功能           |
| :---------: | :----------------------: |
|    NAME     |   命令的名称和单行描述   |
|  SYNOPSIS   |       怎样使用命令       |
| DESCRIPTION |    命令功能的深入讨论    |
|  EXAMPLES   |      命令的使用例子      |
|  SEE ALSO   | 相关主题（通常是手册页） |



## 目录/文件夹：

### 切换目录：

|   命令    |           说明           |
| :-------: | :----------------------: |
| cd 目录名 |       切换该到目录       |
|   cd ..   |     切换到上一层目录     |
|   cd /    |       切换到根目录       |
|   cd ~    | 切换到用户主目录（桌面） |
|   cd -    |   切换到上一个所在目录   |

   在Linux中以 .开头的文件都是隐藏文件。

### 查看目录/文件夹：

##### “ls ”查询文件或目录：

  -a ：所有的目录或文件         -i ： 查看索引号

```java
 ls   //查看目录下的文件或目录
 ls -a //查询所有文件或目录（包含隐藏文件）
 ll (ll -l) //查询所有文件、文件属性、文件所属的用户和组。
 ll -lh   //查询所有文件、文件属性、文件所属的用户和组文件大小用k、M等表示。
 ls -i  //查看当前所有文件或目录的索引号     
 pwd(print working directory):  //查看当前目录的绝对路径
 ip 用户名               //查询Ip
```



#### find 递归查询文件/目录：

​    find 指令将从执定目录下递归地遍历其各个子目录，将满足条件的文件显示终端。

   ```java
命令：  find  [路径（默认当前目录）]    [选项]
   ```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/find%E6%9F%A5%E8%AF%A2%E6%8C%87%E4%BB%A4.png)

```java
例：
  find -name '*.txt'  //从当前目录查询所有的.txt文件
  find /home -user qsl  //重home目录下查找qsl用户的所有文件
  find -size +14M    // 查询当前目录大于14M的文件（+n：大于；  -n:(小于)  =：（等于，可不填））  

```



##### locate数据库查询：

   locate指令使用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。Locate指令无需遍历整个文件系统，查询速度较快。为了保证查询结果,管理员必须定期更新locate。(默认每天更新1次)

```java
updatedb   //更新locate数据库
locate 文件或目录  //查询文件或目录（使用前必须先更新一下locate数据库）

```



#####  "grep"过滤查询及 "|"管道符及"wc":

​     “|” ：管道符，表示将前一个命令的处理结果传输给后面的命令处理(类似：链式编程)。

```shell
 grep [选项] 查询内容 文件  #从文件里查询指定内容
       -n: 显示匹配行及行号。
       -i: 忽略大小写影响。
       -m <数字>:  显示查询结果的行数  例： -m 2 #查询2行 
    例：grep -n bb b.txt  #从b文件中显示行号的查找bb内容
    
[ | ]  # 管道符（类似链式编程）
   例：ls | grep '.txt'  #查询当前目录下所有的“.txt”文件
      ll | grep '.txt'

 wc  文件名 #根据文件查询行数、总单词数、文件占用内存大小
    例： wc a.txt  
        grep -n bb b.txt | wc 
```


#### 创建目录和移除目录

   mkdir(Make directory):   创建目录

  rmdir(Remove directory):  删除目录

​     -p: 多个目录

|          命令          |               说明                |
| :--------------------: | :-------------------------------: |
|      mkdir 目录名      |             创建目录              |
| mkdir 目录1  目录2 ... |    在当前目录同时创建多个目录     |
|  mkdir -p 目录1/目录2  | 创建多级目录（目录1不存在就创建） |
|      rmdir 目录名      |             删除目录              |
| rmdir 目录1  目录2 ... |    在当前目录同时删除多个目录     |
|  rmdir -p 目录1/目录2  |           删除多级目录            |

### 文件：

#### 创建空文件：

```java
touch 文件名     // 创建空文件
vim/vi  文件名   //打开文件（没有就创建）
```



#### 浏览文件：

1. ​    more指令是一个基于VI编辑器的文本过滤器，他以全屏幕的方式按页显示文本文件的内容。
2.      less指令用来分屏查看文件内容，支持各种显示终端；less指令根据需要加载内容。（一般加载大的文件...）
3.    ln软链接（符合链接），类似win10的快捷方式，有自己的数据块，主要存放了链接其他文件的路径。

  

- cat：

​        -n：显示所有行的行号，包括空行。

- head:（查看文件头部内容）

     -n<行数>:     查看头部内容的行数 

-  tail:（查看文件尾部内容）

​         -n<行数>:     查看尾部内容的行数 

​         -f:     查看文件最新追加的内容，监视文件变化（动态查看日志）

```java

   cat 文件名            // 查看文件所有内容    
   cat -n 文件名       // 查看文件所有内容（显示行号，包括空行）
   more  文件名    // 只查看一页内容（按空格: 查看下一页； 回车: 查看下一行内容； ...） 
   less  文件名    //  more功能都有，再加PgUp和PgDn上下翻页...
      
 //------查看文件头部内容         
   head 文件名   //查看文件头部内容 (默认10行)
   head -n 行数 文件名    //查看文件头部内容的行数
     例：head -n 5 a.txt //查看文件头5行内容
//------查看文件尾部内容    
   tail 文件名   //查看文件尾部内容 (默认10行)    
   tail -行数 文件名  // 查看后10行数据
   tail -f 文件名 // 动态查看日志  （Ctrl+s：暂停查看；  Ctrl+q: 继续动态查看...   Ctrl+C：退出；）
      
       
  ln  [原文件或目录]  [软连接名]   //给原文件创建一个硬链接
  ln -s [原文件或目录]  [软连接名（默认原名）]//在当前目录下给原文件创建一个快捷方式（链接变黑为无效，ll命令查看）
  rm -rf  软连接名    //删除软连接
  rm -rf 软连接名/   //删除软连接对应目录的真实内容
       
  例：[root@localhost cc]# ln -s ../bb/f.txt a 
     
```

more命令：

|    命令     |           说明           |
| :---------: | :----------------------: |
|    空格     |        查看下一页        |
|    回车     |      查看下一行内容      |
|  q/Ctrl+C   |         退出查看         |
|      =      |     查看当前行的行号     |
|     :f      | 查看文件名和当前行的行号 |
|  f(Ctrl+F)  |        查看下一屏        |
| b（Ctrl+B） |        查看上一屏        |

less命令：

  在more的基础上添加以下命令....

| 命令 |     说明     |
| :--: | :----------: |
| PgUp | 查看向上一页 |
| PgDn | 查看向下一页 |
|  /名称  |  向下查询【名称】(n:向下查找； N：向上查找)  |
|  ?名称 | 向上查询【名称】(n:向下查找； N：向上查找) |
| Shift+G | 查看结尾 |
| g | 查看开头 |

find：查询符合条件的文件

```java
例:  
  find / -name 'ins*' //查找文件名称以ins开头的文件
  find / -user itcast -ls   //查找用户itcast的文件
  find / -user itcast -type d  -ls //查找用户itcast的目录
       
```



##### echo（控制台输出）：

  $：代表系统变量

```java
echo  [] [输出内容] (一般配合 >和 >> 使用 )
    -e:  支持反斜线控制的字符转换（例：\\：\;   \n：换行符； \t:制表符） 
echo $ （按着tab键,选y）  //查看      
例: echo -e "张楚岚 \n 张灵玉"       
    
```



#### 修改文件：

" > ":输出重定向（覆盖写）; ">>":追加内容

```java
命令  > 文件名      //向文件写入命令里的内容 （覆盖写）
命令  >> 文件名    // 向文件追加命令里的内容

例：echo -e  "张楚岚 \n 张灵玉" > c.txt 
    cat c.txt >> b.txt   //查询c.txt的文本内容并追加到b.txt中
    ls >> c.txt  
```



#### 删除文件：

-f :不询问              -r：可以删除目录（ 递归操作）

|       命令       |              说明               |
| :--------------: | :-----------------------------: |
|    rm 文件名     | 删除文件(确定是否需要删除：y/n) |
|   rm -f 文件名   |      不询问，直接删除文件       |
|   rm -r 文件名   |   询问递归删除（文件和目录）    |
|  rm -rf 文件名   |  不询问递归删除（文件和目录）   |
|    rm -rf  *     |      删除该目录下所有文件       |
| rm -rf /* 文件名 |   删除所有目录的文件（自杀）    |

。。。

#### 复制/移动/重命名文件（目录）：

 cp(copy): 复制文件或目录
​      -r :  递归操作
 mv: 移动或重命名

```java
   cp 文件1 文件2   //将文件1 复制为文件2（复制文件并改名；没文件2为改名，有文件2为覆盖）
   \cp 文件1 文件2   //文件2为覆盖无提示（在命令前加\为原生命令）
   cp 文件名 路径    //将文件复制到该的路径下 
   cp 文件名 ../路径     // 将文件复制到上一层目录的路径 
   cp 目录名 /新名    //把文件复制到指定目录并修改文件名
   cp -r 目录名  新名    //复制当前目录并修改文件名
       
   mv 目录名(文件)  ../  //将文件移动到上一层目录
   mv 文件1 文件2      //将文件1重命名为文件2
```



## 压缩及解压缩：

#### gzip/gunzip压缩：

1. ​    只能压缩文件不能压缩目录
2. ​    不保留原文件
3. ​    同时多个文件会产生多个压缩包

```java
gzip 文件 //压缩文件，只能将文件压缩为.gz格式（可多个） 
   例： gzip a.txt  b.txt   //同时压缩a和d文件
    
gunzip  文件.gz   //解压文件 （可多个）
   例： gunzip a.txt.gz b.txt.gz 
    
```



#### zip/unzip压缩：

   zip压缩linux和wenows都通用，可以压缩目录且保留源文件。

```java
   zip [选项]  XXX.zip  压缩文件或目录  //压缩文件或目录
        -r:   压缩目录（递归压缩）
     例：zip b.zip a.txt //压缩a.txt文件
         zip -r b.zip aa //压缩aa目录下所有的东西，压缩包名为b.zip
            
   unzip  [选项]   XXX.zip  //解压文件(保留压缩包)
           -d:   指定解压目录
     例：unzip -d /ffffff/  b.zip //将b压缩包解压在ffffff目录下
               
```



#### tar打包或解压：

​    tar可配合其他压缩使用...

| 命令 |                说明                |
| :--: | :--------------------------------: |
|  -c  |        创建一个.tar打包文件        |
|  -v  |       显示指令的详细执行过程       |
|  -f  |         指定压缩后的文件名         |
|  -z  | 调用gzip压缩命令压缩，打包同时压缩 |
|  -t  |         查看压缩文件的内容         |
|  -x  |            解包.tar文件            |
|  -C  |           解压到指定目录           |

```javascript
   tar -cvf  打包后的jar名.tar  要打包的文件/目录名      //打包
      例：tar -cvf g.tar a.txt  b.txt  //打包a和b文件,名称为g.tar

   tar -zcvf 文件名.tar.gz 要打包的文件/目录名（可多个） //打包并压缩（-f放后面，其他命令随意）
        例：tar -zcvf c.tar.gz a.txt b.txt //打包并压缩a和b文件，名称为c.tar.gz

   tar -xvf 文件名.tar  //解压文件
   tar -zxvf 要解压的jar名.tar.gz -C 目录 //解压到指定的目录（gz和tar格式同理，去-z或-x就行了）
   
```

命令 usage

## 时间日期类：

#### 查看时间

  s：  秒的时间戳     S: 秒。。。其他类似

```java
 date         //查询当前时间
 date +%Y    //cake当前年份
 date +%Y-%m-%d-%H:%M:%S   //查看2022-09-26-23:43:17格式的时间
 date '+%Y-%m-%d %H:%M:%S'  //查看2022-09-26 23:44:09格式的时间（-可换为/等）

 date -d 时间  //查看指定时间
   例： date -d '1 days ago' // 查看上一天的时间
        date -d  '-1 days ago' //查看明年的时间
 
//----------------cal------------------
    -3： 查看上一个、现在、下一个的月份
    -y:  查看当前年的全部月份 
    -j:  查看当前月在整年的天数
 cal        //查看当前日期日历    
 cal  年份   //查看指定年份的日历
 cal  -3    //查看上一个、现在、下一个的月份 
      
```

#### 设置/修改时间：

```java
date -s 时间   //设置时间  
  例：date -s '2022-09-27 10:19:57'
   
ntpdate 时间服务器  //使用网络同步时间
    例：ntpdate edu.ntp.org.cn
    
```



## 用户管理：

  /etc/sudoers    //用户文件。。。

####  查询用户：

.../nologin结尾的为：伪用户      

.../bash结尾的为： 真实用户

```java
  id 用户名  // 查询用户是否存在
  cat /etc/passwd   //查询所有用户（包含伪用户）    
  
  who  //查看当前正在使用的所有用户
  who -T  //查看当前正在使用的所有用户，是否打开接收消息功能。（+: 打开； -：关闭）
  who am i  //查询当前正在使用的根（主）用户及登录时间（su切换没用）
  whoami   //查询当前正在使用的用户
```

cat /etc/passwd 查询概要：

1.    第一字段：用户名；         第二字段： 密码占位符；   

2.    第三字段：用户UID；         第四字段：用户GID; 

3.    第五字段： 用户全名；     第六字段：用户主目录；

4.    第七字段：用户登录的shell

   

#### 新增用户/修改密码：

```java
useradd 用户名   //添加新用户
useradd -g 组名  用户名   //添加新用户到指定组里
    
passwd 用户名   //设置/修改用户密码
```

#### 删除用户：

  -r ： 删除用户的同时，删除与该用户相关的所有文件。

```java
  userdel  用户名  //只删除用户
  userdel -r 用户名  //删除用户和用户主目录
```



#### 修改用户：

   -g:  修改用户的初始登录组，给定的组必须存在。默认组id为1。

```java
  usermod -g 用户组名 用户名  //修改用户名修改用户组
```



#### 切换用户：

```java
   su 用户名   // 切换用户，只有的执行权限，没有环境变量（套娃式切换，exit：退出）
   su - 用户名  //切换用户，有权限和环境变量  
```



#### 用户权限设置：

```java
 vim /etc/sudoers    /* 修改权限配置文件（在100行左右复制并修改 root ALL=(ALL) ALL,把root该成要权限的用户名）
    例：root ALL=(ALL) ALL 
        qsl ALL=(ALL) ALL //使用sudo命令时，需要输入密码
        qsl ALL=(ALL) NOPASSWD:ALL  //使用sudo命令时，不需要输入密码  
        */
 sudo 命令   //  让普通用户成root用户 
    例： sudo ls  //从root用户切换成普通用户，查询root用户的目录
        sudo vim /etc/sudoers // 使用vim编辑器打开sudoers文件
```

##### 文件权限类：

###### 概念：

​    Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（目录）的权限做了不同规定。

  文件属性：

​      ll命令：  查询所有文件、文件属性、文件所属的用户和组。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%96%87%E4%BB%B6%E5%B1%9E%E6%80%A7.jpg)

0位表是类型：-代表文件，d代表目录，l代表链接文档。c代表:字符类型的设备软件（鼠标、键盘...）。     b: 跨设备软件（硬盘）。 

1~3位确定属主（该文件的所有者）拥有该文件的权限。   --User

4~6位确定属组（所有则的同组用户）拥有该文件的权限。--Group

7~9位确定其他用户拥有该文件的权限。 --Other

2.rwx作用文件和目录的不同解释。

1. 文件

   [r]代表可读（read）:可以读取，查看。

   [w]代表可写（write）:可以修改，但不代表可删除该文件，除非有该文件所在目录的写权限。

   [x]代表可执行（execute）:可以被系统执行。

2. 目录

   [r]代表可读(read)：可以读取，ls查看目录内容。

   [w]代表可写（write）:可修改，目录内创建、删除、重命名目录。

   [x]代表可执行（execute）:可以进入该目录。

   

   ​                                   文件基本属性介绍：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%96%87%E4%BB%B6%E5%9F%BA%E6%9C%AC%E5%B1%9E%E6%80%A7.png)

###### 修改文件、目录权限

 ![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%96%87%E4%BB%B6%E5%B1%9E%E6%80%A7.jpg)

方式1：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%96%87%E4%BB%B6%E5%B1%9E%E6%80%A71.jpg)

-   u:创建者     g:该组所有人       o:所有人      a:所有人(u、g、o的总和)
-   +-=：增加、删除、 权限
-   rwx:读、写、执行权限操作

方式2：

​     权限对应数字： 0：-      1：X             2：W           3：WX 

​                               4：r       5： r-x          6：rw-         7：rwx      

​       例：读写执行权限都执行：   rwx=4+2+1=7

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E6%96%87%E4%BB%B6%E5%B1%9E%E6%80%A72.jpg)

```java
  选项 ==>   -R: 递归
方式1： 
      chmod [选项]  [{ugoa}{+-=}{rwx}]  文件或目录
         例:  chmod o+r a.txt  //设置a文件为所有人都可读文件 
              chmod +x c.sh //设置c文件把所有人都可执行该文件
              chmod -R o+w aa //设置a目录及a目录里的所有东西的权限为所有用户都能进行写操作.
方式2：
      chmod  [选项]  [数字]   [文件或目录]  
         例：  chmod 644 a.txt  //修改a文件的属主权限为rw（读写）,属组为r（读）,前他用户为r（读）。        
  
   
```



###### 文件创建者、所属组：

​    -R：递归

```java
chown [选项] [最终用户] [文件或目录]  //修改文件或目录的拥有者（创建者）
   例：chmod qsl b.txt //原b.txt文件是root用户拥有者的改为qsl用户
       chmod -R qsl bb//原bb目录下的所有文件（目录）都是root用户拥有者的改为qsl用户
    
chgrp [最终用户组] [文件或目录]    //修改文件或目录的所属组
    例：chgrp Array b.txt //原b.txt文件是root用户组的改为Array组
    
```





## 用户组管理：

​       每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux系统对用户组的规定不同。

​     例： Cenos 7的用户组跟用户同名，这个用户组跟用户同时创建。

​     vim /etc/group   //用户组文件目录 ...

##### 查询用户组：

```java
cat /etc/group  //查询所有组
    
```



##### 增、删、改用户组：

```java
groupadd  用户组名   //新增组

groupdel  用户组名   //删除组
    
groupmod  -n 新组名 旧组名  //修改用户组名

```



## 磁盘查看和分区类：

####    du查看文件/目录占用的磁盘空间：

​      du(disk usage ):磁盘占用情况

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/du%E5%91%BD%E4%BB%A4.png)

```java
du  [选项]  目录/文件  //查看目录下每个子目录的磁盘使用情况
  例： du -hac root  // 查看root目录下的占用内存
       du -h --max-depth=3 root  //查询到root子目录的第三层
```



#### df 查看磁盘空间：

   df(disk free): 空余磁盘

 ```java
  df [选项]  //列出文件系统的整体磁盘使用量，检查文件系统的磁盘空间占用情况。
     -h: 内存以G、M、K等格式显示。
         
  free -h   //查看内存的使用情况...
     
   yum install tree //安装tree  
   tree 路径      //查询路径下所有内容（树状图查询）
     例：  tree  ../aa   //查询上一级aa目录下的所有内容 
   
 ```



#### lsblk查看设备挂载：

​    lsblk   [选项] 

​         -f： 查看详细的设备挂载情况，显示文件系统信息。

  hda :硬盘        sda:   SATA /SCSI

```java
  lsblk:  查看设备挂载情况
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/lsblk%E6%8C%87%E4%BB%A4.png)

#### mount/umount 挂载/卸载：

​    对于Linux用户来说，它就是一个根目录、一个独立且唯一的文件结构。

​    Linux中每个分区都是用来组成整个文件系统的一部分，它在用一种叫做“挂载”的处理方法，它整个文件系统中包含了一整套的文件和目录，并将一个分区和一个目录联系起来，要载入的那个分区将是他的存储空间再这个目录下获得。

```java
mount [-t vfstype] [-o options] 设备名 目录名（挂载点） //挂载设备
   例：mount /dev/cdrom guazai/  //在guazai目录下挂载 
umont 设备名或挂载点     //卸载设备
    例： umount guazai/

    fsck ：检查和修复系统
```

挂载成功图：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/mount%E6%8C%82%E8%BD%BD%E6%88%90%E5%8A%9F.png)

##### 开机自动挂载：

```java
vim /etc/fstab   //设置开机自动挂载，在fstab文档修改
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E5%BC%80%E6%9C%BA%E8%87%AA%E5%8A%A8%E6%8C%82%E8%BD%BD.png)



#### fdisk硬盘分区：

​    reboot  //重启linux系统

```java
fdisk -l:    查看磁盘分区详情
    
fdisk 硬盘设备名：   对新增硬盘进行分区操作 
      例：fdisk /dev/sdb  //1.对新增硬盘进行分区操作 
        
mkfs   //给指定文件创建文件系统 
    -t 文件系统类型：指定文件系统类型（xfs,swap ...）
   例：mkfs -t xfs /dev/sdb1 //2.给硬盘指定文件系统
    
    mount /dev/sbd1   /home/qsl  //3.把硬盘给挂载上
```

 分区操作按键说明：

​     m：显示命令列表               p：显示当前磁盘分区 

​     n：新增分区                       w：写入分区信息并退出 

​     q：不保存分区信息直接退出

分区成功查询图：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/%E5%88%86%E5%8C%BA%E8%AF%B4%E6%98%8E.png)

一个磁盘最多只有4个主分区，主分区再划分为逻辑分区。最多有12个扩展分区。



##    系统进程：

####  ps查看当前系统进程：

​     ps（process status） :   进程状态

   ps系统进程选项：

​      UNIX：选项带-为UNIX风格。         BSD：选项只有小写字母的为BSD风格。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/ps%E7%B3%BB%E7%BB%9F%E8%BF%9B%E7%A8%8B.png)

```java
//aux: 一般查看进程CPU和内存占用率。   -ef: 一般查看进程的父进程ID。
 ps aux | grep xxx  //查看系统中所有进程
     例：ps aux | grep color (redix,mysql) //查询进程名为color的进程。
     
  ps -ef | grep xxx  //查看子父进程之间的关系。
       
```

##### ps aux 显示信息说明：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/ps%20aux.png)

- USER：该进程是由哪个用户产生的 
- PID：进程的 ID 号 

- %CPU：该进程占用 CPU 资源的百分比，占用越高，进程越耗费资源； 
- %MEM：该进程占用物理内存的百分比，占用越高，进程越耗费资源； 
- VSZ：该进程占用虚拟内存的大小，单位 KB； 
- RSS：该进程占用实际物理内存的大小，单位 KB； 
- TTY：该进程是在哪个终端中运行的。对于 CentOS 来说，？代表没有任何终端，tty1 是图形化终端， tty2-tty6 是本地的字符界面终端。pts/0-255 代表虚拟终端。 
- STAT：进程状态。常见的状态有：R：运行状态、S：睡眠状态、T：暂停状态、 Z：僵尸状态、s：包含子进程、l：多线程、+：前台显示、<：进程优先级较高 、N:优先级较低。
- START：该进程的启动时间。

- TIME：该进程占用 CPU 的运算时间，注意不是系统时间 。
- COMMAND：产生此进程的命令名

##### ps -ef 显示信息说明：

   watchodog: 看门狗进程...

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/ps%20-ef.png)

- UID：用户 ID 
- PID：进程 ID 
- PPID：父进程 ID 
- C：CPU 用于计算执行优先级的因子。数值越大，表明进程是 CPU 密集型运算， 
- 执行优先级会降低；数值越小，表明进程是 I/O 密集型运算，执行优先级会提高 
- STIME：进程启动的时间 
- TTY：完整的终端名称 
- TIME：CPU 时间 
- CMD：启动进程所用的命令和参数



#### kill 终止进程：

  注意：sshd的守护进程也踢掉了，已连接的用户可以继续用，但不可连接新用户；把sshd服务开启，新用户就又可以连接使用了。

```java
kill -l //查看强制进程的所有选项  
kill [选项] 进程号  //通过进程号杀死进程
     例：kill 3016  //关闭青衫泪用户连接
         kill -9 3016 //强制关闭青衫泪用户连接
     
 killall 进程名称   //通过进程名称杀死进程，支持通配符，在系统因负载过大而变慢时很有用（守护进程也关闭...）。
     例：killall sshd //关闭所有linux远程连接工具连接。
   
```

| 选项 |         功能         |
| :--: | :------------------: |
|  -9  | 表示强迫进程立即停止 |

tasklist:

```shell
tasklist | findstr "进程PID号"  #根据进程PID查询进程名称
taskkill /F /PID "进程PID号"   #根据PID杀死任务
taskkill -f -t -im "进程名称" #根据进程名称杀死任务
```



#### pstree 查看进程树：

```java
 pstree [选项] //查看进程树
     例：pstree -p  //显示进程 pid
         pstree -u  //显示进程所属用户
```

| 选项 |        功能        |
| :--: | :----------------: |
|  -p  |   显示进程的PID    |
|  -u  | 显示进程的所属用户 |



#### top 实时监控系统进程状态：

```java
top [选项]  
    -d <秒数> ：  指定top命令每隔几秒更新。默认3秒在top命令的交互模式当中可以执行的命令。
    -i：使top不显示任何闲置或者僵尸进程。
    -p: 通过指定监控进程ID来仅仅监控某个进程的状态。
```

|    操作    |          功能           |
| :--------: | :---------------------: |
|     P      | 以CPU使用率排序（默认） |
|     M      |   以内存的使用率排序    |
|     N      |  以PID排序（从大到小）  |
| u [用户名] |        查看用户         |
|  k [PID]   |    根据PID终止该进程    |
|     q      |         退出top         |

top命令结果：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E5%91%BD%E4%BB%A4.png)

第一行信息为任务队列信息：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E7%AC%AC%E4%B8%80%E8%A1%8C%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E.png)

第二行为进程信息:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E7%AC%AC%E4%BA%8C%E8%A1%8C%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E.png)

第三行为 CPU 信息:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E7%AC%AC%E4%B8%89%E8%A1%8C%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E.png)

第四行为物理内存信息:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E7%AC%AC%E5%9B%9B%E8%A1%8C%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E.png)

第五行为交换分区（swap）信息:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/top%E7%AC%AC%E4%BA%94%E8%A1%8C%E4%BF%A1%E6%81%AF%E8%AF%B4%E6%98%8E.png)



#### netstat 显示网络状态和端口占用信息：

```java
 netstat [选项]   
 netstat -anp | grep 进程号 //查看该进程网络信息
 netstat -nlp | grep 端口号 //查看网络端口号占用情况
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/netstat%E9%80%89%E9%A1%B9.png)



#### crontab 设置定时任务：

```java
systemctl status crond   //检查crond服务是否开启
    
crontab [选项]
    例： crontab -e // ①编辑定时任务（vim编辑器）
           */1 * * * * echo "张灵玉" >> /root/a.txt //②在vim编辑器里，设置每隔1分钟在a.txt文件追加“张灵玉”的内容。
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/crontab%E9%80%89%E9%A1%B9.png)

进入 crontab 编辑界面。会打开 vim 编辑器。 

vim 编辑器编辑定义命令：    * * * * * 执行的任务（命令）

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/crontab%20-e.png)

特殊符号(配合*使用)：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/crontab%20-e2.png)



## 软件包管理：

ubuntu（乌班图）系列， .rpm格式,     apt工具

RedHat（红帽）系列 ,    .deb格式，  rpm工具

注意：RedHat和RedHat命令基本相同。

```js
yum install 软件名称  //RedHat（红帽）系列
apt install 软件名称 //ubuntu（乌班图）系列
```



### rpm:

#### 概念：

​    RPM（RedHat Package Manager），RedHat软件包管理工具，类似windows里面的setup.exe 是Linux这系列操作系统里面的打包安装工具，它虽然是RedHat的标志，但理念是通用的。 

   RPM包的名称格式 :

- Apache-1.3.23-11.i386.rpm 
- “apache” 软件名称 
- “1.3.23-11”软件的版本号，主版本和此版本 
- “i386”是软件所运行的硬件平台，Intel 32位处理器的统称 
- “rpm”文件扩展名，代表RPM包

#### 查询：

```java
java -version  //查询安装java版本信息
rpm  -qa //查询所有安装的rpm软件
rpm  -qa  | grep rpm软件包 //查询指定软件包
  例：rpm -qa | grep firefox //查询火狐浏览器
rpm  -qi firefox //查询安装火狐详情版本
```

| 选项 |     功能     |
| :--: | :----------: |
|  -q  | 查询指定参数 |
|  -a  |   查询所有   |
|  -i  | 查询详细信息 |



####  卸载：

```java
  rpm -e RPM软件包（软件名）   // 根据软件名卸载。
     例：rpm -e firefox
  rpm -e --nodeps 软件包     // 不检查软件依赖卸载,依赖此软件的可能就不能用了。 
    
```

|   选项   |        功能        |
| :------: | :----------------: |
|    -e    |     卸载软件包     |
| --nodeps | 不检查软件依赖卸载 |



#### 安装：

​    注意： linux系统自带的安装包在Packages目录下，想使用要先挂载Linux镜像。（注意：要安装rpm软件，必须进入到Linux镜像的Packages目录下才能安装软件）

```java
 rpm -ivh RPM包全名（软件名）  //根据软件名安装软件
   例：rpm -ivh firefox-68.10.0-1.el7.centos.x86_64.rpm //安装火狐
```

|     选项      |       功能       |
| :-----------: | :--------------: |
|  -i(install)  |       安装       |
| -v(--verbose) |   显示详细信息   |
|  -h(--hash)   |      进度条      |
|   --nodeps    | 安装前不检查依赖 |



### yum:

yum（Yellow dog Updater, Modified）:是一个在 Fedora 和 RedHat 以及 CentOS 中的 Shell 前端软件包管理器。基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包 并且安装，**可以自动处理依赖性关系**，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/yum%E6%A6%82%E5%BF%B5.png)

注意：yum命令需要root权限

#### yum常用命令：

```java
getconf LONG_BIT  //查询linux系统版本的是32,或64位。
yum [选项] [参数] 软件名
     例： yum -y remove firefox //不询问删除火狐浏览器
     yum list | grep firefox//查询火狐
```

   选项：

| 选项 |       功能        |
| :--: | :---------------: |
|  -y  | 对所有提问都回答“ |
|  y   |        yes        |
|  n   |        no         |

   参数：

|     参数     |            功能             |
| :----------: | :-------------------------: |
|  ==search==  |            搜索             |
|   install    |        安装rpm软件包        |
|    update    |        更新rpm软件包        |
| check-update |      检查是否可用更新       |
|    remove    |     删除指定的rpm软件包     |
|     list     |       查询软件包信息        |
|    clean     |      清理yum过期的缓存      |
|   deplist    | 查询yum软件包的所有依赖关系 |

#### 修改yum仓库：

​    yum仓库是根据ID地址选择ID地址最近的yum仓库源（例：国内使用阿里镜像 ...）



### wget:

wget是非交互式的文件下载器，可以在命令行内下载网络文件。

​    注意：无论是否下载完成，都会生成下载的文件，如果下载未完成，请及时清理为完成的文件。

```shell
wget [-b] url
# -b: 后台下载，会将日志写入到当前工作目录的wget-log文件

例：wget http://archive.apache.org/dist/zookeeper/zookeeper-3.5.9/apache-zookeeper-3.5.9-bin.tar.gz # 下载Zookeeper安装包
```













## 安装软件：

​    华为镜像：https://repo.huaweicloud.com/

### 软件安装目录：

  注意：①Linux下的/usr/local类似win系统的C:\Program Files目录。
            ②第三方软件一般安装在Liunx系统的opt文件里。

```
  /opt/
  /usr/local
```

### JDK:

  华为镜像：https://repo.huaweicloud.com/java/jdk/

```sh
java -version # 查看JDK版本（java中自带）
```

安装

1.上传并解压JDK：

2.配置/etc/profile文件：

```sh
#set java enviroment
JAVA_HOME=/opt/jdk1.8.0_201 # 软件安装路径
CLASSPATH=:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

3.退出并重新profile文件：

```sh
source /etc/profile  # 重新加载profile文件

which javac  # 查看jdk安装目录
java -version # 检查Jdk是否安装成功（成功：有版本信息)
```



### Maven:

1.上传并解压JDK：

2.配置/etc/profile文件：

```sh
# Maven配置
MAVEN_HOME=/opt/Maven1.8.0_201 # 软件安装路径
CLASSPATH=:$MAVEN_HOME/lib.tools.jar
PATH=$MAVEN_HOME/bin:$PATH
export MAVEN_HOME CLASSPATH PATH

```

3.退出并重新profile文件：

```sh
source /etc/profile  # 重新加载profile文件

mvn -version # 检查是否安装成功（成功：有版本信息）
```





### mysql:



#### rpm版：

​    因为MySQL不在CentOS的官方仓库，所有需要我们自己**导入MySQL仓库的秘钥**和**配置MySQL的yum仓库**。



##### 安装：

```sh
 # Mysql5.x版本密钥
  rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
 
# 安装Mysql yum库
  rpm -Uvh http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm # 注意：①这个安装包是社区版的 ②这种方式安装会自动配置mysqld服务。
  # Mysql8.x版本密钥
   rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022

# 安装Mysql8.x版本 yum库
   rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm

   yum -f install mysql-community-servero  #安装mySQl

   systemctl start mysqld #启动mysql服务
   systemctl enable mysqld #开机自启服务
```



##### 配置：

 ```sql
-- 以 root 账户连接库时没有密码。
Access denied for user 'root'@'localhost' (using password: NO)
 ```



主要配置管理员用户root的密码以及配置允许远程登录的权限。

1. 获取MySQL的初始密码

```shell
   # 通过grep命令，在/var/log/mysqld.log文件中，过滤temporary password关键字，得到初始密码
     grep 'temporary password' /var/log/mysqld.log
   
   # 执行
     mysql -uroot -p   换行输入密码   #登录mysql
   
     ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';	#修改root用户密码  （ 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc）
```

2.配置root的简单密码:


```shell
# 如果你想设置简单密码，需要降低Mysql的密码安全级别
set global validate_password_policy=LOW; # 密码安全级别低
set global validate_password_length=4;	 # 密码长度最低4位即可

# 然后就可以用简单密码了（课程中使用简单密码，为了方便，生产中不要这样）
ALTER USER 'root'@'localhost' IDENTIFIED BY '简单密码';

```

3.配置root运行远程登录：

  默认，root用户是不运行远程登录的，只允许在MySQL所在的Linux服务器登陆MySQL系统。
注意：允许root远程登录会带来安全风险。

```shell
# 授权root远程登录
grant all privileges on *.* to root@"IP地址或%" identified by '密码' with grant option;  
# IP地址即允许登陆的IP地址，也可以填写%，表示允许任何地址
# 密码表示给远程登录独立设置密码，和本地登陆的密码可以不同

grant all privileges on *.* to root@"%" identified by 'S234sdfsdf@243' with grant option; 
# 刷新权限，生效
flush privileges;
```



#### yum版：

阿里镜像：https://mirrors.aliyun.com/mysql/MySQL-8.0/

 注意：mysql的跟目录要改成mysql不然可能启动不了。


```sh
# 配置环境变量
export PATH= /opt/mysql-8.0.28/bin:PATH  

# 重载profile文件：
source /etc/profile
```

vim /etc/my.cnf

```sh
# =========新增========
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8

[mysqld]
#设置端口
port=3306
socket=/tmp/mysql.sock
#设置mysql根目录
basedir=/opt/mysql-8.0.28
#设置数据库的数据存放目录
datadir=/opt/mysql-8.0.28/MyData
#设置最大连接数
max_connections=200
#设置mysql服务端字符集，默认为latin1
character-set-server=UTF8MB4
#设置默认存储引擎
default-storage-engine=INNODB
#设置密码永不过期
default_password_lifetime=0
#设置 server接受的数据包大小
max_allowed_packet=16M
```



```sh
# 添加用户
groupadd mysql

# 变更用户和用户组
chown -R mysql:mysql /opt/mysql-8.0.28/

# 安装SSL
mysql_ssl_rsa_setup --datadir=/opt/mysql-8.0.28/MyData

# 添加权限
chmod -R a+r /opt/mysql-8.0.28/MyData/server-key.pem

# 复制启动脚本到资源目录
cp /opt/mysql-8.0.28/support-files/mysql.server /etc/rc.d/init.d/mysqld

# mysqld文件添加执行权限
chmod +x /etc/rc.d/init.d/mysqld

# mysqld服务添加至系统服务
chkconfig --add mysqld

# mysql-8.0.31: 版本号。    -1：本次版本发布的次数。 .el9.x86_64：系统建构   
mysql-8.0.31-1.el9.x86_64.rpm-bundle.tar

安装icu那个后再安装server
```






### Tomcat:

1. 安装JDK环境
2. 上传并解压Tomcat
3. 注意：10.0.27版本，需要Java（JDK）版本最低为JDK8或更高版本。

启动、关闭Tomcat: 

```shell
./startup.sh  # 启动Tomcat
./shutdown.sh  # 关闭Tomcat

#  腾讯或阿里服务器注意放行端口(TCP协议)。 
```

1.**上传并解压Tomcat**到linux上

2.执行bin目录下 .\startup.sh（注意：必须**关闭防火墙**或**放行8080端口**）

浏览器访问[Tomcat](http://192.168.139.140:8080/)，出现官网即可。

3.查看目标 tomcat/logs/catalina.out





# VI/VIM编辑器：*

​    替代软件：EditPlus

### vi编辑器概念：

​      VI是Unix操作系统和类Unix操作系统中最通用的文本编辑器。

​      VIM 编辑器是从 VI 发展出来的一个性能更强大的文本编辑器。可以主动的以字体颜色辨别语法的正确性，方便程序设计。VIM 与 VI 编辑器完全兼容。

   编辑器：VI、VIM、Emacs ...

​    VIM :编辑器之神（专攻编辑器）。

​    emacs：文本编辑器（神之编辑器，什么都做）

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/vi%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

### 一般模式：

  注意： 在一般模式中可以进行删除、复制、粘贴等的动作，但是却无法编辑文件内容的！

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/vi%E9%94%AE%E7%9B%98%E5%9B%BE.jpg)

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/vi%E9%94%AE%E7%9B%98%E5%9B%BE.gif)

```sh
 vi/vim  文件名  -----------打开文件
按Insert键 -----------编辑文件
按ESC键输入：wq  -----------退出vi编辑器并保存
$(Shift+4) -----------移动到行尾
^(Shift+6) -----------移动到行头
w  -----------跳转到下一个单词（词头）
e  -----------跳转到下一个单词（词尾）
b  -----------跳转到上一个单词（词头）
p -----------粘贴
gg / H -----------跳转到页头
G /  L -----------跳转到页尾
  
y$   -----------复制当前行，从当前单词到行尾的所有内容
y^   -----------复制当前行，从当前单词到行头的所有内容    
s    -----------（剪切当前字母）
X    ----------- 剪切前一个字母
r    -----------替换当前字母
R    -----------替换当前开始的字母（Esc退出替换）     
行数 dd -----------删除多行
行数 yy -----------复制多行
shift + v(可视化操作) 再加 G +复制键 -----复制当前文本所有内容

   
     
ifconfig -----------查询ip地址（ens33里查看）
  ip地址详解：   
     lo里的是：回环地址（127.0.0.1）
```



### 编辑模式：

1.进入编辑模式

   注意：出现『INSERT或 REPLACE』的字样，为进行编辑。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/vi%E7%BC%96%E8%BE%91%E6%A8%A1%E5%BC%8F.png)

2.按『Esc』键 退出编辑模式，退出为一般模式。

### 命令模式：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/vi%E5%91%BD%E4%BB%A4%E8%A1%8C%E6%A8%A1%E5%BC%8F.png)



```sh
:1,.d ----删除当前屏幕前部内容（拉倒最后,删除文本全部内容）

```



# Shell脚本:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%A6%82%E5%BF%B5.png)

**Linux提供的Shell解析器**：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/Linux%20%E6%8F%90%E4%BE%9B%E7%9A%84%20Shell%20%E8%A7%A3%E6%9E%90%E5%99%A8.png)

**bash 跟 sh的关系：**

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/bash%20%E5%92%8C%20sh%20%E7%9A%84%E5%85%B3%E7%B3%BB.png)

**bash(Centos 默认解析器)：**

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/Centos%20%E9%BB%98%E8%AE%A4%E7%9A%84%E8%A7%A3%E6%9E%90%E5%99%A8%E6%98%AF%20bash.png)

​    Red Hat系列 ==》bash

​    debian系列==> dash

##  创建一个基本shell脚本：

1. 在当前目录下创建并打开helloShell脚本
2.  脚本以#!/bin/bash 开头（指定解析器）
3. 控制台输出

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/shell%E8%84%9A%E6%9C%AC%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.png)

## 脚本格式：

​     脚本以#!/bin/bash 开头（指定解析器）



## 脚本的三种执行方式：

   注意：

1.   第一种执行方法，本质是bash解析器执行脚本；第二种执行方法，本质是自己执行脚本，所以需要执行权限。
2.   前两种方式都是在当前shell中打开一个子shell来执行脚本内容，当脚本内容执行完毕，则子shell关闭，回到父shell中；而第三种脚本内容则在当前shell里执行，而无需子shell。

#### bash/sh + 脚本的相对/绝对路径(脚本不需要x权限)：

```shell
  bash  ./脚本名    
  
# bash
   bash  ./helloShell.sh    #根据-相对路径执行脚本
   bash  /home/qsl/helloShell.sh #根据-绝对路径执行脚本
# sh
   sh ./helloShell.sh       #根据-相对路径执行脚本
   sh  /home/qsl/helloShell.sh #根据-绝对路径执行脚本 
   
```



#### 脚本的相对/绝对路径（必须有x权限）*：

```shell
 chmod +x helloShell.sh #所有人都有helloShell脚本的执行权限

  ./helloShell.sh # 根据-相对路径执行脚本
  /home/qsl/helloShell.sh  #根据-绝对路径执行脚本
  
```



#### . / source:

```shell
 . 脚本名 
 . helloShell.sh #根据.,执行helloShell脚本
 source helloShell.sh #根据source,执行helloShell脚本
 
```



## 变量：

#### 系统变量：

```shell
  env # 查看所有变量
   例： env | less     # 分屏查看所有变量  
  printenv # 查看系统所有全局变量
  set  # 查看当前shell中所有变量

# 常用系统变量：
   $HOME  #  
   $PWD   # 查看当前目录
   $SHELL #  
   $USER # 查看当前使用的用户   
```



#### 自定义变量：

##### 自定义变量：

```shell
  变量名=变量值   #自定义变量（=号前后无空格,默认局部变量）
    #字符串：
       my_var="hello,Linux"
    #数值型： $((运算式)) 或$[运算式]
       my_var=$((1+5))
       my_var=$[5+9]
       
       $()     #命令替换
   readonly 变量名  #设置为静态/只读变量,只读变量不能删除
      readonly my_a=5 #设置为只读变量
    
```

   变量名定义规则：

1.   变量名称可以由字母、数字和下划线组成，但是不能以数字开头，**环境变量名建议大写**。
2. 在bash中，变量默认类型都是字符串，无法直接进行数值运算。
3. 等号两侧不能有空格。



##### 删除自定义变量：

```shell
 unset 变量名   # 删除自定义变量
   例：unset my_var  //删除自定义变量my_var
```



##### 检查是否为局部变量：

```shell
bash # ①进入子Shell（exit：退出子Shell） 
ps -f # ②查看当前系统进程，是否有子shell
set | grep 变量名 # ③检查是否为局部变量（有变量值：全局； 无提示：局部） 
```



##### 设置局部变量为全局变量：

```shell
  export 变量名 #把局部变量定义为全局变量
    例：exprot my_var #把my_var变量设置为全局变量
```



#### 特性变量:

##### $n配合basename:

​    $n:   n为数字，$0代表带当前使用的路径脚本名称，$1~$9代表1~9个参数，10以上的参数需要用大括号包含，例：${10}。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%89%A7%E8%A1%8C$n%E5%91%BD%E4%BB%A4.png)





##### $#:

   $#: 获取所有输入参数个数，常用于循环，判断参数的个数是否正确以及加强脚本的健壮性。	

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%89%A7%E8%A1%8C$#%E5%91%BD%E4%BB%A4.png)

##### $*、$@:

  $*: 代表命令行中所有的参数，$*把所有的参数看成一个整体。

  $@：也代表命令行中所有的参数，不过$@把每个参数区分对待。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%89%A7%E8%A1%8C$%E6%98%9F%E3%80%81$@%E5%91%BD%E4%BB%A4.png)

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%89%A7%E8%A1%8C$%E6%98%9F%E3%80%81$@%E5%91%BD%E4%BB%A42.png)

$*和$@的区别：

   **增强for循环的时候：**

1. ​    带" "双引号时（被“”包含时）： 
   -   $*会将所有的参数作为一个整体，以“$1 $2 ...$n” 的形式输出。
   -   $@会将各个参数分开，以“$1” “$2” “$n”的形式输出所有参数。
2.    不带" "双引号：
   -   $*和$@都以 $1” “$2” “$n”的形式输出所有参数。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/$%E6%98%9F%E5%92%8C$@%E7%9A%84%E5%8C%BA%E5%88%AB.png)

##### $?：

   $?:   最后一次执行的命令的返回状态。0：上一个命令执行成功； 非0（具体数，由命令决定），则表明上一个命令执行没完成。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%89%A7%E8%A1%8C$%EF%BC%9F%E5%91%BD%E4%BB%A4.png)

#### 运算符：

```shell
$((运算式)) 或$[运算式]
   例：my_var=$((1+5))
       my_var=$[5+9]
       
expr 1 + 6    #算法6+1
expr 6 \* 2   #算法6乘2
  例：a=$(expr 5 + 2)  #定义a变量
     a=`expr 5 + 1`   #定义a变量   


```



## 条件判断：*

### 基本语法：

 ```shell
   test 条件（condition）   #方式1: 条件判断(条件的=前后有空格，不然成了定义变量)
   [ 条件 ]      #方式2： 条件判断(条件和=前后都有空格，不然成了定义变量)

 ```

   注意:   条件非空既为true：例：[ abcdefg ] 为true；  [   ]为false。

#### test 条件：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%ADtest.png)

#### [ 条件 ]：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%AD%5B%5D.png)

### 常用判断条件：

#### 字符串判断：

   ```shell
 =: 等于                     !=: 不等于
   ```

#### 整数判断：

  注意：这里的>、<没用。

  ```shell
 -eq：等于（equal）               -ne: 不等于（not equal）
 -lt: 小于（less than）           -le: 小于等于（less equal）
 -gt: 大于(greater than)         -ge: 大于等于(greater equal)
  ```

#### 多条件判断&&与 ||  ：

 ```shell
 &&:  #前一条命令执行成功时，才执行后一条命令。
 ||:  #上一条命令执行失败后，才执行下一条命令。
 
 -a: and   -o: or
 ```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E9%80%BB%E8%BE%91%E4%B8%8Eand%E9%80%BB%E8%BE%91%E6%88%96.png)



#### 流程控制：*

##### if判断：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD.png)

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD1.png)

if参数...：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD%E5%8F%82%E6%95%B0.png)

###### 单分支：

**方法1：**

```shell
if [ 条件判断式 ];then
    代码
fi
```

  例：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD-%E5%8D%95%E5%88%86%E6%94%AF1.png)

**方法2：**

```shell
if [ 条件判断式 ]
then
 代码
fi
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD-%E5%8D%95%E5%88%86%E6%94%AF2.png)



###### 多分支：

```shell
if [ 条件判断式 ] then
  代码
elif [ 条件判断式 ]
  代码
else
  代码
fi
```

例：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/if%E5%88%A4%E6%96%AD-%E5%A4%9A%E5%88%86%E6%94%AF.png)



##### case语句：

```shell
 case $变量名  in
 "值1")
     代码1
  ;;
  "值2")
     代码2
   ;;
     ...省略其他分支
   *)  # 默认分支
     代码...
   ;;
   esac
  
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/case%E5%88%A4%E6%96%AD.png)

注意：

   注意事项： 

1. case 行尾必须为单词“**in**”，每一个模式匹配必须以右括号“ **）**”结束。 
2. 双分号“**;;**”表示命令序列结束，相当于 java 中的 break。 
3.   最后的“ ***）**”表示默认模式，相当于 java 中的 default。



##### for循环：

###### 方式1：

```shell
 for (( 初始值；循环控制条件；变量变化 ))
 do
   代码
 done
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/for%E5%88%A4%E6%96%AD1.png)

###### 方式2：

```shell
for 变量 in 值1 值2 值3 ...  
do 
  代码
done       #类似增强for
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/for%E5%88%A4%E6%96%AD2.png)

##### while循环：

 ```shell
while [ 条件判断式 ]
do
  代码
done
 ```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/while%E5%BE%AA%E7%8E%AF.png)





#### 文件权限判断：

  权限： ①-r：读权限  ②-w: 写权限  ③-x:  执行权限

##### [ 权限   文件名 ]: 

```sh
[ 权限   文件名 ]   # 方法1：文件权限判断
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90%E5%88%A4%E6%96%AD%5B%5D.png)

##### test 权限   文件名:

```shell
test 权限   文件名  # 方法2：文件权限判断
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90%E5%88%A4%E6%96%ADtest.png)

#### 文件类型判断：

-    -e:  文件存在或木（existence）。
-    -f:  文件存在(file)。
-    -d: 目录存在(directory)。

 [ 选项  文件]  ：

```shell
 [ 选项  文件]      #方法1： 文件类型判断
 test 选项   文件   #方法2： 文件类型判断
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD%5B%5D.png)

## read读取控制台输入：

```shell
 read [选项] [参数]   #读取控制台输入内容
```

| 选项 |                         说明                         |
| :--: | :--------------------------------------------------: |
|  -p  |                 指定读取值时的提示符                 |
|  -t  | 指定读取值时等待的时间（秒），如果-t不加表示一直等待 |

| 参数 |        说明        |
| :--: | :----------------: |
| 变量 | 指定读取值的变量名 |

## 函数：

### 系统函数：

#### basename 取路径里的文件名称 配合$0：

```shell
   basename  路径名(String/pathname) [suffix]
```

​     basename: 命令会删除所有的前缀包括最后一个 “ / ”字符，然后将字符串显示出来。

​      suffix(后缀)： 如果指定后缀，basename会将pathname或string中得suffix去掉。

 ![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/basename%E5%87%BD%E6%95%B0.png)

#### dirname 取文件绝对路径

```shell
    dirname  文件绝对路径 #根据绝对路径的文件名中去掉文件名（非目录部分），返回路径(目录部分)。
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/dirname%E5%87%BD%E6%95%B0.png)



### 自定义函数：

#### 自定义函数：

```shell
 [function] funname[()]
 {
     Action;
     [return int;]
 }
```

注意：

1.   必须在调用函数地方之前，先声明函数，shell脚本是逐步执行的。
2.   函数返回值，只能通过$?系统变量获得;   可以显示加：return返回。默认，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0.png)



#### 解决自定义函数返回值大于255:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E8%A7%A3%E5%86%B3%E5%87%BD%E6%95%B0%E8%BF%94%E5%9B%9E%E5%80%BC%E5%A4%A7%E4%BA%8E255%E9%97%AE%E9%A2%98.png)

## 正则表达式：

其他的类似：就是使用{ }的时候要使用-E。

```shell
    匹配11位手机号-方法1：^1[3-8][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]$
    匹配11位手机号-方法2: -E ^1[3-8][0-9]{9}$  
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F-%E6%9F%A5%E8%AF%A2%E6%89%8B%E6%9C%BA%E5%8F%B7.png)



## 文本处理工具：

### cut 剪切:

   cut命令从文件中的每一行剪切字节、字符和字段并将这些字节、字符和字段输出(要计算所有空格)。

 ```shell
 cut [选项 参数] 文件名  # 默认分隔符就是制表符
 ```

| 选项 参数 |                       功能                       |
| :-------: | :----------------------------------------------: |
|    -f     |                 列号，提取第几列                 |
|    -d     |  分隔符，按照指定分隔符分割列，默认十制表符"\t"  |
|    -c     | 按字符进行切割，切割后加n,表示去第几列。例：-c 2 |

实战：分割IP

   ![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E5%AE%9E%E6%88%98-cut%E5%88%86%E5%89%B2Ip.png)

#### 分割多列-指定n列和n列：

   根据“**|**”分割第2和第5列 ...

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%88%86%E5%89%B2%E5%A4%9A%E5%88%97--%E6%8C%87%E5%AE%9A%E5%88%97.png)



#### 分割多列-指定n列~n列：

   根据“**|**”分割2~4列。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%88%86%E5%89%B2%E5%A4%9A%E5%88%97--%E6%8C%87%E5%AE%9An%E5%88%97%20%E2%80%94%20n%E5%88%97.png)



#### 分割多列-分割前n列：

​    根据“**|**”分割前3列。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%88%86%E5%89%B2%E5%A4%9A%E5%88%97--%E5%88%86%E5%89%B2%E5%89%8Dn%E5%88%97.png)



#### 分割多列-分割后n列：

​       根据“**|**”分割后3列。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%88%86%E5%89%B2%E5%A4%9A%E5%88%97--%E5%88%86%E5%89%B2%E5%90%8En%E5%88%97.png)



### awk:

   一个强大的**文件分析工具**，把文件逐行的读入，以空格为默认分割符将每行切片，切开的部分再进行分析处理(只计算一个空格)。

#### 基本使用:

```shell
 awk [选项 参数] '/patternl/{action1}  /pattern2/{action2}...' 文件名
  例：cat /etc/passwd | awk -F ":" '/^root/{print $7}'
     print: #类似控制台输出
awk [选项 参数] '[/patternl][/{action1}] [/pattern2][/{action2}]...'  文件名   

awk [选项 参数] '{action}...' 文件名  
awk [选项 参数] '[BEGIN{action}] {action} [AND{action}]' 文件名     
```

  pattern: 表示awk在数据中查找的内容，就是匹配模式。

  action: 在找到匹配内容时所执行的一系列命令(类似：脚本、代码块...)。

​    注意：只有匹配了 pattern 的行才会执行 action。

​    BEGIN：在所有数据读取行之前执行。

​    END:  在所有数据执行之后执行。

| 选项 参数 | 功能                           |
| :-------: | ------------------------------ |
|    -F     | 指定输入文件分隔符(默认：空格) |
|    -v     | 赋值一个用户定义变量           |

#### 查询多列-指定n列和n列：

​     查询 passwd 文件以 root 关键字开头的所有行，并输出该行的第 1 列和第 7 列， 中间以“，”号分割。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/awk%E6%9F%A5%E8%AF%A2%E5%A4%9A%E5%88%97-%E6%8C%87%E5%AE%9An%E5%88%97.png)



#### 内置变量：

|   变量   |   说明   |
| :------: | :------: |
| FILENAME |  文件名  |
|    NR    |   行号   |
|    NF    | 列的个数 |

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/awk%E5%86%85%E7%BD%AE%E5%8F%98%E9%87%8F.png)



### ctu和awk的区别：

区别1：

   搜索 passwd 文件以 root 关键字开头的所有行，并输出该行的第 7 列内容。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%92%8Cawkde%20%E5%8C%BA%E5%88%AB.png)

区别2：

​      查询ifconfig中的空行行号。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/cut%E5%92%8Cawkde%20%E5%8C%BA%E5%88%AB2.png)



## 发送消息：

### 发送消息：

1.   mesg工具开启或关闭接收消息功能。
2.   write工具发送消息。

```shell
mesg  (is y：已打开；  is n：已关闭)   #查看是否打开接收消息功能
mesg y #打开接收消息功能
mesg n #关闭接收消息功能

write 用户名 接收窗口  #向用户发送消息。
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF.png)



## 综合使用：

### 自动备份

​     需求：实现一个每天对指定目录归档备份的脚本，输入一个目录名称（末尾不带/），将目录下所有文件按天，归档保存，并将归档日期附加在归档文件名上，放在/root/archive下。

​     需求：实现一个每天对指定目录归档备份的脚本，输入一个目录名称（末尾不带/），将目录下所有文件按天，归档保存，并将归档日期附加在归档文件名上，放在/root/archive下。

```shell
1 #!/bin/bash
  2 
  3  # 首先判断输入参数是否为1
  4  if [ $# -ne 1 ]  # $#:获取所有输入参数个数
  5  then
  6    echo "参数错误！只能输入一个参数，作为归档目录名"
  7    exit
  8  fi
  9 
 10  # 从参数中获取目录名称
 11  if [ -d $1 ] # -d: 目录存在
 12  then
 13    echo
 14  else
 15    echo 
 16    echo "目录不存在!"
 17    echo 
 18    exit
 19  fi
 20 
 21   DIR_NAME=$(basename $1) #  basename 取路径里的文件名称
 22   DIR_PATH=$(cd $(dirname $1); pwd) # birname 取文件的绝对路径
 23 
 24 # 获取当前日期
 25   DATE=$(date +%y%m%d)
 26 # 定义生成的归档文件名称
 27   FILE=archive_${DIR_NAME}_$DATE.tar.gz
 28   DEST=/root/archive/$FILE # 定义归档路径
 29 
 30 # 开始归档目录文件
 31   echo "开始归档..." 
 32   echo 
 33   tar -czf $DEST $DIR_PATH/$DIR_NAME
 34 
 35   if [ $? -eq 0 ]
 36   then
 37     echo 
 38     echo "归档成功！"
 39     echo "归档文件为：$DEST"
 40     echo
 41   else
 42     echo "归档失败！" 
 43     echo
 44   fi
 45 exit

```



### 发送消息脚本：

​         需求：实现一个向某个用户快速发送消息的脚本，输入用户名作为第一个参数，第二个参数为要发送的消息。脚本需要检测用户是否登录在系统中，是否打开消息功能，以及当前发送消息是否为空。

​    脚本使用图：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Linux/shell/%E5%8F%91%E9%80%81%E6%B6%88%E6%81%AF%E8%84%9A%E6%9C%AC.png)

   定义脚本：

```shell
  1 #!/bin/bash
  2 
  3 # 查看用户是否登录(用户名忽略大小写)
  4 login_user=$(who | grep -i -m 1 $1 | awk '{print $1}')
  5 
  6 if [ -z  $login_user ]
  7 then
  8    echo '$1 不在线！'
  9    echo '退出脚本...'
 10    exit
 11 fi
 12 
 13 # 查看用户是否开启消息
 14 is_allowed=$(who -T | grep -i -m 1 $1 | awk '{print $2}')
 15 
 16 if [ $is_allowed != "+" ]
 17 then
 18   echo '$1，没有开启消息功能'
 19   echo '退出脚本...'
 20   exit
 21 fi
 22 
 23 # 确认是否有消息发送
 24   if [ -z $2 ]
 25   then
 26     echo '没有消息发送'
  1 #!/bin/bash
  2 
  3 # 查看用户是否登录(用户名忽略大小写)
  4 login_user=$(who | grep -i -m 1 $1 | awk '{print $1}')
  5 
  6 if [ -z  $login_user ]
  7 then
  8    echo '$1 不在线！'
  9    echo '退出脚本...'
 10    exit
 11 fi
 12 
 13 # 查看用户是否开启消息
 14 is_allowed=$(who -T | grep -i -m 1 $1 | awk '{print $2}')
 15 
 16 if [ $is_allowed != "+" ]
 17 then
 18   echo '$1，没有开启消息功能'
 19   echo '退出脚本...'
 20   exit
 21 fi
 22 
 23 # 确认是否有消息发送
 24   if [ -z $2 ]
 25   then
 26     echo '没有消息发送'
 27     echo '退出脚本...'
 28     exit
 29   fi
 30 
 31 # 从参数中获取要发送的消息(消息从第二列开始提取)
 32 whole_msg=$( echo $* | cut -d ' ' -f 2- )
 33 
 34 # 获取用户登录的终端
 35 user_terminal=$( who | grep -i -m 1 $1 | awk '{print $2}')
 36 
 37 # 写入要发送的消息
 38 echo $whole_msg | write $login_user $user_terminal
 39 
 40 if [ $? != 0 ]
 41 then
 42   echo '发送失败！'
 43 else
 44   echo '发送成功！'
 45 fi

```


























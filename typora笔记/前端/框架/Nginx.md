# 简介：

默认端口为：80

​    *Nginx* (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。官方测试Nginx能够支持5万并发连接，并且cpu、内存等资源消耗却非常低，运行非常稳定。

​     同Tomcat一样，Nginx可以托管用户编写的WEB应用程序成为可访问的网页服务，同时也可以作为流量代理服务器，控制流量的中转。

​    Nginx只能部署静态网站，但能通过Tomcat部署网站搭集群。

Nginx使用场景：

1. http服务器。Nginx是一个http服务可以独立提供http服务，可做网页静态服务器器。
2. 虚拟主机。可以实现在一台服务器虚拟多个网站。例：个人网站使用的虚拟主机。
3. 反向代理、负载均衡。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器时；可以使用Nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载搞宕机而某台服务器闲置的情况。



# 安装：

## 前提：

前提：安装Nginx必须具备gcc环境。

```nginx
yum install gcc-c++
```

其他环境：

​     PCRE(Perl Compatible Regular Expressions)是一个Rerl库，包括perl兼容的正则表达式库，nginx的http模块使用pcre来解析正则表达式。
​     注：pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库。

```shell
yum install -y pcre pcre-devel   # 安装PCRE
```

   zlib库提供了很多中压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip。

```shell
yum install -y zlib zlib-devel # 安装zlib
```

   OpenSSl是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的秘钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试及其他目的使用。Nginx不仅支持http协议，还支持https(既在ssl协议上传输http)。

```shell
yum install -y openssl openssl-devel # 一键安装openssl
```

## Nginx安装:

1.上传并解压Nginx。

2.生成Makefile文件

​     进入nginx的configure的同级目录，执行以下命令。具体请看configure参数

```shell
./configure \
--prefix=/opt/nginx \ 
--pid-path=/var/run/nginx/nginx.pid \ 
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \ 
--http-log-path=/var/log/nginx/access.log \ 
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi

--prefix=/usr/local/nginx  /opt/nginx \ 
--with-http_gzip_static_module \ #  nginx-1.8.0版本不需要此操作
```



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/Nginx%E5%AE%89%E8%A3%85.png)



3.执行make命令

```shell
make  # 编译-生成.gc文件
make install # 安装
```

启动Nginx

```shell
mkdir /var/temp/nginx/client -p  # 新增临时文件目录
cd /usr/local/ngiux/sbin  # 进入sbin目录
./nginx   # 启动nginx（启动完浏览器输入ip打开网页）

./nginx -s stop # 关闭nginx  
./nginx -s quit # 保存配置再退出nginx
./nginx -s reload   # 重启nginx
```





​     Makefile是一种配置文件，   Makefile 一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为 makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。

## configure参数

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/Nginx%E5%8F%82%E6%95%B0.png)







# 前端项目部署：

## 项目部署：

   前端项目替换Html里的文件：

```js
nginx.exe  // 命令提示符执行命令-启动Nginx

nginx.exe -s stop // 关闭Nginx（另开窗口）
```



解决部署前端404问题：

nginx.conf新增:

```nginx
location /prod-api/ { # 拦截路径,只允许带prod-api路径访问
      proxy_pass   http://localhost:8080/; # 代理服务器-后端端口
 }
```

### 部署多个：

通过配置多个server部署多个项目。

   浏览器输入http://192.168.168.140/:80 可以看到首页面

   浏览器输入http://192.168.168.140/:80 可以看到注册页面

```nginx
server { # 黑马旅游网-官网
    listen       80; # 浏览器-访问端口
    server_name  www.qsl.com; # 浏览器-访问域名或ip
    location / { # 拦截路径，/为不拦截
        root  html/hmTourismNetwork/index/;   # 根目录
        index  index.html ;  # 默认首页
    }   
     error_page   500 502 503 504  /50x.html;	# 错误页面
    location = /50x.html {
        root   html;
    }
}   

server { # 黑马旅游网-登录
    listen       80; 
    server_name  regist.qsl.com;
    location / { 
        root  html/hmTourismNetwork/regist/ ;
        index  regist.html;
    }   
}   
```



## 域名与IP绑定：

一个域名对应一个 ip 地址，一个 ip 地址可以被多个域名绑定。

### win10:

​    Win10修改hosts 文件，路径： C:\Windows\System32\drivers\etc\hosts 

​    可以配置域名和 ip 的映射关系，如果 hosts 文件中配置了域名和 ip 的对应关系，不需要走dns 服务器。

```js
192.168.177.129	www.qsl.com
192.168.177.129	regist.qsl.com
```

### nginx/html:

  html/hmTourismNetwork的**index和regist**目录分别放置**官网和登录**入口网址。

### nginx.conf:

修改nginx配置文件，重启nginx

```nginx
server { # 黑马旅游网-官网
    listen       80; # 端口
    server_name  www.qsl.com; # 域名或ip
    location / { # 访问路径配置
        root  html/hmTourismNetwork/index/;   # 根目录
        index  index.html ;  # 默认首页
    }   
}   

server { # 黑马旅游网-登录
    listen       80; 
    server_name  regist.qsl.com;
    location / { 
        root  html/hmTourismNetwork/regist/ ;
        index  regist.html;
    }   
}   
```

浏览器输入网址-结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/IP%E4%B8%8E%E5%9F%9F%E5%90%8D%E7%BB%91%E5%AE%9A.png)

## 反向代理与负载均衡:

注意：**正向代理**是代理你的客户端，而**反向代理**是代理“服务端/服务器”的。

### 正向代理/代理：



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/%E6%AD%A3%E5%90%91%E4%BB%A3%E7%90%86.png)



### 反向代理:

#### 原理：

​    反向代理（Reverse Proxy）方式是指以[代理服务器](http://baike.baidu.com/item/代理服务器)来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86.png)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%862.png)

#### 配置：

反向代理流程图：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Nginx/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

##### Tomcat、Nginx:

1.Jar包名打成ROOT.war，并上传到Tomcat/webapps目录。

2.启动并访问Tomcat，有网站成功。

3.配置nginx/conf/nginx.conf文件：

反向代理：Nginx代理多个Tomcat。

```nginx
upstream tomcat-travel{
    server 192.168.139.140:8080; # ip+Tomcat端口
    server 192.168.139.140:8081 ;
    server 192.168.139.140:8082 ;
}
# Nginx三模块：handler、filter和upstream。
# handler：使nginx跨越单机的限制，完成网络数据的接收、处理和转发。
# upstream：模块提供数据转发功能。

server {
    listen       80; # 监听的端口
    server_name  www.qsl.com; # 域名或ip
    location / {	# 访问路径配置
        proxy_pass http://tomcat-travel;
        index  index.html index.htm; # 默认首页
    }
}
```



##### win10:

Win10配置hosts 文件 C:\Windows\System32\drivers\etc\hosts 

```js
...
192.168.139.140	www.qsl.com
...
```



### 负载均衡：*

#### 原理：

​    负载均衡 建立在现有网络结构之上，它提供了一种廉价有效透明的方法扩展[网络设备](http://baike.baidu.com/item/网络设备)和[服务器](http://baike.baidu.com/item/服务器)的带宽、增加[吞吐量](http://baike.baidu.com/item/吞吐量)、加强网络数据处理能力、提高网络的灵活性和可用性。

   负载均衡(Load Balance)，其意思就是分摊到多个操作单元上进行执行，例如Web[服务器](http://baike.baidu.com/item/服务器)、[FTP服务器](http://baike.baidu.com/item/FTP服务器)、[企业](http://baike.baidu.com/item/企业)关键应用服务器和其它关键任务服务器等，从而共同完成工作任务。

​    负载均衡：一对多的形式配置Nginx和Tomcat。

#### 配置：

1.配置并启动Tomcat集群：8080,8081,8082

2.为了能够区分Tomcat,在首页加端口号

3.配置nginx/conf/nginx.conf文件：

```nginx
# Tomcat集群   weight:设置权重（默认1）。 
# 定义一组服务器， 这些服务器可以监听不同的端口。 而且，监听在tcp和unix域套接字的服务器可以混用。
upstream tomcat-travel { 
    server 192.168.139.140:8080 ; # ip+Tomcat端口
    server 192.168.139.140:8081 weight=2 ;
    server 192.168.139.140:8082 ;
}

server {
    listen       80; # 监听的端口
    server_name  www.qsl.com; # 域名或ip
    location / {	# 访问路径配置
        # root   index;# 根目录
        proxy_pass http://tomcat-travel;

        index  index.html index.htm; # 默认首页
    }
    error_page   500 502 503 504  /50x.html;	# 错误页面
    location = /50x.html {
        root   html;
    }
}
```



4.Win10配置hosts 文件 C:\Windows\System32\drivers\etc\hosts 

```js
...
192.168.139.140	www.qsl.com
...
```



# upstream参数说明：



| 参数                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| weight                                                       | 设置权重（默认1）                                            |
| down                                                         | 移除服务器                                                   |
| max_fails                                                    | 设定nginx与服务器通信的尝试失败的次数                        |
| fail_timeou                                                  | 规定的时间内，失败的次数达到此值，nginx服务器不可用。在下一个fail_timeout时间段，服务器不会再被尝试。失败的尝试次数默认是1。设为0停止统计尝试次数，服务器一直可用的。 |
| backup                                                       | 设为备用服务器。当主服务器不可用后使用。                     |
| http_404                                                     | 不被认为是失败的尝试                                         |
| 指令proxy_next_upstream、 fastcgi_next_upstream和memcached_next_upstream | 配置失败尝试。                                               |

nginx/conf/nginx.conf文件：

```nginx
# Tomcat集群   weight:设置权重（默认1）。    down：移除服务器。
# max_fails=number设定nginx与服务器通信的尝试失败的次数。
upstream tomcat-travel {  # 定义一组服务器， 这些服务器可以监听不同的端口。 而且，监听在tcp和unix域套接字的服务器可以混用。
    server 192.168.139.140:8080 down; # ip+Tomcat端口
    server 192.168.139.140:8081 weight=2 ;
    server 192.168.139.140:8082 max_fails=2 fail_timeou=30s backup ;
}

server {
    listen       80; # 监听的端口
    server_name  www.qsl.com; # 域名或ip
    location / {	# 访问路径配置
        # root   index;# 根目录
        proxy_pass http://tomcat-travel;

        index  index.html index.htm; # 默认首页
    }
    error_page   500 502 503 504  /50x.html;	# 错误页面
    location = /50x.html {
        root   html;
    }
}
```


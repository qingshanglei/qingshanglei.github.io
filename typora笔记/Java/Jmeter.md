## 概念：

APache组织开发的基于java的压力测试工具。
100%纯Java开发、完全的可移植性。
可以用于测试静态和动态资源。
多协议——http/ftp/scoket/Java/数据库（JDBC）
完全多线程
高可扩展性



## 安装：

前提：必须安装JDK。

JMeter与JDK版本关系:
   JMeter2.X  => jdk1.6
   JMeter3.0/3.1  => 最低jdk1.7
   JMeter3.2/3.3  => 最低jdk1.8 

系统要求：Java开发，可跨平台。
==Windows:== apche-jmeter-xx.zip
==Linux/Mac:== apache-jmeter-xx.tgz

官网：[JMeter](https://jmeter.apache.org/download_jmeter.cgi)

下载解压，及成功安装。

Jmeter/bin/jmeter.bat双击即可运行。

### 配置环境变量：

配置环境变量：



## 启动Jmeter:

```sh
jmeter --version # 检查jmeter是否安装成功（有版本号，成功）
jmeter  # 命令行提示符输入
```

出现以下窗口表示启动启动Jmeter成功：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Jmeter/%E5%90%AF%E5%8A%A8Jmater.png)



注意：开启Jmeter，不能关闭以下窗口。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Jmeter/%E5%90%AF%E5%8A%A8Jmater2.png)



## 设置中文：

### 临时设置：

 选择Options—>Choose Language—>Chinese（Simplified）简体

  注意：此设置重启Jmeter又会恢复英文。

### 永久性生效：

- 进入到Jmeter的bin目录下，找到jmeter.properties文档以记事本的方式打开
- 查找language，找到language=en的行改为cn
- 重启jmeter，语言设置成功

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Jmeter/Jmeter%E4%B8%AD%E6%96%87%E8%AE%BE%E7%BD%AE.png)



## 基本使用：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Jmeter/Jmeter/%E6%96%B0%E5%BB%BA%E7%BA%BF%E7%A8%8B%E7%BB%84.png)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Jmeter/Jmeter/%E5%8F%91%E9%80%81%E8%AF%B7%E6%B1%82.png)
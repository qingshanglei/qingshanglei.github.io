# 简介：

  nodejs是JavaScript运行环境，类似java的jdk,不需要浏览器通过nodejs直接运行js文件。

  nodejs作为服务端使用。

​     ①Node.js 就是运行在服务端的 JavaScript。
   ② Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。
​    ③Node.js 是运行在服务端的 JavaScript。
​    ④Node.js可部署后端的高性能的服务。
​    ⑤使用Node.js前端人员不需要懂得PHP、Python或Ruby等动态编程语言，即可创建服务。

  

注意：node命令不能在vscode里使用。

# 安装：

官网：https://nodejs.org/en/ 
中文网：http://nodejs.cn/ 
版本说明：
   LTS：长期支持版本
   Current：最新版

  安装：默认下一步。



命名提示符或VsCode:

```sh
node -v  // 检查是否安装成功(成功：有版本号)


# 配置安装路径
setx PATH "%PATH%;F:\Nodejs\node14.14.0" /M

```

## 安装错误：

###    将VS Code以管理员方式运行即可

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/NodeJs/node%20-v%E6%8A%A5%E9%94%99.bmp)



### 其他：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/NodeJs/nodejs%E5%AE%89%E8%A3%85%E9%97%AE%E9%A2%98.png)



## 切换node版本：

### n模块：

1.解压node安装包

2.安装n模块，并切换版本

```sh
npm install -g n   # 安装 n 模块 注意：只能Linux系统使用。
sudo n <version>  # 切换 Node.js 版本，需要管理员权限
```



## nvm:

```sh
#安装nvm
npm install -g nvm 

# 查看安装版本（有版本号即可）
nvm -v

# 切换node版本为14.17.0，默认版本为v16.17.0
nvm install 14.17.0

# 检查版本
node -v 

```
























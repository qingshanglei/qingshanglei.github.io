# 简介：

​     NPM全称Node Package Manager，是Node.js包管理工具，是全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的包管理工具，相当于后端的Maven 。

​     Npm: ①构建前端项目   ②管理前端依赖



​    Npm不需要安装，安装Node-js后自带安装。

​    查看是否安装成功：

​     在命令提示符或Vs Code

```js
npm -v  // 查看是否安装成功    成功：出现npm版本号
```





# 创建Npm项目：

## 命令

| 选项 | 说明  |
| :--: | ----- |
|  -y  | 全yes |



```shell
#建立一个空文件夹，在命令提示符进入该文件夹  执行命令初始化
npm init  [选项]
#按照提示输入相关信息，如果是用默认值则直接回车即可。
# name: 项目名称
# version: 项目版本号
# description: 项目描述
# keywords: {Array}关键词，便于用户搜索到我们的项目
# 最后会生成package.json文件，这个是包的配置文件，相当于maven的pom.xml
```



## 结果：

### package.json:

​    注意：在 JSON 格式中，没有原生支持注释的语法。因此 // 或 /*...*/等注释不能使用； 只能通过"private" 字段设置为 true，以防止该包被意外地发布到 npm 或其他源代码管理系统中，达到注释效果。

```json
{
    "name": "npmdemo",
    "version": "1.0.0",
    "description": "",
    "main": "index.js", 
    "private": true, // 
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "",
    "license": "ISC"
}
```

| 参数                | 内容                                                         |
| ------------------- | ------------------------------------------------------------ |
| **name**            | 项目/模块名称，长度必须小于等于214个字符，不能以"."(点)或者"_"(下划线)开头，不能包含大写字母 |
| **version**         | 项目版本                                                     |
| author              | 项目开发者，它的值是你在https://npmjs.org网站的有效账户名，遵循“账户名<邮件>”的规则，例如：zhangsan zhangsan@163.com |
| **description**     | 项目描述，是一个字符串。它可以帮助人们在使用npm search时找到这个包 |
| **keywords**        | 项目关键字，是一个字符串数组。它可以帮助人们在使用npm search时找到这个包 |
| private             | 是否私有，设置为 true 时，npm 拒绝发布                       |
| license             | 软件授权条款，让用户知道他们的使用权利和限制                 |
| bugs                | bug 提交地址                                                 |
| contributors        | 项目贡献者                                                   |
| repository          | 项目仓库地址                                                 |
| homepage            | 项目包的官网 URL                                             |
| **dependencies**    | 生产环境下，项目运行所需依赖                                 |
| **devDependencies** | 开发环境下，项目所需依赖                                     |
| **scripts**         | 执行 npm 脚本命令简写，比如 “start”: “react-scripts start”, 执行 npm start 就是运行 “react-scripts start” |
| bin                 | 内部命令对应的可执行文件的路径                               |
| main                | 项目默认执行文件，比如 require(‘webpack’)；就会默认加载 lib 目录下的 webpack.js 文件，如果没有设置，则默认加载项目跟目录下的 index.js 文件<br/>module	以 ES Module(也就是 ES6)模块化方式进行加载，因为早期没有 ES6 模块化方案时，都是遵循 CommonJS 规范，而 CommonJS 规范的包是以 main 的方式表示入口文件的，为了区分就新增了 module 方式，但是 ES6 模块化方案效率更高，所以会优先查看是否有 module 字段，没有才使用 main 字段 |
| **eslintConfig**    | EsLint 检查文件配置，自动读取验证                            |
| engines             | 项目运行的平台                                               |
| browserslist        | 供浏览器使用的版本列表                                       |
| style               | 供浏览器使用时，样式文件所在的位置；样式文件打包工具parcelify，通过它知道样式文件的打包位置 |
| files               | 被项目包含的文件名数组                                       |



# Npm目录说明：



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Npm/npm%E7%9B%AE%E5%BD%95%E8%AF%B4%E6%98%8E.png)

# 修改npm镜像和仓库：

   NPM官方 http://npmjs.com
   淘宝 NPM 镜像 http://npm.taobao.org/  （淘宝 NPM 镜像是一个完整 npmjs.com 镜像，同步频率目前为 10分钟一次，以保证尽量与官方服务同步）



## 修改镜像地址：

```shell
# 修改npm地址为淘宝镜像-好像过期了
npm config set registry https://registry.npm.taobao.org 
# 修改npm地址为淘宝镜像2
npm config set registry https://registry.npmjs.org/

# 查看npm配置信息
npm config list

# 清除npm的缓存，删除npm缓存中存储的所有数据(一般包安装失败、版本不匹配使用)
npm cache clean --force
```



## 修改仓库地址：

 默认仓库地址是当前用户目录下，如果当前用户**目录有中文**，需要修改

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Npm/Npm%E4%BB%93%E5%BA%93.png)

```shell
# 配置全局安装：
npm config set prefix D:\atguigu\node-global

# 配置缓存路径：
npm config set cache D:\atguigu\node-cache

#查看npm配置信息
npm config list
```



# Npm命令：

## npm install命令：

| 选项 | 说明 |
| ---- | ---- |
|      |      |

1. 模块安装的位置：项目目录\node_modules。
2. 安装会自动在项目目录下添加 package-lock.json文件，这个文件帮助锁定安装包的版本。
3. 同时package.json 文件中，依赖包会被添加到dependencies节点下，类似maven中的 <dependencies>。

```shell
#使用 npm install 安装依赖包的最新版，
npm install jquery    # 安装jquery
npm i jquery    # 简写形式安装jquery
#npm管理的项目在备份和传输的时候一般不携带node_modules文件夹
npm install # 根据package.json中的配置下载依赖，初始化项目
# 安装jquery指定版本，版本降级或升级也是此命令。
npm install jquery@2.1.x 

# 局部安装
#devDependencies节点：开发时的依赖包，项目打包到生产环境的时候不包含的依赖
# 使用 -D参数将依赖添加到devDependencies节点
npm install --save-dev eslint
#或
npm install -D eslint

#全局安装
#Node.js全局安装的npm包和工具的位置：用户目录\AppData\Roaming\npm\node_modules
# 全局安装webpack，-g 或 --global都表示全局安装
npm install -g webpack --global  # 方式1
npm install  webpack --global # 方式2

npm rebuild # 重新构建依赖
npm cache clean # 清除本地的 npm 缓存。
npm cache clean --force # 清除本地的 npm 缓存，带 "npm WARN using --force. Recommended protections disabled." 的警告提示

```





## 其它命令：

```shell
# 更新包（更新到最新版本）
npm update 包名
# 全局更新
npm update -g 包名
# 卸载包
npm uninstall 包名
# 全局卸载
npm uninstall -g 包名
```





## 镜像：

cnpm:

​     cnpm是淘宝镜像为了解决npm在国内下载速度慢而推出的npm加速工具,使用方式与npm基本相同,但cnpm有安全性、源代码方面等问题。
​    但不推荐直接使用 cnpm 安装以来，会有各种诡异的 bug。

```sh
npm install cnpm # 安装cnpm

cnpm install  node-sass # 使用cnpm安装node-sass
```






































































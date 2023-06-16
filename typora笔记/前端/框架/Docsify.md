# 简介：

官网：https://docsify.js.org/#/

注意：docsify自带热启动。

1. 前提：安装Node
2. 命令行安装Docsify

```js
//全局安装Docsify
npm i docsify-cli -g 
```

安装成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E5%AE%89%E8%A3%85Docsify%E6%88%90%E5%8A%9F.png)



# 创建项目：

1. 创建项目

```js
cd Desktop/ // 切到桌面(win10)
   
// 创建项目
docsify init ./blogs(项目名)

```

2. 编辑README.md(主页面)文件

   

2. 启动项目：

```js

// 启动项目：http://localhost:3000 （注意：要在项目的上一级启动。）
docsify serve blogs(项目名)
docsify serve blogs -p 8080  // 指定端口启动项目
```

启动成功：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E5%90%AF%E5%8A%A8%E6%88%90%E5%8A%9F.png)



## 项目目录详情：

```js
index.html  //入口文件
README.md //主页
.nojekyll   // GitHub 页面忽略以下划线开头的文件
```



# 多页文档：

   README.md配置跳转页面(a标签):

```js
// [名称](路径)   跳转页面，类似a标签  (注意：①文件尾缀可不写，②忽略大小写,③主页面路径即可“ / ”，当然文件名(readme.md)也可以。）
[Guide](guide.md)
[<<返回主页面](/)
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E8%B7%B3%E8%BD%AC%E9%A1%B5%E9%9D%A2.png)



# _sidebar.md(页面侧边栏)：

## 单级目录：

1. 创建_sidebar.md文件,并在侧边栏指定文件路径

```js
[首页](/) # 跳转到首页  注意：目录带空格必须转码“%20”  例：====>

[操作指南](readme.md) # 跳转到操作指南页面
[操作指南](java%20SE.md) #  java SE.md目录
```



2. index.html文件，开启侧边栏

```js
<script>
  window.$docsify = {
    loadSidebar: true, //开启侧边栏
  }
</script>
```

结果：  没居中对齐等，很丑。。。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E9%A1%B5%E9%9D%A2%E4%BE%A7%E8%BE%B9%E6%A0%8F.png)



## 多级目录：

​    目录结构：

在`_sidebar.md` 文件中写下目录链接：

```markdown
* 前端技术
  * [javascript](01/javascript/javascript.md) 
  * [echarts](01/echarts/)
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E9%A1%B5%E9%9D%A2%E4%BE%A7%E8%BE%B9%E6%A0%8F-%E5%A4%9A%E7%BA%A7%E7%9B%AE%E5%BD%95.png)





## 搜索框：

index.html

```html
<script>
    window.$docsify = {
        search: { // 开启导航栏-搜索框
            maxAge: 86400000, // 过期时间(毫秒)，默认一天
            noData: '无数据!', // 搜索不到结果是显示
            // paths: 'auto', // auto:自动
            placeholder:  {
                '/zh-cn/': '搜索',
                '/': '🔍点击这里搜索'
            },, // 搜索框提示
            hideOtherSidebarContent: false, // 是否隐藏其他侧边栏内容           
            namespace: '技术文档',
        }
    }
</script>
<!--   搜索框-开启 -->
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
<!-- 搜索插件支持会忽略双音符（例如，"cafe" 也会匹配 "café"） 注意：IE11等旧版浏览器需要使用以下 String.normalize() polyfill 来忽略双音符 -->
<script src="//polyfill.io/v3/polyfill.min.js?features=String.prototype.normalize"></script>
<!--   搜索框-结束 -->

<!--  注意不要引入<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>插件，否则导航栏不能使用 -->
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E4%BE%A7%E8%BE%B9%E6%A0%8F-%E6%90%9C%E7%B4%A2%E6%A1%86.png)







## 显示文件的标题:

说明

```js
.
└── docsify //项目名
    ├── README.md  //
    ├── index.html  // 
    ├── guide.md
    └── _sidebar.md // 定义侧边栏
        ├── 01 // 文件夹
          └── echarts.md   // 文件
          └── javascript.md // 文件
        ├── 02 // 文件夹
           ...
    └── _navbar.md // 定义侧边栏
```

index.html文件:

```java
<script>
    window.$docsify = {
    name: '',
    repo: '',
    loadSidebar: true, //开启侧边栏
    subMaxLevel:1, // 显示文件的1级标题（1-6等级）
}
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E9%A1%B5%E9%9D%A2%E4%BE%A7%E8%BE%B9%E6%A0%8F-%E6%98%BE%E7%A4%BA%E6%A0%87%E9%A2%98.png)

# _navbar.md(导航栏):

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E5%AF%BC%E8%88%AA%E6%A0%8F.png)

## HTML方式：

index.html

```html
<body>
    <nav>     <!-- 导航栏 -->
        <a href="#/">EN</a>
        <a href="#/zh-cn/">中文</a>
    </nav>
    <div id="app"></div>
    <script>
        window.$docsify = {
            name: '',
            repo: '',
            loadSidebar: true, //开启侧边栏
            loadNavbar: true, // 开启导航栏 
        }
    </script>
</body>
```





## Markdown:

```html
<script>
    window.$docsify = {
    name: '',
    repo: '',
    loadSidebar: true, //开启侧边栏
    loadNavbar: true, // 开启导航栏 
}
</script>
```





### 单级目录：

1. 创建_navbar.md文件,并在侧边栏指定文件路径

​    导航栏同页面侧边栏。。。。

# 博客封面

​    注意：博客封面不是主页面。

## 博客封面:

index.html文件：

```js
<script>
    window.$docsify = {
    // 仓库地址，点击右上角的Github章鱼猫头像会跳转到此地址
    repo: 'https://github.com/ixfsz001',
    coverpage:true , // 使用封面配置
    onlyCover:false, // true:只显示封面配置，  false(默认):可滚动到主页面
}
</script>
```

_coverpage.md文件：

```markdown
<!-- 封面配置 -->

<!-- 封面路径 -->
![logo](1.jpg)

<!-- 标题，   small：小字体 -->
# docsify <small>3.5</small>

<!-- 介绍： 字体默认居中 -->
<!-- > A magical documentation site generator. -->

- 简单、轻便（压缩后 ~21kB）
- 无需生成Html文件
- 最多主题

<!-- 按钮   路径带#：禁用按钮 -->
[GitHub](https://github.com/docsifyjs/docsify/)
[主页面](01/javascript/javascript.md) 
<!--  -->

<!-- 自定义背景色（默认随机） -->
<!-- ![color](#f0f0f0) -->
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E4%B8%BB%E9%A1%B5%E9%9D%A2.png)



## 是否只显示封面配置：

​    注意：默认封面和首页同时出现，就是封面可滚动到主页面。

index.html文件：

```js
<script>
    window.$docsify = {
    onlyCover:false, // true:只显示封面配置，  false(默认):可滚动到主页面
}
</script>
```

主页面只显示封面配置:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E4%B8%BB%E9%A1%B5%E9%9D%A2.png)

主页面可滚动到项目：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Docsify/%E4%B8%BB%E9%A1%B5%E9%9D%A2-%E6%98%AF%E5%90%A6%E5%8F%AA%E6%98%BE%E7%A4%BA%E5%B0%81%E9%9D%A2%E9%85%8D%E7%BD%AE.png)



# 切换主题：

index.html引入即可：

长短连接的区别：

1.   第一个链接使用了JSdelivr CDN（内容分发网络），它会将文件缓存到离用户最近的节点，从而加速文件的加载速度。因此，如果你的网站访问者来自世界各地，使用CDN可以提高网站的响应速度和用户体验。
2.  第二个链接则相对较慢，因为是从本地主题目录中引用CSS文件，在网络环境较差的情况下，可能需要花费更长的时间才能加载完毕。

长连接： 

```html
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/vue.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/buble.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dark.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/pure.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dolphin.css">
```

短连接：

```html
<!-- 其他主题（可选） -->
<link rel="stylesheet" href="./themes/buble.css"> <!-- 蓝色 -->
<link rel="stylesheet" href="./themes/dark.css">  <!--黑色 -->
<link rel="stylesheet" href="./themes/dolphin.css">  <!-- 蓝色 -->
<link rel="stylesheet" href="./themes/pure.css">   <!-- 黑灰色 -->
```







​      注意： docsify的侧边栏默认是不支持折叠的，要实现折叠效果必须使用docsify-sidebar-flexible或docsify-sidebar-flexible插件。

docsify-sidebar-flexible插件 :

docsify-sidebar-flexible插件




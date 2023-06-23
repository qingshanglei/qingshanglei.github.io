# 概念：

​    微信⼩程序( Mini Program)，简称⼩程序，以前是叫 应⽤号  ，是⼀种不需要下载安装即可使⽤的应⽤，它实现 应⽤“触⼿可及”的梦想，⽤⼾扫⼀扫或搜⼀下即可打开应⽤。



其他的⼩程序：
1. ⽀付宝⼩程序 
2. 百度⼩程序 
3. QQ⼩程序 
4. 今⽇头条 + 抖⾳⼩程序 

[官⽅微信⼩程序体验源码 ](https://github.com/wechat-miniprogram/miniprogram-demo)

官⽅微信⼩程序体验:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%AE%98%E2%BD%85%E5%BE%AE%E4%BF%A1%E2%BC%A9%E7%A8%8B%E5%BA%8F%E4%BD%93%E9%AA%8C.png)

其他优秀的第三⽅⼩程序 

- 拼多多
- 滴滴出⾏ 
- 欢乐⽃地主 
- 智⾏⽕⻋票 
- 唯品会
。。。



## ⼩程序结构⽬录 :

⼩程序框架的⽬标是通过尽可能简单、⾼效的⽅式让开发者可以在微信中开发具有原⽣APP体验的服务。

  ⼩程序框架提供了⾃⼰的视图层描述语⾔ WXML 和 WXSS ，以及 JavaScript ，并在视图层与逻 辑层间提供了数据传输和事件系统，让开发者能够专注于数据与逻辑。 

⼩程序⽂件结构和传统web对⽐ ：

​     传统web是三层结构，微信小程序是四层结构。

| 结构 | 传统web    | 微信小程序 |
| ---- | ---------- | ---------- |
| 结构 | HTML       | WXML       |
| 样式 | CSS        | WXSS       |
| 逻辑 | Javascript | Javascript |
| 配置 | 无         | JSON       |

项⽬的基本⽬录：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E9%A1%B9%E2%BD%AC%E7%9A%84%E5%9F%BA%E6%9C%AC%E2%BD%AC%E5%BD%95.png)









## 小程序的第三方框架

1. 腾讯wepy 类似vue
2. 美团mpvue语法类似vue
3. 京东taro类似react

4. 滴滴 chameleon
5. uni-app 类似vue

6. 源生框架 MINA



## 开发⼯具： 

​    [微信开发工具官网](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)        [微信开发工具文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)

​    微信⼩程序开发者⼯具，集 开发 预览 调试 发布 于⼀⾝的 完整环境。  
​    但是由于编码的体验不算好，推荐 vs code + 微信小程序编辑工具 来实现编码vs code 负责敲代码， 微信编辑工具 负责预览 。
  注意 第⼀次登录的时候，需要微信扫码登录。



# 创建项⽬:

1.官网注册/登录小程序并获取APPID

​    ==建议使用全新的邮箱，没有注册过其他小程序或者公众号的。==

   [官网地址](https://mp.weixin.qq.com/)

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%88%9B%E5%BB%BA%E2%BC%A9%E7%A8%8B%E5%BA%8F.png)



2.新建⼩程序项⽬

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E6%96%B0%E5%BB%BA%E9%A1%B9%E7%9B%AE.png)

3.选择路径并填写小程序AppID.

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%88%9B%E5%BB%BA%E2%BC%A9%E7%A8%8B%E5%BA%8F2.png)

项目创建成功图

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E9%A1%B9%E7%9B%AE%E5%88%9B%E5%BB%BA%E6%88%90%E5%8A%9F.png)



# 配置文件：

   ⼀个⼩程序应⽤程序会包括最基本的两种配置⽂件。⼀种是全局的 app.json 和 ⻚⾯⾃⼰的page.json。（主要在这配置导航栏，及页面路径等）
       ==注意：JSON配置文件中不能注释。==

​     [⼩程序配置文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/sitemap.html)   [⼩程序配置文档2](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)  

1. 小程序根目录下的 `app.json` 文件用来对微信小程序进行全局配置。文件内容为一个 JSON 对象。
2. app.json 是当前⼩程序的全局配置，包括了⼩程序的所有⻚⾯路径、界⾯表现、⽹络超时时间、底 部 tab 等。

## 全局配置app.json：

  [全局配置文档 ](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#window)     [微信官方文档 ](https://developers.weixin.qq.com/doc/)

常用参数：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AEapp%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0.png)

```json
{  
    "entryPagePath":"pages/my/my", // 默认启动路径（首页），常见情景是从微信聊天列表页下拉启动、小程序列表启动等。默认为 pages 列表的第一项(上面第一个)。不支持页面传参参数。
    "pages":[   // 四个文件后缀可省略.json, .js, .wxml, .wxss。
        "pages/index/index", 
        "pages/logs/logs"
    ], 
    "window":{ // 用于设置小程序的状态栏、导航条、标题、窗口背景色。
        "backgroundTextStyle": "dark", // 下拉 loading 的样式，仅支持 dark / light
        "navigationBarBackgroundColor": "#10f993", // 导航栏背景颜色
        "navigationBarTitleText": "青衫泪", // 导航栏标题文字内容
        "navigationBarTextStyle": "white", // 导航栏标题颜色，仅支持 black / white
        "enablePullDownRefresh":true // 是否开启全局的下拉刷新。
    },
    "tabBar": { // 导航栏
        "list": [
            {
                "pagePath": "pages/index/index", // 页面路径
                "text": "首页", 
                "iconPath": "icon/_home.png", // 未选中图标
                "selectedIconPath": "icon/home.png" // 选中图标
            },  
            .....   ],
        "color":"#000000",  //tab上的文字默认颜色，仅支持十六进制颜色
        "selectedColor": "#b44051" //tab上的文字选中时的颜色，仅支持十六进制颜色
    },
    "sitemapLocation": "sitemap.json" // 指明 sitemap.json 的位置；  默认'sitemap.json'和 app.json 为同级目录。
}
```



​    [tabbar文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

导航栏(tabbar)对应图:

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%AF%BC%E8%88%AA%E6%A0%8Ftabbar.png)

这里只说导航栏，其他省略。。。。





## ⻚⾯配置page.json ：

   [页面配置文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/page.html)

这⾥的 page.json 其实⽤来表⽰⻚⾯⽬录下的 page.json 这类和⼩程序⻚⾯相关的配置。 开发者可以独⽴定义每个⻚⾯的⼀些属性，如顶部颜⾊、是否允许下拉刷新等等。 ⻚⾯的配置只能设置 app.json 中部分 window 配置项的内容，⻚⾯中配置项会覆盖 app.json的 window 中相同的配置项。 

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E2%BB%9A%E2%BE%AF%E9%85%8D%E7%BD%AEpage.png)



## sitemap 配置-了解即可 

​    ⼩程序根⽬录下的 sitemap.json ⽂件⽤于配置⼩程序及其⻚⾯是否允许被微信索引。 

# 模板语法： 57

​    WXML（WeiXin Markup Language）是框架设计的⼀套标签语⾔，结合[基础组件](https://developers.weixin.qq.com/miniprogram/dev/component/)、[事件系统](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)，可以构 建出⻚⾯的结构。
1. 框架的视图层由 WXML 与 WXSS 编写，由组件来进行展示。
2. 将逻辑层的数据反映成视图，同时将视图层的事件发送给逻辑层。
3. WXML(WeiXin Markup language) 用于描述页面的结构。
4. WXS(WeiXin Script) 是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。
5. WXSS(WeiXin Style Sheet) 用于描述页面的样式。
6. 组件(Component)是视图的基本组成单元。

常用标签：

```html
<view/> <!--  相当于web中的 div标签 块级元素 会换行 -->
<text/>  <!--  span标签   -->
<view/>  <!--   -->

<!--  不能写成checked="false"，其计算结果是⼀个字符串  -->
<checkbox checked="{{false}}"> </checkbox> 

                        
<text>:
  selectable:-------------长按文字复制          decode:-------------对文本内容进行 解码（有这个 &lt;才能用）
```

js:

```js
// pages/my/my.js
Page({

    /**
   * 页面的初始数据
   */
    data: {

    },

    /**
   * 生命周期函数--监听页面加载
   */
    onLoad: function (options) {

    },

    /**
   * 生命周期函数--监听页面初次渲染完成
   */
    onReady: function () {

    },

    /**
   * 生命周期函数--监听页面显示
   */
    onShow: function () {

    },

    /**
   * 生命周期函数--监听页面隐藏
   */
    onHide: function () {

    },

    /**
   * 生命周期函数--监听页面卸载
   */
    onUnload: function () {

    },

    /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
    onPullDownRefresh: function () {

    },

    /**
   * 页面上拉触底事件的处理函数
   */
    onReachBottom: function () {

    },

    /**
   * 用户点击右上角分享
   */
    onShareAppMessage: function () {

    }
})
```





### 数据绑定

普通写法 ：

```vue
<view>{{msg}}</view>      //dem3.wxml
```

```vue
page({
  data:{
     msg:"hello mina"
}
})
```

结果：



![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/wx-web%20(2).png)



组件属性：

```html
<view id="item-{{id}}"> </view>
```

 ```js
Page({
  data: {
    id: 0
 }
})
 ```



   

# wxml:

##  字符串&& 数据类型 &&布尔类型&& object类型&& 复选框&& 循环

```html
### <!-- 1 字符串 -->

<view>{{msg}}</view>

###  <!-- 2 数据类型 -->

<view>{{num}}</view>

###  <!-- 3 布尔类型 bool -->

<view>是否是伪娘:{{isGirl}}</view>

### <!-- 4 object类型 -->

<view>{{person.name}}</view>

<view>{{person.age}}</view>

<view>{{person.hegion}}</view>

<view>{{person.width}}</view>



### <!-- 5 在标签的属性中使用 -->

<view data-unm="{{num}}">自定义属性</view>



### <!-- 复选框 checkbox -->

<!-- 6 使用bool类型充当属性 checked 

​    1.字符串和 花括号之间一定不要用空格 否则会导致识别失败

​     <checkbox checked="       {{isChecked}}"></checkbox> 

-->

<checkbox checked="{{true}}"></checkbox> <!--直接使用 -->

<checkbox checked="{{isChecked}}"></checkbox> <!-- 通過wxml渲染使用 -->

```



### 7 运算 =》 表达式

​    1 可以在花括号中加入 表达式 --“语句"

​    2 表达式 

​    指的是一些简单 运算 数字运算 字符串 拼接 逻辑运算

​    1 数字的加减..

​    2 字符串拼接

​    3 三元表达式

​    3 语句

​     1 复杂的代码段

​      1 if else

​      2 switch

​      3 do while...

​      4 for 

 -->



```html
<view>{{1+1}}</view> <!-- 数字的加减 -->

<view>{{"1"+"1"}}</view> <!--拼接 -->

<view>{{ (5>3) ? 15:6}}</view><!--三元表达式-->
```



### 8 列表循环 

​    1 wx:for="{{数组或者对象}}"    wx:for-item="循环项的名称"   wx:for-index="循环项的索引"

​    2 we:key="唯一的值"用来提高列表渲染的性能

​    1 wx:key 绑定一个普通字符串的时候，那么这个字符串名称， 肯定是循环数组中的对象的 唯一属性 (例如：Id)

​    2 wx:key=" *this "  代表数组是一个普通的数组 *this表示就是循环项

​     普通的数组:[1,2,3,4]

​     循环项（要使用*this）：["1","4324", "josdfas"]

   3 当出现 数组的嵌套循环的时候 尤其要注意 以下绑定的名称 不要重名

​    wx:for-item="item"  wx:for-index="index"

   4 默认情况下我们不写

​    wx:for-item="item"  wx:for-index="index"

​    小程序也会把 循环项的名称 和 索引的名称 item 和 index 补上

​    只要一层循环的话 (wx:for-item="item" wx:for-index="index") 可以省略

 -->



### 列表循环

<view> 列表循环：

<view wx:for="{{list}}" wx:for-item="item" wx:for-index="index" wx:key="id">

  索引：{{index}}

  \-- 

  值：{{item.name}}

</view>

</view>



### 对象循环

<!-- 9 对象循环

​    1 wx:for="{{数组或者对象}}"   wx:for-item="对象的值" wx:for-index="对象的属性"

​    2 循环对象的时候 最好把 item 和 index的名称修改一下

​     wx:for-item="value" wx:for-index="key"

​    

​    -->

<view>对象循环：

 <view wx:for="{{person}}" wx:for-item="value" wx:for-index="key" wx:key="age">

   属性：{{key}}

   值：{{value}} 

 </view>

</view>



<!-- 10 block

​    1 占位符的标签

​    2 写代码的时候，可以看到标签存在

​    3 页面渲染时（启动时），小程序会打他移除

​     -->

<view>

列表循环：

<block wx:for="{{list}}" class="my-sjfos" wx:for-item="item" wx:for-index="index" wx:key="id">

  索引：{{index}}

  \-- 

  值：{{item.name}}

</block>

</view>



<!-- 11 条件渲染

​    1 wx:if="{{true/false}}" 

​     1 if , else ,if else

​      wx:if

​      wx:elif

​      wx:else

​     2 hidden

​      1 在标签上直接加入属性 hidden

​      2 hidden="{{true}}"

​     3 什么场景下用哪个

​      1 当标签不是频繁的切换显示 使用 wx:if

​       直接把标签从 页面结构给移除掉

​      2 当标签频繁的切换显示的时候 使用 hidden

​       通过添加样式的方式来切换显示（display:block或者none）

​         例： <view hidden style="display:flex">hidden</view> 

 --> 

<view>

 条件渲染：

 <view wx:if="{{true}}">显示</view>

 <view wx:if="{{false}}">隐藏</view>



 <view>-----------分割线----------------</view>



<!-- 当第1个为true是，2和3不在执行；当第1个为false是，再执行2（2位flase时）；然后执行3 -->

 <view wx:if="{{false}}"> 1</view>

 <view wx:elif="{{false}}">2</view>

 <view wx:else>3</view>



 <view>------------分割线---------------</view>

  <view hidden>hidden</view>

  <view hidden="{{false}}" > hidden="{{true}}" </view>





 <view>------------分割线000---------------</view>

  <view hidden style="display:flex">hidden</view>



</view>



```js
 data: {
     msg:"Hello mina",
     num:252525,
     isGirl:true,
     person:{
         name:"灵能",
         age:21,
        hegion:175,
         width:70
     },
     isChecked:false,
       list:[
     {
       id:0,
       name:"猪八戒",
       age:30
     },{
      id:1,
      name:"天逢元帅",
   },{
      id:2,
      name:"悟能"
   }
       ]
  },
```

## 点击事件：

### 注意：

<!-- 1 需要给input 标签绑定 input事件 

​     绑定关键字 bindinput

   2 如何获取 输入框的值

​    通过事件源对象来获取

​    e.detail.detail获取

   3 把输入框的值 赋值到 date当中

​    不能直接 

​    1 this.date.num = e.detail.value

​    2 this.num = e.detail.value

   正确的写法：

​    this，setData({

​    num: e.detail.value

​    })

   4 需要加入一个点击事件  click ==> bindtap

​    1 bindtap    :点击事件   

​    2 无法再小程序当中的 事件中 直接传参

​    3 通过自定义属性的方式来传参

​    4 事件源中获取 自定义属性     -->

<!-- <input type="text" > 记得要加上 " /" , 微信不会自动加-->

```html
   wxml:
<input type="text"  bindinput="bingha" />
<button bindtap="handletap" data-operation="{{1}}" >+</button>
<button bindtap="handletap" data-operation="{{-1}}" >-</button>
<view>{{num}}</view>                             
```

```js
Page({
  /**
   * 页面的初始数据
   */
  data: {
      num:0
  },
//输入框的input事件的执行逻辑
   bingha(e){
    //  console.log("点击事件");
    //  console.log(e.detail.value);
      this.setData({
    num: e.detail.value
      })
   },
  //  加 减 按钮的事件
  handletap(e){
console.log(e);
// 1 获取自定义属性 operation
      const operation=e.currentTarget.dataset.operation;
      this.setData({
        num: this.data.num + operation
          }) 
        }
})
```

## rpx宽度的使用 :   1px = 1rpx              14px=28rpx

### 注意： 1、小程序中 不需要主动引入样式文件

  2、需要把页面中某些元素的单位 由 px 改 rpx

   1 设置稿 750x

​    750 px = 750 rpx

​    1px = 1rpx              14px=28rpx

   2 把屏幕宽度 改成 375px

​    375 px = 750 rpx

​    1 px =2rpx

​    1 rpx =0.5px

  3、存在一个设计稿 宽度414 或者 未知 page

​    1 设置稿 page 存在一个元素 宽度 100px

​    2 拿以上的需求 去实现 不同的页面适配

​    page = 750 rpx

​    1px =750rpx / page

​    100 px =750 rpx * 100 / page

​    假设 page = 375px

  4 利用 一个属性 calc属性 css 和 wxss 都支持 一个属性

   1 750 和 rpx 中间不要留空格

   2 运算符的两边也不要留空格 （有待考证，目前好像行）

```html
<view> rpx宽度的使用</view>               wxml:
```

```css
                                    wxss:
view{
  /* width: 200rpx; */

  height: 200rpx;

  font-size: 40rpx;

   background-color: red;
   
  width:calc(750rpx * 100 / 375) ;
}                                                                    
```

## @import: 样式的引用

### 	注意：

 1、引入的代码是通过 @import来引入

 2、路径 只能写相对路径

```html
<view>@import: 样式的引用 具体看common.wxss..wxss </view>             wxml:
```

```css
@import "../../styles/common.wxss.wxss"                            wxss:
    
    /* 相对路径 @import  */
/* 样式导入  在需要的页面的 wxml加上 @import " 路径 " */
view{
  color: red;
   font-size: 100px;
 }
```

## less语法：

 dem7是less语法，有点难，

```html
 wxml:
<text selectable decode>长按文字复制：selectable &&  decode:解码    &lt;</text>
```

# WXSS

主题颜色 通过变量来实现

```css

 /* 定义主题颜色 */
 --themeColor:#f85850;
   /* 使用主题颜色 */
  color: var(--themeColor);
伪类：
  &：nth-last-child(-n+4){   } ----------------后四个超链接



```

# js

```js
     

方法：getSwiperList(){ }             方法调用：   this.getSwiperList()；
##.bilibili-player-electric-panel-wrap
```





# 快捷键：

Ctrl+S：保存文件
Ctrl+【， Ctrl+】：代码行缩进
Ctrl+Shift+【， Ctrl+Shift+】：折叠打开代码块
Ctrl+C Ctrl+V：复制粘贴，如果没有选中任何文字则复制粘贴一行
Shift+Alt+F：代码格式化
Alt+Up，Alt+Down：上下移动一行
Shift+Alt+Up，Shift+Alt+Down：向上向下复制一行
Ctrl+Shift+Enter：在当前行上方插入一行

Ctrl+End：移动到文件结尾
Ctrl+Home：移动到文件开头
Ctrl+i：选中当前行
Shift+End：选择从光标到行尾
Shift+Home：选择从行首到光标处
Ctrl+Shift+L：选中所有匹配
Ctrl+D：选中匹配
Ctrl+U：光标回退



Shift+Alt+A：注释

组合键：Ctrl+Shift+Alt；  Ctrl+Shift+左右移动

# 解构赋值

```xquery
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number); //结果： 42, "OK", [867, 5309]
```


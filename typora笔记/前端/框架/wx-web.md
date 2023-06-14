# 小程序的第三方框架

1 腾讯wepy 类似vue

2 美团mpvue语法类似vue

3 京东taro类似react

4 滴滴 chameleon

5 uni-app 类似vue

6 源生框架 MINA

黑马的APPID:wxfb52f2d7b2f6123a    

1665461689 AppID(小程序ID)	wxe147f862f6068b22   测试号：wx11b52a185d23d41a

2276916510  AppID(小程序ID)	wxe785a5dea677643c 测试号：wx5a16d52f467da886

# 57

##### text:相当于web中的span标签  形内元素 不会换行

```vue
<text> </text>
```

##### view:相当于 div标签 块级元素 会换行

```vue
<view></view>
```

### 数据绑定

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

## button按钮

# json

```json
                         pages:   链接路径
 "pages/my/my",链接路径

                         window：
"backgroundTextStyle":-------------下拉loading的样式，仅⽀持dark/light
"navigationBarBackgroundColor":-------------导航栏背景颜⾊
"navigationBarTitleText":-------------导航栏标题⽂字内容
"enablePullDownRefresh":-------------是否全局开启下拉刷新

                      list：
"pagePath":-------------跳转链接              "text":-------------命名
"iconPath":-------------未选中的图片（图标）   "iconPath":-------------选中后的图片（图标）



```

```html
                                   wxml:
<text>:
  selectable:-------------长按文字复制          decode:-------------对文本内容进行 解码（有这个 &lt;才能用）
 
    
    
```



```json
 "tabBar": {
"list": [
     {
        "pagePath": "pages/index/index",  //跳转链接
        "text": "首页",          //命名
        "iconPath": "icon/_home.png",   //未选中的图片（图标）
        "selectedIconPath": "icon/home.png"  //选中后的图片（图标）
      }, 
     ...      
        ]   
}


 "pages/dem13/dem13", 
    "pages/dem12/dem12", 
    "pages/dem11/dem11", 
    "pages/dem10/dem10", 
   "pages/dem9/dem9", 
    "pages/dem8/dem8", 
    "pages/dem6/dem6", 

    "pages/dem5/dem5",   
     "pages/dem4/dem4",    
    "pages/dem3/dem3", 
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


#  概念：

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
  注意 ：第⼀次使用微信开发者⼯具，需要微信扫码登录。



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





## ⻚⾯配置page(名字自定义).json ：

   [页面配置文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/page.html)

这⾥的 page.json 其实⽤来表⽰⻚⾯⽬录下的 page.json 这类和⼩程序⻚⾯相关的配置。 开发者可以独⽴定义每个⻚⾯的⼀些属性，如顶部颜⾊、是否允许下拉刷新等等。 ⻚⾯的配置只能设置 app.json 中部分 window 配置项的内容，⻚⾯中配置项会覆盖 app.json的 window 中相同的配置项。 

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E2%BB%9A%E2%BE%AF%E9%85%8D%E7%BD%AEpage.png)

wxml:

```html
<text>pages/img/img.wxml</text>
```

img.json:

```json
{
    "usingComponents": {},
    "navigationBarBackgroundColor": "#eee", // 导航栏背景颜色
    "navigationBarTitleText": "图片", // 导航栏标题
    "navigationBarTextStyle": "black" //导航栏标题颜色，只支持black /white 
}
```

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E2%BB%9A%E2%BE%AF%E9%85%8D%E7%BD%AEpage%E7%BB%93%E6%9E%9C.png)







## sitemap 配置-了解即可 

​    ⼩程序根⽬录下的 sitemap.json ⽂件⽤于配置⼩程序及其⻚⾯是否允许被微信索引。 



## app.js:

```js
App({

    // 1 应用第一次启动的就会触发的事件
    //使用场景：在应用第一次启动的时候 获取用户的个人信息
    onLaunch(){
        // console.log("onLaunch");

        //通过js的方式跳转    注意：此方法不会触发onPageNotFound事件
        //  wx.navigateTo({
        //    url: '/pages/sgsg/sg'
        //  });
        // onError 代码发生了报错的时候 就会触发
    }

    // 2 应用 被用户看到
    //使用场景：一般是小程序或应用之间切换使用的 ；对应用的数据或者页面效果 重置
    ,onShow(){
        console.log("onshow: 应用 被用户看到");
    }

    // 3 应用 被隐藏了
    //使用场景： 暂停或者清除定时器
    ,onHide(){
        console.log("onHide:  应用 被隐藏了");
    }

    // 4 应用的代码发生了报错的时候 就会触发
    //使用场景：在应用发生代码报错的时候，收集用户的错误信息，通过异步请求，将错误的信息发送后台去
    ,onError(eer){

        console.log("onError: 应用的代码发生了报错的时候 就会触发",eer);

    }

    // 5 应用找不到就会触发
    // 应用第一次启动的时候，如果找不到第一个入口页面 才会触发
    ,onPageNotFound(){
        console.log("onPageNotFound: 应用找不到就会触发");

        // 如果页面不存在了 通过js的方式来重新跳转页面，重新跳转到第二个首页
        // 不能跳到tabbar页面 导航组件类型
        wx.navigateTo({
            url: '/pages/dem4/dem4',
        })
    }
})
```





# wxml:

​    WXML（WeiXin Markup Language）是框架设计的⼀套标签语⾔，结合[基础组件](https://developers.weixin.qq.com/miniprogram/dev/component/)、[事件系统](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)，可以构 建出⻚⾯的结构。
1. 框架的视图层由 WXML 与 WXSS 编写，由组件来进行展示。
2. 将逻辑层的数据反映成视图，同时将视图层的事件发送给逻辑层。
3. WXML(WeiXin Markup language) 用于描述页面的结构。
4. WXS(WeiXin Script) 是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。
5. WXSS(WeiXin Style Sheet) 用于描述页面的样式。
6. 组件(Component)是视图的基本组成单元。

## 总结：

​    总结：使用小程序跟使用Vue差不多。。。

```html
<view/> <!--  相当于web中的 div标签 块级元素 会换行 -->
<text/>  <!--  span标签   -->
<view/>  <!--   -->

<!--  不能写成checked="false"，其计算结果是⼀个字符串  -->
<checkbox checked="{{false}}"> </checkbox>  <!-- 复选框   -->

                       
<text>:
  selectable:-------------长按文字复制          decode:-------------对文本内容进行 解码（有这个 &lt;才能用）
```



注意：此处[组件](https://developers.weixin.qq.com/miniprogram/dev/component/) == 标签。

常用组件: `view,text,rich-text,button,image,navigator,icon,swiper,radio,checkbox`。



```js
async freeze(){
    var sendData = {
        token: app.globalData.token,
        carId: this.data.car.carId,
        frozenAmount:this.data.freezeAmount
    }
    const r = await api({
        url:"/alipay/goAliPayCarDeposit/"+this.data.car.carId+"/"+this.data.freezeAmount,      
        method:'POST',
        data:sendData
    });

    if(r.data.data.success){
        my.redirectTo({url:'/page/order/index/index'})
    }else{
        my.alert({content:r.data.data.msg})
    }
}
```



## [text(span标签)](https://developers.weixin.qq.com/miniprogram/dev/component/text.html):

1. ⽂本标签 
2. 只能嵌套text 
3. ⻓按⽂字可以复制（只有该标签有这个功能） 
4.  可以对空格 回⻋ 进⾏编码 

| 属性名     | 类型    | 默认值 | 说明                                       |
| ---------- | ------- | ------ | ------------------------------------------ |
| decode     | boolean | false  | ⽂本是否可选(长按文字可复制)               |
| selectable | boolean | false  | 是否解码（例： `&lt; `  解码成  “ &lt; ”） |

```html
<text selectable="{{false}}" decode="{{false}}">
    普&nbsp;通
</text>
```



## [image](https://developers.weixin.qq.com/miniprogram/dev/component/image.html):

1. 图⽚标签，image组件默认宽度320px、⾼度240px 
2. ⽀持懒加载 

| 属性名    | 类型    | 默认值      | 必填 | 说明                                         |
| :-------- | :------ | :---------- | :--- | :------------------------------------------- |
| src       | string  |             | 否   | 图片资源地址                                 |
| mode      | string  | scaleToFill | 否   | 图片裁剪、缩放的模式                         |
| lazy-load | boolean | false       | 否   | 图片懒加载，在即将进入(上下三屏)时才开始加载 |

  mode 有效值： 
   mode 有 13 种模式，其中 4 种是缩放模式，9种是裁剪模式。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/image-mode13%E4%B8%AA%20%E6%9C%89%E6%95%88%E5%80%BC.png)

```html
<image mode="aspectFill" lazy-load src="https://img14.360buyimg.com/imgzone/jfs/t1/190629/1/11484/70757/60dfcf4bE211ab3dd/1fd53d5343d22676.jpg" />
```





## [swiper(轮播图)](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html):

 swiper:微信内置轮播图组件。(默认宽度 100%, ⾼度 150px )

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/swiper(%E8%BD%AE%E6%92%AD%E5%9B%BE).png)

swiper组件属性

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/swiper%E7%BB%84%E4%BB%B6%E5%B1%9E%E6%80%A7.png)

```js
    1 轮播图外层容器 swiper
    2 每一个轮播项 swiper-item
    3 swiper标签 存在默认样式
      1 width 100%
      2 height 150px       image 存在默认宽度和高度 320 * 240
      3 swiper 高度 无法实现由内容撑开 ,宽度可以
    4 先找出 原图的宽度和高度 等比例 给swiper定宽度和高度
      原图的宽度和高度 1125* 352px
      swiper 宽度/swiper 高度=原图的宽度 / 原图的高度
      swiper高度 = swiper宽度  *   原图的高度 / 原图的宽度
      height: 100vw(100% && 750rpx) * 352 /1125
    5 autoplay 自动轮播
    6 interval 修改轮播时间    interval 必须配合 circular使用
    7 circular 衔接轮播
    8 indicator-dots 圆形按钮显示器（指示器&& 分页器&& 索引器）
```





轮播图：

```html
<swiper autoplay interval="1000" circular indicator-dots indicator-color="pink" indicator-active-color="#fff">
    <swiper-item>
        <image src="https://images.wallpaperscraft.com/image/single/railway_bridge_trees_976049_1280x720.jpg" />
    </swiper-item>
    <swiper-item>
        <image src="https://images.wallpaperscraft.com/image/single/bouquet_flowers_composition_137663_1280x720.jpg" />
    </swiper-item>
    <swiper-item>
        <image src="https://images.wallpaperscraft.com/image/single/girl_piercing_cap_972493_1280x720.jpg" />
    </swiper-item>
    <swiper-item>
        <image src="https://images.wallpaperscraft.com/image/single/roses_bush_bloom_128901_1280x720.jpg"
               style="background-size:cover " />
    </swiper-item>
    <swiper-item>
        <image src="https://images.wallpaperscraft.com/image/single/silhouette_city_street_123496_1280x720.jpg" />
    </swiper-item>
    <swiper-item>
        <image mode="aspectFill" lazy-load
               src="https://images.wallpaperscraft.com/image/single/girl_backpack_building_974824_1280x720.jpg" />
    </swiper-item>
</swiper>
```





## [swiper-item](https://developers.weixin.qq.com/miniprogram/dev/component/swiper-item.html):

​    swiper-item(滑块)：仅可放置在[swiper](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html)组件中，宽高默认为100%。





## [navigator(a标签)](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html):

​    navigator: 导航组件 类似超链接标签。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/navigator-%E8%B6%85%E9%93%BE%E6%8E%A5%E6%A0%87%E7%AD%BE.png)

```html
<navigator url="/pages/dem10/dem10">跳到轮播图页面</navigator>
<navigator url="/pages/imdex/imdex">直接跳转到 tabbar页面</navigator>
<navigator open-type="redirect" url="/pages/dem10/dem10">跳到轮页面 redirect</navigator>
<navigator open-type="switchTab" url="/pages/index/index"> switchTab 直接跳转到 tabbar页面</navigator>
<navigator open-type="reLaunch" url="/pages/index/index"> relaunch 可以随便跳转</navigator>
```





## [rich-text(富文本标签)](https://developers.weixin.qq.com/miniprogram/dev/component/rich-text.html):

  rich-text(富文本标签): 可以将字符串解析成 对应标签，类似 vue中 v-html 功能。
  nodes属性：
     nodes 属性⽀持 字符串 和 标签节点数组。

| 属性     | 说明       | 类型   | 必填 | 备注                                     |
| :------- | :--------- | :----- | :--- | :--------------------------------------- |
| name     | 标签名     | string | 是   | 支持部分受信任的 HTML 节点               |
| attrs    | 属性       | object | 否   | 支持部分受信任的属性，遵循 Pascal 命名法 |
| children | 子节点列表 | array  | 否   | 结构和 nodes 一致                        |

文本节点：type = text

| 属性 | 说明 | 类型   | 必填 | 备注         |
| :--- | :--- | :----- | :--- | :----------- |
| text | 文本 | string | 是   | 支持entities |

备注：
1.  nodes 不推荐使用 String 类型，性能会有所下降。
2. `rich-text` 组件内屏蔽所有节点的事件。
3. attrs 属性不支持 id ，支持 class 。
4. name 属性大小写不敏感。
5. 如果使用了不受信任的HTML节点，该节点及其所有子节点将会被移除。
6. img 标签仅支持网络图片。
7. 如果在自定义组件中使用 `rich-text` 组件，那么仅自定义组件的 wxss 样式对 `rich-text` 中的 class 生效。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/rich-text(%E5%AF%8C%E6%96%87%E6%9C%AC%E6%A0%87%E7%AD%BE).png)

```js
// 1   index.wxml 加载 节点数组 
<rich-text nodes="{{nodes}}" bindtap="tap"></rich-text>
// 2 加载 字符串 
<rich-text nodes='<img
src="https://developers.weixin.qq.com/miniprogram/assets/images/head_global_z_@all.p
ng" alt="">'></rich-text>
    
// index.js
Page({
  data: {
    nodes: [{
      name: 'div',
      attrs: {
        class: 'div_class',
        style: 'line-height: 60px; color: red;'
     },
      children: [{
        type: 'text',
        text: 'Hello&nbsp;World!'
     }]
   }]
 },
  tap() {
    console.log('tap')
 }
})
```



## [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html):

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/button.png)

| 属性      | 类型    | 默认值  | 必填 | 说明                                                         |
| :-------- | :------ | ------- | :--- | :----------------------------------------------------------- |
| size      | string  | default | 否   | 按钮的大小                                                   |
| type      | string  | default | 否   | 按钮的样式类型                                               |
| plain     | boolean | false   | 否   | 按钮是否镂空，背景色透明                                     |
| disabled  | boolean | false   | 否   | 是否禁用                                                     |
| loading   | boolean | false   | 否   | 名称前是否带 loading 图标                                    |
| form-type | string  |         | 否   | 用于 [form](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 组件，点击分别会触发 [form](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 组件的 submit/reset 事件 |
| open-type | string  |         | 否   | 微信开放能力                                                 |

### **size 的合法值 :**

| 合法值  | 说明     |
| :------ | :------- |
| default | 默认大小 |
| mini    | 小尺寸   |

### **type 的合法值 :**

| 合法值  | 说明 |
| :------ | :--- |
| primary | 绿色 |
| default | 白色 |
| warn    | 红色 |

### **form-type 的合法值 :**

| 合法值 | 说明     |
| :----- | :------- |
| submit | 提交表单 |
| reset  | 重置表单 |

### **open-type 的合法值:**

| 合法值         | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| contact        | 打开客服会话，如果用户在会话中点击消息卡片后返回小程序，可以从 bindcontact 回调中获得具体信息，[具体说明](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/customer-message/customer-message.html) （*小程序插件中不能使用*） |
| share          | 触发用户转发，使用前建议先阅读[使用指引](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share.html#使用指引) |
| getPhoneNumber | 手机号快速验证，可以从bindgetphonenumber回调中获取到用户信息，[具体说明](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/getPhoneNumber.html) （*小程序插件中不能使用*） |
| getUserInfo    | 获取用户信息，可以从bindgetuserinfo回调中获取到用户信息 （*小程序插件中不能使用*） |
| launchApp      | 打开APP，可以通过app-parameter属性设定向APP传的参数[具体说明](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/launchApp.html) |
| openSetting    | 打开授权设置页                                               |
| feedback       | 打开“意见反馈”页面，用户可提交反馈内容并上传[日志](https://developers.weixin.qq.com/miniprogram/dev/api/base/debug/wx.getLogManager.html)，开发者可以登录[小程序管理后台](https://mp.weixin.qq.com/)后进入左侧菜单“客服反馈”页面获取到反馈内容 |
| chooseAvatar   | 获取用户头像，可以从bindchooseavatar回调中获取到头像信息     |

open-type 的 contact的实现流程 :
1. 将⼩程序 的 appid 由测试号改为 ⾃⼰的 appid
2. 登录微信⼩程序官⽹，添加 客服--微信
- 普通⽤⼾ A 
- 客服-微信 B 

```html
<button
        type="default"
        size="{{defaultSize}}"
        loading="{{loading}}"
        plain="{{plain}}"
        >
    default
</button>

<button>默认按钮</button>
<!-- size -->
<button size="mini">小按钮</button>

<!--  type -->
<button type="primary"> primary 绿⾊按鈕</button>
<button type="default"> default 白色按鈕</button>
<button type="warn"> warn 红色按鈕</button>

<button type="warn" plain> plain 按钮是否镂空，背景⾊透明</button>
<button type="warn" loading> loading 名称前是否带 loading 图标</button>

<!--  open-type -->
<button open-type="contact"> contact 客服</button>
<button open-type="share">share  转发当前的小程序(不包括朋友圈)</button>
<button open-type="getPhoneNumber" size="mini" bindgetphonenumber="getPhoneNumber">getPhoneNumber 获取当前用户的手机号（要企业号）《10月18号21:46分 ：真机调试可以使用》</button>
<button open-type="getUserInfo" bindgetuserinfo="getUserInfo"> getUserInfo 获取当前用户的个人信息</button>
<button open-type="launchApp">launchApp 在小程序当中 直接打开App </button>
<button open-type="openSetting"> 打开小程序内置的 授权页面</button>
<button open-type="feedback"> 客服反馈 feedback</button>
```

js:

```js
Page({

    //获取用户的手机号码
    getPhoneNumber(e){
        console.log(e);
    } ,

    //获取用户个人信息
    getUserInfo(e) {
        console.log(e);
    }
})
```







## [icon](https://developers.weixin.qq.com/miniprogram/dev/component/icon.html):

 

icon所有图标：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/icon.png)

属性说明：

| 属性  | 类型          | 默认值 | 必填 | 说明                                                         |
| :---- | :------------ | :----- | :--- | :----------------------------------------------------------- |
| type  | string        |        | 是   | icon的类型，有效值：success, success_no_circle, info, warn, waiting, cancel, download, search, clear |
| size  | number/string | 23     | 否   | icon的大小，单位默认为px，[2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入单位(rpx/px)，[2.21.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入其余单位(rem 等)。 |
| color | string        |        | 否   | icon的颜色，同css的color                                     |

```html
<icon type="success" size="60" color="bulu"> </icon>
```




wxml:

```html
<view class="group">
    <block wx:for="{{iconSize}}">
        <icon type="success" size="{{item}}"/>
    </block>
</view> <view class="group">
    <block wx:for="{{iconType}}">
        <icon type="{{item}}" size="40"/>
    </block>
</view> <view class="group">
    <block wx:for="{{iconColor}}">
        <icon type="success" size="40" color="{{item}}"/>
    </block>
</view>
```

js:

```js
Page({
    data: {
        iconSize: [20, 30, 40, 50, 60, 70],
        iconType: [
            'success', 'success_no_circle', 'info', 'warn', 'waiting', 'cancel',
            'download', 'search', 'clear'
        ],
        iconColor: [
            'red', 'orange', 'yellow', 'green', 'rgb(0,255,255)', 'blue', 'purple'
        ],
    }
})
```



## [radio(单选框)](https://developers.weixin.qq.com/miniprogram/dev/component/radio.html)：

​    注意：①可以通过 color属性来修改颜色。
​               ②需要搭配 [radio-group](https://developers.weixin.qq.com/miniprogram/dev/component/radio-group.html)⼀起使⽤。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%8D%95%E9%80%89%E6%A1%86.png)

```html
<radio-group bindchange="handleChange"> 
    <radio value="male">男</radio>
    <radio value="female">女</radio>
</radio-group>

<view>你选中的是：{{gender}}</view>
```

js：

```js
Page({
    data:{ // 数据区
        gender:""
    },

    handleChange(e){
        console.log(e);
        // 1 获取单选框中的值
        let gender=e.detail.value;    //let 代表局部变量,类型以 var
        // 2 把值赋值给data中
        this.setData({
            // gender:gender  // 方法1
            gender  // 方法2
        })
        // this.gender=gender; // 无效，获取到了数据，但页面不加载
        console.log(this.gender);
    }
})
```





## [checkbox(复选框)](https://developers.weixin.qq.com/miniprogram/dev/component/checkbox.html):

注意：①可以通过 color属性来修改颜色。
           ②需要搭配 [checkbox-group](https://developers.weixin.qq.com/miniprogram/dev/component/checkbox-group.html)⼀起使⽤。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%A4%8D%E9%80%89%E6%A1%86.png):

wxml:

```html
<view>
    <checkbox-group bindchange="handleItemChange">
        <checkbox value="{{item.value}}" wx:for="{{list}}" wx:key="id"> 
            <!--wx:key  ⽤来提⾼数组渲染的性能,表⽰ 循环项中的唯⼀属性 如 id-->
            {{item.name}}
        </checkbox>
    </checkbox-group>

    <view>
        选中的水果：{{checkedList}}
    </view>
</view>
```

js:

```js
Page({
    data:{
        list:[
            {
                id:0,
                name:"🍎",
                value:"apple"
            }, {
                id:1,
                name:"🍌",
                value:"bananer"
            }, {
                id:2,
                name:"🤫",
                value:"grape"
            }
        ],
        //定义一个空数组
        checkedList:[]
    },

    //复选框的选中事件（单击事件）
    handleItemChange(e){
        console.log(e);
        //赋值给空数组
        const checkedList=e.detail.value;
        
        this.setData({ // 
            checkedList
        })
    }
})
```







## [自定义组件 (自定义标签)](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)：

- 类似vue或者react中的自定义组件。
- ⼩程序允许我们使⽤⾃定义组件的⽅式来构建⻚⾯。
- 类似页面，一个自定义组件由`json ` `wxml ` `wxss ` `js` 4个文件组成。

### 创建⾃定义组件:

- ​    在微信开发者⼯具中快速创建组件的⽂件结构 
- ​    在⽂件夹内 components/myHeader ，创建组件 名为 myHeader

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%88%9B%E5%BB%BA%E2%BE%83%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6.png)

 

### 声明组件: 

⾸先需要在组件的 json ⽂件中进⾏⾃定义组件声明 `myHeader.json`。

```json
{
    "component": true //代表自定义组件，在小程序中可以在不同的页面或组件中进行复用。
    "usingComponents": {} 
}
```

### 编辑组件 :

​     同时，还要在组件的 wxml ⽂件中编写组件模板，在 wxss ⽂件中加⼊组件样式slot 表⽰插槽，类似vue中的slot。

####    myHeader.wxml:

```html
<!-- 这是自定义组件的内部WXML结构 -->
<view class="inner">
    {{innerText}}
    <slot></slot>
</view>
```

   在组件的 wxss ⽂件中编写样式
注意：在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。

####    myHeader.wxss:

```css
/* 这里的样式只应用于这个自定义组件 */
.inner {
    color: red;
}
```

### 注册组件: 

   在组件的 js ⽂件中，需要使⽤ Component() 来注册组件，并提供组件的属性定义、内部数据和 ⾃定义⽅法。
   myHeader.js

```js
Component({
    properties: {
        // 这里定义了innerText属性，属性值可以在组件使用时指定
        innerText: {
            // 期望要的数据是 string类型
            type: String,
            value: 'default value',
        }
    },
    data: { // 数据区
        someData: {}
    },
    methods: { // 方法区
        customMethod: function(){}
    }
})
```

 

### 声明引⼊⾃定义组件 :

 ⾸先要在⻚⾯的 json ⽂件中进⾏引⽤声明。还要提供对应的组件名和组件路径

#### index.wxml:

```js
{
    // 引用声明
    "usingComponents": {
        // 要使用的组件的名称     // 组件的路径
        "my-header":"/components/myHeader/myHeader"
    }
}
```

### ⻚⾯中使⽤⾃定义组件:

```html
<view>
    <!-- 以下是对一个自定义组件的引用 -->
    <my-header inner-text="Some text">
        <view>用来替代slot的</view>
    </my-header>
</view>
```

### 组件-⾃定义组件传参 :

1. ⽗组件通过属性的⽅式给⼦组件传递参数 
2. ⼦组件通过事件的⽅式向⽗组件传递参数 




总结：
1. 标签名 是 中划线的⽅式 
2. 属性的⽅式 也是要中划线的⽅式 
3. 其他情况可以使⽤驼峰命名 
1. 组件的⽂件名如 myHeader.js 的等 
2. 组件内的要接收的属性名 如 innerText
4. 更多。。



## 自定义组件:



### 定义段与⽰例⽅法:

   定义段与⽰例⽅法 Component 构造器可⽤于定义组件，调⽤ Component 构造器时可以指定组件的属性、数据、⽅法 等。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%AE%9A%E4%B9%89%E6%AE%B5%E4%B8%8E%E2%BD%B0%E4%BE%8B%E2%BD%85%E6%B3%95.png)





```html
 wxml:
<text selectable decode>长按文字复制：selectable &&  decode:解码    &lt;</text>
```

# WXSS:

​    WXSS( WeiXin Style Sheets )是⼀套样式语⾔，⽤于描述 WXML 的组件样式。 
   与 CSS 相⽐，WXSS 扩展的特性有： 

   1. 响应式⻓度单位 rpx
   2. 样式导⼊ 

主题颜色 通过变量来实现

```css

 /* 定义主题颜色 */
 --themeColor:#f85850;
   /* 使用主题颜色 */
  color: var(--themeColor);
伪类：
  &：nth-last-child(-n+4){   } ----------------后四个超链接



```

 注意: ⼩程序 不⽀持通配符 `*`。

```css
*{   /** 代码无效 **/
    margin:0;
    padding:0;
    box-sizing:border-box; 
}
```

### 小程序只⽀持以下选择器：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E2%BD%AC%E5%89%8D%E2%BD%80%E6%8C%81%E7%9A%84%E9%80%89%E6%8B%A9%E5%99%A8.png)

 

## ⼩程序中使⽤less: 

   原⽣⼩程序不⽀持 less ，其他基于⼩程序的框架⼤体都⽀持，如 wepy ， mpvue ， taro 等。 但是仅仅因为⼀个less功能，⽽去引⼊⼀个框架，肯定是不可取的。
  解决方法：

1. VS Code编辑器，安装插件 easy less插件。

   ![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/easy%20less%E6%8F%92%E4%BB%B6.png)

2.  在vs code的设置中加⼊如下配置 ,即可使用less

   ```js
   "less.compile": {
       "outExt":       ".wxss"
   }
   ```

# 事件：

```vue
<!-- 点击事件（tap/click）：当用户点击组件时触发该事件。-->
<view bindtap="handleTap">点击我</view>

<!-- 触发事件：当用户点击组件时触发该事件。-->
<radio-group bindchange="handleChange"> 
      <radio value="male">男</radio>
      <radio value="female">女</radio>
</radio-group>

<!-- 长按事件（longpress）：当用户长时间按压组件时触发该事件。 -->
<view bindlongpress="handleLongPress">长按我</view>

<!-- 输入input）：当用户在输入框中输入内容时触发该事件。 -->
<input bindinput="handleInput" />

<!-- 表单提交事件（submit）：当用户提交表单时触发该事件。 -->
<form bindsubmit="handleSubmit">
  <input name="username" placeholder="用户名" />
  < name="password" type="password" placeholder="密码" />
  <button formType="submit">提交</button>
</form>


<!-- 滚动事件（scroll）：当元素滚动时触发该事件。 -->
<scroll-view bindscroll="handlescroll-view"> </scroll-view>

```

js：

```js
Page({

    data:{
        gender:""
    },

    handleChange(e){
        console.log(e);
        // 1 获取单选框中的值
        let gender=e.detail.value;           //let 代表局部变量,类型以 var
        // 2 把值赋值给data中
        this.setData({
            // gender:gender  // 方法1
            gender  // 方法2
        })
        // this.gender=gender; // 无效，获取到了数据，但页面不加载
        console.log(this.gender);
    }
})
```







```js
1. 页面级事件：
   - onLoad：页面加载时触发的事件。
   - onShow：页面显示时触发的事件。
   - onHide：页面隐藏时触发的事件。
   - onUnload：页面卸载时触发的事件。

2. 网络事件：
   - onNetworkStatusChange：网络状态变化时触发的事件。

3. 用户触摸事件：
   - onTouchStart：手指触摸屏幕时触发的事件。
 - onTouchMove：手指在屏幕上移动时触发的事件。
   - onTouchEnd：手指离开屏幕时触发的事件。
   - onTouchCancel：触摸被打断时触发的事件。

4. 表单事件：
   - onSubmit：表单提交时触发的事件。
   - onReset：表单重置时触发的事件。

5. 用户滚动事件：
   - onScroll：页面滚动时触发的事件。

6. 视图事件：
   - onClick：点击事件。
   - onLongPress：长按事件。
   - onTransitionEnd：CSS 动画或过渡结束时触发的画开始时触发的事件。
   - onAnimationIteration：CSS 动画一次循环完成时触发的事件。
   - onAnimationEnd：CSS 动画结束时触发的事件。

7. 媒体事件：
   - onAudioPlay：音频播放时触发的事件。
   - onAudioPause：音频暂停时触发的事件。
   - onAudioStop：音频停止时触发的事件。
   - onAudioError：音频播放错误时触发的事件。
   - onVideoPlay：视频播放时触发的事件。
   - onVideoPause：视频暂停时触发的事件。
   - onVideoEnded：视频播放到末尾时触发的事件。
   - onVideoError：视频播放错误时触发的事件。

```



# 生命周期:

  生命周期: 分为应⽤⽣命周期和⻚⾯⽣命周期。
关于小程序前后台的定义和小程序的运行机制，请参考[运行机制](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/operating-mechanism.html)章节。

⻚⾯⽣命周期流程：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E2%BB%9A%E2%BE%AF%E2%BD%A3%E5%91%BD%E5%91%A8%E6%9C%9F%E6%B5%81%E7%A8%8B.png)



## [应⽤⽣命周期](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html)：

| 属性                                                         | 类型     | 说明                                                         |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
| [onLaunch](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onLaunch-Object-object) | function | 生命周期回调——监听小程序初始化。                             |
| [onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onShow-Object-object) | function | 生命周期回调——监听小程序启动或切前台。                       |
| [onHide](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onHide) | function | 生命周期回调——监听小程序切后台。                             |
| [onError](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onError-String-error) | function | 错误监听函数。                                               |
| [onPageNotFound](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onPageNotFound-Object-object) | function | 页面不存在监听函数。                                         |
| [onUnhandledRejection](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onUnhandledRejection-Object-object) | function | 未处理的 Promise 拒绝事件监听函数。                          |
| [onThemeChange](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onThemeChange-Object-object) | function | 监听系统主题变化                                             |
| 其他                                                         | any      | 开发者可以添加任意的函数或数据变量到 `Object` 参数中，用 `this` 可以访问 |







## [⻚⾯⽣命周期](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)：



| 属性                                                         | 类型         | 说明                                                         |
| :----------------------------------------------------------- | :----------- | :----------------------------------------------------------- |
| [data](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#data) | Object       | 页面的初始数据                                               |
| options                                                      | Object       | 页面的组件选项，同 [`Component` 构造器](https://developers.weixin.qq.com/miniprogram/dev/api/xr-frame/classes/Component.html) 中的 `options` ，需要基础库版本 [2.10.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html) | String Array | 类似于mixins和traits的组件间代码复用机制，参见 [behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html)，需要基础库版本 [2.9.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onLoad](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onLoad-Object-query) | function     | 生命周期回调—监听页面加载                                    |
| [onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShow) | function     | 生命周期回调—监听页面显示                                    |
| [onReady](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onReady) | function     | 生命周期回调—监听页面初次渲染完成                            |
| [onHide](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onHide) | function     | 生命周期回调—监听页面隐藏                                    |
| [onUnload](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onUnload) | function     | 生命周期回调—监听页面卸载                                    |
| [onRouteDone](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onRouteDone) | function     | 生命周期回调—监听路由动画完成                                |
| [onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onPullDownRefresh) | function     | 监听用户下拉动作                                             |
| [onReachBottom](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onReachBottom) | function     | 页面上拉触底事件的处理函数                                   |
| [onShareAppMessage](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareAppMessage-Object-object) | function     | 用户点击右上角转发                                           |
| [onShareTimeline](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareTimeline) | function     | 用户点击右上角转发到朋友圈                                   |
| [onAddToFavorites](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onAddToFavorites-Object-object) | function     | 用户点击右上角收藏                                           |
| [onPageScroll](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onPageScroll-Object-object) | function     | 页面滚动触发事件的处理函数                                   |
| [onResize](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onResize-Object-object) | function     | 页面尺寸改变时触发，详见 [响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html#在手机上启用屏幕旋转支持) |
| [onTabItemTap](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onTabItemTap-Object-object) | function     | 当前是 tab 页时，点击 tab 时触发                             |
| [onSaveExitState](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onSaveExitState) | function     | 页面销毁前保留状态回调                                       |
| 其他                                                         | any          | 开发者可以添加任意的函数或数据到 `Object` 参数中，在页面的函数中用 `this` 可以访问。**这部分属性会在页面实例创建时进行一次深拷贝**。 |

js文件：

```js
// pages/my/my.js
Page({

    /** 页面的初始数据*/
    data: {
       msg:"hello"
    },

    /** 生命周期函数--监听页面加载 */
    onLoad: function (options) {

    },

    /** 生命周期函数--监听页面初次渲染完成 */
    onReady: function () {

    },

    /** 生命周期函数--监听页面显示 */
    onShow: function () {

    },

    /** 生命周期函数--监听页面隐藏*/
    onHide: function () {

    },

    /** 生命周期函数--监听页面卸载*/
    onUnload: function () {

    },

    /** 页面相关事件处理函数--监听用户下拉动作 */
    onPullDownRefresh: function () {

    },

    /** 页面上拉触底事件的处理函数 */
    onReachBottom: function () {

    },

    /** 用户点击右上角分享 */
    onShareAppMessage: function () {

    }
})
```





# WXML+WXSS：





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



##  字符串&& 数据类型 &&布尔类型&& object类型&& 复选框&& 循环：

### wxml:

```html
<!--pages/dem3/dem3.wxml-->
<text>pages/dem3/dem3.wxml</text>
<view>字符串&& 数据类型 &&布尔类型&& object类型&& 复选框&& 循环</view>
<!-- 1 字符串 -->
<view>{{msg}}</view>
<!-- 2 数据类型 -->
<view>{{num}}</view>
<!-- 3 布尔类型 bool -->
<view>是否是伪娘:{{isGirl}}</view>
<!-- 4 object类型 -->
<view>{{person.name}}</view>
<view>{{person.age}}</view>
<view>{{person.hegion}}</view>
<view>{{person.width}}</view>

<!-- 5 在标签的属性中使用 -->
<view data-unm="{{num}}">自定义属性</view>

<!-- 复选框 checkbox -->
<!-- 6 使用bool类型充当属性 checked 
1.字符串和 花括号之间一定不要用空格 否则会导致识别失败
<checkbox checked="              {{isChecked}}"></checkbox> 
-->
<checkbox checked="{{true}}"></checkbox> <!--直接使用 -->
<checkbox checked="{{isChecked}}"></checkbox> <!-- 通過wxml渲染使用 -->

<!-- 7 运算 =》 表达式 
1 可以在花括号中加入 表达式 --“语句"
2 表达式 
指的是一些简单 运算 数字运算 字符串 拼接 逻辑运算
1 数字的加减..
2 字符串拼接
3 三元表达式
3 语句
1 复杂的代码段
1 if else
2 switch
3 do while...
4 for 
-->

<view>{{1+1}}</view> <!-- 数字的加减 -->

<view>{{"1"+"1"}}</view>  <!--拼接 -->

<view>{{ (5>3) ? 15:6}}</view><!--三元表达式-->
<!-- 8 列表循环  
1 wx:for="{{数组或者对象}}"        wx:for-item="循环项的名称"      wx:for-index="循环项的索引"
2 we:key="唯一的值"用来提高列表渲染的性能
1 wx:key 绑定一个普通字符串的时候，那么这个字符串名称， 肯定是循环数组中的对象的 唯一属性 (例如：Id)
2 wx:key=" *this "   代表数组是一个普通的数组 *this表示就是循环项
普通的数组:[1,2,3,4]
循环项（要使用*this）：["1","4324", "josdfas"]
3 当出现 数组的嵌套循环的时候 尤其要注意 以下绑定的名称 不要重名
wx:for-item="item"   wx:for-index="index"
4 默认情况下我们不写
wx:for-item="item"   wx:for-index="index"
小程序也会把 循环项的名称 和 索引的名称 item 和 index 补上
只要一层循环的话 (wx:for-item="item"  wx:for-index="index") 可以省略
-->

<view> 列表循环：
    <view  wx:for="{{list}}"  wx:for-item="item"  wx:for-index="index" wx:key="id">
        索引：{{index}}
        --  
        值：{{item.name}}
    </view>
</view>

<!-- 9 对象循环
1 wx:for="{{数组或者对象}}"     wx:for-item="对象的值"  wx:for-index="对象的属性"
2 循环对象的时候 最好把 item 和 index的名称修改一下
wx:for-item="value"  wx:for-index="key"

-->
<view>对象循环：
    <view wx:for="{{person}}" wx:for-item="value"  wx:for-index="key" wx:key="age">
        属性：{{key}}
        值：{{value}} 
    </view>
</view>

<!-- 10 block
1 占位符的标签
2 写代码的时候，可以看到标签存在
3 页面渲染时（启动时），小程序会打他移除
-->
<view>
    列表循环：
    <block  wx:for="{{list}}" class="my-sjfos"  wx:for-item="item"  wx:for-index="index" wx:key="id">
        索引：{{index}}
        --  
        值：{{item.name}}
    </block>
</view>

<!-- 11 条件渲染
1 wx:if="{{true/false}}" 
1 if , else ,if else
wx:if
wx:elif
wx:else
2 hidden
1 在标签上直接加入属性 hidden
2 hidden="{{true}}"
3 什么场景下用哪个
1 当标签不是频繁的切换显示 使用 wx:if
直接把标签从 页面结构给移除掉
2 当标签频繁的切换显示的时候 使用 hidden
通过添加样式的方式来切换显示（display:block或者none）
例： <view hidden style="display:flex">hidden</view>  
--> 
<view>
    条件渲染：
    <view wx:if="{{true}}">显示</view>
    <view wx:if="{{false}}">隐藏</view>

    <view>-----------分割线----------------</view>

    <!-- 当第1个为true是，2和3不在执行；当第1个为false是，再执行2（2位flase时）；然后执行3 -->
    <view wx:if="{{false}}">  1</view>
    <view wx:elif="{{false}}">2</view>
    <view wx:else>3</view>

    <view>------------分割线---------------</view>
    <view hidden>hidden</view>
    <view hidden="{{false}}" > hidden="{{true}}" </view>


    <view>------------分割线000---------------</view>
    <view hidden style="display:flex">hidden</view>

</view>
```

### js:

```js
// pages/dem3/dem3.js
Page({

    /**
   * 页面的初始数据
   */
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





## 点击事件：

### wxml:

```html
<!--pages/dem4/dem4.wxml-->
<text>pages/dem4/dem4.wxml</text>
<view>点击事件</view>


<!--  1 需要给input 标签绑定 input事件 
         绑定关键字 bindinput
      2 如何获取 输入框的值
        通过事件源对象来获取
        e.detail.detail获取
      3 把输入框的值 赋值到 date当中
        不能直接 
        1 this.date.num = e.detail.value
        2 this.num = e.detail.value
      正确的写法：
        this，setData({
        num: e.detail.value
        })
      4 需要加入一个点击事件    click ==> bindtap
        1 bindtap       :点击事件     
        2 无法再小程序当中的 事件中 直接传参
        3 通过自定义属性的方式来传参
        4 事件源中获取 自定义属性

         -->
<!--  <input type="text" >  记得要加上 " /" , 微信不会自动加-->
<input type="text"  bindinput="bingha" />
<button bindtap="handletap" data-operation="{{1}}" >+</button>
<button bindtap="handletap" data-operation="{{-1}}" >-</button>
<view>{{num}}</view>
```

### wxss:

```js
// pages/dem4/dem4.js
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

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/%E5%8D%95%E5%87%BB%E4%BA%8B%E4%BB%B6.png)



## px ==rpx:

wxml：

```html
<!--pages/dem5/dem5.wxml-->
<text>pages/dem5/dem5.wxml</text>
<view>
rpx
</view>
<view>  rpx宽度的使用</view>
```

wxss:

```css
/* pages/dem5/dem5.wxss */
/* 
   1、小程序中 不需要主动引入样式文件
   2、需要把页面中某些元素的单位 由 px 改 rpx
     1 设置稿 750x
       750 px = 750 rpx
       1px = 1rpx
      2 把屏幕宽度 改成 375px
        375 px = 750 rpx
        1 px =2rpx
        1 rpx =0.5px
    3、存在一个设计稿 宽度414 或者 未知 page
       1 设置稿 page 存在一个元素 宽度 100px
       2 拿以上的需求 去实现 不同的页面适配

       page = 750 rpx
       1px =750rpx / page
       100 px =750 rpx * 100 / page
       假设 page = 375px
    4 利用 一个属性 calc属性 css 和 wxss 都支持 一个属性
      1 750 和 rpx 中间不要留空格
      2 运算符的两边也不要留空格 （有待考证，目前好像行）
*/
view{
    /* width: 200rpx; */
    height: 200rpx;
    font-size: 40rpx;
     background-color: red;
    width:calc(750rpx * 100 / 375) ;
}
```

结果：不同手机rpx宽高正常转换

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/rpx%E5%AE%BD%E9%AB%98%E8%BD%AC%E6%8D%A2.png)



## @import(样式的引用)：

注意：
     1、引入的代码是通过 @import来引入
     2、路径 只能写相对路径

wxml:

```html
<!--pages/dem6/dem6.wxml-->
<view>pages/dem6/dem6.wxml</view>
<view>@import: 样式的引用 具体看common.wxss..wxss </view>
```

wxss:

```css
/* pages/dem6/dem6.wxss */
/* 
  1、引入的代码是通过  @import来引入
  2、路径 只能写相对路径
*/
@import "../../styles/common.wxss.wxss"
```

styles/common.wxss.wxss:

```css
/* styles/common.wxss.wxss */

/* 相对路径 @import  */
/* 样式导入  在需要的页面的 wxml加上 @import " 路径 " */
view{
    color: red;
    font-size: 100px;
}
```

结果：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/wx-web/@import(%E5%BC%95%E5%85%A5).png)







## less语法：

 dem7是less语法，有点难。。。

```js
less中 支持 两种注释  多行  单行
wxss 不能写 单行注释 因为 写了 和没写是一样！！！
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


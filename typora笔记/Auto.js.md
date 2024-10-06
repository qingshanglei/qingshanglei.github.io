视频教学：https://www.bilibili.com/video/BV1Kf421Q7q9/?p=10&share_source=copy_web&vd_source=6117f6e013994472a5ec3c858ac511ea

文档：https://easydoc.net/doc/25791054/uw2FUUiw/TeRm1WHA#nav_3https://easydoc.net/doc/25791054/uw2FUUiw/TeRm1WHA#nav_3





Vs code安装两插件：

1. Auto.js-Autox.js-VSCodeExt
2. Auto.js-VSCodeExt-Fixed

```js

toastLog('你好，Auto.js')   // 控制台输出


// Auto.js代码提示插件
npm i @autojs/types-pro8 --no-bin-links   // v8项目- Auto.js代码提示插件
npm i @autojs/types-pro9 --no-bin-links   // v9项目- Auto.js代码提示插件
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Autojs/%E8%BF%9E%E6%8E%A5%E7%94%B5%E8%84%91%E6%8A%A5%E9%94%99.png)

1.   `ipconfig  `命令 ping，是否ping的通
2.  关闭防火墙
3. 手机《连接电脑》的ip使用电脑的 **ip4**。
4. VsCode安装Auto.js-Autox.js-VSCodeExt插件，查看-》命令面板开启下面功能。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Autojs/%E8%BF%9E%E6%8E%A5%E7%94%B5%E8%84%91%E6%8A%A5%E9%94%992.png)



```js

 log("log,你好，Auto.js");     // 控制台输出
 toast("toast,你好，Auto.js");   //手机显示屏输出
toastLog('toastLog,你好，Auto.js')  // 控制台输出 和手机显示屏输出
alert("alert,你好，Auto.js"); //弹出对话框
```



```js

// Auto.js脚本全局变量与局部变量的定义是  字母/中文都可以. 
// Auto.js脚本函数名字母/中文都可以. 

//      例如：局部变量：    var 变量=3    var bl=3 
//      例如：全局变量：     变量=3;    bl=3;

function  函数体(){
    变量1=1,变量2=2  // 简写
}


函数体()
   var 变量3= 变量1+变量2;

log(变量3);
```



## 点击事件:

SimpleActionAutomator.setScreenMetrics(x,y);

```js

setScreenMetrics(1080,2400);  //解决屏幕宽度不一致软件坐标问题。 设置脚本坐标点击所适合的屏幕宽高。
//  1080,2400为真我Q2手机分辨率，
//怎么查看手机分辨率，手机截屏相片查看。
    click(931,2193);   //单击按钮,x要点击的坐标的x值,y要点击的坐标的y值 ,前提是这个控件的clickable属性为true
    // 注意：①前提把开发者模式的指针位置打开获取安卓上按钮的位置。
    // ②此命令只在当前分辨率有效，如需要在其他分辨率可以使用需要在前面加 setScreenMetrics(x,y)命令
longClick(x, y) //长按按钮,前提是这个控件的longClickable属性为true
press(x, y, duration) // x:要按住的坐标的x值,y:要按住的坐标的y值,duration:按住时长，单位毫秒,超过500毫秒，则认为是长按。注意：按住过程中被其他事件中断才会操作失败。

```



## 滚动条事件:

```js
swipe(405, 2040, 405, 405, 100) // 滚动条事件,  
         //  x1: 滚动的起始坐标的x值,y1:滚动起始坐标的y值,
        //  x2: 滚动的结束坐标的x值,y2:滚动的结束坐标的y值,
        // duration: 滚动时长，单位毫秒

swipeEx(405, 2040, 405, 405, 100) //滚动条事件, 非官方接口 ,不推荐使用有bug

gesture(duration, [x1, y1], [x2, y2], …)  //滚动条事件,数值-常用 duration手势的时长,[x, y] … 手势滑动路径的一系列坐标
```



## 控件操作:

```js
//  =====> 控件的常用属性：
//text() ==》desc() ==》id() ==》className() id软件升级后可能会变动。
className 类名。类名表示一个控件的类型，例如文本控件为"android.widget.TextView", 图片控件为"android.widget.ImageView"等。
packageName 包名。包名表示控件所在的应用包名，例如QQ界面的控件的包名为"com.tencent.mobileqq"。
bounds 控件在屏幕上的范围。
drawingOrder 控件在父控件的绘制顺序。
indexInParent 控件在父控件的位置。
clickable 控件是否可点击。
longClickable 控件是否可长按。
checkable 控件是否可勾选。
checked 控件是否可已勾选。
scrollable 控件是否可滑动。
selected 控件是否已选择。
editable 控件是否可编辑。
visibleToUser 控件是否可见，可以筛选在屏幕可视范围内的组件。
enabled 控件是否已启用。
depth 控件的布局深度。

```



## 软件打包：

   前提：①VsCode安装Auto.js-Autox.js-VSCodeExt插件。

​              ②手机需安装Auto和打包插件Alpha-release软件。

​      快捷键Ctrl+Shift+P(F1或 查看--》命令面板) ,保存到指定的设备。









# 优秀代码：

## 抖音自动刷视频：

[参考文档如下](https://zhuanlan.zhihu.com/p/430977461)

```js
auto.waitFor()

//打开抖音App
var appName = "抖音";
(appName);

//等待进入主界面成功
text("首页").waitFor();

toast("准备开始滑动")

//滑动（Root+坐标点）
while (true) {
    Swipe(200, 1000, 210, 400, 500);
    //休息5s钟
    sleep(5000);
    toast("继续滑动。。。")
}
```



## **淘宝点击领喵币按钮：**

前提：淘宝预先打开喵铺主页。

```js
auto.waitFor()
var appName = "手机淘宝";
launchApp(appName);
sleep(3000);
//寻找领喵币按钮并点击
var lingmiaobi = text("领喵币").findOnce();
if (lingmiaobi) {
    lingmiaobi.click();
    sleep(1000);
}
else {
    toast("未检查到领喵币按钮");
    //中止脚本
    exit();
}
```

**2.点击去进店/去浏览**

```js
//开始执行任务
execTask();
function execTask() {
    while(true) {
        var target =  text("去进店").findOnce() || text("去浏览").findOnce();
        if (target == null) {
            toast("任务完成");
            break;
        }
        target.click();
        sleep(3000);
        //浏览网页20s
        viewWeb(20);
        back();
        sleep(1000);
    }
}
```

**3.浏览广告**

```js
function viewWeb(time) {
    gesture(1000, [300, 600], [300, 300]);
    var cnt = 1;
    while(true) {
        var finish = desc("任务完成").exists() || textStartsWith("已获得").exists();
        if (finish || cnt > time) {
            break;
        }
        sleep(1000);
        cnt += 1;
    }
    //模拟返回键，返回到任务栏页面
    back();
}
```


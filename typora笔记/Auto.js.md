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

//  =====> 控件的常用属性：
// text() ==》desc() ==》id() ==》className() id软件升级后可能会变动。  其他类似
id(resId) // id查询
idContains(str) // 查询"id 包含字符串 str"的筛选条件
idStartsWith(prefix) // 查询"id 需要以 prefix 开头"的筛选条件
idEndsWith(suffix) // 查询"id 需要以 suffix 结束"的筛选条件
idMatches(reg)  // 查询正则表达式
.children() // 
.child()
  例如： textMatches("\\d+")  // 匹配多位数字
         textMatches(/\d+/)  // 匹配多位数字
         idMatches("[a-zA-Z]+")  // 匹配a-z

// 遍历该控件的所有子控件组成的控件集合
className("AbsListView").findOne().children()
    .forEach(function(child){
        log(child.className());
    });
```





## 软件打包：

   前提：①VsCode安装Auto.js-Autox.js-VSCodeExt插件。

​              ②手机需安装Auto和打包插件Alpha-release软件。

​      快捷键Ctrl+Shift+P(F1或 查看--》命令面板) ,保存到指定的设备。









# 优秀代码：

## 手机五种锁屏&解屏方案：

锁屏方式：
1. 不锁屏
2. 滑动
3. 图案(手势)
4. PIN码(纯数字)
5. 密码(字母 + 数字)

```js
# 唤醒屏幕
device.wakeUp()

# 6.模拟按键  
 KeyCode(66)   // 回车：要root
```

### 不锁屏:

```js
device.wakeUp()
sleep(1000);
```

### 滑动锁屏:

```js
device.wakeUp()
sleep(1000);
swipe(device.width/2,device.height/8*7 , device.width/2, device.height/8, 1000)
sleep(1500)
```

### 图案(手势)锁屏:

```js
device.wakeUp()
sleep(1000);
swipe(device.width/2,device.height/8*7 , device.width/2, device.height/8, 1000)
sleep(1500)
gesture(1000, [540, 1577], [540, 1302],[540, 1862],[819, 1584])
sleep(2000)
```

### PIN码(纯数字)锁屏:

```js
device.wakeUp()
sleep(1000);
swipe(device.width/2,device.height/8*7 , device.width/2, device.height/8, 1000)
sleep(1500);
var 密码="4404"
for (var i=0; i<密码.length; i++) {
    // 熄屏状态下，点击不了，可能要root，改为位置
    click(密码[i])
    sleep(500)
};
KeyCode(66)
```



### 密码(字母 + 数字):

```js
device.wakeUp()
sleep(1000);
swipe(device.width/2,device.height/8*7 , device.width/2, device.height/8, 1000)
sleep(1500);
setText("abc12345")
sleep(500)
KeyCode(66)
```

### 密码(字母 + 数字)

```js
device.wakeUp()
sleep(1000);
swipe(device.width/2,device.height/8*7 , device.width/2, device.height/8, 1000)
sleep(1500);
setText("abc12345")
sleep(500)
KeyCode(66)
```





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

## action汇总:

### 系统：

```js
{action:"android.settings.ACCESSIBILITY_SETTINGS"}    //    无障碍
{action:"android.net.vpn.SETTINGS"}    //    VPN
{action:"android.settings.ACCESSIBILITY_SETTINGS"}    //    辅助功能
{action:"android.settings.ADD_ACCOUNT_SETTINGS"}    //    添加账户
{action:"android.settings.SETTINGS"}    //    系统设置首页
{action:"android.settings.APN_SETTINGS"}    //    APN设置
{action:"android.settings.APPLICATION_SETTINGS"}    //    应用管理
{action:"android.settings.BATTERY_SAVER_SETTINGS"}    //    节点助手
{action:"android.settings.BLUETOOTH_SETTINGS"}    //    蓝牙
{action:"android.settings.CAPTIONING_SETTINGS"}    //    字幕
{action:"android.settings.CAST_SETTINGS"}    //    无线显示
{action:"android.settings.DATA_ROAMING_SETTINGS"}    //    移动网络
{action:"android.settings.DATE_SETTINGS"}    //    日期和时间设置
{action:"android.settings.DEVICE_INFO_SETTINGS"}    //    关于手机
{action:"android.settings.DISPLAY_SETTINGS"}    //    显示设置
{action:"android.settings.DREAM_SETTINGS"}    //    互动屏保设置
{action:"android.settings.HARD_KEYBOARD_SETTINGS"}    //    实体键盘
{action:"android.settings.HOME_SETTINGS"}    //    应用权限,默认应用设置,特殊权限
{action:"android.settings.IGNORE_BATTERY_OPTIMIZATION_SETTINGS"}    //    忽略电池优化设置
{action:"android.settings.INPUT_METHOD_SETTINGS"}    //    可用虚拟键盘设置
{action:"android.settings.INPUT_METHOD_SUBTYPE_SETTINGS"}    //    安卓键盘语言设置（AOSP)
{action:"android.settings.INTERNAL_STORAGE_SETTINGS"}    //    内存和存储
{action:"android.settings.LOCALE_SETTINGS"}    //    语言偏好设置
{action:"android.settings.LOCATION_SOURCE_SETTINGS"}    //    定位服务设置
{action:"android.settings.MANAGE_ALL_APPLICATIONS_SETTINGS"}    //    所有应用
{action:"android.settings.MANAGE_APPLICATIONS_SETTINGS"}    //    应用管理
{action:"android.settings.MANAGE_DEFAULT_APPS_SETTINGS"}    //    与ACTION_HOME_SETTINGS相同
{action:"android.settings.action.MANAGE_OVERLAY_PERMISSION"}    //    在其他应用上层显示,悬浮窗
{action:"android.settings.MANAGE_UNKNOWN_APP_SOURCES"}    //    安装未知应用 安卓8.0
{action:"android.settings.action.MANAGE_WRITE_SETTINGS"}    //    可修改系统设置权限
{action:"android.settings.MEMORY_CARD_SETTINGS"}    //    内存与存储
{action:"android.settings.NETWORK_OPERATOR_SETTINGS"}    //    可用网络选择
{action:"android.settings.NFCSHARING_SETTINGS"}    //    NFC设置
{action:"android.settings.NFC_SETTINGS"}    //    网络中的更多设置
{action:"android.settings.ACTION_NOTIFICATION_LISTENER_SETTINGS"}    //    知权限设置
{action:"android.settings.NOTIFICATION_POLICY_ACCESS_SETTINGS"}    //    勿扰权限设置
{action:"android.settings.ACTION_PRINT_SETTINGS"}    //    印服务设置
{action:"android.settings.PRIVACY_SETTINGS"}    //    备份和重置
{action:"android.settings.SECURITY_SETTINGS"}    //    安全设置
{action:"android.settings.SHOW_REGULATORY_INFO"}    //    监管信息
{action:"android.settings.SOUND_SETTINGS"}    //    声音设置
{action:"android.settings.SYNC_SETTINGS"}    //    添加账户设置
{action:"android.settings.USAGE_ACCESS_SETTINGS"}    //    有权查看使用情况的应用
{action:"android.settings.USER_DICTIONARY_SETTINGS"}    //    个人词典
{action:"android.settings.VOICE_INPUT_SETTINGS"}    //    辅助应用和语音输入
{action:"android.settings.VPN_SETTINGS"}    //    VPN设置
{action:"android.settings.VR_LISTENER_SETTINGS"}    //    VR助手
{action:"android.settings.WEBVIEW_SETTINGS"}    //    选择webview
{action:"android.settings.WIFI_IP_SETTINGS"}    //    高级WLAN设置
{action:"android.settings.WIFI_SETTINGS"}    //    选择WIFI,连接WIFI
```

### 快手：

```js
kwai://gamezone/home  打开游戏专区
kwai://gamezone/game/[游戏ID]  打开某个游戏
kwai://webview?url=[URL链接]  在快手中打开指定URL
kwai://tag/topic/哒视眼镜  不知道什么玩意
kwai://home/following  打开关注
kwai://home/hot  打开发现  kwai://promotion
kwai://home/local  打开同城
kwai://profile/[用户UID]  打开用户主页
kwai://profilesetting  编辑个人资料
kwai://business/poi    地理位置
kwai://business/location  定位界面
kwai://work/[作品ID]  打开某作品
kwai://work/[PhotoId]?userId=[UserId]
kwai://live/play/[LiveStreamId]  上面两个应该是 图片作品  这个是小视频作品
kwai://liveaggregate?sourceType=[不知道什么参数]
kwai://liveaggregate/[未知参数]?sourceType=[未知参数]
kwai://musicstation/[PhotoId]?userId=[UserId]&sourceType=[Integer.valueOf(13)]
kwai://musicstation    快手音悦台
kwai://followers  粉丝列表
kwai://followings  关注列表
kwai://tube/square  小剧场 
```

### 微信：

```js
weixin://dl/scan 扫一扫
weixin://dl/feedback 反馈
weixin://dl/moments 朋友圈
weixin://dl/settings 设置
weixin://dl/notifications 消息通知设置
weixin://dl/chat 聊天设置
weixin://dl/general 通用设置
weixin://dl/officialaccounts 公众号
weixin://dl/games 游戏
weixin://dl/help 帮助
weixin://dl/feedback 反馈
weixin://dl/profile 个人信息
weixin://dl/features 功能插件
```

### 抖音：

个人主页

```js
/*** 自己的个人页面*/
function jumpMyProfile() {
  app.startActivity({
    action: "android.intent.action.VIEW",
    data: "snssdk1128://user/profile/" ,
  });
}

jumpMyProfile();
```

抖音热榜

```javascript
function jumpTrending() {
    app.startActivity({
        action: "android.intent.action.VIEW",
        data: "snssdk1128://search/trending",
    });
}
jumpTrending();
```

 跳转原声音乐作品

```javascript
/**
 * 跳转原声音乐作品
 * @param {*} wid 音乐id
 */
function jumpMusic(mid) {
  app.startActivity({
    action: "android.intent.action.VIEW",
    data: "snssdk1128://music/detail/" + mid,
  });
}

jumpMusic("7284990191742274364");
```

打开指定视频作品

```javascript
/**
 * 打开指定视频作品
 * @param {*} wid 视频id
 */

function jumpVideo(wid) {
  app.startActivity({
    action: "android.intent.action.VIEW",
    data: "snssdk1128://aweme/detail/" + wid,
  });
}
jumpVideo("7241321383081381178");
```

跳转指定用户主页

```javascript
/**
 * 跳转指定用户主页
* @param {string} uid 用户uid*/

function jumpUserProfile(uid) {
    app.startActivity({
        action: "android.intent.action.VIEW",
        data: "snssdk1128://user/profile/" + uid,
    });
}

jumpUserProfile("58804298436");
```

## 配置AutoPro服务端：

### 服务器环境要求

- 操作系统：Linux Centos 7.6
- 宝塔面板
- Nginx 1.22.x
- Node.js v16.2.2

###  服务器 AutojsPro 接口源码

```js
var express = require('express');
const { createProxyMiddleware } = require('http-proxy-middleware');

var app = express();

app.use(function (req, res, next) {
   var path = req.path;
   console.log("访问路径：" + path);
   next();
});

app.get("/", (req, res) => {
    res.send('博客: <a href="http://www.wuyunai.com">http://www.wuyunai.com</a><br> AutojsPro文档: <a href="http://www.wuyunai.com/docs">http://www.wuyunai.com/docs</a>');
});

app.use('/docs', createProxyMiddleware({
   target: 'http://wuyunai.com',
   changeOrigin: true,
   pathRewrite: {
      '^/docs': '/docs'
   }
}));

app.all('/api/v1/plugins', (req, res) => {
  const plugins = [
    {
      package_name: 'org.autojs.plugin.ffmpeg',
      name: '官方FFMpeg插件',
      version: '1.1',
      version_code: 1,
      summary: "FFmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序。本插件用于利用ffmpeg处理音视频文件，比如从格式转换等。",
      icon: 'https://www.wuyunai.com/docs/assets/image/ffmpeg-plugin.png',
      url: 'https://www.wuyunai.com/docs/blog/ffmpeg-plugin.html',
      installed: false,
      update_timestamp: 0
    },
    {
      package_name: 'org.autojs.plugin.mlkit',
      name: '官方MLKitOCR插件',
      version: '1.1',
      version_code: 1,
      summary: "FFmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序。本插件用于利用ffmpeg处理音视频文件，比如从格式转换等。",
      icon: 'https://www.wuyunai.com/docs/assets/image/mlkit-ocr-plugin.png',
      url: 'https://www.wuyunai.com/docs/blog/mlkit-ocr-plugin.html',
      installed: false,
      update_timestamp: 0
    },
    {
      package_name: 'cn.lzx284.p7zip',
      name: '7Zip通用压缩插件',
      version: '1.2.1',
      version_code: 4,
      summary: '本插件基于p7zip 16.02制作，支持多种格式文件的压缩与解压。7-Zip是一款完全免费而且开源的压缩软件，相比其他软件有更高的压缩比但同时耗费的资源也相对更多，能提供比使用 PKZip 及 WinZip 高2~10%的压缩比率。',
      icon: 'https://www.wuyunai.com/docs/assets/image/7zip-plugin.png',
      url: 'https://www.wuyunai.com/docs/blog/7zip-plugin.html',
      documentation_url: 'https://www.wuyunai.com/docs/blog/7zip-plugin.html',
      installed: false,
      update_timestamp: 0
    },
    {
      package_name: 'com.hraps.pytorch',
      name: 'Pytorch插件',
      version: '1.0.0',
      version_code: 1,
      summary: 'Pytorch模块提供了已完成的深度学习神经网络模型在安卓设备上执行的功能，可以实现常规程序难以实现的功能，如：图像识别，语言翻译，语言问答等。',
      icon: 'https://www.wuyunai.com/docs/assets/image/pytorch-logo.png',
      url: 'https://www.wuyunai.com/docs/v8/thirdPartyPlugins.html',
      documentation_url: 'https://www.wuyunai.com/docs/v8/thirdPartyPlugins.html#pytorch插件',
      installed: false,
      update_timestamp: 0
    }
  ];
  res.json(plugins);
});

app.get('/api/v1/account', function (req, res) {

   var response = {
      "id": "6131f76468e4553fba39ae4c",
      "now": Date.now(),
      "emailAddress": "五云学习",
      "fullName": "AutojsPro9.3.11",
      "paidServices": {
         "v8": {
            "expires": 3207475409760
         }
      },
      "permissions": {}
   };

   res.set({
      "Server": "nginx/1.12.2",
      "Date": new Date().toUTCString(),
      "Content-Type": "application/json; charset=utf-8",
      "X-Powered-By": "Sails <sailsjs.com>",
      "ETag": 'W/"b1-VocwenWVuO/o20Vz9VD1X4Qvte4"',
      "Cache-Control": "no-cache, no-store",
      "Connection": "keep-alive",
      "Keep-Alive": "timeout=5",
   });

   res.status(200).json(response);
});

app.get('/static/legal/version.json', function (req, res) {
   // 提取 x-csrf-token 头部信息
   var csrfToken = req.headers['x-csrf-token'];
   var response = {
      "version": 20230211,
      "wording": "为了给您提供更好的服务，我们更新了“服务协议”和“隐私政策”，特别提示您重新阅读并充分理解“服务协议”和“隐私政策”各条款。\n您可以阅读%s和%s全文了解详细信息。如您同意并接受更新后的“服务协议”和“隐私政策”各条款，请点击“同意”继续接受我们的服务。"
   };
   res.set({
      "Server": "nginx/1.12.2",
      "Date": new Date().toUTCString(),
      "Content-Type": "application/json; charset=utf-8",
      "X-Powered-By": "Sails <sailsjs.com>",
      "ETag": 'W/"b1-VocwenWVuO/o20Vz9VD1X4Qvte4"',
      "Cache-Control": "no-cache, no-store",
      "Connection": "keep-alive",
      "Keep-Alive": "timeout=5"
   });

   res.status(200).json(response);
});

app.post('/api/v1/security/validation2', function (req, res) {

   // 提取 x-csrf-token 头部信息
   var csrfToken = req.headers['x-csrf-token'];
   var response;
   if (csrfToken != "Tbs6hIVo--Ngb_G9VJ3lnoMR1EYRnQli5bEY") {
      response = {
         "data": "uNl8AK0WM6mIAQAAM9bHGgAAAACaX4kztI8jdDdMKBwYbba4oNAKCHba0nRgN7zXoP0IzjEyM2NjZjgzMmFiZTg5OGYAAQAAAAAAAAAyWsXfnWpHYVlJ4ZPT/u3n+ZH3NLvubrTRJnas08r0ijocgKnKqCxTFvJgeZnWx2omp6CzeSFWEG8aEaarJ4XMkp9+F8sdy2yFkqkOrp41KmCfShbIQX4hCYeD0mVOOwfOVLpQLJjg18FvFvHm9TKYzK5ysfv9UHuHn8+dexgnLM28j5BDrIFv9B9XS+UW1x/lLAwe+QzBEAWzsYFKPkVJ9Mc0L5lG/i8Eh7bxcGHIg1L+VbC4t9+CZXcF6DOoy75I40omuQs/gtbLCsMEr7fdsiDQ76iukr1SwLHVIEaXrNutrvvqKp+UBcq4WGQEM+aMj46S3pd7+h17J8vKdTVknI2IOJPZM2mVjGCQ3MBriG5HQqghbFE3y/VEPWpmtkgjDXqc09vuYA4PLxnV1AbvoAEvy8FgqxY00MXANK2MMixzZorUIC2Jk1hBLgPYHd1lMPlAMt8Deab3KZ0sJNLMo/7tAzk50DrPse3onAg5oA5QTSDfKBI2AtZP+DmPYrtsa96iUFK9iz8/18Pnhw/GBd+ceDR00dpQRVGjqTFxftAtZFr9kFYXTfz94+uq/fnVlH4eDGQiNAvuPg/4nQLXlde3lDYp5loaN2MkjL4uK9m8uQjH68217L195jsXANSo8IKjJYqWzcA1oCF/Smnmwc03k0Uk5OcfunIF/AGJ1g=="
      };
   } else {
      response = {
         "data": "XFSLAJob6izJAzdNz4d7zvnjSYFK3DiLgFMUaZZp4uWAdvyxAH7AeN152Gf8Qpoofi9+DLdMaVhgKTvvRwV8qd2IjybaGw4y6qZRqrktuMC0+jzG1ZGndkV3pAD7J7kdEZYeA62vZGZLyAotpgKArQrOkSi0LWy2ll/f6c99hb4aIiHyNAC+K8t4SzFiGjw7BGojPEm4VdIwOOKGwGN+cCBdAPFTtxz+RpYT4d4oiOa4287wKfJKqK2HNPz7+d3ctyS1blJXpf0MGEI+kZc/sWAKduyZgCfu7n0GA85Bqj4cl9z2TsZeUD3MG2Sr5+mzgbDmxJc9FSGdw1bpn3PT1Z1t5NsCy/fR3csBrsEQgqX0w6j10D29p0WxrM5Irk0BbehR53GLPvoROXVTFIdGjk7+mJIFvTvGIbq9lsgPABHR8MLsnrJZDwVUQ4feXuQDCROmXTeOJ6ixwqzsPjW4BEAmH3kg1DVF8IWyXPQg3t52voiPUzm/k1/XdAcUeOrmwi5H2g9L8chjK9sDLIzYUCUKmaLf2XxtM5RVL0QnlZQ/7EdZJ6yPD7RnFwN3Fiuwq4e9xTOqyzuu/S+bYfw0dM+c4hgNCTUnN8IWZMK0eApXXgP1zUlIBHWfPOdpor+Ajmbln2QRny8Gqfm3lB4SydB+wnA1TL/HyxFgwHyKtJ9Eb1Ql"
      };
   }


   res.set({
      "Server": "nginx/1.12.2",
      "Date": new Date().toUTCString(),
      "Content-Type": "application/json; charset=utf-8",
      "X-Powered-By": "Sails <sailsjs.com>",
      "ETag": 'W/"b1-VocwenWVuO/o20Vz9VD1X4Qvte4"',
      "Cache-Control": "no-cache, no-store",
      "Connection": "keep-alive",
      "Keep-Alive": "timeout=5",
      "Transfer-Encoding": "chunked",
   });

   res.status(200).json(response);
});

app.get('/api/v1/config', function (req, res) {

   var response = {
      "wl": "0a4fd5d5accf385b8d5f382d7abcfea7"
   };


   res.set({
      "Server": "nginx/1.12.2",
      "Date": new Date().toUTCString(),
      "Content-Type": "application/json; charset=utf-8",
      "X-Powered-By": "Sails <sailsjs.com>",
      "ETag": 'W/"b1-VocwenWVuO/o20Vz9VD1X4Qvte4"',
      "Cache-Control": "no-cache, no-store",
      "Connection": "keep-alive",
      "Keep-Alive": "timeout=5"
   });

   res.status(200).json(response);
});

app.get('/csrfToken', function (req, res) {
   var response = {
      "_csrf": "Tbs6hIVo--Ngb_G9VJ3lnoMR1EYRnQli5bEY"
   };

   // 计算过期时间，当前时间加一个月
   var expiryDate = new Date();
   expiryDate.setMonth(expiryDate.getMonth() + 1);

   res.set({
      "Server": "nginx/1.12.2",
      "Date": new Date().toUTCString(),
      "Content-Type": "application/json; charset=utf-8",
      "X-Powered-By": "Sails <sailsjs.com>",
      "ETag": 'W/"b1-VocwenWVuO/o20Vz9VD1X4Qvte4"',
      "Cache-Control": "no-cache, no-store",
      "Connection": "keep-alive",
      "Keep-Alive": "timeout=5",
      "Set-Cookie": "sails.sid=s%3AZiwwINUFknF0pT7ncljTzWh-w1e-q2Wr.WYipgZnGJb2RubVHpzJlpjyuP9%2FPwCMoE8FIPP1sQGc; Path=/; Expires=" + expiryDate.toUTCString() + "; HttpOnly; Secure"
   });

   res.status(200).json(response);
});

var server = app.listen(8881, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("欢迎使用，访问地址为 http://%s:%s，五云学习(wuyunai.com)", host, port);
});

```

###  开始部署

#### 安装 Node.js 并使用 nvm 管理

1. 安装 nvm

```js
//  两个随便选一个，第一个链接要梯子
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash

curl -o- https://gitee.com/RubyMetric/nvm-cn/raw/main/install.sh | bash
```

2. **配置 nvm 环境**

```js
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

3. **安装 Node.js**

```js
nvm install v16.20.2
```

1. **切换到 Node.js 版本 16.20.2 并设置为默认版本**

```js
nvm use v16.20.2
nvm alias default v16.20.2
```

可以通过运行`node -v`命令来验证切换和默认版本是否设置成功。输出应该显示`v16.20.2`。

#### 使用宝塔面板添加站点

1. 打开宝塔面板，进入网站管理页面。

2. 添加新的网站，域名填写两栏

   ```js
   pro.autojs.org
   服务器ip
   ```

3. 配置其他参数，如网站目录和 SSL 证书等。

   关于SSL证书怎么自制可以查看

   [b2_insert_post id=”16″]

4. 保存并应用配置，完成站点的添加

#### 部署 Node.js 应用

  PM2是一个流行的 Node.js 进程管理工具，可以用来监控和管理应用程序。使用 PM2，你可以轻松地启动、停止、重启和监视你的 Node.js 应用程序。

1.使用终端进入你的刚创建网站目录，将 `server.js` 文件放在网站目录下。

```js
// 安装 Express 框架
npm install express

// 启动node服务
node server.js

// 安装
npm install -g pm2

// 启动
pm2 start server.js -i 0
```

#### 配置Nginx文件

最顶部填写

```js
upstream autojs {
    server 127.0.0.1:8881;
    keepalive 10240;
}
```

![](D:\blog\笔记图片\前端\Autojs\AutoPro服务器.png)

server里面增加 ，放在其他的localtion最上面

```js
location ^~ / {
    proxy_pass http://autojs;
}
```

![](D:\blog\笔记图片\前端\Autojs\AutoPro服务器2.png)

配置AutoPro服务器成功，手机使用配置 ip就破解了。
















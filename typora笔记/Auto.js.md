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

快手：

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

微信：

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

抖音：

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


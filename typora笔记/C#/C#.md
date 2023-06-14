

 运行程序：Ctrl+F5 

```c#
Console.Write();   //向控制台界面不换行输出内容

Console.WriteLine(); //想控制台界面输出内容
```

  

### 二进制 装换 十进制
$$
二进制   01    ==>   十进制  1                                 
二进制   10    ==>   十进制  2
二进制   100   ==>   十进制  4

公式：   0* 2^1  + 1*2^0    =  1
       1*2^2 + 0*2^1 + 0*2^0 = 4
$$

### 静态调用静态

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/C#/%E9%9D%99%E6%80%81%E8%B0%83%E7%94%A8%E9%9D%99%E6%80%81.png)



### 重载

#####  1、函数名相同，参数列表相同。





## 重定向

### 方法一：

Redirect: 创建一个重定向到指定的URL

```c#
public ActionResult InsertNotice(){
    if(Session["UserID"==null])
    {
        return Redirect("/Main/Login");
    }
};

```

### 方法二：

 优点：写一次整个控制器可用

```c#
       private int userID;
        //复写父类的该方法。执行控制器中的方法之前先执行该方法。从而实现过滤的功能。
        protected override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            base.OnActionExecuting(filterContext);

            //验证用户登录
            if (Session["UserID"] == null)
            {
                //没有登录，重定向 ,不会执行后续的方法，而是直接跳转到登录页面
                filterContext.Result = Redirect("/Main/Login");
            }
            else
            {
                userID = (int)Session["UserID"];
            }
        }

```





## 查找&创建指定文件夹（目录），除非已存在

Directory.Exists:确定指定目录是否有此目录

Directory.CreateDirectory:创建目录和子目录

Server.MapPath:返回虚拟路径（相对路径）相对应的物理文件路径

```c#
//判断是否有此路径，没有就新建
if(!Directory.Exists(Server.MapPath("~/Document/Notice/Text/"))){
    //新建路径
    irectory.CreateDirectory(Server.MapPath("~/Document/Notice/Text/"));
   }
```

### 替换 ： Replace

$$
替换： Replace(string oldValue, string newValue):       参数：oldValue:要替换的字符串  ；   newValue:要替换oldValu所有匹配项的字符串
$$

```c#
//txt文件内容
string textCount = pwNotice.NoticeContent;
//替换所有图片路径
textCount =textCount.Peplace("/Document/Temp/", "/Document/Notice/Image/");

//控制台
```

### 保存为Txt文件： TextWriter

```c#
TextWriter textWriter = new StreamWriter(filePath, false, new System.Text.UTF8Encoding(false));//如果该文件存在，则可以将其覆盖或向其追加。
textWriter.Write(textContent);//将此对象的文本表示形式写入文本字符串或流。
textWriter.Close();//Close() 关闭当前编写器并释放与该编写器关联的系统资源                                //控制台
```



## 关键字

###           default

 //C#中的关键词default函数，其作用是default(T)返回一个该类型T的默认值，一般情况下用于在不知道类型参数具体为值类型还是引用类型的情况

```C#
 SYS_Academe academe =(from tbAcademe in myModel.SYS_Academe).SingleOrDefault();
 //判断是否为空
if(academe!=default(SYS_Academe)){   //引用类型：null , 值类型：0
    pwNotice.AcademeID=academe.AcademeID;
  }
```

###       as

//转换为某种类型

```c#
 List<FilesVO> sessionFiles = new List<FilesVO>();
 if (Session["sessionFiles"]!=null)
    {
         sessionFiles = Session["sessionFiles"] as List<FilesVO>; //as: 转换为某种类型
     }
```

## 提供用于创建、复制、删除、移动和打开单一文件的静态方法

System.IO.File：提供用于创建、复制、删除、移动和打开单一文件的静态方法

#### ReadAllText

ReadAllText：打开一个文本文件，读取文件的所有行，然后关闭该文件

```c#
//控制台
notice.NoticeContent = System.IO.File.ReadAllText(textFileName);
```



## Html.Raw( ) : 输出带有html标签的字符串



![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/html/Html.Rau().png)



```html
@Html.Raw(notice.NoticeCotent)
//Html
```

## 事务回滚



```c#
//需要添加引用 System.Transactions
using(var scope = new TransactionScope()){
    //提交事务(设定交易完成)
    //作用：可以让我们在处理交易的時候，可以确保交易的完整性。
    scope.Complete();
}
```

## encodeURIComponent() :编码 &解码

####     对题目信息进行编码，将字符串作为URI组件进行编码：encodeURIComponent（）

html( 原始容器 ) ：

```html
    <div class="col-8 left"> 
<div class="left-titles" id="titlesInfor" contenteditable="true" style="overflow:auto;">
       @*题目信息*@
     </div>
  </div>
    
```

jS( 编码 ):

```js
 
var titlesInfor = $("#titlesInfor").html();//题目信息框
   //对题目信息进行编码，将字符串作为URI组件进行编码
   titlesInfor=encodeURIComponent(titlesInfor);
    $.post("SaveTitle", {TitleInfor: titlesInfor}，function(rtMsg){
        
    }
```

控制台( 解码 )： 

```c#

public ActionResult SaveTitle(string TitleInfor){       //解码
 var titleInfor=Server.UrlDecode(TitleInfor);
    
}
```

### 序列化

## 序列化：serialze

![serialize序列化](F:\Typora\笔记图片\serialize序列化.png)

![]()

```js
var MAudiState = $("#formClass").serialize();
            console.log(MAudiState);

```



## 序列化集合：serializeArray

![serializeArray序列化集合](F:\Typora\笔记图片\serializeArray序列化集合.png)

```js
var frRecordInsert = $("#frRecordInsert").serializeArray();// 
           console.log(frRecordInsert); 

```

## 获取单选框radio

获取 审核通过中的value

```html
 <input type="radio" name="MAudiState"  value="1"/>待审核
 <input type="radio" name="MAudiState" value="2"/>审核通过
 <input type="radio" name="MAudiState" value="3"/>审核不通过
```

js:

```js
var MAudiState = $("#formClass").serialize();//或serializeArray集合序列化
var MAudiState =$("#formClass  input[type='radio']:checked").val();

```





## 传参报错

```c#
 [ValidateInput(false)  //由于要存入HTML,关闭验证。
//取消危险字符的验证，如”<> %$# “等。
//为了安全起见，正确的Post提交诸如<>$/等敏感字符的（有点类似脚本注入），
//如果你要提交这些东西的话，就需要加上ValidateInput标签，比如富文本编辑的时候。
```



```c#
//控制器
[ValidateInput(false)]
public ActionResult InsertNoticeInfor(){

    return Json(null, JsonRequestBehavior.AllowGet);
}
```

## 在主页面 往回嵌套者 其他页面

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/html/%E5%B5%8C%E5%A5%97%E9%A1%B5%E9%9D%A2.png)

在登录页面写上：

```js
//判断主页面是否 往回嵌套者其他页面
$(document).ready(function () {
    if (window.top.location.href != window.location.href) {
        window.top.location.href = window.location.href;
    }
});
```


















































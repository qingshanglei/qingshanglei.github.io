# HTML

```html



      
  <button></button>-----按钮标签                                      <ul></ul>-------段落标签

  <u></u>---------------下划线      

  <link rel="icon" type="image/x-icom"href="../images/icon2.ico">---引入网页图标 

  <a href="#"></a>---------------里面加跳转网页，锚标签    
ink+fab=<link rel="stylesheet" href="css">-----------css引入
  
  <s></s>---------------删除线         
script+fab= <script></script>+src= <script src=""></script>---------js引入；
     
  <span></span>--------（行内）                        <input type="text">---------文本框      快捷键：input : text   
  
  <textarea rows(高度)="3" cols(宽度)="30"></textarea>---------文本输入区域

  <b></b>---------------粗体                             <input type="password">-----密码框    去掉input的光标效果:outline:none;

  <i></i> --------------斜体                                              <input type="submit">-------提交按扭

  <hr></hr>-------------水平线                
  
  <input type="checkbox"  @(ViewBag.UserTypeClass=="1"?"selected":"")>-----复选框        @():在标签里判断
                                                
  <p></p>---------------段落                                           <input type="radio" checked>--------单选框      disabled  :  这个加在input标签后面；禁用input          checked:默认勾选

  <br></br>-------------换行                                           <input type="reset">--------重置按扭

  <img>-----------插入图片（自闭和标签）                                     <input type="image"  alt="双击选择图片">--------图片按扭       alt-------图片里面的提示框               autocomplete="off"   -------删除历史记录                                                  

  <h1></h1>-------------标题标签1>2>3>4>5>6                                     <input type="button" value="按钮">------按钮   <button onclick="window.location.href = '/Marketing_administration/Marketing/Newly'">新增营销方案</button>---------跳转路径
 
  <hr>------------------线条                                                                        <input,type="file"  accept="image/jpg,image/jpeg,image/png,image/gif,image/bmp">---------------------浏览文件     accept:格式过滤

  <select name="" id=""></select>-----下拉框                                           <option value=""></option>）------------选项标签

  <input  type="checkbox">------------复选框                                            <input  type="date">--------------------日历
 
  <label for="Name">姓名</label>------for="Name"：光标聚集效果         <input type="text"  placeholder="提示">------//placeholder="提示">文本框里提示

  <select>----------------------------下选框                                                 <input,type="hidden">-------------------隐藏域                                                                                                
            <option>1</option>
            <option>2</option>                                                                    alert('提示框');
            <option>3</option>
            <option>4</option>             
            <option>5</option>                                        
  </select>       
  
var MAudiStatess = $('#MAudiStates [type="radio"]').prop("checked"); ------------------//获取复\单选框的值

 fd.append("noticeCarouseImage", $('#fromNotice input[name=noticeCarouseImage"]').prop("files")[0]);//文件添加到FormData
<big> -------------------------------定义大号字。 
<em> --------------------------------定义着重文字。 
<i> ---------------------------------定义斜体字。 
<small> -----------------------------定义小号字。 
<strong> ----------------------------定义加重语气。 
<sub> -------------------------------定义下标字。 
<sup> -------------------------------定义上标字。 
<ins> -------------------------------定义插入字。 
<del> -------------------------------定义删除字 











```

# CSS



```css
                                                                                                                                                         
    transform: translateY(-110px);--y轴运动 
overflow:hidden;-----------------------隐藏    display:none;      
overflow:auto;--------------------------------------------居中  （可使方框出现滚动条）

    style-----------------------放在div里面可以写css样式（行内样式）                 border-color:transparent transparent transparent #65aac3; ------- 制作三角形

    width:300px；---------------宽度；                                       display: inline-block;----------------块级元素横排显示
                                                                                                         
    height: 300px;--------------高度；                                      text-decoration: underline;-----------添加文字下划线                                         
                                                                                                         
    background：#fff;-----------背景颜色；                                   outline: none;------------------------取消文本框选择效果 
 
    opacity: 0.5;------------------透明度                                   float:left;------------------------------浮动
                                                                                                     
    line-height: 20px;----------行高                                        background-size: cover----------------图片大小自适应 
                                                                                                       
    font-size: 12px;------------字体大小                                    contenteditable="true"----------------div编辑文本
       
    font-style: italic;---------文字斜体                                   font-style: oblique;------------------文字倾体
                                                                                                 
    color: #fff;----------------字体颜色                                     font-weight: normal;------------------字体正常 
                                                                                                        
    font-weight: bold;----------字体加粗          
font-style:---------------------------设置字体的风格。属性值有：normal(正常),italic(斜体),oblique(倾斜)。
                                
    text-decoration:line-through;----------------------中划线                text-decoration: overline;------------上划线 

    text-decoration: line-through;---------------------中划线                text-decoration:none; ----------------取消<a>下划线
                                                                       
    overflow: hidden;----------------------------------超出部分隐藏                    text-shadow: #f00 2px 5px 5px;--------该属性用于设置的文字是否有阴影效果。 color:指定该阴影的颜色。 xoffset:指定阴影在横向上的偏移。 yoffset:指定阴影在纵向上的偏移。 radius：A指定阴影的模糊半径。模糊半径越大，阴影看上去越模糊。 多重阴影：多加几组数据即可。 sytle="text-shadow:5px 5px 2px #cf0"                  
                                                                                                
    Font-----------------------------------------------字体          
    style="display: none;"----------------行内样式。隐藏     （display: none）                                                                

    ：last-child---------------------------------------选择到最后一个元素             :hover--------------------------------伪类

    :first-child--------------------选择到第一个元素                   :nth-child(2)--------------------选择到第2（几个）元素

    border-radius: 5px;--------------------------------圆角样式                           border-radius:0px 0px 100% ;  --------扇形圆角 （	border-radius:0px 100% 0px 0px ;      	border-radius:100% 0px 0px ;      	border-radius:0px 0px 0px 100%;      border-radius:  0px 0px 100%; ）                   

    display: block;（display:inline-block;）-----------行内元素转换为块级元素     overflow-x: hidden;--------------------去掉下拉条；

    display: inline-block;-----------------------------行内块级元素

    background: url();---------------------------------背景图片

    list-style: none;----------------------------------取消无序列表中的默认样式

    text-decoration: none;-----------------------------取消下划线

    list-style: none;----------------------------------取消 li 标点的点；

    background-color: #e64b16;-------------------------伪类

    box-shadow：5px 5px 5px #111;----------------------盒子阴影；设置四边阴影多加一个数值，如=0px 1px 2px 3px #111;

    ！！！！（h-shadow：-------------------------------水平方向阴影

    v-shadow：-----------------------------------------垂直方向的阴影

    blur：---------------------------------------------模糊距离

    color：--------------------------------------------字体颜色          font-family: 'Lobster',cursive;-----------------字体楷体

    transparent----------------------------------------透明色

    no-repeat------------------------------------------不重复出现

    cursor: pointer;-----------------------------------鼠标移入变成手的形状

    z-index: ------------------------------------------层次（和定位一起使用）

    even---------------------------------------------—选择序列位偶数的元素、odd选择序列为奇数的元素

    background—size：cover；

  overflow: scroll;-----------------------------------超出部分鼠标滚轮
```







# 特殊符号代码



```css
   ||   ∴  &there4;        ∞  &infin;     ⊕ &oplus;      Δ   &Delta;        Θ  &Theta;  ||                         
   ||   ?  &copy;           ?  &reg;         ? &trade;       &nbsp;-----空格              ||                           
   ||                                                                                        ||                                                               +
```



# js

```js
   serialize()---------序列化
   serializeArray()---------序列化数组
   $("#frUserInsert")[0].reset(); //清空模态框
   split(/[,，]/))-------分割多个字符      split(","))-------分割1个字符 
```

## js方法命名术

```js
打开新增模态框---openInsertModal                    保存---save
打开修改模态框---openUpdateModal                    删除---del
下载模板---downImportTemplate
上传模板---uploadExcelFile
导出---exportExcel



```

# Controller

```c#
       //查询时间段
            if (!string.IsNullOrEmpty(test1)&&!string.IsNullOrEmpty(test2))
            {
                DateTime test1s = Convert.ToDateTime(test1);
                DateTime test2s = Convert.ToDateTime(test2);
                tbFlowStandard = tbFlowStandard.Where(m => m.borrowDate >= test1s
                && m.borrowDate<=test2s).ToList();
            }
```


































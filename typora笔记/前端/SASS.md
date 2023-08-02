# 概念：

## 一、CSS预处理器出现的原因

1. 无法嵌套书写导致代码繁重、冗杂、逻辑混乱。
2. 没有变量和样式复用机制，属性值只能以字面量的形式重复输出。

~~~shell
# 总结：代码复用性低；不易于维护
# 注：现在，现在的CSS是可以定义变量的！！！
~~~

~~~css
:root{
    --red: #f3e1e1;
}
~~~

-----

## 二、CSS预处理器介绍

### 1. SCSS/SASS

SASS (.scss)。于2007年诞生，最早也是最成熟的CSS预处理器，拥有ruby社区的支持和compass这一最强大的css框架，目前受LESS影响，已经进化到了全面兼容CSS的SCSS。

### 2. LESS

LESS (.less)。于2009年诞生，借鉴了SASS的长处，并兼容了CSS语法，使得开发者使用起来更为方便顺手，但是相比于SASS，其编程功能不够丰富，反而促使SASS进化成为了SCSS。

### 3. Stylus

Stylus (.styl)。于2010年诞生，出自Node.js社区，主要用来给Node项目进行CSS预处理支持，人气较前两者偏低。



## SCSS和SASS之间的关系

![image-20200706194107318](https://raw.githubusercontent.com/ggdream/scss/master/sources.assets/image-20200706194107318.png)

```
Sass有两套语法：

1.第一种或更新的语法被称为SCSS。它是CSS语法的扩展。这意味着每个有效的CSS样式表都是具有相同含义的有效SCSS文件。下文描述的Sass功能增强了此语法。使用此语法的文件扩展名为.scss。

2.第二种或更旧的语法被称为SASS。提供了一种更为简洁的CSS编写方式。它使用缩进而不是方括号来表示选择器的嵌套，并使用换行符而不是分号来分隔属性。使用此语法的文件扩展名为.sass。

任何一种格式可以直接 导入 (@import) 到另一种格式中使用，或者通过 sass-convert 命令行工具转换成另一种格式
```

## CSS预处理器的优劣:

​      优点:CSS预处理器为CSS增加一些编程的特性，无需考虑浏览器的兼容性问题。支持嵌套、变量和逻辑等。可以让CSS更加简洁、提高代码复用性、逻辑分明等等
​    缺点: css的文件体积和复杂度不可控；增加了调试难度和成本等。



1. 官方介绍

   ~~~
   Sass 是一款强化 CSS 的辅助工具，它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能，这些拓展令 CSS 更加强大与优雅。使用 Sass 以及 Sass 的样式库（如 Compass）有助于更好地组织管理样式文件，以及更高效地开发项目。
   ~~~

2. 特色功能

   - 完全兼容 CSS3
   - 在 CSS 基础上增加变量、嵌套 (nesting)、混合 (mixins) 等功能
   - 通过函数进行颜色值与属性值的运算
   - 提供控制指令 (control directives)等高级功能
   - 自定义输出格式

​     注意：  less、scss(sass)和stylus代码并不能被浏览器直接解析，所以必须先将它们编译成css代码。
​     解决方法：vscode   安装Easy Sass（编译）和Sass（代码提示）两个插件





# SASS总结：

```scss
  <style lang="scss" ></style>  -----------html中引有Scss（默认css）
     font:10px/20px Helvetica;  字体大小/行高
嵌套规则：
① 选择器嵌套: #container{  .header{color: #F00; } }      css：#container .header{color: #F00; }
②父选择器&：a{ &:hover{  color: #F00;  } }     css:a:hover{color: #F00;}
③属性嵌套：font: {
          size: 14px;
          font-family: sans-serif;/*字体类型*/
          weight: bold; /*字体加粗*/}
④占位符选择器：%名称{   }-------------------定义(方法)占位符选择器     @extend %名称(); -------------------使用

$名称：变量类型; -------------------定义变量       font（类型）:$名称-------------------使用变量
@import "url"； -------------------导入文件
@mixin 名称(参数){} ---------定义(方法)混用指令     @include 名称(参数)；-------------------使用(方法)混用指令    
@extend  类名或ID; ----------------继承
!global;    -------------------局部变量 转 全局变量
 #{ } -------------------插值语句
 
              运算：
    % 不能与别的单位一起运算
    纯数字与单位运算时会自动转换成相应的单位

函数：
                       颜色函数：
 opacify() 
  &:after{  content: quote(要填写的内容);  } -------------------向里面插入内容
 unqote() -------------------去除""

                 数值函数：
 str-length() --------------计算长度
 abs() ---------获取整数            ceil()----------正向上取整，负向下取整
 max() -----------取最大值    random() --------随机0-1
 length()--------计算长度，用空格分割
 index(参数，获取参数的位置) ----------获取参数的位置
 append(值,参数)-----------在列表末尾添加一个值
 nth(值,参数) --------------获取第几个参数的值 
               Map函数：
$font:("key":val,"key":val ...);
 map-has-key()--------根据参数返回true或false； 配合map-get()使用
 map-get()---------根据键值获取map中的对应值
 map-merge()-----------将两个map合并成一个新的map
 map-values()----------映射中的所有值
 map-keys()------------获取所有的key值
 map-values()----------获取所有的value值
            selector选择器函数
  selector-append()----------可以把一个选择符附加到另一个选择符
  selector-unify()-----------将两组选择器合成一个复合选择器
             自检函数
  feature-exists(如果要写变量，不能带$)---------检查当前Sass版本是否存在某个特性 
  variable-exists()---------检查当前作用域是否存在某个变量
  mixin-exists()-----------检查某个mixin是否存在

             流程控制指令：
@if (条件){  }   @ease(条件){  }
@for： 判断1到4的数
   ① @for $参数 from  1  through 4{  } ---------through包含4
   ② @for $参数 from  1  to  4 {   } --------------to不包含4 

@each $var($自命名) in 数组（集合） {   }   -------------判断集合或数组好用
@while  条件{   条件参数：条件参数 - 1；} -----------（必须要减1要不然跳不出）
 
```

# 语法嵌套规则与注释

 

## 语法嵌套规则

 

### 选择器嵌套

例如有这么一段css，正常CSS的写法

```css
.container{width:1200px; margin: 0 auto;}
.container .header{height: 90px; line-height: 90px;}
.container .header .log{width:100px; height:60px;}
.container .center{height: 600px; background-color: #F00;}
.container .footer{font-size: 16px;text-align: center;}
```

改成写SASS的方法

```scss
.container {
    width: 1200px;
    margin: 0 auto;
    .header {
        height: 90px;
        line-height: 90px;
        .log {
            width: 100px;
            height: 60px;
        }
    }
    .center {
        height: 600px;
        background-color: #F00;
    }
    .footer {
        font-size: 16px;
        text-align: center;
    }
}
```

避免了重复输入父选择器，复杂的 CSS 结构更易于管理

 

### 父选择器 &

在嵌套 CSS 规则时，有时也需要直接使用嵌套外层的父选择器，例如，当给某个元素设定 hover 样式时，或者当 body 元素有某个 classname 时，可以用 & 代表嵌套规则外层的父选择器。

例如有这么一段样式：

```scss
.container{width: 1200px;margin: 0 auto;}
.container a{color: #333;}
.container a:hover{text-decoration: underline;color: #F00;}
.container .top{border:1px #f2f2f2 solid;}
.container .top-left{float: left; width: 200px;} 
```

用sass编写

```scss
.container {
    width: 1200px;
    margin: 0 auto;
    a {
        color: #333;
        &:hover {
            text-decoration: underline;
            color: #F00;
        }
    }
    .top {
        border: 1px #f2f2f2 solid;
        &-left {
            float: left;
            width: 200px;
        }
    }
}
```

 

 

### 属性嵌套

有些 CSS 属性遵循相同的命名空间 (namespace)，比如 font-family, font-size, font-weight 都以 font 作为属性的命名空间。为了便于管理这样的属性，同时也为了避免了重复输入，Sass 允许将属性嵌套在命名空间中

例如：

```scss
.container a {
    color: #333;
    font-size: 14px;
    font-family: sans-serif;
    font-weight: bold;
}
```

用SASS的方式写

```scss
.container {
    a {
        color: #333;
        font: {
            size: 14px;
           font-family: sans-serif;
            weight: bold;
        }
    }
}
```

注意font：后面要加一个空格

 

### 占位符选择器 %foo 必须通过 @extend

有时，需要定义一套样式并不是给某个元素用，而是只通过  指令使用，尤其是在制作 Sass 样式库的时候，希望 Sass 能够忽略用不到的样式。`@extend`

例如：

```scss
.button%base {
    display: inline-block;
    margin-bottom: 0;
    font-weight: normal;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    background-image: none;
    border: 1px solid transparent;
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.42857143;
    border-radius: 4px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

.btn-default {
    @extend %base;
    color: #333;
    background-color: #fff;
    border-color: #ccc;
}

.btn-success {
    @extend %base;
    color: #fff;
    background-color: #5cb85c;
    border-color: #4cae4c;
}

.btn-danger {
    @extend %base;
    color: #fff;
    background-color: #d9534f;
    border-color: #d43f3a;
}
```

 

 

## 注释

 

Sass支持两种注释

- 标准的css多行注释 /* ... */			会编译到.css文件中
- 单行注释 //						不会编译到.css文件

```scss
.container {
    width: 1200px;
    .swiper {
        // 网站幻灯片相关css
        width: 100%;
        height: 260px;
        .dot {
            /* 
                    幻灯片指示点
                    默认白色
                    选中时蓝色
                */
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: #FFF;
            &.active {
                background-color: blue;
            }
        }
    }
}
```

 

------

 

# 变量

 

## css变量的定义与书写

 

### CSS定义变量的方法

```scss
:root {
    --color: #F00;
}

body {
    --border-color: #f2f2f2;
}

.header {
    --background-color: #f8f8f8;
}

p {
    color: var(--color);
    border-color: var(--border-color);
}

.header {
    background-color: var(--background-color);
}
```

### SASS的写法

```scss
$font-size:14px;
.container {
    font-size: $font-size;
}
```

 

 

## Sass变量的定义

 

### 定义规则

1. 变量以美元符号($)开头，后面跟变量名；
2. 变量名是不以数字开头的可包含字母、数字、下划线、横线（连接符）；
3. 写法同css，即变量名和值之间用冒号(:)分隔；
4. 变量一定要先定义，后使用；

 

### 连接符与下划线

通过连接符与下划线 定义的同名变量为同一变量，建议使用连接符

```scss
$font-size:14px;
$font_size:16px;
.container{font-size: $font-size;}
```

 

## 变量的作用域

### 局部变量

定义：在选择器内容定义的变量，只能在选择器范围内使用

```scss
.container {
    $font-size: 14px;
    font-size: $font-size;
}
```

 

### 全局变量

定义后能全局使用的变量

#### 第一种：在选择器外面的最前面定义的变量

```scss
$font-size:16px;
.container {
    font-size: $font-size;
}
.footer {
    font-size: $font-size;
}
```

#### 第二种：使用 !global 标志定义全局变量

```scss
.container {
    $font-size: 16px !global;
    font-size: $font-size;
}
.footer {
    font-size: $font-size;
}
```

 

## 变量值类型

变量值的类型可以有很多种

SASS支持 7 种主要的数据类型

- 数字，1, 2, 13, 10px，30%
- 字符串，有引号字符串与无引号字符串，"foo", 'bar', baz
- 颜色，blue, #04a3f9, rgba(255,0,0,0.5)
- 布尔型，true, false
- 空值，null
- 数组 (list)，用空格或逗号作分隔符，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
- maps, 相当于 JavaScript 的 object，(key1: value1, key2: value2)

 

例如

```scss
$layer-index:10;
$border-width:3px;
$font-base-family:'Open Sans', Helvetica, Sans-Serif;
$top-bg-color:rgba(255,147,29,0.6);
$block-base-padding:6px 10px 6px 10px;
$blank-mode:true;
$var:null; // 值null是其类型的唯一值。它表示缺少值，通常由函数返回以指示缺少结果。
$color-map: (color1: #fa0000, color2: #fbe200, color3: #95d7eb);

$fonts: (serif: "Helvetica Neue",monospace: "Consolas");
```

使用

```scss
.container {
    $font-size: 16px !global;
    font-size: $font-size;
    @if $blank-mode {
        background-color: #000;
    }
    @else {
        background-color: #fff;
    }
    content: type-of($var);
    content:length($var);
    color: map-get($color-map, color2);
}

.footer {
    font-size: $font-size;
}

// 如果列表中包含空值，则生成的CSS中将忽略该空值。
.wrap {
    font: 18px bold map-get($fonts, "sans");
}
```

 

## 默认值

```scss
$color:#333;
// 如果$color之前没定义就使用如下的默认值
$color:#666 !default;
.container {
    border-color: $color;
}
```

 

------

# @import

 

## @import

Sass 拓展了 @import 的功能，允许其导入 SCSS 或 Sass 文件。被导入的文件将合并编译到同一个 CSS 文件中，另外，被导入的文件中所包含的变量或者混合指令 (mixin) 都可以在导入的文件中使用。

**例如**

public.scss

```scss
$font-base-color:#333;
```

在index.scss里面使用

```scss
@import "public";
$color:#666;
.container {
    border-color: $color;
    color: $font-base-color;
}
```

**注意：**跟我们普通css里面@import的区别

 

##### 如下几种方式，都将作为普通的 CSS 语句，不会导入任何 Sass 文件

1. 文件拓展名是 .css；
2. 文件名以 http:// 开头；
3. 文件名是 url()；
4. @import 包含 media queries。

```scss
@import "public.css";
@import url(public);
@import "http://xxx.com/xxx";
@import 'landscape' screen and (orientation:landscape);
```

 

 

## 局部文件(Partials)

Sass源文件中可以通过@import指令导入其他Sass源文件，被导入的文件就是***局部文件，局部文件让Sass模块化编写更加容易。

如果一个目录正在被Sass程序监测，目录下的所有scss/sass源文件都会被编译，但通常不希望局部文件被编译，因为局部文件是用来被导入到其他文件的。如果不想局部文件被编译，文件名可以以下划线 （_）开头

例如：

_theme.scss

```scss
$border-color:#999;
$background-color:#f2f2f2;
```

使用

```scss
@import "theme";
.container {
    border-color: $border-color;
    background-color: $background-color;
}
```

可以看到，@import 引入的*theme.scss，可以没有*，这是允许的，这也就意味着，同一个目录下不能同时出现两个相关名的sass文件（一个不带*，一个带*），添加下划线的文件将会被忽略。

 

 

## 嵌套 @import

大多数情况下，一般在文件的最外层（不在嵌套规则内）使用 @import，其实，也可以将 @import 嵌套进 CSS 样式或者 @media 中，与平时的用法效果相同，只是这样导入的样式只能出现在嵌套的层中。

例如

_base.scss

```scss
.main-color {
    color: #F00;
}
```

使用

```scss
.container {
    @import "base";
}
```

 

**注意：**@import不能嵌套使用在控制指令或混入中

 

------

# 混合指令 (Mixin Directives)

​	混合指令（Mixin）用于定义可重复使用的样式。混合指令可以包含所有的 CSS 规则，绝大部分 Sass 规则，甚至通过参数功能引入变量，输出多样化的样式。

 

## 定义与使用混合指令 @mixin

```scss
@mixin mixin-name() {
    /* css 声明 */
}
```

### 例1：标准形式

定义

```scss
// 定义页面一个区块基本的样式
@mixin block {
    width: 96%;
    margin-left: 2%;
    border-radius: 8px;
    border: 1px #f6f6f6 solid;
}
```

使用

```scss
// 使用混入
.container {
    .block {
        @include block;
    }
}
```

 

### 例2：嵌入选择器

例如

```scss
// 定义警告字体样式,下划线（_）与横线（-）是一样的
@mixin warning-text {
    .warn-text {
        font-size: 12px;
        color: rgb(255, 253, 123);
        line-height: 180%;
    }
}
```

使用

```scss
// 使用混入
.container {
    @include warning-text;
}
```

 

### 例3：使用变量

定义

```scss
// 定义flex布局元素纵轴的排列方式
@mixin flex-align($aitem) {
    -webkit-box-align: $aitem;
    -webkit-align-items: $aitem;
    -ms-flex-align: $aitem;
    align-items: $aitem;
}
```

使用

```scss
// 只有一个参数，直接传递参数
.container {
    @include flex-align(center);
}

// 给指定参数指定值
.footer {
    @include flex-align($aitem: center);
}
```

 

### 例4：使用变量（多参数）

例如

```scss
// 定义块元素内边距
@mixin block-padding($top, $right, $bottom, $left) {
    padding-top: $top;
    padding-right: $right;
    padding-bottom: $bottom;
    padding-left: $left;
}
```

使用一

```scss
// 按照参数顺序赋值
.container {
    @include block-padding(10px, 20px, 30px, 40px);
}
```

使用二

```scss
// 可指定参数赋值
.container {
    @include block-padding($left: 20px, $top: 10px, $bottom: 10px, $right: 30px);
}
```

使用三：只设置两边

```scss
// 可指定参数赋值
.container {
    @include block-padding($left: 10px, $top: 10px, $bottom: 0, $right: 0);
}
```

**问题：**必须指定4个值

 

### 例5：指定默认值

定义

```scss
// 定义块元素内边距，参数指定默认值
@mixin block-padding($top:0, $right:0, $bottom:0, $left:0) {
    padding-top: $top;
    padding-right: $right;
    padding-bottom: $bottom;
    padding-left: $left;
}
```

使用

```scss
// 可指定参数赋值
.container {
    // 不带参数
    //@include block-padding;
    //按顺序指定参数值
    //@include block-padding(10px,20px);
    //给指定参数指定值
    @include block-padding($left: 10px, $top: 20px)
}
```

### 例6：可变参数

参数不固定的情况

```scss
/** 
    *定义线性渐变
    *@param $direction  方向
    *@param $gradients  颜色过度的值列表
 */

@mixin linear-gradient($direction, $gradients...) {
    background-color: nth($gradients, 1);
    background-image: linear-gradient($direction, $gradients);
}
```

使用

```scss
.table-data {
    @include linear-gradient(to right, #F00, orange, yellow);
}
```

 

## @mixin混入总结

- mixin是可以重复使用的一组CSS声明
- mixin有助于减少重复代码，只需声明一次，就可在文件中引用
- 混合指令可以包含所有的 CSS 规则，绝大部分 Sass 规则，甚至通过参数功能引入变量，输出多样化的样式。
- 使用参数时建议加上默认值

 

**什么时候用？？？？** 很多地方都会用到却能根据不同场景灵活使用的样式

 

------

# @extend（继承）指令

​	在设计网页的时候通常遇到这样的情况：一个元素使用的样式与另一个元素完全相同，但又添加了额外的样式。通常会在 HTML 中给元素定义两个 class，一个通用样式，一个特殊样式。

![image-20211127090211485](data:image/png;base64,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)

 

## CSS案例

接下来以警告框为例进行讲解4种类型

| 标记    | 说明                     |
| ------- | ------------------------ |
| info    | 信息！请注意这个信息。   |
| success | 成功！很好地完成了提交。 |
| warning | 警告！请不要提交。       |
| danger  | 错误！请进行一些更改。   |

所有警告框的基本样式（风格、字体大小、内边距、边框等...） ，因为我们通常会定义一个通用alert样式

```scss
.alert {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 12px;
}
```

不同警告框单独风格

```scss
.alert-info {
    color: #31708f;
    background-color: #d9edf7;
    border-color: #bce8f1;
}

.alert-success {
    color: #3c763d;
    background-color: #dff0d8;
    border-color: #d6e9c6;
}

.alert-warning {
    color: #8a6d3b;
    background-color: #fcf8e3;
    border-color: #faebcc;
}

.alert-danger {
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}
```

使用

```scss
<div class="alert alert-info">
    信息！请注意这个信息。
</div>

<div class="alert alert-success">
    成功！很好地完成了提交。
</div>

<div class="alert alert-warning">
    警告！请不要提交。
</div>

<div class="alert alert-danger">
    错误！请进行一些更改。
</div>
```

 

## 使用继承@extend改进

基本样式我们没有变，主要是各个警告框单独的样式

```scss
.alert-info {
    @extend .alert;
    color: #31708f;
    background-color: #d9edf7;
    border-color: #bce8f1;
}

.alert-success {
    @extend .alert;
    color: #3c763d;
    background-color: #dff0d8;
    border-color: #d6e9c6;
}

.alert-warning {
    @extend .alert;
    color: #8a6d3b;
    background-color: #fcf8e3;
    border-color: #faebcc;
}

.alert-danger {
    @extend .alert;
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}
```

使用时就不须要再写基本类了

```scss
<div class="alert-info">
    信息！请注意这个信息。
</div>

<div class="alert-success">
    成功！很好地完成了提交。
</div>

<div class="alert-warning">
    警告！请不要提交。
</div>

<div class="alert-danger">
    错误！请进行一些更改。
</div>
```

 

## 使用多个@extend

定义两个类

```scss
.alert {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 12px;
}

.important {
    font-weight: bold;
    font-size: 14px;
}
```

使用

```scss
.alert-danger {
    @extend .alert;
    @extend .important;
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}
```

 

## @extend多层继承

第一层继承

```scss
.alert {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 12px;
}

.important {
    @extend .alert;
    font-weight: bold;
    font-size: 14px;
}
```

再继承

```scss
.alert-danger {
    @extend .important;
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}
```

 

## 占位符%

你可能发现被继承的css父类并没有被实际应用，也就是说html代码中没有使用该类，它的唯一目的就是扩展其他选择器。

对于该类，可能不希望被编译输出到最终的css文件中，它只会增加CSS文件的大小，永远不会被使用。

这就是占位符选择器的作用。

占位符选择器类似于类选择器，但是，它们不是以句点(.)开头，而是以百分号(%)开头。

当在Sass文件中使用占位符选择器时，它可以用于扩展其他选择器，但不会被编译成最终的CSS。

改写

```scss
%alert {
    padding: 15px;
    margin-bottom: 20px;
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: 12px;
}

.alert-info {
    @extend %alert;
    color: #31708f;
    background-color: #d9edf7;
    border-color: #bce8f1;
}

.alert-success {
    @extend %alert;
    color: #3c763d;
    background-color: #dff0d8;
    border-color: #d6e9c6;
}

.alert-warning {
    @extend %alert;
    color: #8a6d3b;
    background-color: #fcf8e3;
    border-color: #faebcc;
}

.alert-danger {
    @extend %alert;
    color: #a94442;
    background-color: #f2dede;
    border-color: #ebccd1;
}
```

还可以用@mixin，@import等实现并改进

@import      导入局部模块化样式（类似功能、同一组件）	  多次导入

@minix	  定义可重复使用的样式

 

------

#  运算符 (Operations)

 

## 等号操作符

所有数据类型都支持等号运算符:

| 符号 | 说明   |
| ---- | ------ |
| ==   | 等于   |
| !=   | 不等于 |

例1数字比较：

```scss
$theme:1;
.container {
    @if $theme==1 {
        background-color: red;
    }
    @else {
        background-color: blue;
    }
}
```

例2字符串比较：

```scss
$theme:"blue";
.container {
    @if $theme !="blue" {
        background-color: red;
    }
    @else {
        background-color: blue;
    }
}
```

所有数据类型均支持相等运算  或 ，此外，每种数据类型也有其各自支持的运算方式。`==``!=`

 

## 关系（比较）运行符

 

| 符号       | 说明     |
| ---------- | -------- |
| < （lt）   | 小于     |
| > （gt）   | 大于     |
| <= （lte） | 小于等于 |
| >= （gte） | 大于等于 |

例

```
$theme:3;
.container {
    @if $theme >= 5 {
        background-color: red;
    }
    @else {
        background-color: blue;
    }
}
```

## 逻辑运行符

 

| 符号 | 说明   |
| ---- | ------ |
| and  | 逻辑与 |
| or   | 逻辑或 |
| not  | 逻辑非 |

例

```scss
$width:100;
$height:200;
$last:false;
div {
    @if $width>50 and $height<300 {
        font-size: 16px;
    }
    @else {
        font-size: 14px;
    }
    @if not $last {
        border-color: red;
    }
    @else {
        border-color: blue;
    }
}
```

 

## 数字操作符

| 符号 | 说明 |
| ---- | ---- |
| +    | 加   |
| -    | 减   |
| *    | 乘   |
| /    | 除   |
| %    | 取模 |

例

```
/* 
    +、-、*、/、%
    线数字、百分号、css部分单位（px、pt、in...）
    +
    线数字与百分号或单位运算时会自动转化成相应的百分比与单位值
*/
.container {
    /* ==================+ 运算===================== */
    width: 50 + 20;
    width: 50 + 20%;
    width: 50% + 20%;
    width: 10px + 20px;
    width: 10pt + 20px;
    width: 10pt + 20;
    width: 10px + 10;
    /* ==================- 运算===================== */
    height: 50 - 30;
    height: 10 - 30%;
    height: 60% - 30%;
    height: 50px - 20px;
    height: 50pt - 20px;
    height: 50pt - 40;
    /* ==================* 运算===================== */
    height: 50 * 30;
    height: 10 * 30%;
    /* height: 60% * 30%; 出现了两个百分号*/
    /* height: 50px * 20px; 出现了两个单位*/
    height: 50 * 2px;
    height: 50pt * 4;
    /* ==================/运算 (除完后最多只能保留一种单位)===================== */
    $width: 100px;
    width: 10 / 5;
    width: 10px / 5px;
    width: 10px / 10 * 2;
    width: 20px / 2px * 5%;
    width: ($width/2); // 使用变量与括号
    z-index: round(10)/2; // 使用了函数
    height: (500px/2); // 使用了括号
    /* ==================% 运算===================== */
    width: 10 % 3;
    width: 50 % 3px;
    width: 50px % 4px;
    width: 50px % 7;
    width: 50% % 7;
    width: 50% % 9%;
    width: 50px % 10pt; // 50px % 13.33333px  
    width: 50px % 13.33333px;
    width: 50px + 10pt;
    /* width: 50px % 5%; 单位不统一*/
}
```

 

/ 在 CSS 中通常起到分隔数字的用途，SassScript 作为 CSS 语言的拓展当然也支持这个功能，同时也赋予了 / 除法运算的功能。也就是说，如果 / 在 SassScript 中把两个数字分隔，编译后的 CSS 文件中也是同样的作用。

 

**以下三种情况 / 将被视为除法运算符号：**

- 如果值或值的一部分，是变量或者函数的返回值
- 如果值被圆括号包裹
- 如果值是算数表达式的一部分

 

例

```scss
$width: 1000px;
div {
    font: 16px/30px Arial, Helvetica, sans-serif; // 不运算
    width: ($width/2); // 使用变量与括号
    z-index: round(10)/2; // 使用了函数
    height: (500px/2); // 使用了括号
    margin-left: 5px + 8px/2px; // 使用了+表达式
}
```

如果需要使用变量，同时又要确保 / 不做除法运算而是完整地编译到 CSS 文件中，只需要用 #{} 插值语句将变量包裹。

 

## 字符串运算

`+` 可用于连接字符串

**注意**：如果有引号字符串（位于 + 左侧）连接无引号字符串，运算结果是有引号的，相反，无引号字符串（位于 + 左侧）连接有引号字符串，运算结果则没有引号。

有问题？？？？   如果有一个值是函数返回的，情况可能不一样

例如

```scss
.container {
    content: "Foo " + Bar;
    font-family: sans- + "serif";
}
```

 

------

# SASS 插值语句 #{ }

引入之前的案例发出一个问题

例如

```scss
p{
    font: 16px/30px Arial, Helvetica, sans-serif; 
}
```

如果需要使用变量，同时又要确保 / 不做除法运算而是完整地编译到 CSS 文件中，只需要用 #{} 插值语句将变量包裹。

使用插值语法：

```scss
p {
    $font-size: 12px;
    $line-height: 30px;
    font: #{$font-size}/#{$line-height} Helvetica,
    sans-serif;
}
```

通过  插值语句可以在选择器、属性名、注释中使用变量：`#{}`

```scss
$class-name: danger;
$attr: color;
$author:'老姚';

/* 
   * 这是文件的说明部分
    @author: #{\$author}
 */

a.#{$class-name} {
    border-#{$attr}: #F00;
}
```

------

# 常见函数

 

常见函数简介，更多函数列表可看：https://sass-lang.com/documentation/modules 

 

## Color(颜色函数)

sass包含很多操作颜色的函数。例如：lighten() 与 darken()函数可用于调亮或调暗颜色，opacify()函数使颜色透明度减少，transparent()函数使颜色透明度增加，mix()函数可用来混合两种颜色。

```scss
p {
    height: 30px;
}

.p0 {
    background-color: #5c7a29;
}

.p1 {
    /* 
        让颜色变亮
        lighten($color, $amount)
        $amount 的取值在0% - 100% 之间
     */
    background-color: lighten(#5c7a29, 30%);
}

.p2 {
    // 让颜色变暗  通常使用color.scale()代替该方案
    background-color: darken(#5c7a29, 15%);
}

.p3 {
    // 降低颜色透明度  通常使用color.scale()代替该方案
    // background-color: opacify(#5c7a29,0.5);
    background-color: opacify(rgba(#5c7a29, 0.1), 0.5);
}
```

使用

```scss
<p></p>
<p class="p0"></p>
<p class="p1"></p>
<p class="p2"></p>
<p class="p3"></p>
```

 

## String（字符串函数）

Sass有许多处理字符串的函数，比如向字符串添加引号的quote()、获取字符串长度的string-length()和将内容插入字符串给定位置的string-insert()。

例

```scss
p {
    &:after {
        content: quote(这是里面的内容);
    }
    background-color: unquote($string: "#F00");
    z-index:str-length("sass学习");
}
```

 

## Math(数值函数)

数值函数处理数值计算，例如：percentage()将无单元的数值转换为百分比，round()将数字四舍五入为最接近的整数，min()和max()获取几个数字中的最小值或最大值，random()返回一个随机数。

例如

```scss
p {
    z-index: abs($number: -15); // 15
    z-index: ceil(5.8); //6
    z-index: max(5, 1, 6, 8, 3); //8
    opacity: random(); // 随机 0-1
}
```

 

## List函数

List函数操作List，length()返回列表长度，nth()返回列表中的特定项，join()将两个列表连接在一起，append()在列表末尾添加一个值。

例如：

```scss
p {
    z-index: length(12px); //1
    z-index: length(12px 5px 8px); //3
    z-index: index(a b c d, c); //3
    padding: append(10px 20px, 30px); // 10px 20px 30px
    color: nth($list: red blue green, $n: 2); // blue
}
```

 

## Map函数

Map函数操作Map，map-get()根据键值获取map中的对应值，map-merge()来将两个map合并成一个新的map，map-values()映射中的所有值。

```scss
$font-sizes: ("small": 12px, "normal": 18px, "large": 24px);
$padding:(top:10px, right:20px, bottom:10px, left:30px);
p {
    font-size: map-get($font-sizes, "normal"); //18px
    @if map-has-key($padding, "right") {
        padding-right: map-get($padding, "right");
    }
    &:after {
        content: map-keys($font-sizes) + " "+ map-values($padding) + "";
    }
}
```

 

## selector选择器函数

​	选择符相关函数可对CSS选择进行一些相应的操作，例如：selector-append()可以把一个选择符附加到另一个选择符，selector-unify()将两组选择器合成一个复合选择器。

例如

```scss
.header {
    background-color: #000;
    content: selector-append(".a", ".b", ".c") + '';
    content: selector-unify("a", ".disabled") + '';
}
```

 

## 自检函数

​	自检相关函数，例如：feature-exists()检查当前Sass版本是否存在某个特性，variable-exists()检查当前作用域中是否存在某个变量，mixin-exists()检查某个mixin是否存在。

例如：

```scss
$color:#F00;
@mixin padding($left:0, $top:0, $right:0, $bottom:0) {
    padding: $top $right $bottom $left;
}

.container {
    @if variable-exists(color) {
        color: $color;
    }
    @else {
        content: "$color不存在";
    }
    @if mixin-exists(padding) {
        @include padding($left: 10px, $right: 10px);
    }
}
```

自检函数通常用在代码的调试上

------

# 流程控制指令@if、@for、@each、@while

 

## @if控制指令

@if()函数允许您根据条件进行分支，并仅返回两种可能结果中的一种。

语法方式同js的if....else if ...else

看流程图

代码形式：

```scss
.container{
    // 第一种
    @if(/* 条件 */){
        // ...
    }

    // 第二种
    @if(/* 条件 */){
        // ...
    }@else{
        // ...
    }
    
    // 第三种
    @if(/* 条件 */){
        // ...
    }@else if(){
        // ...
    }@else{
        // ...
    }
}
```

例1

```scss
$theme:"green";
.container {
    @if $theme=="red" {
        color: red;
    }
    @else if $theme=="blue" {
        color: blue;
    }
    @else if $theme=="green" {
        color: green;
    }
    @else {
        color: darkgray;
    }
}
```

例如，定义一个css的三角形@mixin声明

```scss
@mixin triangle($direction:top, $size:30px, $border-color:black) {
    width: 0px;
    height: 0px;
    display: inline-block;
    border-width: $size;
    border-#{$direction}-width: 0;
    @if ($direction==top) {
        border-color: transparent transparent $border-color transparent;
        border-style: dashed dashed solid dashed;
    }
    @else if($direction==right) {
        border-color: transparent transparent transparent $border-color;
        border-style: dashed dashed dashed solid;
    }
    @else if($direction==bottom) {
        border-color: $border-color transparent transparent transparent;
        border-style: solid dashed dashed dashed;
    }
    @else if($direction==left) {
        border-color: transparent $border-color transparent transparent;
        border-style: dashed solid dashed dashed;
    }
}
```

使用

```scss
.p0 {
    @include triangle();
}

.p1 {
    @include triangle(right, 50px, red);
}

.p2 {
    @include triangle(bottom, 50px, blue);
}

.p3 {
    @include triangle(left, 50px, green);
}
```

html

```scss
<p class="p0"></p>
<p class="p1"></p>
<p class="p2"></p>
<p class="p3"></p>
```

 

## @for指令

@for 指令可以在限制的范围内重复输出格式，每次按要求（变量的值）对输出结果做出变动。这个指令包含两种格式：@for $var from  through ，或者 @for $var from  to 

区别在于 through 与 to 的含义：

- 当使用`through`时，条件范围包含与的值。
- 而使用`to` 时条件范围只包含的值不包含  的值。
- 另外，$var 可以是任何变量，比如 $i； 和  必须是整数值。

例1

```scss
@for $i from 1 to 4 {
    .p#{$i} {
        width: 10px * $i;
        height: 30px;
        background-color: red;
    }
}

@for $i from 1 through 3 {
    .p#{$i} {
        width: 10px * $i;
        height: 30px;
        background-color: red;
    }
}
```

使用

```scss
<p class="p1"></p>
<p class="p2"></p>
<p class="p3"></p>
```

 

例2：加载动画

```scss
#loading {
    position: fixed;
    top: 200px;
    left: 46%;
}

#loading span {
    position: absolute;
    width: 20px;
    height: 20px;
    background: #3498db;
    opacity: 0.5;
    border-radius: 50%;
    animation: loading 1s infinite ease-in-out;
}

#loading span:nth-child(1) {
    left: 0;
    animation-delay: 0s;
}

#loading span:nth-child(2) {
    left: 20px;
    animation-delay: 0.2s;
}

#loading span:nth-child(3) {
    left: 40px;
    animation-delay: 0.4s;
}

#loading span:nth-child(4) {
    left: 60px;
    animation-delay: 0.6s;
}

#loading span:nth-child(5) {
    left: 80px;
    animation-delay: .8s;
}

@keyframes loading {
    0% {
        opacity: 0.3;
        transform: translateY(0px);
    }
    50% {
        opacity: 1;
        transform: translateY(-20px);
        background: green;
    }
    100% {
        opacity: 0.3;
        transform: translateY(0px);
    }
}
```

html

```scss
<div id="loading">
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
</div>
```

用@for改进动画部分

```scss
@for $i from 1 to 5 {
    #loading span:nth-child(#{$i}) {
        left: 20 * ($i - 1) + px;
        /* animation-delay: 20 * ($i - 1) / 100 + s; */
        animation-delay: unquote($string: "0.") + ($i - 1) * 2 + s;
    }
}
```

 

## @each指令

@each 指令的格式是 `$var in , $var `可以是任何变量名，比如 `$length `或者 `$name`，而  是一连串的值，也就是值列表。

例如做如下效果

![image-20211129101633273](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANcAAABICAIAAADqL/d0AAAACXBIWXMAAA7EAAAOxAGVKw4bAAABEElEQVR4nO3csQ3CMBBAUYzYgxGYgZ3SI3p2YgZGYBKzQQpk8Snea0/RJdKXKytjznmA1LF+AVAhf0CF9FRIT4X0VEhPhfRUSE+F9FRIT4X0VEhPhfRUSE+F9FRI7/TNQ2PsTdddmx33vUXztmbRr77mcH2/dqbP82XVosd47Ey3ua1atIqzkJ4K6amQngrpqZCeCumpkJ4K6amQ3vCHEHLOQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpKdCeiqkp0J6KqSnQnoqpPcBXV4Ti47PTEgAAAAASUVORK5CYII=)

 

普通CSS的写法

```scss
p{
    width: 10px;
    height: 10px;
    display: inline-block;
    margin: 10px;
}
.p0{
    background-color: red;
}
.p1{
    background-color: green;
}
.p2{
    background-color: blue;
}

.p3{
    background-color:turquoise;
}

.p4{
    background-color: darkmagenta;
}
```

 

用@each改进

```scss
$color-list:red green blue turquoise darkmagenta;
@each $color in $color-list {
    $index: index($color-list, $color);
    .p#{$index - 1} {
        background-color: $color;
    }
}
```

 

## @while 指令

@while 指令重复输出格式直到表达式返回结果为 false。这样可以实现比 @for 更复杂的循环。

用sass实现bootstrap中css的这么一段代码

https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.css 

```scss
.col-sm-12 {
    width: 100%;
}

.col-sm-11 {
    width: 91.66666667%;
}

.col-sm-10 {
    width: 83.33333333%;
}

.col-sm-9 {
    width: 75%;
}

.col-sm-8 {
    width: 66.66666667%;
}

.col-sm-7 {
    width: 58.33333333%;
}

.col-sm-6 {
    width: 50%;
}

.col-sm-5 {
    width: 41.66666667%;
}

.col-sm-4 {
    width: 33.33333333%;
}

.col-sm-3 {
    width: 25%;
}

.col-sm-2 {
    width: 16.66666667%;
}

.col-sm-1 {
    width: 8.33333333%;
}
```

用@while实现

```scss
$column:12;
@while $column>0 {
    .col-sm-#{$column} {
        width: $column / 12 * 100%;
        // width: $column / 12 * 100 + %; 会标红
        width: $column / 12 * 100#{"%"};
        width: unquote($string: $column / 12 * 100 + "%");
    }
    $column:$column - 1;
}
```

 
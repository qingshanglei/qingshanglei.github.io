# 简介：

​        ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

​       ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 的实现还有 Jscript 和 ActionScript）





# 其他：

## reduce:

```js
    对象.reduce((上一次值，当前值)=>{ console.log("@"); },默认值) //对象多少个调用多少次。

return this.todos.reduce((pre, todo) => {
  return pre + (todo.done ? 1 : 0);
}, 0);
```



```vue
<input type="checkbox" :checked="isAll" @click="checkedAll" />
<input type="text"  @click="checkedAll"/>
<script>
    methods: {
        //方法区
        checkedAll(e) {
            console.log(e.target.checked) //获取复选框的true或false
            console.log(e.target.value) //获取input的值
        },
    },
    };
</script>
```

## 模板字符串：

```js
` ${}`  //模板字符串   

```

## 对象合并：

​     对象合并：有则覆盖，无则合并。

```js
var date1 = { a: 1, b: 2, c: 3 };
var date2 = { a: 4, b: 5, f: 6, g: 7 };
// 对象合并：有则覆盖，无则合并。       
var date3 = { ...this.date1, ...date2 }; //date1被date2合并  
console.log(date3);
```

## 解构运算符-对象:

```js
var obj = { x: 100, y: 200 };
var obj2 = {
    a: 1,
    ...obj, //解开obj对象
    b: 2
};
console.log(obj2)
```

## 解构赋值：

### 单个解构赋值：



### 连续解构赋值：

```js
VueComponent{ //原数据
    ...
    $route:{
        ...
        params:{
            id: "003",
            title: "消息003"
        }
    }
   ...
}

props({params:{id,title}}) { //连续解构赋值
    return {
        id,title
    }
}
```





# 模块化：

​       Javascript不是一种模块化编程语言，它不支持"类"（class），包（package）等概念，更遑论"模块"（module）了。



## js模块化：

  01.js:

```js
// 方法
function sum(a,b){
    return a+b;
}
function sub(m,n){
    return m-n;
}

// 默认暴露
module.exports={
    sum,sub
}
```

 02.js:

```js
// 引入调用方法所在的js文件   注意：js尾缀可不写
var m=require("./01.js")

// 调用01文件方法
var v1=m.sum(3,3);
console.log(v1);

var v1=m.sub(2,1);
console.log(v1);
```

运行模块：

```shell
node .\02.js   # 运行02.js文件
```









## Es6模块化：

### 方式1：

  01.js:

```js

// 默认暴露
export function list(){
    console.log(" list....")
}

export function add(){
    console.log(" add....")
}
```

02.js:

```js
// 引入
import { list,add } from "./01";

// 调用01文件的方法
list();
add();
```

运行Es6模块化：

  注意：Es6模块化无法在Node.js中执行，必须转成Es5后才能运行。

```sh
node .\02.js   # 运行02.js文件
```



### 方式2：

01.js

```js
// ES6写法2
// 默认暴露
export default{
    getList() {
        console.log('getList...')
    },
    save() {
        console.log('save...')
    }
}
```

02.js

```js
// 引入
import m from "./01.js";

m.getList();
m.save();
```

转成Es5后运行：

```shell
node 02.js  # 运行02.js文件
```





## Es6模块化运行报错：

 注意：ES6的模块化无法在Node.js中执行，需要用Babel编辑成ES5后再执行。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/ES6/Es6%E6%A8%A1%E5%9D%97%E5%8C%96%E8%BF%90%E8%A1%8C%E6%8A%A5%E9%94%99.png)



# Babel编译器：

​    Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。

   Babel提供babel-cli工具，用于命令行转码。

## 命令行安装:

```shell
npm install --global babel-cli   # 局部安装

babel --version #  查看是否安装成功
```



## 安装Babel编译器报错:

   解决安装Babel编译器报错：
​     ①在电脑中直接搜索PowerShell 以管理员方式运行
​     ②执行 set-ExecutionPolicy RemoteSigned，输入y敲回车

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/ES6/%E5%AE%89%E8%A3%85Babel%E7%BC%96%E8%AF%91%E5%99%A8%E6%8A%A5%E9%94%99.png)



## 项目配置.babelrc：

   Babel的配置文件是.babelrc，存放在项目的根目录下，该文件用来设置转码规则和插件，presets字段设定转码规则，将es2015规则加入 .babelrc：

```js
{
    "presets": ["es2015"],
    "plugins": []
}
```



## 安装转码器:

### 在项目中安装

```shell
npm install --save-dev babel-preset-es2015
```

### 转码

```shell
# 创建目录
mkdir 目录名 
# --out-dir 或 -d 参数指定输出目录
babel Es6文件 -d Es5文件  # Es6转码Es5

```






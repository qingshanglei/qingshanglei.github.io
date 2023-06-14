# Vue3介绍：

<img src="https://user-images.githubusercontent.com/499550/93624428-53932780-f9ae-11ea-8d16-af949e16a09f.png" style="width:200px" />



## 1.Vue3简介

- 2020年9月18日，Vue.js发布3.0版本，代号：One Piece（海贼王）
- 耗时2年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99位贡献者](https://github.com/vuejs/vue-next/graphs/contributors) 
- github上的tags地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0

## 2.Vue3带来了什么

### 1.性能的提升

- 打包大小减少41%

- 初次渲染快55%, 更新渲染快133%

- 内存减少54%

  ......

### 2.源码的升级

- 使用Proxy代替defineProperty实现响应式

- 重写虚拟DOM的实现和Tree-Shaking

  ......

### 3.拥抱TypeScript

- Vue3可以更好的支持TypeScript

### 4.新的特性

1. Composition API（组合API）

   - setup配置
   - ref与reactive
   - watch与watchEffect
   - provide与inject
   - ......
2. 新的内置组件
   - Fragment 
   - Teleport
   - Suspense
3. 其他改变

   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除keyCode支持作为 v-on 的修饰符
   - ......

# 创建Vue3.0工程

## 1.使用 vue-cli 创建

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

## 2.使用 vite 创建

官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite官网：https://vitejs.cn

- 什么是vite？—— 新一代前端构建工具。 （现在的前端构建工具是：webpack）
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。
- 传统构建 与 vite构建对比图

<div > 
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/%E4%BC%A0%E7%BB%9F%E6%9E%84%E5%BB%BA%20%E4%B8%8E%20vite%E6%9E%84%E5%BB%BA%E5%AF%B9%E6%AF%94%E5%9B%BE1.png" style="width:374px;height:280px; float:left" />
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/%E4%BC%A0%E7%BB%9F%E6%9E%84%E5%BB%BA%20%E4%B8%8E%20vite%E6%9E%84%E5%BB%BA%E5%AF%B9%E6%AF%94%E5%9B%BE2.png"  style="width:480px; height:280px;" />  </div>


```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

# 常用 Composition API

官方文档: https://v3.cn.vuejs.org/guide/composition-api-introduction.html

## setup:


   1.拉开序幕的setup
1. 理解：Vue3.0中一个新的配置项，值为一个函数。

2. setup是所有<strong style="color:#DD5145">Composition API（组合API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。

3. 组件中所用到的：数据、方法等等，均要配置在setup中。

5. setup函数的两种返回值：
   1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
   2. <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span>
   
5. 注意点：
   1. 尽量不要与Vue2.x配置混用
      - Vue2.x配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup中的属性、方法。
      - 但在setup中<strong style="color:#DD5145">不能访问到</strong>Vue2.x配置（data、methos、computed...）。
      - 如果有重名, setup优先。
   2. setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步引入的配合）

   


### 返回值：

#### setup对象返回值：

App组件：

```vue
<template>
  <h1>一个人的信息：</h1>
  <h2>姓名:{{name}}</h2>
  <h2>年龄:{{age}}</h2>
  <button @click="sayHello">自我介绍</button>
</template>

<script>
export default {
  name: "App",
  //注意：①组件中所用到的：数据、方法等等，均要配置在setup中。
  //     ②此处只是测试一下setup，暂时不考虑响应式的问题。
  setup() {
    //数据
    let name = "张三";
    let age = 23;

    //方法
    function sayHello() {
      alert(`我叫${name},我${age}岁了，你好啊！`);
    }

   //setup函数返回值-对象
    return {
      name,
      age,
      sayHello,
    };
  },
};
</script>

```

#### 渲染函数返回值：

```vue
<template>
  <h1>一个人的信息：</h1>
  <h2>姓名:{{name}}</h2>
  <h2>年龄:{{age}}</h2>
  <button @click="sayHello">自我介绍</button>
</template>

<script>
import {h} from 'vue'
export default {
  name: "App",
  //注意：①组件中所用到的：数据、方法等等，均要配置在setup中。
  //     ②此处只是测试一下setup，暂时不考虑响应式的问题。
  setup() { //setup比beforeCreate要创建早
    //数据
    let name = "张三";
    let age = 23;

    //方法
    function sayHello() {
      alert(`我叫${name},我${age}岁了，你好啊！`);
    }

   //setup函数返回值2-渲染函数 (注意：渲染函数会把html标签的替代)
   return()=>(h('h1','尚硅谷'))
  },
};
</script>
```



#### toRef、toRefs(简写返回值)：

- 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
- 语法：```const name = toRef(person,'name')```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```

```vue
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>工资: {{ salary }}</h2>
</template>

<script>
import { reactive, toRef } from "vue";
export default {
  name: "Demo",
  //注意：①组件中所用到的：数据、方法等，都要配置在setup中。
  setup() {
    //数据
    let person = reactive({
      name: "张三",
      age: 18,
      obj: {
        j1: {
          salary: 5000,
        },
      },
    });

    const name = toRef(person, "name");
    console.log('@@@@',name);
    //setup函数返回值-对象
    return {
      person,
      // 方法1: toRef
      name: toRef(person, "name"),
      age: toRef(person, "age"),
      salary: toRef(person.obj.j1, "salary"),
        
       // 方法2: toRefs——解构赋值_对象
      ...toRefs(person), //解开person对象  
    };
  },
};
</script>
```



### 数据：

####  ref函数：

- 作用: 定义一个响应式的数据
- 语法: ```const xxx = ref(initValue)``` 
  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference对象，简称ref对象）</strong>。
  - JS中操作数据： ```xxx.value```
  - 模板中读取数据: 不需要.value，直接：```<div>{{xxx}}</div>```
- 备注：
  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠``Object.defineProperty()``的```get```与```set```完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了Vue3.0中的一个新函数—— ```reactive```函数。

App组件：

```vue
<template>
  <h1>一个人的信息：</h1>
  <h2>姓名:{{ name }}</h2>
  <h2>年龄:{{ age }}</h2>
  <h3>工作:{{ job.type }}</h3>
  <h3>薪资:{{ job.salary }}</h3>
  <button @click="changeInfo">修改人的信息</button>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  //注意：①组件中所用到的：数据、方法等等，均要配置在setup中。
  setup() {
    //数据
    let name = ref("张三"); //字符串
    let age = ref(23); //数值
    let job = ref({   //对象
      type: "前端工程师",
      salary: "30k",
    });

    //方法
    function changeInfo() {
      name.value = "李四";
      age.value = 18;
      job.value.type = "UI设置师";
      job.value.salary = "50k";
      console.log(name, age);
    }

    //setup函数返回值-对象
    return { name, age,job, changeInfo };
  },
};
</script>
```



#### reactive函数：

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用```ref```函数）
- 语法：```const 代理对象= reactive(源对象)```接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy的实例对象，简称proxy对象）</strong>
- reactive定义的响应式数据是“深层次的”。
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

App组件：

```vue
<template>
  <h1>一个人的信息：</h1>
  <h2>姓名:{{ job.name }}</h2>
  <h2>年龄:{{ job.age }}</h2>
  <h2 v-show="job.sex">年龄:{{ job.sex }}</h2>
  <h3>工作:{{ job.type }}</h3>
  <h3>爱好:{{ job.hobby }}</h3>
  <h3>测试对象嵌套c:{{ job.a.b.c }}</h3>
  <h3>薪资:{{ job.salary }}</h3>
  <button @click="changeInfo">修改人的信息</button>
  <button @click="addSex">添加一个sex属性</button>
  <button @click="delName">删除Name属性</button>
</template>

<script>
import { ref, reactive } from "vue";
export default {
  name: "App",
  //注意：①组件中所用到的：数据、方法等等，均要配置在setup中。
  setup() {
    //数据
    /* 作用: 定义一个对象类型的响应式数据（基本类型不要用它，要用ref函数） */
    let job = reactive({
      name: "张三",
      age: 23,
      type: "前端工程师",
      salary: "30k",
      a: {
        b: {
          c: 666,
        },
      },
      hobby: ["抽烟", "喝酒", "烫头"],
    });

    //方法
    function changeInfo() {
      job.name = "李四";
      job.age = 18;
      job.type = "UI设置师";
      job.salary = "50k";
      job.hobby[0] = "学习";
      console.log(job.name, job.age);
    }
    function addSex() {
      job.sex='男';
    }

    function delName() {
      delete job.name;
    }

    //setup函数返回值-对象
    return { job, changeInfo,addSex,delName };
  },
};
</script>
```



#### reactive对比ref：

- 从定义数据角度对比：

  -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
  -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
  -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。

- 从原理角度对比：

  -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
  -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。

- 从使用角度对比：

  -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
  -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

  

### 2参数：

#### App组件：

```vue
<template>
  <Demo @hello="showHello" msg="你好啊" school="尚硅谷"> 
    <!--  插槽-定义插槽
      注意：Vue3的具名插槽只能使用v-slot插槽。 -->
    <template v-slot:qwe>
      <div>尚硅谷</div>
    </template>
  </Demo>
</template>

<script>
import Demo from "./components/Demo.vue";
export default {
  name: "App",
  components: { Demo }, //注册组件
  setup(props, context) {
    function showHello(val) {
      alert(`你好啊,你触发了hello事件,参数为${val}`);
    }

    return {showHello}
  },
};
</script>
```

#### Demo组件：

```vue
<template>
  <h1>一个人的信息：</h1>
  <h2>姓名:{{ job.name }}</h2>
  <h2>年龄:{{ job.age }}</h2>
  <button @click="Test">测试Demo的test事件</button>
 
  <!-- 插槽-使用插槽  -->
  <slot name="qwe">插槽</slot>
</template>

<script>
import { ref, reactive } from "vue";
export default {
  name: "Demo",
  //注意：①组件中所用到的：数据、方法等等，均要配置在setup中。
  beforeCreate() {
    //创建前
    console.log("-----beforeCreate---------");
  },
  props: ["msg", "school"],
  emits:['hello'],// 去除App组件内hello事件的警告
  setup(props, context) {// props,上下文
    //数据
    console.log("-------setup-------", props); //setup比beforeCreate更早注册
    /*  context：上下文  
           attrs: 详情与Vue2中的$attrs
           emit: 触发自定义事件
           slots:插槽   (注意：Vue3的具名插槽只能使用v-slot插槽。)  */
      console.log('-------setup-------',context.slots)

    /* 作用: 定义一个对象类型的响应式数据（基本类型不要用它，要用ref函数） */
    let job = reactive({
      name: "张三",
      age: 23,
    });

    //方法
    function Test() {
        context.emit('hello',666);
    }
    //setup函数返回值-对象
    return { job,Test };
  },
};
</script>
```





## 4.Vue3.0中的响应式原理

### vue2.x的响应式

- 实现原理：
  - 对象类型：通过```Object.defineProperty()```对属性的读取、修改进行拦截（数据劫持）。
  
  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。
  
    ```js
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```

- 存在问题：
  - 新增属性、删除属性, 界面不会更新。
  - 直接通过下标修改数组, 界面不会自动更新。

### Vue3.0的响应式

- 实现原理: 
  - 通过Proxy（代理）:  拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
  - 通过Reflect（反射）:  对源对象的属性进行操作。
  - MDN文档中描述的Proxy与Reflect：
    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
    
    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
    
      ```js
      new Proxy(data, {
      	// 拦截读取属性值
          get (target, prop) {
          	return Reflect.get(target, prop)
          },
          // 拦截设置属性值或添加新属性
          set (target, prop, value) {
          	return Reflect.set(target, prop, value)
          },
          // 拦截删除属性
          deleteProperty (target, prop) {
          	return Reflect.deleteProperty(target, prop)
          }
      })
      
      proxy.name = 'tom'   
      ```

## setup的两个注意点：

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。
  
- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。

## 计算属性与监视：

### 1.computed函数(计算属性)

- 与Vue2.x中computed配置功能一致

- 写法

  ```js
  import {computed} from 'vue'
  
  setup(){
      ...
  	//计算属性——简写
      let fullName = computed(()=>{
          return person.firstName + '-' + person.lastName
      })
      
      //计算属性——完整写法
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
  }
  ```

### 2.watch函数(监视)

- 与Vue2.x中watch配置功能一致

- 两个小“坑”：

  - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
  - 监视reactive定义的响应式数据中某个属性时：deep配置有效。
  
  
  

#### 监视三参数：

监视3参数:监视谁,监视回调,监视配置。

```vue
<script>
import { ref, watch } from "vue";
export default {
  name: "Demo",
  setup() { //注意：①组件中所用到的：数据、方法等，都要配置在setup中。
    //数据
    let sum = ref(0);
    let msg = ref("你好啊");

    /* ②监视ref定义的多个响应式数据  
          3参数:监视谁,监视回调,监视配置 */
    watch(
      [sum, msg],
      (newValue, oldValue) => {
        console.log("sum和msg变了", newValue, oldValue);
      },
      {
        immediate: true, //立即监视
      }
    );

    //setup函数返回值-对象
    return { sum, msg };
  },
};
</script>
```






#### 监视ref数据：

监视ref数据：单个，多个ref数据

  ```vue
<template>
  <h2>当前求和为:{{ sum }}</h2>
  <button @click="sum++">点击sum+1</button>
  <br />
  <h2>当前的信息为:{{ msg }}</h2>
  <button @click="msg += '!'">msg信息+!</button>
</template>

<script>
import { ref, watch } from "vue";
export default {
  name: "Demo",
  setup() { //注意：①组件中所用到的：数据、方法等，都要配置在setup中。
    //数据
    let sum = ref(0);
    let msg = ref("你好啊");
      
  //情况一：监视ref定义的响应式数据
  watch(sum,(newValue,oldValue)=>{
  	console.log('sum变化了',newValue,oldValue)
  },{immediate:true})
  
  //情况二：监视多个ref定义的响应式数据
  watch([sum,msg],(newValue,oldValue)=>{
  	console.log('sum或msg变化了',newValue,oldValue)
  }) 
      
  //setup函数返回值-对象
  return { sum, msg };    
<script/>
  ```

#### 监视reactive数据：

监视reactive数据：全部属性，某个属性，某些属性。

注意：①监视reactive获取oldValue数据异常。 
           ②强制开启了深度监视（deep配置无效） 
           ③若监视的是reactive数据中的对象，则必须配置deep。



```js
export default {
  name: "Demo", //组件中所用到的：数据、方法等，都要配置在setup中。
  setup() {//数据
    let person = reactive({
      name: "张三",
      age: 18,
      obj: {
        j1: {
          salary: 5000,
        },
      },
    });

/* 情况三：监视reactive定义的响应式数据的全部属性 
	若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
	若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 */
watch(person,(newValue,oldValue)=>{
	console.log('person变化了',newValue,oldValue)
},{immediate:true,deep:false}) //此处的deep配置不再奏效

//情况四：监视reactive定义的响应式数据中的某个属性
watch(()=>person.job,(newValue,oldValue)=>{
	console.log('person的job变化了',newValue,oldValue)
},{immediate:true,deep:true}) 

//情况五：监视reactive定义的响应式数据中的某些属性
watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
	console.log('person的job变化了',newValue,oldValue)
},{immediate:true,deep:true})

//特殊情况
watch(()=>person.job,(newValue,oldValue)=>{
    console.log('person的job变化了',newValue,oldValue)
},{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
      
}}
```



### 3.watchEffect函数(监视)

- watch的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect有点像computed：

  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  export default {
    name: "Demo",
    //注意：①组件中所用到的：数据、方法等，都要配置在setup中。
    setup() {//数据
      let sum = ref(0);
      let msg = ref("你好啊");
      let person = reactive({
        name: "张三",
        age: 18,
        obj: {
          j1: {
            salary: 5000,
          },
        },
      });
  
     //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(()=>{// 监视属性中用到那个属性，就监视那个属性。
      const x1 = sum.value
      const x2 = person.age
      console.log('watchEffect配置的回调执行了')
  })
      
       //setup函数返回值-对象
      return { sum, msg, person };
    },
  };      
  ```

## 生命周期:

vue2.x的生命周期 & vue3.0的生命周期

<div style="border:1px solid black; float:left;margin-right:20px;width:400px; height: 780px;">
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%B5%81%E7%A8%8B%E5%9B%BE.png"  style="width:100%; height: 100%; " />
</div>
<div style="border:1px solid black;float:right; width:430px;">
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/Vue3%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png"  style="width:100% " />
</div>





















1

- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
  - ```beforeDestroy```改名为 ```beforeUnmount```
  - ```destroyed```改名为 ```unmounted```
- Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
  - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

```vue
<script>
    import {
        ref,
        onBeforeMount,onMounted,onBeforeUpdate,
        onUpdated,onBeforeUnmount,onUnmounted,
    } from "vue";

    export default {
      name: "Demo",
      setup() {
          
          //  生命周期钩子②——通过组合式API的形式
           onMounted(() => {// 挂载完成
                console.log("-----onMounted(挂载完成)-----");
           });
           ...
           
          //生命周期钩子①——通过配置项的形式
          mounted() {//挂载完成
              console.log("----mounted(挂载完成)----");
          },
          ...
        }
</script>
```



## hook函数(mixin混入):

- 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。

- 类似于vue2.x中的mixin。

- 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。

### hooks/usePoint.js文件：

```js
//   Point函数，复用代码
import { reactive, onMounted,onBeforeUnmount } from "vue";

 // 暴露savePoint
 export default function savePoint() {
    //数据
    let point = reactive({
        x: 0,
        y: 0,
    });

    //方法
    function savePoint(event) {
        point.x = event.pageX;
        point.y = event.pageY;

        console.log(event.pageX, event.pageY);
    }

    // 生命周期-组合式API
    onMounted(() => {//挂载完成
        window.addEventListener("click", savePoint);
    });

    onBeforeUnmount(() => {//卸载前

        window.removeEventListener('click', savePoint);
    });

    return point;
}
```

Text组件:

```vue
<template>
  <h2>我是Test组件</h2>
  <h3>当前鼠标的坐标为:x{{ point.x }}, y{{ point.y }}</h3>
</template>

<script>
// 引入usePoint函数
import usePoint from "../hooks/usePoint";
export default {
  name: "Test",
  setup() {
    //数据
    let point = usePoint();

    //setup函数返回值-对象
    return {  point };
  },
};
</script>
```

# 其它 Composition API

## shallowReactive 与 shallowRef:

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

App组件：

```vue

<script>
import {toRefs, shallowReactive, shallowRef } from "vue";
export default {
  name: "Demo",
  //注意：①组件中所用到的：数据、方法等，都要配置在setup中。
  setup() {
    //数据
    let person = shallowReactive({//浅层式响应式（只考虑第一层数据的响应式）
      name: "张三",
      age: 18,
      obj: {
        j1: {
          salary: 5000,
        },
      },
    });

      //只处理基本数据类型的响应式, 不进行对象的响应式处理
    let x = shallowRef({ y: 0 });
   
    return {  //setup函数返回值-对象
      person,
      x,
      // 方法2: toRefs——解构赋值_对象
      ...toRefs(person), //解开person对象
    };
  },
};
</script>
```





## readonly(深只读) 与shallowReadonly(浅只读):

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

```vue
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>工资: {{ obj.j1.salary }}</h2>
  <h2>b:{{ obj.b }}</h2>
  <button @click="name += '~'">修改姓名</button>
  <button @click="age++">年龄+1</button>
  <button @click="obj.b++ ">B++</button>
  <button @click="obj.j1.salary++">涨薪</button>
</template>

<script>
import {reactive, toRefs, readonly, shallowReadonly } from "vue";
export default {
  name: "Demo",
  setup() {//注意：①组件中所用到的：数据、方法等，都要配置在setup中。
    //数据
    let person = reactive({
      //浅层式响应式（只考虑第一层数据的响应式）
      name: "张三",
      age: 18,
      obj: {
        j1: {
          salary: 5000,
        },
      },
    });

    // person = readonly(person);//只读（深只读）
    person = shallowReadonly(person); //浅只读（仅第一层数据是只读的）

    //setup函数返回值-对象
    return {
      // 方法2: toRefs——解构赋值_对象
      ...toRefs(person), //解开person对象
    };
  },
};
</script>
```





## toRaw 与 markRaw:

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

```vue

```





## customRef(自定义ref):

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

  ```vue
  <template>
    <input type="text" v-model="keyVal" />
    <h1>自定义数据:{{ keyVal }}</h1>
  </template>
  
  <script>
  import { ref, customRef } from "vue";
  export default {
    name: "App",
    setup(props) {
      //使用自定义ref: 数据，参数
      let keyVal = myRef("hello",666);
  
      //自定义ref
      function myRef(value,delay) {//数据，参数
        let timer; //定时器
        return customRef((track, trigger) => {
          return {
            get() {
              console.log(`myRef容器读取数据为${value}`);
              track(); //通知Vue追踪value的变化
              return value;
            },
            set(newValue) {
              console.log(`myRe容器数据修改为: ${newValue}`);
              clearTimeout(timer); //清除定时器
              timer = setTimeout(() => {
                //定时器
                value = newValue;
                trigger(); //通知Vue去重新解析模板
              }, 500);
            },
          };
        });
      }
  
      return {
        keyVal,
      };
  },
  };
  </script>
  ```
  



## 响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

# 组件间的通信：

## provide 与 inject(提供与注入)

<img src="https://v3.cn.vuejs.org/images/components_provide.png" style="width:300px" />

![]()

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```js
     setup(){
     	...... // 传递数据给Son组件
         let car = reactive({name:'奔驰',price:'40万'})
         provide('car',car)//名称，参数
         ......
     }
     ```

  2. 后代组件中：

     ```js
     setup(props,context){
     	......
         const car = inject('car')  // 接收App组件传递的数据
         return {car}
     	......
     }
     ```

# Composition API 的优势

## 1.Options API 存在的问题

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。

<div style="width:1000px;height:370px;overflow:hidden;">
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/Options%20API%20%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%981.gif" style="zoom:50%;width:1200px; height:100%; float:left"/> 
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/Options%20API%20%E5%AD%98%E5%9C%A8%E7%9A%84%E9%97%AE%E9%A2%982.gif" style="zoom:50%;width:800px;height:100%;float:right" /> 
</div>



## 2.Composition API 的优势

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

<div style="width:1000px;height:340px;overflow:hidden;">
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/Composition%20API%20%E7%9A%84%E4%BC%98%E5%8A%BF1.gif"style="height:100%"/>
    <img src="../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/Vue3/Composition%20API%20%E7%9A%84%E4%BC%98%E5%8A%BF2.gif"style="height:100%"/>
</div>


# 新的组件

## 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

## 2.Teleport 

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。   to：标签或Css选择器

index.html页面：

 ```html
<html>
    <body>
        ...
        <div id="app"></div>
        <div id="atguigu"></div>
    </body>
</html>
 ```

 组件：

  ```vue
  <template>  
  <button @click="isShow = true">点我弹窗</button>
    <!-- <teleport to="body"> -->
  <teleport to="移动位置">
      <div v-if="isShow" class="mask">
          <div class="dialog">
              <h3>我是一个弹窗</h3>
              <button @click="isShow = false">关闭弹窗</button>
      </div>
      </div>
      </teleport>
  </template>
  ```

## 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验。底层是利用插槽实现该功能。

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```

## 引入Vue3：

```vue
<script>
    // import Child from "./components/Child.vue"; //静态引入
    import {defineAsyncComponent} from 'vue' //异步(动态)引入 （懒加载）
    const Child=defineAsyncComponent(()=>import('./components/Child.vue'))
    export default {
        name: "App",
        components: { Child }, //注册组件
    };
</script>
```

# 其他

## 1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。
  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |
  

## 2.其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......


```js
<script type="text/javascript">
        let number=19;
        let person = {
            name: "张灵玉",
            sex: "男",
            // age:18
            age:number
        }

        // 往person数据添加年龄 （defineProperty，不可被枚举<不能别遍历>）  
        Object.defineProperty(person, "age", {
            // value: 18,
            // enumerable: true, //控制属性是否可以枚举(遍历)，默认false
            // writable:true, //控制属性是否可以被修改，默认false
            // configurable:true  //控制属性是否可以被删除，默认false  (通过控制台 delete person.age 删除)   

            //当有人读取person的age属性时，set函数（getter）就会被调用，且会收到修改的具体值。
           /*  get:function(){   //get简写前
                return number;
            } */

            get(){ //get简写后
                console.log("有人读取age属性了");
                return number;
            }   

            //当有人修改person的age属性时，get函数（getter）就会被调用
            ,set(value){
                console.log("有人修改age属性了,且值是"+value);
                number=value;
            }



        })

        // 遍历person数据
       /*  for (let key in person) {
            console.log("@", key)
        } */

        console.log(person)
    </script>
```



# 数组：

遍历数组：

```js
const keys = ['name', 'address']; //定义数组
keys.forEach((k) => { //遍历数组
    console.log(k);
});
```

把对象转为一个数组：

```js
let data = {
    name: "尚硅谷",
    address: "北京",
}
//把对象中所有的属性转为一个数组。
const keys = Object.keys(obj);
```



# Object:



## defineProperty

​    **`Object.defineProperty()`** 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```html
<h1 id="">往person数据添加年龄</h1>

<script type="text/javascript">
    let number = 19;
    let person = {
        name: "张灵玉",
        sex: "男",
        // age:18
        age: number
    }

    // 往person数据添加年龄 （defineProperty，不可被枚举<不能别遍历>） 
    Object.defineProperty(person, "age", {
        // value: 18,
        // enumerable: true, //控制属性是否可以枚举(遍历)，默认false
        // writable:true, //控制属性是否可以被修改，默认false
        // configurable:true  //控制属性是否可以被删除，默认false  (通过控制台 delete person.age 删除)   

        //当有人读取person的age属性时，get函数（getter）就会被调用，且会收到修改的具体值。
        /*  get:function(){   //get简写前
                 return number;
             } */

        get() { //get简写后
            console.log("有人读取age属性了");
            return number;
        },

        //当有人修改person的age属性时，set函数（setter）就会被调用
        set(value) {
            console.log("有人修改age属性了,且值是" + value);
            number = value;
        }
    })
```

# Math:

```js
 Math.floor:  向下取整
 Math.random(): 随机数
```

# filter（过滤器）：

```js
//根据personList数组过滤p。     filter:过滤器
    数组.filter((自命名) => {  }

return this.personList.filter((per) => {
    //  //判断是否有指定值，有返回指定值的索引，无返-1（注意：空字符串返0，字符串里默认带空字符串）
    return per.name.indexOf(this.keyWord) !== -1;
})
```





# 其他

```js
    toUpperCase()  //单词全大写
    Date.now():    //获取当前时间戳
```

# 浏览器本地存储：

## **localStorage**：

### 增、删：

```js
window.localStorage.setItem("msg", "hello");  //新增

localStorage.removeItem("msg") //根据key删除
localStorage.clear();     //删除所有local Storage
```

### 查、改：

```js
localStorage.getItem("msg")  //根据key查询local Storage
```



## sessionStorage：

### 增、删：

```js
window.sessionStorage.setItem("msg", "hello");  //新增

sessionStorage.removeItem("msg") //根据key删除
sessionStorage.clear();     //删除所有local Storage
```

### 查、改：

```js
sessionStorage.getItem("msg")  //根据key查询local Storage
```





















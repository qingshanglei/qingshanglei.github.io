# Element

# 概念：

Element：是饿了么公司前端开发团队提供的一套基于 Vue 的网站组件库。

# Element的基本使用：

​      注意：Element 是基于 Vue 的，所以使用Element时必须要创建 Vue 对象

1. 在Html中引入Vue.js和Element的css、js文件。

   ```html
   <script src="vue.js"></script> 
   <script src="element-ui/lib/index.js"></script>
   <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
   ```

2. 创建Vue核心对象

   ```html
   <script>
       new Vue({ 
           el:"#app"
       })
   </script>
   ```

3. 官网复制Element组件代码



```vue
inline:  <!-- 让表单域变为形内的表单域-->
   例： <el-form :data="formInline" :inline="true">代码...</el-form> 
content-header： <!-- 下划线-->
label-width="90px" <!-- from表单下的label标签的宽度-->
```



```html
<el-form :label-position="labelPosition" label-width="150px" :rules="rules" :model="formLabelAlign">

    <el-form-item label="性别" prop="region">
        <el-select v-model="formLabelAlign.region" placeholder="请选择性别">
            <el-option label="男" value="shanghai"></el-option>
            <el-option label="女" value="beijing"></el-option>
        </el-select>
    </el-form-item>
</el-form>  
<script>
    var vue = new Vue({
        el: '#app',
        data: {  /* 数据区*/
            rules: {
                region: [
                    {required: true, message: '请选择性别', trigger: 'change'}
                ],
                date1: [
                    {type: 'date', required: true, message: '请选择日期', trigger: 'change'}
                ]

            }  
</script>
```

# Layout 布局：

​    通过基础的 24 分栏，迅速简便地创建布局。

​    注意：24 分栏里面的24 分栏又是新的分栏布局。

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Element/Layout%20%E5%B8%83%E5%B1%800.png)



布局代码：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/%E6%A1%86%E6%9E%B6/Element/Layout%20%E5%B8%83%E5%B1%80-%E4%BB%A3%E7%A0%81.png)



# 标签：

```html
 <el-divider></el-divider>  <!-- 分割线-->
 <el-divider direction="vertical"></el-divider>  <!-- 垂直分割 -->
 <el-card class="box-card">      </el-card>  <!-- 空白框 -->

```

# js:

```vue
<script>
    <!-- 新增Tabl事件 -->
        this.dynamicTags.push("新增的Tabl");

</script>
```




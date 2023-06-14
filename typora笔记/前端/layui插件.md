##  layui框里的字体颜色

```html
 tabReturn = table.render({
  cols: [[
    { title: " 借书状态", field: "bookState", templet: '#titleTpl' }//模拟元素选择器
  ]]

});
```


```javascript
<script type="text/html" id="titleTpl">
           {{#  if(d.bookState =="在借"){ }}
            <div style="color:green;">{{d.bookState}}</div>
            {{#   } }}
             {{#  if(d.bookState =="延期"){ }}
            <div style="color:pink;">{{d.bookState}}</div>
            {{#   } }}
            {{#  if(d.bookState =="超期"){ }}
          <div style="color:red;">{{d.bookState}}</div>
            {{#  } }}
</script>
```

## layui监听事件（单/双击事件）

注：event为内置事件名/ row单击事件，过滤器值为容器lay-filter设定的值 

原始容器：

```html
  <table id="tbReturn" lay-filter="tabReturn" ></table>
```

  row:单击事件        rowDouble：双击事件

   公式：layuiTable.on("row( lay-filter中的数 )"，function(e){       })

```javascript
  layui.use(["layer", "table"], function () {

      //监听事件
  layuiTable.on('row(tabReturn)', function (obj) {

​      console.log(obj)
​    } ）

 }
```

## Layui富文本编辑器

![layui富文本编辑器](C:\Users\GT\Pictures\layui\layui富文本编辑器.png)

原始容器渲染：

```html
    <textarea id="NoticeContent" name="NoticeContent" class="form-control"></textarea>
```



```js
<script>
    var editorNotice;//富文本编辑器
    $(function () {
        //1.0 初始化 ckeditor富文本编辑器
                    $.ajaxSettings.async = false;//
        editorNotice = CKEDITOR.replace("NoticeContent");//通知正文：NoticeContent
        
              var NoticeContent = editorNotice.getData();//利用畗文本编辑器 （获取val）中的getData查找。    
         
          loadDatatoForm("fromNotice", rowDate);//回填数据（图片）   需要引用 customfuntion.js
          $("#NoticeContent").html(rowDate.ArticlesCount););//回填数据（图片）   
        
        
    })
 </script>
```





## 获取选中行

  原始容器渲染：

```html
                <table class="layui-hide" id="tabCertificate" lay-filter="abCertificate"></table>
```



```js
    <script>
        var layer, layuiTable;
        var tabCertificate;
 
             //加载&初始化layui组件
            layui.use(["layer", "table"], function () {
                layer = layui.layer;
                layuiTable = layui.table;
                
                  //渲染证书表
                tabCertificate = layuiTable.render({
             
                });
                
            }


       //获取选中行
         function downloadCert() {
            //获取选中行
            var checkStatus = layuiTable.checkStatus("tabCertificate");    //参数: ID
                      
            }
  </script>
```



## 弹出层



#### 多窗口模式，层叠置顶（多层置顶）:layer.setTop()



```js

        var tabUserType;//表格
        var layerIndex = 0;//弹出层
        var layer, layuiTable;//保存layui模块以便全局使用
        layui.use(["layer", "table"], function () {
           layer = layui.layer, layuiTable = layui.table;//加载模块
        };

      function Update(UserTypeID) {
            //多窗口模式，层叠置顶
            layerIndex=layer.open({
                 tyep:2,//type=1: 页面层, 2: iframe层, 4: tips层
                 area:["1000px","600px"],//弹出层宽、高
                 title:"修改角色",//弹出层名称
                 maxmin:true,//最大、最小化
                 offset,:"10px",//坐标：只定义top坐标，水平保持居中
                 content:"JurisdictionUpdate?UserTypeID="+UserTypeID //内容：可传路径、普通的html内容，还可以指定DOM
            });
        };          

```



##  表格头部工具栏模板

![士大夫](F:\Typora\笔记图片\插件\士大夫.png)



 <!--表格头部工具栏模板-->
    



```js
<script type="text/html" id="toolbarDemo">
    <div class="layui-btn-container">
        <button class="layui-btn layui-btn-sm" onclick="openInsertModal()">新增考生信息</button>

<button class="layui-btn layui-btn-sm" onclick="openImportModal()">批量导入考生</button>
<button class="layui-btn layui-btn-sm" onclick="exportExcel()">批量导出考生</button>
</div>
</script>
layui.use(['layer', 'table'], function () {

    layer = layui.layer;
    layuiTable = layui.table;
    //渲染表格
    tabStudents = layuiTable.render({
        elem: "#tabStudents",
        //url: "selectStudentAll",
        cols: [[
            { type: 'numbers' },
            { title: '学号', field: 'StudentNumber', align: 'center' },
            { title: '姓名', field: 'StudentName', align: 'center', width: 80 },
            { title: '身份证号', field: 'StudentIDNum', align: 'center' },
            { title: '性别', field: 'StudentSex', align: 'center', width: 60 },
            { title: '学院', field: 'AcademeName', align: 'center' },
            { title: '专业', field: 'SpecialtyName', align: 'center' },
            { title: '年级', field: 'GradeName', align: 'center', width: 90 },
            { title: '班级', field: 'ClassName', align: 'center' },
            { title: '账号', field: 'UserNuber', align: 'center' },
            { title: '操作', templet: setOperate, align: 'center' }
        ]]
        , page: true,
        toolbar: "#toolbarDemo" //toolbar：工具条     参数：ID
```
























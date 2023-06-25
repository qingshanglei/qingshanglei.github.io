## ajaxSubmit()上传

html :

```html
 @*文件上传表单*@
    <div style="display: none">
        <form id="frmUpWord" action="UpLoadWord" method="post" enctype="multipart/form-data">
            @*限制上传的文件类型为“.doc,.docx”*@
            <input type="file" name="file" accept=".doc,.docx" onchange="UpLoadWord()" />
        </form>
    </div>

```

js

```js
  //需要引用：    <script src="~/Content/js/jquery.form.js"></script>

 function UpLoadWord() {
  $("#frmUpWord").ajaxSubmit(function (rtMsg) {
      
      
  }
}



```



网页截图插件html2canvas
## 网页截图

```js

html2canvas(document.querySelector("#certificateBox")).then(canvas)=>{  //参数：要截图部分的ID
                        document.body.appendChild(canvas) //在body标签结束之前渲染出来
    
  /*canvas.toDataURL(type,encoderOptions)
                  图片转化为base64位编码，加载图片时候解码并渲染出来
                  type:可选，图片格式，默认为 image/png
                  encoderOptions：可选，在指定图片格式为 image/jpeg 或 image/webp的情况下，
                  可以从 0 到 1 的区间内选择图片的质量。如果超出取值范围，将会使用默认值 0.92。其他参数会被忽略。
    */          
      $("#modCertificateImg").attr("src", canvas.toDataURL());
      $("#modelImage").modal("show");

}


```


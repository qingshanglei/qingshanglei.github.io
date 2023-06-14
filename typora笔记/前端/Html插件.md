

# 时间处理插件—dayjs：

[Day.js官网](https://dayjs.fenxianglu.cn/)

```html
<script src="../js/dayjs.min.js"></script> <!-- 时间处理插件
 注意：vue的npm引入不用带.js。  例：import dayjs from "dayjs";  -->
<script>
    const val=Date.now(); //获取当前的时间戳
    dayjs(val).format('YYYY年MM月DD');  //要转换的时间戳，要装换的格式
    
    // 设置时间为2018-05-06
    dayjs().startOf('month').add(1, 'day').set('year', 2018).format('YYYY-MM-DD HH:mm:ss');
</script>
```





# 网页截图—html2canvas：

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





## ECharts：

​    ECharts是百度的一个项目，后来百度把Echart捐给apache，用于图表展示，提供了常规的[折线图](https://echarts.baidu.com/option.html#series-line)、[柱状图](https://echarts.baidu.com/option.html#series-line)、[散点图](https://echarts.baidu.com/option.html#series-scatter)、[饼图](https://echarts.baidu.com/option.html#series-pie)、[K线图](https://echarts.baidu.com/option.html#series-candlestick)，用于统计的[盒形图](https://echarts.baidu.com/option.html#series-boxplot)，用于地理数据可视化的[地图](https://echarts.baidu.com/option.html#series-map)、[热力图](https://echarts.baidu.com/option.html#series-heatmap)、[线图](https://echarts.baidu.com/option.html#series-lines)，用于关系数据可视化的[关系图](https://echarts.baidu.com/option.html#series-graph)、[treemap](https://echarts.baidu.com/option.html#series-treemap)、[旭日图](https://echarts.baidu.com/option.html#series-sunburst)，多维数据可视化的[平行坐标](https://echarts.baidu.com/option.html#series-parallel)，还有用于 BI 的[漏斗图](https://echarts.baidu.com/option.html#series-funnel)，[仪表盘](https://echarts.baidu.com/option.html#series-gauge)，并且支持图与图之间的混搭。

官方网站：https://echarts.apache.org/zh/index.html
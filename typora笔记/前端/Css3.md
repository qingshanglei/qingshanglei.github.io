# 使用Css3:

## 定义关键帧：

```css

@keyframes 关键帧名{ ------- changecolor动画名称（定义一个动画名称）
    0%{
        background:#f00;
    }
    50%{
        background:#f55;
    }
    100%{
        background:#fff;
    }
}

或  @keyframes changecolor{
    from { /* 开始 */
        transform: translate(-100%);
    }
    to{ /* 结束 */
        transform: translate(0px);
    }
}
```

## 使用关键帧:

```css
=======》  使用关键帧:  animation: 关键帧名 1s;
.box{
    width：10px
    height：100px
    background-color:   aquamarine;
    position： relative；
    left：0;
    animation： 关键帧名 2s  ease-in-out forwards
}
```



## html:

```html
<body>
    <div class="box"> </div>
</body>
```


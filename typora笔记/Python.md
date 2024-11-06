







## conda

```python
# 创建虚拟环境
conda create-n textpython=3.9
# 二、进入虚拟环境
conda activate text
# 三、安装依赖库
conda install -c conda-forge polygon3
# 四、安装cnocr
pip install cnocr -i https://mirrors.aliyun.com/pypi/simple

#  五、安装 onnxruntime
pip install onnxruntime

# 安装 onnxruntime不成功，则使用
conda install -cconda -forgeonnxruntime


# 使用 conda 查看虚拟环境
conda info --envs  或 conda env list
```

Visual Studio Code创建尾缀 `py` 的文件运行以下代码：（没别的编译器）
```js
from cnocr import CnOcr
img_fp ='huochepiao.jpeg'  //图片路径
ocr=CnOcr0
out=ocr.ocr(img_fp)
print(out)
```

## pip:

```python
# 查询
pip list

# 安装
pip install cnocr(包名)
# 指定源安装-清华大学源
pip install cnocr -i https://pypi.tuna.tsinghua.edu.cn/simple

# 指定版本
pip install cnocr==2.2.0 -i https://pypi.doubanio.com/simple/

pip cache purge
```

常用源：

```python
 https://mirrors.aliyun.com/pypi/simple/  # 阿里云源
 https://pypi.doubanio.com/simple/   # 豆瓣源
```



## frida：

```python

#  进入连接的设备的 shell 环境，允许你在设备上执行各种命令
adb shell

su  # 切换到超级用户（root 用户）权限
ls # 列出当前目录下的文件和文件夹

/frida-server64

# 开放所有权限
chmod 777./frida-server64

# 转发端口：27042
adb forward tcp:27042

frida-ps        # 

frida-ps -U   # 列出 USB 连接的设备上的进程
```



```python
代码实现步骤
1.发送请求
   ①正常去发送请求，是需要做IS逆向
    ②加密参数：a_bogus
     模拟浏览器对于url地址发送请求 ---requests模块去发送请求获取数据
模拟浏览器
  使用开发者工具-〉网络->点击对应数据包->标头-〉请求标头(相关参数)
cookie
referer
-host
User-Agent  
    
2.获取数据
3.解析数据
4.保存数据
可以使用DrissionPage自动化模块进行采集
  1.打开浏览器
  2.访问网站 
  3.监听数据包，获取响应数据
  4.解析数据
  5.保存数据

技术不断的更新选代的
-自动化模块：selenium
目前很多网站对于selenium有检测的（淘宝boss  前程无忧..）
-推荐：DrissionPage

```

## 爬取抖音：

```python

# 小红书网页搜索框-采集接口
   ①直接搜索《search》单词，出现notes文件，里面的《Preview》就有数据返回

// ==爬取抖音视频评论
#导入自动化模块
from DrissionPage import ChromiumPage
#打开浏览器
driver = ChromiumPage()
#监听数据包
driver.listen.start('aweme/v1/web/comment/list/')
#访问网站
driver.get(https://www.douyin.c0m/video/7353500880198536457)
#等待数据包加载
resp = driver.listen.wait()
#直接获取数据包返回的响应数据
json_data = resp.response.body
print(json_data)

```










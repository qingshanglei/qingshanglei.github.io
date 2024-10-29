







## conda

```js
// 创建虚拟环境
conda create-n textpython=3.9
// 二、进入虚拟环境
conda activate text
// 三、安装依赖库
conda install -c conda-forge polygon3
// 四、安装cnocr
pip install cnocr -i https://mirrors.aliyun.com/pypi/simple

//  无、安装 onnxruntime
pip install onnxruntime

// 安装 onnxruntime不成功，则使用
conda install -cconda -forgeonnxruntime
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
ip install cnocr(包名)
# 指定源安装-清华大学源
pip install cnocr -i https://pypi.tuna.tsinghua.edu.cn/simple




```






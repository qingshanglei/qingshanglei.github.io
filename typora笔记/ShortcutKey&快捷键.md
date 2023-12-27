# Win10

|     命令     |         说明         |
| :----------: | :------------------: |
|    Ctrl+v    |    ==弹出剪切板==    |
|    Alt+FN    |        键盘锁        |
|    空格+M    |         静音         |
|   win+Tab    |     切换虚拟桌面     |
|    win+D     |     新建虚拟桌面     |
|   alt+tab    |       切换软件       |
| Ctrl+Shift+N | ==新建文件夹、目录== |
|              |                      |

   **Windonws端口被占用：**

```sh
# 必须管理员权限打开cmd
netstat -ano   #查询所有使用的端口
netstat -ano | findstr "端口号" #查询指定端口
taskkill /F /PID "进程PID号"    #根据PID杀死任务
tasklist | findstr "进程PID号"  #根据PID查询进程名称
taskkill -f -t -im "进程名称"   #根据进程名称杀死任务
```

win10服务：

```sh
# 查询指定服务
sc query service name(服务名称)
# 关闭指定服务
sc stop service name

net start 服务名（MySQL80）   // 启动mysql服务
net stop  服务名（MySQL80）  // 停止mysql服务
```



# Linux:

|     命令     |                             说明                             |
| :----------: | :----------------------------------------------------------: |
|   ifconfig   | 查询ip地址（ens33里查看）<br/>     lo里的是：回环地址（127.0.0.1） |
| Ctrl+Shift+c |                                                              |
|              |                                                              |
|              |                                                              |



# IDEA：

```java
Ctrl+Alt+T --------------快速使用 for...快捷键
    System.out.println("展示");  ------------ "展示".sout+Tab键    
    itit+Tab键 ------------迭代器遍历  
```

|      Ctrl+j      |         查看所有快捷键         |
| :--------------: | :----------------------------: |
|    Ctrl+Alt+T    |     快速使用 for...快捷键      |
|      Ctrl+j      |  ==查看所有输出语句的快捷键==  |
| Ctrl + Shift + U |         ==字母变大写==         |
|    Ctrl+Alt+L    |           代码格式化           |
|     Ctrl+F12     |    ==查询当前接口所有方法==    |
|  Ctrl+Alt+空格   |       内容提示，代码补全       |
|      Alt+F5      |           更改文件名           |
|      Ctrl+B      |            查看源码            |
|  Ctrl+Alt+Shift  |          获取对象的图          |
|      Ctrl+O      |            重写方法            |
|   双击Shift键    |            弹出搜索            |
|      Ctrl+h      | ==查看当前接口及其子接口名称== |
| Ctrl+Alt+Shift+u |  ==查看当前项目所有依赖关系==  |



​    

# VS Code:

|       快捷键       |      说明      |
| :----------------: | :------------: |
|    Alt+Shift+F     |   代码格式化   |
|    Shift+Alt+↓     |   复制当前行   |
|    Alt+Shift+A     |     块注释     |
|      Ctrl + /      |    单行注释    |
| Ctrl + K ,Ctrl + 3 | 按层数折叠代码 |
|                    |                |





# Typora：

```
  Shift+tab  ------------代码格式化       
  ```或```.java +回车 ------------插入代码块(多行)
  `代码` ------------插入代码块(单行)
  ![]() ------------插入图片
  []() ------------超链接标签
  Ctrl+T ------------ 插入表格
  == ==  ------------ 黄色字体
  ** **或Ctrl+T键  ------------ 字体加粗


```






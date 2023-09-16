

### IDEA：

#### 隐藏指定文件/文件夹

Setting → File Types → Ignored Files and Folders

以下为隐藏springBoot项目多余文件...

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E8%BD%AF%E4%BB%B6/IDEA/%E9%9A%90%E8%97%8F%E6%8C%87%E5%AE%9A%E6%96%87%E4%BB%B6and%E6%96%87%E4%BB%B6%E5%A4%B9.png)

#### 制作~使用SpringBoot项目模块

- 在工作空间中复制对应工程，并修改工程名称
- 删除与Idea相关配置文件，仅保留src目录与pom.xml文件
- 修改pom.xml文件中的artifactId与新工程/模块名相同
- 删除name标签（可选）

1.制作SpringBoot项目模块

##### 前提：创建一个SpringBoot项目

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E8%BD%AF%E4%BB%B6/IDEA/%E5%88%B6%E4%BD%9CSpringBoot%E9%A1%B9%E7%9B%AE%E6%A8%A1%E6%9D%BF.png)

##### 2.使用SpringBoot模板

把项目名称修改一下，用IDEA启动就好了

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E8%BD%AF%E4%BB%B6/IDEA/%E4%BD%BF%E7%94%A8SpringBoot%E9%A1%B9%E7%9B%AE%E6%A8%A1%E6%9D%BF.png)



# [VS Code](https://code.visualstudio.com/):



```html
 !+回车键  <!-- 生成Html头部-->
```

# utools:

| Alt+空格 | 弹出搜索框 |
| -------- | ---------- |
|          |            |
|          |            |
|          |            |



# win10

## 批量修改文件名：

```
dir /b 文件名 >names.txt //批量获取文件名
ren 旧名 新名 // 批量修改文件名
```

​    使用slsx表格替换要修改的名字 （Ctrl+H：替换）

​    双击slsx表格单元格右下角，复制当前行的所有行。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%B8%B8%E8%AF%86/%E6%89%B9%E9%87%8F%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%E5%90%8D%E7%A7%B0.png)



# [cpolar](https://www.cpolar.com/?channel=0&invite=4Fqp):

1.    cpolar内网穿透工具(部署本地项目让被人能访问)
2.    其他看官网，写这个是因为官网的地址写错了。。。。
3.    无脑安装后，浏览器访问: http://localhost:9200/  

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Tool/Cpolar/%E8%AE%BF%E9%97%AECpolar.png)



## 将隧道配置为后台服务：

路径：C:\Users\GT\.cpolar\cpolar.yml,注意不是官网的 c:\Users\用户名.cpolar\cpolar.yml路径。

```yaml
authtoken: xxxxxxxxxxxx #认证token
tunnels:
# 配置硅谷课堂前台域名
  ggktFront:   # 隧道名称 
    proto: http  #协议：http
    addr: "8080"  # 本地端口
    region: cn_vip  #地区：cn_vip，可选:us,hk,cn,cn_vip(中国)
# 配置硅谷课堂前台后台
  ggktEnd:
    proto: http
    addr: "8333"
```

管理员身份打开Developer PowerShell， 关闭并重启cpolar服务 

```sh
# 关闭cpolar服务 
Stop-Service cpolar
```

打开cpolar官网或软件出现隧道端口即可。。。








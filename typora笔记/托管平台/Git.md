	**常用的托管服务[远程仓库]**

-    **gitHub**（ 地址：https://github.com/ ）是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名gitHub 

-    **码云或Gitee**（地址： https://gitee.com/ ）是国内的一个代码托管平台，由于服务器在国内，所以相比于 GitHub，码云速度会更快 

-    **GitLab** （地址： https://about.gitlab.com/ ）是一个用于仓库管理系统的开源项目，使用Git作 为代码管理工具，并在此基础上搭建起来的web服务,一般用于在企业、学校等内部网络搭建git私服。

# Git

## 简介：

​     Git 是一个免费的、开源的 分布式版本控制系统，可以快速高效地处理从小型到大型的各种项目
​     Git 易于学习，占地面积小，性能极快。它具有廉价的本地库，方便的暂存区域和多个工作流分支等特性
​      其性能优于 Subversion、CVS、Perforce 和 ClearCase 等版本控制工具

何为版本控制：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E4%BD%95%E4%B8%BA%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6.png)



版本控制工具：① 集中式版本控制工具   ②分布式版本控制工具



**集中式版本控制工具：**

CVS、SVN（Subversion）、VSS.......

​     集中化的版本控制系统诸如 CVS、SVN 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

​     优点：可以看到其他人正在做些什么；开发者权限控制

​     缺点：中央服务器的单点故障，无法提交历史记录.

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E9%9B%86%E4%B8%AD%E5%BC%8F%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6.png)

**分布式版本控制工具：**

Git、Mercurial、Bazaar、Darcs.......

优点：

 ●版本控制在本地，可以断网开发。
 ●保存完整项目，包含历史记录，更安全。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%86%E5%B8%83%E5%BC%8F%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6.png)





## git安装&环境配置：

### git傻瓜式安装。



###  仓库初始化

```
  git init --仓库初始 (初始化成功后可在文件夹下看到隐藏的.git目录)
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E4%BB%93%E5%BA%93%E5%88%9D%E5%A7%8B%E5%8C%96.png)



### 配置用户信息

1. 打开Git Bash

2. 设置用户信息（用户名和email地址）

​    注意：①Git 首次安装必须设置一下用户签名，否则无法提交代码。
​               ②这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

```js
git config --global user.name “qingshanglei”  //设置用户名
git config --global user.email “2274916510@qq.com” //设置邮箱

git config user.name // 查看用户名
git config user.password // 查看密码
git config user.email // 查看邮箱
git config --list  // 查看配置信息

git config --global user.name "xxxx(新的用户名)" //修改用户名
git config --global user.password "xxxx(新的密码)" //修改密码
git config --global user.enail "xxxx(新的邮箱)"//修改用户邮箱
```





常用指令配置别名：

1. 打开用户目录，创建 .bashrc 文件

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E6%8C%87%E4%BB%A4%E8%AE%BE%E7%BD%AE%E5%88%AB%E5%90%8D.png)

   

2. 在 .bashrc 文件中输入如下内容：

```java
#用于输出git提交日志
    alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' 
#用于输出当前目录所有文件及基本信息
     alias ll='ls -al'
```

3. 打开gitBash，执行 source ~/.bashrc

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E6%8C%87%E4%BB%A4%E8%AE%BE%E7%BD%AE%E5%88%AB%E5%90%8D2.png)
   
   

##  解决GitBash乱码问题:

1. 打开GitBash执行下面命令

```
git config --global core.quotepath false
```

2. ${git_home}/etc/bash.bashrc 文件最后加入下面两行

```
export LANG="zh_CN.UTF-8" export LC_ALL="zh_CN.UTF-8"
```



## Get工作机制：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/Get%E5%B7%A5%E4%BD%9C%E6%9C%BA%E5%88%B6.png)



## Git指令常用：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/git%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.png)

## 增删查改：

从**工作区** 向**仓库**  提交**增删查改**（文件或文档）的操作。

~~~shell

  git add 文件名 -----------提交文件到暂存区
  git add .  -----------提交所有文件到暂存区
  git commit -m "日志信息" 文件名(可省略) -----------把文件从暂存区 提交到仓库        （-m 表示添加一个版本日志信息，不写此参数也会打开日志信息的文件框。）
  
git remote remove origin   -------------Gitee删除与远程仓库的关联
~~~

## 状态查看:

```shell
git status         # 查看文件的状态(暂存区和工作区的状态)
git diff           #查看那些更新还没有暂存
git diff --cached  #查看哪些暂存还没有提交
git diff --staged  #查看哪些暂存还没有提交

```



## 版本、日志查看：

```shell
git reflog  # 查看精简版本信息
git log [option]-----------查看提交日志，详细版本信息
    * option 
        --all  -----------显示所有分支
        --pretty=oneline ----------- 将提交信息显示为一行
        --abbrev-commit -----------使得输出的commitld更简短
        --graph -----------以图的形式显示
```



## **版本回退**

根据commitID切换 提交到仓库里文档或文件的 版本(找回以前的备份)

Git 切换版本，底层其实是移动的 HEAD 指针。

~~~shell
   注意：commitID 可以使用 git-log 或 git log 指令查看.
   git reset --hard commitID（版本号）   -----------版本切换（双击选中即可复制，点击鼠标滚轮即可粘贴）

   git reflog  -----------查看已经删除的记录 或commitID 

   git reflflog  -----------查看已经删除的记录 或commitID 
~~~



## 分支：

#### 概念：

   分支：在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%86%E6%94%AF.png)

​    使用分支： 把工作开发从主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

#### 分支优点:

​     ①同时并行推进多个功能开发，提高开发效率。

​     ②各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

①web页面切换分支：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/GitHup/web%E9%A1%B5%E9%9D%A2%E4%BF%AE%E6%94%B9%E5%88%86%E6%94%AF.png)

#### 命令：

~~~shell
git fetch     # 更新本地分支列表
git checkout -b main origin/<remote-branch>    # 创建本地分支(main)并关联到对应的远程分支(remote-branch)

git branch 分支名  -----------  创建本地分支

删除分支（不能删除当前分支，只能删除其他分支）
     git branch -d 分支名  ----------- 删除分支时，会检查
     git branch -D 分支名 -----------  强制删除，不做任何检查

  git branch -----------查看本地分支
  git branch -v  -----------查看本地分支
  
  git checkout 分支名 ----------- 切换分支
  git checkout -b 分支名 -----------切换分支（切换前判断是否存在，不存在创建）
  
  git merge 分支名称 -----------合并分支（把指定的分支合并到当前分支）

~~~



# 解决冲突:

​     冲突产生的原因：合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改。Git无法替我们决定使用哪一个。必须人为决定新代码内容。

解决冲突步骤如下：

1. 处理文件中冲突的地方（若count 1和2都要，删除 `====》HEAD`  或  `====》`在add、push代码就行了。）

2. 将解决完冲突的文件加入暂存区(add)

3. 提交到仓库(commit)

   

   解决冲突的，Git Bush Here命令图：
   

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/10.png)



**3.4.9**、开发中分支使用原则与流程

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/11.png)

在开发中，一般有如下分支使用原则与流程：

- master （生产） 分支

  线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；

- develop（开发）分支

​    是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支,准备上线。

- feature/xxxx分支

  从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支。

- hotfifix/xxxx分支，

  从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、

- develop分支。

  还有一些其他分支，在此不再详述，例如test分支（用于代码测试）、pre分支（预上线分支）等等。



# Git 团队协作机制：

### Git 团队协作机制：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%9B%A2%E9%98%9F%E5%86%85%E5%8D%8F%E4%BD%9C.png)



### Git 团队协作机制：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E8%B7%A8%E5%9B%A2%E9%98%9F%E5%8D%8F%E4%BD%9C.png)



   注意：克隆别的项目到本地，本地上传到自己的仓库，需要删除克隆自带的.git文件，不然会显示权限不足等。



## **配置 Git 忽略文件**:

创建**xxxx.ignore（例：git.ignore）**

```c#
# 不允许以下文件上传到Gitee 
//   (注意：这里要使用正斜线（/），不要使用反斜线（\）)

/* 忽略文件夹 */
Typora安装包/*        

/* 忽略文件 */
.gitignore

```



java忽略模板：

```java
# Compiled class file 
*.class 
 
# Log file 
*.log 
 
# BlueJ files 
*.ctxt 
 
# Mobile Tools for Java (J2ME) 
.mtj.tmp/ 
 
# Package Files # 
*.jar 
*.war 
*.nar 
*.ear 
*.zip 
*.tar.gz 
*.rar 
 
# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml 
hs_err_pid* 
.classpath 
.project .settings target .idea 
*.iml 	crash 	logs,  	see
```



## IDEA 集成 Git:







# Gitee远程仓库

操作Git远程仓库-Gitee

| 命令                             | 作用                                                     |
| -------------------------------- | -------------------------------------------------------- |
| git remote add 别名 远程地址     | 起别名方式-添加远程仓库                                  |
| git remote -v                    | 查看当前所有远程别名                                     |
| git clone 远程地址               | 将远程仓库的内容克隆到本地                               |
| git pull 远程地址别名 远程分支名 | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 |
| git push 别名 分支               | 推送本地分支上的内容到远程仓库                           |



### **创建远程仓库**

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)

仓库创建完成后可以看到仓库地址

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/14.png)



### **添加远程仓库**

**此操作是先初始化本地库，然后与已创建的Gitee远程库进行对接**

~~~jsp
   命令： git remote add <分支名称，别名> <仓库路径>
<--! 远端名称，默认是origin，取决于远端服务器设置 -->

<--! 仓库路径，从远端服务器获取此url:SSH-->
    git remote -v <!-- 查看远程连接-->
    <!-- 不报错即为添加仓库成功 -->
例如: git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git  
 
~~~

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/16.png)

###  **查看远程仓库**

命令：git remote

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/17.png)

### **推送**

把本地的仓库推送到 Gitee(远程仓库)中


注意：如果远程分支名和本地分支名称相同，则可以只写本地分支。

注意：如果当前分支已经和远端分支关联，则可以省略分支名和远端名。

~~~sh
命令：git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名] ]
    git push # 推送（默认分支main）
    git push  # 将master分支推送到已关联的远端分支。
    git push origin master(分支名)   # 推送指定分支
    git push --set-upstream origin master

-f 表示强制覆盖
--set-upstream  推送到远端的同时并且建立起和远端分支的关联关系。
如果远程分支名和本地分支名称相同，则可以只写本地分支
如果当前分支已经和远端分支关联，则可以省略分支名和远端名。

~~~

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/18.png)

### 查询远程仓库

如在Gitee中出现类似于以下界面，则表示本地库 推送成功

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/19.png)

###  本地分支与远程分支的关联关系

```
git branch -vv   //查看本地分支 与远程分支的关联关系
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/20.png)

### **克隆**

如果已经有一个远端仓库，我们可以直接clone(克隆)到本地。 

注意：克隆不需要登录账号。

克隆默认执行操作：
    ●1、拉取代码
    ●2、初始化本地仓库
    ●3、创建别名（默认origin）

```
 git clone <仓库路径，https,ssh> [本地目录]   // 本地目录可省略,会自动生成。
```

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/21.png)

###   **抓取和拉取**

   远程分支和本地的分支一样，我们可以进行merge操作，只是需要先把远端仓库里的更新都下载到本地，修改操作在上传。

注意：第一次需要输入Gitee账号和密码。

  ~~~

  抓取命令：git fetch [SSH/HTTPS，别名] [分支名]    (抓取指令就是将仓库里的更新都抓取到本地，不会进行合并,如不指定远端名称和分支名，则抓取所有分支。 )  
 
  拉取命令：git pull  [SSH/HTTPS，别名] [分支名]    (拉取指令就是将远端仓库的修改拉到本地并自动进行合并，等同于fetch+merge)
  ~~~

### 解决合并冲突

​    因A、B用户修改了同一个文件，A先上传，**故需要先拉取远程仓库的提交，经过合并后才能推送到远端分支。**（本地分支操作同远程分支）



### SSH 免密登录:

####  在**Git**中配置SSH公钥

​     注意: Gitee 或GitHub上未添加任何公钥之前，Code—SSH会有警告提示信息，表示目前 SSH 方式是没有权限的。(无警告/提示信息了，表示可用)

1.在**Git**中生成、获取**SSH公钥**

~~~sql
  ssh-keygen -t rsa --生成SSH公钥；输入命令，连敲三次回车即可;(如存在，则自动覆盖)
 ssh-keygen -t rsa -C 描述   ---t指定加密算法，-C添加注释 
 cat ~/.ssh/id_rsa.pub   --获取公钥
~~~

#### 2.在Gitee配置仓库的SSH公钥

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/15.png)

2.在**Git中**验证是否配置成功

``` Bash 
ssh -T git@gitee.com   
```



# **在Idea中使用Git**

### 总结:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/IDEA%E5%B8%B8%E7%94%A8GIT%E6%93%8D%E4%BD%9C.png)

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/IDEA%E5%B8%B8%E7%94%A8GIT%E6%93%8D%E4%BD%9C2.png)

### 1、在Idea中配置Git

   若Git安装在默认路径下，idea会自动找到git的位置；如果更改了Git的安

装位置则需要手动配置下Git的路径。

​    选择File→Settings打开设置窗口，找到Version Control下的git选项

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/IDEA%E4%BF%AE%E6%94%B9Git%E8%B7%AF%E5%BE%84.png)

点击Apply，修改完成

### 2、在Idea中操作Git

1.创建远程仓库（Gitee）

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)

**2.初始化本地仓库**

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/IDEA%E5%88%9D%E5%A7%8B%E5%8C%96%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93.png)

###  3、设置远程仓库：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E8%AE%BE%E7%BD%AE%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93.png)

### 4、提交到本地仓库

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E6%8F%90%E4%BA%A4%E5%88%B0%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93.png)

### 5、推送到远程仓库

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E6%8E%A8%E9%80%81%E5%88%B0%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93.png)

### 7、克隆远程仓库到本地

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%85%8B%E9%9A%86%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E5%88%B0%E6%9C%AC%E5%9C%B0.png)

### 8、创建分支

1. 最常规的方式

   ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%9B%E5%BB%BA%E5%88%86%E6%94%AF-%E6%96%B9%E5%BC%8F1.png)

2. 最强大的的方式

 ![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%9B%E5%BB%BA%E5%88%86%E6%94%AF-%E6%96%B9%E5%BC%8F2.png)

### 9、切换分支及其他分支相关操作

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/%E5%88%87%E6%8D%A2%E5%88%86%E6%94%AF%E5%8F%8A%E5%85%B6%E4%BB%96%E5%88%86%E6%94%AF%E7%9B%B8%E5%85%B3%E6%93%8D%E4%BD%9C.png)

### 10、解决冲突

1. 执行merge或pull操作时，可能发生冲突

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/Git/IDEA%E8%A7%A3%E5%86%B3%E5%88%86%E6%94%AF.png)

2. 冲突解决后加入暂存区,  略

3. 提交到本地仓库,  略

4. 推送到远程仓库, 略









插件：ignoare有模板







# GitHud

repositiory:仓库

star:收藏

fork:复制克隆项目

pull request:发起请求

push request:

Watch:关注

lssue:事物卡片

Download ZIP:下载



# GitHub Pages：

官网文档：(https://docs.github.com/zh/pages/quickstart

## GitHub Pages说明：

 GitHub Pages 支持从三个地方读取文件：

- `docs(项目)/` 目录      推荐使用
- master 分支
- gh-pages 分支

注意：GitHub Pages 不支持服务器端语言，例如 PHP、Ruby 或 Python。



GitHub Pages 站点受到以下使用限制的约束：

- GitHub Pages 源存储库的建议限制为 1 GB。 有关详细信息，请参阅“[关于 GitHub 上的大文件](https://docs.github.com/zh/repositories/working-with-files/managing-large-files/about-large-files-on-github#file-and-repository-size-limitations)”
- 发布的 GitHub Pages 站点不得超过 1 GB。
- GitHub Pages 站点的软带宽限制为每月 100 GB。
- GitHub Pages 站点的软限制为每小时 10 次生成。 如果使用自定义 GitHub Actions 工作流生成和发布站点，则此限制不适用
- 为了为所有 GitHub Pages 站点提供一致的服务质量，可能会实施速率限制。 这些速率限制无意干扰 GitHub Pages 的合法使用。 如果你的请求触发了速率限制，你将收到相应响应，其中包含 HTTP 状态代码 `429` 以及信息性 HTML 正文。

注意：访问 GitHub Pages 站点时，出于安全目的，无论访问者是否已登录 GitHub ，都会记录并存储访问者的 IP 地址。



## Git部署项目(博客)

1.创建项目

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/GitHup/Pages/%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE.png)

```sh
 # 注意：用户名必须是以这个账号的名称才能访问。
 username(用户名).github.io # 根据用户名创建仓库名称 
 <organization>.github.io # 根据组织帐户名创建仓库名称 
 
  除非使用的是自定义域，否则用户和组织站点在 `http(s)://<username>.github.io` 或 `http(s)://<organization>.github.io` 中可用。
```

2.项目上传到gitHub仓库

3.修改setting(设置)Pages的Branch

​    出现路径或者Action里pages build and deployment 变绿代表成功。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/GitHup/Pages/%E9%85%8D%E7%BD%AEBranch.png)



 4.修改域名:

注意：gitbut网速太慢，修改自定义域名可提高访问网站速度。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Git/GitHup/Pages/%E4%BF%AE%E6%94%B9%E5%9F%9F%E5%90%8D.png)




































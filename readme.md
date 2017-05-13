# GIT 教程学习心得
----

**前言**

该文件用来测试和展示书写README的各种markdown语法。GitHub的markdown语法在标准的markdown语法基础上做了扩充，称之为`GitHub Flavored Markdown`。简称`GFM`，GFM在GitHub上有广泛应用，除了README文件外，issues和wiki均支持markdown语法。
本文用MarkdownPad 2 软件进行编写，为了预览效果正常，需要先设置Tools->Markdown->Markdown Processor,选择GitHub Flavored Markdown。
另外，锚点功能在预览时是无效的，必须在推到GitHub仓库中的文件中点击才有效果。


**ks4na**

---
## 目录
* [Git简介](#Git简介)
* [安装Git](#安装Git)
* [创建版本库](#创建版本库)
	* 把文件添加到版本库
* [时光机穿梭](#时光机穿梭)
	* 版本回退
	* 工作区和暂存区
	* 管理修改
	* 撤销修改
	* 删除文件
* [远程仓库](#远程仓库)
	* 添加远程库
	* 克隆远程库
* [分支管理](#分支管理)
	* 创建与合并分支
	* 解决冲突
	* 分支管理策略
	* Bug分支
	* Feature分支
	* 多人协作
* [标签管理](#标签管理)
	* 创建标签
	* 操作标签
---
### Git简介
Git 是分布式版本控制系统。
和集中式版本控制系统相比，安全性更高。

-----

### 安装Git
在windows上安装Git　>> 　[点我跳转][installGit]

------

### 创建版本库
版本库，又名仓库（repository），该目录所有文件都可被Git管理。
```
创建文件夹：$ mkdir learngit
进入文件夹：$ cd learngit 　　　//e.g. 进入d盘的REPO文件夹：cd /d/REPO
显示当前目录：$ pwd
把当前目录变为Git可以管理的仓库：$ git init

```
##### 把文件添加到版本库

真正使用版本控制系统，要以**纯文本方式**编写文件（不要使用记事本，推荐Notepad++ ，并且记得把Notepad++默认编码设置成UTF-8 without BOM）
```
把文件添加到仓库：$ git add <file-name> [<file-name2> <file-name3>]
把文件提交到仓库：$ git commit -m "XXX" (-m "XXX" 添加本次提交的简要描述信息"XXX")
Git可以多次add不同的文件，然后一次commit提交所有文件。
```

##### 小结:

> 初始化一个仓库：**$ git init**
添加文件到Git仓库：**$ git add \<file1> \<file2>**
　　　　　　　　　　**$ git commit -m "XXX"**

----

### 时光机穿梭
```
查看仓库当前状态：$ git status
查看文件修改的内容：$ git diff <file-name>
```
##### 版本回退

每当文件修改到一定程度，就可以“保存一个快照”，即commit一下，一旦把文件改乱了或者误删了文件，还可以从之前的commit恢复。

**//////////////////////////////////////////////////////**

Git 版本回退只是把HEAD指针指向回退到的版本:

|![][picA]|`HEAD` 指针改为指向`add distributed`|![][picB]|
|---------|-------------------------------|--------|

**//////////////////////////////////////////////////////**

> `HEAD` 指向的版本为当前版本
Git 允许在版本历史间穿梭 ： **$ git reset --hard \<commit-id>**
穿梭前用  **$ git log (--pretty=oneline)**  来查看提交（commit）历史，确定要回到哪个版本
重返未来，用  **$ git reflog**  查看命令历史，确定要回到未来哪个版本


##### 工作区和暂存区

**工作区**就是电脑里看到的目录,如："learngit"
**版本库（repository)**：工作区中的隐藏目录.git
版本库中有 **暂存区stage** 和 **分支**（master分支等）

如下图，`git add` 把文件添加进 **暂存区**，`git commit` 把暂存区内容提交到当前**分支master**

![][pic-stage&repo]
##### 管理修改
如上图，如果修改文件之后不 `add` 到暂存区，就不会加入到 `commit` 中。
##### 撤销修改
> **场景1:** 改乱了工作区某文件的内容，想直接丢弃工作区的修改时，**$ git checkout -\<file-name>**
**场景2：** 不但改乱了文件内容，而且添加到了暂存区，想丢弃修改时，分两步：第一步，

---
[installGit]:http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000 "安装Git"
[picA]:/img/A.jpg
[picB]:/img/B.jpg
[pic-stage&repo]:http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0
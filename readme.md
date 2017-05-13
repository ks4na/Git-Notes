# GIT 教程学习心得


**作者：ks4na**

--------


## 前言

该文件用来测试和展示书写README的各种markdown语法。GitHub的markdown语法在标准的markdown语法基础上做了扩充，称之为`GitHub Flavored Markdown`。简称`GFM`，GFM在GitHub上有广泛应用，除了README文件外，issues和wiki均支持markdown语法。  
本文用MarkdownPad 2 软件进行编写，为了预览效果正常，需要先设置Tools->Markdown->Markdown Processor,选择GitHub Flavored Markdown。  
另外，锚点功能在预览时是无效的，必须在推到GitHub仓库中的文件中点击才有效果。

---
## 目录
* [Git简介](#简介)
* [安装Git](#安装)
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
### 简介
Git 是分布式版本控制系统。  
和集中式版本控制系统相比，安全性更高。

-----

### 安装
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
#### 把文件添加到版本库

真正使用版本控制系统，要以**纯文本方式**编写文件（不要使用记事本，推荐Notepad++ ，并且记得把Notepad++默认编码设置成UTF-8 without BOM）
```
把文件添加到仓库：$ git add <file-name> [<file-name2> <file-name3>]
把文件提交到仓库：$ git commit -m "XXX" (-m "XXX" 添加本次提交的简要描述信息"XXX")
Git可以多次add不同的文件，然后一次commit提交所有文件。
```

#### 小结:

> 初始化一个仓库：**$ git init**
> 添加文件到Git仓库：**$ git add \<file1> \<file2>**　　**$ git commit -m "XXX"**

----

### 时光机穿梭
```
查看仓库当前状态：$ git status
查看文件修改的内容：$ git diff <file-name>
```
#### 版本回退

每当文件修改到一定程度，就可以“保存一个快照”，即commit一下，一旦把文件改乱了或者误删了文件，还可以从之前的commit恢复。

**//////////////////////////////////////////////////////**

Git 版本回退只是把HEAD指针指向回退到的版本:

|![][picA]|`HEAD` 指针改为指向`add distributed`|![][picB]|
|---------|-------------------------------|--------|

**//////////////////////////////////////////////////////**

> `HEAD` 指向的版本为当前版本
> Git 允许在版本历史间穿梭 ： **$ git reset --hard \<commit-id>**  
> 穿梭前用  **$ git log (--pretty=oneline)**  来查看提交（commit）历史，确定要回到哪个版本  
> 重返未来，用  **$ git reflog**  查看命令历史，确定要回到未来哪个版本


#### 工作区和暂存区

**工作区**就是电脑里看到的目录,如："learngit"  
**版本库（repository)**：工作区中的隐藏目录.git  
版本库中有 **暂存区stage** 和 **分支**（master分支等）

如下图，`git add` 把文件添加进 **暂存区**，`git commit` 把暂存区内容提交到当前**分支master**

![][pic-stage&repo]
#### 管理修改
如上图，如果修改文件之后不 `add` 到暂存区，就不会加入到 `commit` 中。
#### 撤销修改
> **场景1:** 改乱了工作区文件的内容，想直接丢弃工作区的修改时， 用 `$ git checkout -\<file-name>`  
> **场景2：** 不但改乱了文件内容，而且添加到了暂存区，想丢弃修改时，分两步：第一步，`git reset HEAD <file-name>` 回到场景1，第二步，接着场景1操作  
> **场景3:** 提交了不合适的修改到版本库，想要撤销修改时，参考 [版本回退](#版本回退)。**（前提是没有推送到远程库）**

#### 删除文件
```
$ rm <file-name> 或者直接在文件管理器中手动删除
$ git status 会告诉你那些被删除了
现在有两个选择：1.确实要删除 $ git rm <file-name> -> $ git commit;
　　　　　　　　2.删错了，恢复到最新版本 $ git checkout -<file-name>。
```
---

### 远程仓库
#### 添加远程库
已经在本地创建了一个Git仓库，再想在GitHub上创建一个Git仓库，实现这两个仓库远程同步，这样GitHub上的仓库既可以作为备份，又可以让其他人来协作。

步骤：
1. 在GitHub上创建新仓库，得到SSH（如：git@github.com:ks4na/learngit.git）;
2. 在本地的某个仓库下运行命令 `$ git remote add origin ` + SSH;
3. 将本地库master分支所有内容推送到远程库中 `$ git push -u origin master` **(第一次推送加上 `-u` 将本地和远程的master分支关联)**。

#### 克隆远程库
从零开发最好的方式，是先建立远程库，然后从远程库克隆。

在本地某个目录下  `$ git clone ` + SSH;

---------

### 分支管理
#### 创建与合并分支
**原理**

**a. 主分支master**  
![][pic0]  
`HEAD` 指向master，`master` 指向提交。每次提交，`master` 指针向前一步。

**b. 创建新分支时**  
![][pic1]  
`HEAD` 指向 `dev` ,表明当前在 `dev` 分支上。每次提交，`dev` 指针向前一步，而 `master` 不动。

**c. 在dev分支上工作完成**，把dev合并到master上。合并完成后可以删除dev分支。  

小结:
```
创建dev分支并切换到dev分支： $ git checkout -b dev
查看当前分支： $ git branch
在dev上工作完成后切换回master分支 $ git checkout master
```
此时情形如图：  
![][pic3]
```
把dev分支的成果合并到master分支： $ git merge dev (以Fast forward模式合并)
合并完成，可以删除dev分支: $ git branch -d dev
```
小结：

> 查看分支： `$ git branch`  
> 创建分支： `$ git branch <branch-name>`  
> 切换分支：`$ git checkout <branch-name>`  
> 创建并切换分支：`$ git checkout -b <branch-name>`  
> 合并某分支到当前分支：`$ git merge <branch-name>`  
> 删除分支：`git branch -d <branch-name>`  

#### 解决冲突
先在feature1分支上修改并提交，回到master分支对同一文件修改再提交，如下图：  
![][pic4]  
然后试图合并feature1分支时，可能会出现冲突，此时可以手动修改冲突并提交，改完后如下图：  
![][pic5]  
#### 分支管理策略
合并分支时，Git会尽量用Fast forward模式，这种模式删除分支后会丢掉分支信息；  
强制禁用Fast forward模式，Git会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

**Fast forward模式：**  
![][pic-ff]

**禁用Fast forward模式：**  
![][pic-no-ff]

>禁用Fast forward模式合并dev分支 `$ git merge --no-ff -m "XXX" dev`  
**(因为本次合并会创建一个新commit，所以加上 `-m` 参数，把commit描述加进去)**
合并后可用 `$ git log` 查看分支历史

#### 分支管理
实际开发中，分支管理原则：
```
master 分支仅用来发布新版本，平时不能在上面工作，是稳定的；
dev 分支用于平时干活，发布新版本时，把dev分支合并到 master 上；
每个人都在 dev 上干活，并且有自己的分支，时不时往 dev 上合并自己的分支。
```
如图所示：![][pic6]

#### Bug分支
当在 **dev**分支上工作到一半，还没有办法提交，但是在 **master**分支上有bug要修复时，先把工作现场用 `$ git stash` 保存一下，然后切换到 **master**分支，从**master**分支开始创建临时分支issue来修复bug，完成后切换到**master**分支进行合并，之后再切换回**dev**分支，再用 `$ git stash pop` 恢复工作现场。

```
保存工作现场： $ git stash
恢复工作现场： $ git stash pop
```
#### Feature分支
开发新功能时，新建了一个Feature分支，该分支还未被合并时临时决定放弃，可以通过 `$ git branch -D Feature` 强行删除。

```
强行删除未合并的分支：$ git branch -D <branch-name>
```
#### 多人协作

> 查看远程仓库信息：`$ git remote -v`

>推送分支到远程库：`$ git push origin <branch-name>`
>>删除远程分支：`$ git push origin :<branch-name>`
>
***需要推送的分支有：master（主分支）、dev（开发分支）、其他如bug分支feature分支不一定需要(看是否需要团队协作)。***

>抓取分支：
>>多人协作时，大家都会往**dev**分支上提交自己的修改
>>从远程库克隆时，默认只能看到**master**分支，要在**dev**分支上开发，必须创建远程**origin**的**dev**分支到本地：`$ git checkout -b dev origin/dev` ,然后就可以在**dev**上修改并把dev分支push到远程。 
> 
>>之后，另一个人也修改了同一文件，并试图推送，就会失败，Git提示要先 `$ git pull` 把最新提交从 **origin/dev** 上抓取下来。  
>> ***(git pull失败的话，就根据提示设置dev与origin/dev的链接，然后再 git pull)*** 
>> 
>>因此，**多人协作的工作模式**通常是这样：
>>> 首先尝试 `$ git push origin <branch-name>` 推送自己的修改；  
>>> 推送失败，则是因为远程库比本地更新，先用 `$ git pull` 抓取最新版本，再试图合并；  
>>> 有冲突则解决后再次推送。

***（若 `$ git pull` 提示"no tracking information"，说明没有建立分支链接关系，用 `$ git branch --set-upstream-to origin/<branch-name> <branch-name>` 建立链接。）***

小结：

> 查看远程库信息： `$ git remote -v` ;  
> 本地新建的分支不推送到远程，对其他人就是不可见的;  
> 推送分支： `$ git push origin <branch-name>` ; ***(若推送失败，要先 `$ git pull` 抓取远程的新提交)***  
> 在本地创建与远程分支对应的分支： `$ git checkout -b <branch-name> origin/<branch-name>` ;  
> 建立本地分支与远程分支的链接： `$ git branch --set-upstream-to origin/<branch-name> <branch-name>` 。  

### 标签管理
**标签（tag）**，就是一个容易记住的名字，对应某个**commit**。
#### 创建标签
```
为指定commit打标签： $ git tag <tag-name> <commit-id>     // 不加 commit-id 默认为最近一次提交打标签
创建带有说明的标签：$ git tag -a <tag-name> -m "XXX" <commit-id>
查看所有标签： $ git tag
查看某标签信息： $ git show <tag-name>
```
#### 操作标签
```
推送本地标签： $ git push origin <tag-name>
推送全部未推送的标签：$ git push origin --tags
删除本地标签： $ git tag -d <tag-name>
若标签已经推送到远程，首先要删除本地标签，然后再删除远程标签：$ git push origin :<tag-name>
```




----
## 后记:
终于写完了，自学完Git教程，以及折腾了几天Markdown之后第一次做出点有用的东西来。  
最后提一句，**全文笔记总结自**：[廖雪峰的Git教程][source]，推荐点击学习！

-----

[installGit]:http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000 "安装Git"
[picA]:/img/A.jpg
[picB]:/img/B.jpg
[pic-stage&repo]:http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0
[pic0]:/img/0.png
[pic1]:/img/1.png
[pic3]:/img/3.png
[pic4]:/img/4.png
[pic5]:/img/5.png
[pic-ff]:/img/ff.png
[pic-no-ff]:/img/no-ff.png
[pic6]:/img/6.png
[source]:http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "点击查看来源"
---
layout: post
title:  "git使用笔记"
date:   2016-12-30 11:25:36 +0800
desc: "using git."
keywords: "Jekyll"
categories: [Mess]
tags: [git]
icon: icon-html
---  
## 概念
<span id="concept"></span>
***vcs:版本控制系统***  
　　通过使用git仓库，可以将文件或项目回溯到之前的状态，比较文件的变化细节。  

参考资料：  
1. [Pro git](https://git-scm.com/book/zh/v1)  
2. GitHub入门与实践  

常用命令：  
1. add:使用git add命令将文件加入暂存区，再通过git commit提交,git add -A | git add -u | git add -i
2. commit
3. push:执行push,git上的仓库就会被更新
4. clone
5. status
6. log：see what happened
7. pull and request:

##  1.基本操作
主要是记录GitHub入门与实践的第四章的操作。  
* git init：在一个目录下执行git init来初始化仓库。  
执行成功后会在该目录下生成.git目录。这个.git目录里存储着管理当前目录内容所需的仓库数据。这个目录中的内容被称为*“附属于该仓库的工作树”*。  
  
* git status:查看仓库的状态。  
![](http://i.imgur.com/DLFiaJ5.png)
尚没有可提交的内容，是因为当前我们建立的这个仓库中还没有记录任何文件的任何状态。建立一个README.md文件作为管理对象（似乎不是必要的），位第一次提交做前期准备。（touch命令是将已存在的文件的时间标签更新为系统当前的时间或创建新的空文件的命令）
	$touch README.md
  
* git add：向暂存区中添加文件。  
如果只是使用Git仓库的工作树创建了文件，那么该文件并不会被记录入Git仓库的版本管理对象当中，而显示在**Untracked files**里。  
要想让文件成为GIt仓库的管理对象,就需要使用git add命令将其加入暂存区(Stage 或者 Index)中。暂存区是提交之前的一个临时区域。
	$ git add README.md
	$ git status  
将文件加入暂存区后，git status命令的显示结果发生了变化，添加的文件显示在Changes to be committed中。
  
  
* git commit:保存仓库的历史记录  
　　git commit命令可以将当前暂存区中文件实际保存到仓库的历史记录中。通过这些记录，我们就可以在工作树中复原文件。

* ......记述一行提交信息  
	$ git commit -m "Rename files to english name"  
-m 参数后的""中称作提交信息，是对这个提交的概述。
  
* .....记录详细的提交信息  
想要记述得更加详细，请不加-m，直接执行git commit.执行后编辑器就会启动。  
提交信息的格式如下：
	* 第一行：用一行文字简述提交的更改内容
	* 第二行：空行
	* 第三行以后：记述更改的原因和详细内容
  
按照上面的格式输入， 今后边可以通过确认日志的命令或工具看到这些信息。

* git log:查看提交日志  
可以查看以往仓库中提交的日志，其中包含提交操作的哈希值，在其他命令中，在指向这个提交时会用到这个hash值。
	* git log --pretty=short:只显示提交信息的第一行
	* git log dirname（or filename）：只显示该目录下（文件）的日志
	* git log -p:显示文件的变动。eg.$ git log -p README.md
  
* git diff:查看工作树与暂存区更改前后的差别。在执行git commit之前先执行git diff HEAD命令，查看本次提交与上次提交之间有什么差别。HEAD是指向当前分支中最新一次提交的指针。
  
  

## 2.分支的操作
在进行多个并行作业时，我们会用到分支。master分支是Git默认创建的分支，因此基本上所有的开发都是以这个分支为中心进行的。
![](http://i.imgur.com/vcBzD0T.png)  
在不同的分支中，可以同时进行完全不同的作业。*等该分支的作业完成之后再与master分支合并。比如feature-A分支的作业结束后与master合并，如图4.2。  
![](http://i.imgur.com/RnDd7lR.png)  
  
* git branch:显示分支一览表
* git checkout -b :创建、切换分支，eg. 	$ git checkout -b feature-A,这条命令等价于下面两条命令：  
	$ git branch feature-A
	$ git checkout feature-A
* git checkout -:切换回上一个分支。


* 特性分支：集中实现单一特性，除此之外不进行任何作业的分支。
* 主干分支：是特性分支的原点，同时也是合并的终点，通常使用master分支作为主干分支。
  
  
* git merge:合并分支
假设feature-A已经实现完毕，想要把它合并到主干分支master中。首先切换到master分支。  
	$ git checkout master  
然后合并feature-A分支。为了在历史记录中明确记录下本次分支合并，需要创建合并提交。因此，在合并时加上**--no-ff**参数。  
	$ git merge --no-ff feature-A  
随后编辑器会启动，用于录入合并提交的信息。  
  
* git log --graph:以图表形式查看分支。  
  
  
## 3.更改提交的操作
* git reset:回溯历史版本  
要让仓库的HEAD、暂存区、当前工作树回溯到指定状态，需要用到git reset --hard命令。只要提供目标时间点的hash值，就可以完全恢复到该时间点的状态。  
	$ git reset --hard somehashvalue  
  
* 消除冲突
* 提交解决冲突后的结果：git add,git commit
* git commit -amend:修改提交信息。  
  
## 4.推送至远程仓库
* git remote add:添加远程仓库
	$ git remote add origin git@github.com:username/repositoryname.git  
执行上述命令之后，Git会自动将目标远程仓库名称设置为origin。  
  
* git push :推送至远程仓库
	* 推送至master分支：假设在master分支下进行操作  
		$ git push -u origin master
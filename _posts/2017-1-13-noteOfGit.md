---
layout: post
title:  "git使用笔记"
date:   2017-01-13 11:25:36 +0800
categories: mess
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

##  基本操作
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
	
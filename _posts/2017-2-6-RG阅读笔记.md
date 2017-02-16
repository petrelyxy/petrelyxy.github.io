---
layout: post
title:  "RG阅读笔记!"
date:   2017-02-07 11:25:36 +0800
categories: RG
---
RG项目使用Django框架来做开发，Django框架使用的为MTV模式（Models，Templates，Views），与MVC模式类似  
* **Model** ：负责业务对象和数据库的关系映射（ORM） 。   
* **Template** ： 负责如何把页面展示给用户（html）。    
* **View** ： 负责业务逻辑。  
***需要一个URL分发器***，将URL的页面请求分发给不同的View处理。在RG项目中，它位于project/urls.py    
  
值得注意的是，使用的数据库为SSDB，而Django并没有能与SSDB良好交互的api，所以此项目中并没有定义数据对象的Model，而是在/ResearchGo/BaseSSDB.py中封装了SSDB数据库的CRUD等操作。所以Django自带的后台管理功能也许无法良好的利用。  本文主要关注与View。  
  
在已经写好的项目文件中，view文件位于'ResearchGo/'而不是'ResearchGo/view/'下。  
  
/ResearchGo下主要负责数据库操作封装，业务逻辑和用户交互。  
/server下主要负责处理爬取好的数据，并将它们更新到数据库中。
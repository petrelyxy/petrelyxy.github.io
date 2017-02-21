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

## [页面跳转：urls.py](http://python.usyiyi.cn/translate/django_182/ref/urls.html#url)  
### patterns()
主要使用了patterns()函数（自django1.8版本已被启用），urlpatterns应该是django.conf.urls.url()实例的简单列表。  
一个函数，它接受前缀和任意数量的URL模式，并返回django需要的格式的URL模式列表。  
  
patterns()的第一个参数是字符串prefix。示例如下：  
	from django.conf.urls import patterns, url
	urlpatterns = patterns('',  
		url(r'^articles/([0-9]{4})/$', 'news.views.year_archive'),
		url(r'^articles/([0-9]{4})/([0-9]{2})/$', 'news.views.month_archive'),
		url(r'^articles/([0-9]{4})/(0-9){2})/([0-9]+)$', 'news.views.article_detail'),
)

在此示例中， 每个示例都有一个**公共前缀**-‘news.views’。可以使用patterns()函数的第一个参数来指定要应用于每个视图函数的前缀， 而不是在urlpatterns中为每个条目输入该值。  
则上面的例子可以更简洁地写成：  
	from django.conf.urls import patterns, url
	urlpatterns = patterns('news.views',  
		url(r'^articles/([0-9]{4})/$', 'year_archive'),  
		url(r'^articles/([0-9]{4})/([0-9]{2})/$', 'month_archive'),  
		url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', 'article_detail),  
)
  
patterns()是一个函数调用，它最多接受255参数。意识到patterns()返回一个Python列表，所以可以拆分列表的构造。
	urlpatterns = patterns('',  
	...
	)  
	urlpatterns += patterns('',  
	...
	)  
### url()  
url(regex, view, kwargs = None, name = None, prefix =")[source]
urlpatterns应该是url()实例的列表。例如  
urlpatterns = [
	url(r'^index/$', index_view, name = "main-view"),  
]


---
layout: post
title:  "Python 笔记"
date:   2017-01-31
desc: "some gain from learning"
keywords: "Python"
categories: [Python]
tags: [self-learning,Python]
icon: icon-html
---
# python笔记

* [函数](#function)
* [Modules,Classes and Objects](#ex40)


## 传值还是传引用
python不允许程序员选择采用传值还是传引用。Python参数传递采用的肯定是“传对象引用”的方式。这种方式相当于传值和传引用的一种综合。如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值－－相当于通过“传引用”来传递对象。如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用，就不能直接修改原始对象－－相当于通过“传值'来传递对象。

<span id ="function"></span>
## 函数

### split():  
 split(s [,sep [,maxsplit]]) -> list of strings

    Return a list of the words in the string s, using sep as the
    delimiter string.  If maxsplit is given, splits at no more than
    maxsplit places (resulting in at most maxsplit+1 words).  If sep
    is not specified or is None, any whitespace string is a separator.

### sorted():  
	 sorted(iterable, cmp=None, key=None, reverse=False) --> new sorted list  
### filter():  
filter()函数接受一个函数f和一个list，函数f的作用是对每个元素进行判断，返回True或False，filter根据判断结果自动过滤掉不符合条件的元素，返回由符合条件元素组成的新list。

### lambda():
lambda函数也叫匿名函数，lambda [arg1[,arg2,...]]:expression,冒号前是参数，可以由多个，由逗号隔开，冒号右边为返回值。

<span id = 'ex40'></span>
## Modules,Classes and Objects
参考[LearnPythonTheHardWay][hrefofLearnPythonTheHardWay#40]  
### Modules Are Like Dictionaries
You can think about a module as a specialized dictionary that can store Python code so you can access it with . operator.
1. A python file with some function or variables in it  
2. You import that file  
3. And you can access the functions or  variables in that module with the . operator.  
this means dictionaries and Modules have a very common pattern in Python:  
1. Take a key = value style container.
2. Get something out of it by the key's name.  
In the case of the dictionary,the key is a string and the syntax is \[key].In the case of module,the key is an identifier,and the syntax is .key.Other than that they are nearly the same thing.  

### Classes Are Like Modules
A class is a way to take a grouping of functions and data and place them inside a container so you can access them with the .operator.  

*Here's why classes are used instead of modules*: You can take a class and use it to craft many of them and each one won't interfere with each other.When you import a module there is only one for the entire program unless you do some monster hacks.

### Objects Are Like Import
When you instantiate a class what you get is called an object.You instantiate(create) a class by calling the class like it's a function,like this:
	class MyStuff(object):  
		def __init__(self):  
			self.tangerine = "And now a thousand years between."  

		def apple(self):
			print "I AM CLASSY APPLES!"

### python中的super
python中的子类需要显示的调用父类的构造器，eg.:  
class Person(object,name):
	def __init__(self,name):
		self.name = name
	pass

class Student(Parent):
	def __init__(self,name):
		super(Student,self).__init__(name)
		print "I'm a student,My name is ",self.name

### Inheritance Versus Composition (继承vs组合)
Most of the uses of inheritance can be simplified or replaced with composition, and multiple inheritance should be avoided at all costs.
#### 1.Inheritance
There are three ways that the parent and child classes can interact:  
1. Actions on the child imply an action on the parent.  
2. Actions on the child override the action on the parent.  
3. Actions on the child alter the action on the parent.   

	class Parent(object):
		def override(self):
			print "PARENT override()"
		def implicit(self):
			print "PARENT implicit()"
		def altered(self):
			print "PARENT altered()"
	class Child(Parent):
		def override(self):
			print "Child override()
		def altered(self):
			print "Child,before Parent altered()"
			super(Child,self).altered()
			print "Child,after Parent altered()"
	dad = Parent()
	son = Child()
	dad.implicit()
	son.implicit()

	dad.override()
	son.override()

	dad.altered()
	son.altered()  

#### When to Use inheritance or composition
The question of "inheritance Versus composition" comes down to **an attempt to slove the problem of reusable code**.inheritance solves this problem by creating a mechanism ofr you to have implied features in base classes.Composition solves this by giving you modules and the ability to call function in other classes







-----
[hrefofLearnPythonTheHardWay#40]:https://learnpythonthehardway.org/book/ex40.html

---
title: Hexo 源码分析1
date: 2016-03-16 21:31:09
tags:
	- nodejs
	- mongodb
categories: nodejs
---


## 资源

 - [8天学通MongoDB](http://www.cnblogs.com/huangxincheng/archive/2012/02/18/2356595.html)
 
 
### 开始与安装

mongodb的安装很简单,到官网下载后用shell启动

我们要新建立一个**db**文件夹
```
cd mongodb\bin
mongod --dbpath=E:\mongodb\db
```

这里说可以用浏览器访问,我没有成功

还是在shell 里用**mongo** 命令来访问吧

## 基本操作

这是mongodb 的三层结构
 > 数据库<---集合<---文档
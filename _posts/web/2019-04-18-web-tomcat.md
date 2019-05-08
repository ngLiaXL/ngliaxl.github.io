---
layout: post
title:  "IDEA社区版编译Tomcat源码"
date:   2019-04-18 11:53:27 +0800
categories: web
---

###	下载 [Intellij IDEA](https://www.jetbrains.com/idea/)
[https://www.jetbrains.com/idea/]
### 下载 [Tomcat源码](https://github.com/apache/tomcat)
配置环境变量`CATALINA_HOME=C:\work\tomcat`
### 下载 [Apache Ant](http://ant.apache.org/)
配置环境变量`ANT_HOME=C:\app\apache-ant-1.10.5`
### 编译Tomcat源码
tomcat根目录执行ant等待编译完成，启动bin目录下的startup.bat
### 导入IDEA
编译时会缺失各种jar，到相关网站下载即可，一些jar在ant命令时已经下载，最后报一些junit相关的错误，可以直接移除test这个module
### 运行
Run-->Bootstrap.java 直接启动，不需要任何配置








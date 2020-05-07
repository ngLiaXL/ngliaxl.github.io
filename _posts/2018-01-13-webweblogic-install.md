---
layout: post
title:  "[Java]	Weblogic Installation"
date:   2018-01-13 11:53:27 +0800
categories: web
---

### 下载 fmw_12.1.3.0.0_wls.jar 支持jdk1.8的稳定版本
Jdk1.8

DOS窗口执行```java -jar  fmw_12.1.3.0.0_wls.jar```
![](/assets/web-img08.png)

安装目录```\wlchome```

### 新建域
![](/assets/web-img09.png)

域目录```wlchome\user_projects\domains\easd```

### 启动服务

```wlchome\user_projects\domains\easd\bin\startWebLogic.cmd```

### 测试服务

```localhost:7001/console/```

### 安装带weblogic插件的Eclipse
Eclipse weblogic server tools 插件eclipse版本4.7.1a oxygen

[下载地址](http://download.oracle.com/otn_software/oepe/oxygen/)
![](/assets/web-img10.png)

点开Tools下来菜单，选择Oracle Weblogic Server Tools，点击Next
新建weblogic运行环境


![](/assets/web-img11.png)

### 新建项目
![](/assets/web-img12.png)


### Run on server


![](/assets/web-img13.png)





如遇到以下异常

```java.lang.OutOfMemoryError: GC overhead limit exceeded```

修改weblogic启动内存

```doman--setDomainEnv.cmd``` 修改内存大小

```set WLS_MEM_ARGS_64BIT=-Xms512m -Xmx1024m```

```set WLS_MEM_ARGS_32BIT=-Xms512m -Xmx1024m```






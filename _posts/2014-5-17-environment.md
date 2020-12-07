---
layout: post
title:  "Environment Variables"
date:   2014-05-17 +0800
categories: tools
---


### Java
[下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

安装目录
```
--jdk1.8.0_101
--jre8
```

环境变量
```
JAVA_HOME					D:\app\Java\jdk1.8.0_101
CLASS_PATH					.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
%JAVA_HOME%\bin;
%JAVA_HOME%\jre\bin;
```


### Android Studio
[下载地址](https://developer.android.google.cn/studio/index.html)

环境变量
```
GRADLE_HOME					D:\.gradle\wrapper\dists\gradle-4.1-all\bzyivzo6n839fup2jbap0tjew\gradle-4.1
%GRADLE_HOME%/bin;
```


```
ANDROID_SDK_ROOT
	C:\app\androidsdk
Path
	%ANDROID_SDK_ROOT%\platform-tools
	%ANDROID_SDK_ROOT%\tools


JAVA_HOME
	C:\app\Java\jdk1.8.0_201
Path
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

自定义字体等
```
Font
	Setting--Apperance&Behavior
		Override default fonts by(not recommended) Size 16
	Editor--Font Size 16
	Editor--Color Scheme--General--Text--Default Text Bold
```




#### .AndroidStudio文件夹的修改
进入Android Studio的安装目录，进入bin文件夹，用文本编辑软件打开idea.properties参考下图修改
![](/assets/tools-img01.png)



#### .gradle 文件夹修改  
`安装完成打开设置 Build -->Build Tools -->Gradle 选择Service directory path`

### MAC ENV

```
# Java
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
# Android
export ANDROID_SDK_ROOT=/Users/xianglong.liang/Library/Android/sdk
export PATH=$PATH:$ANDROID_SDK_ROOT/tools
export PATH=$PATH:$ANDROID_SDK_ROOT/platform-tools
export AAPT_HOME=~/Library/Android/sdk/build-tools/30.0.2
export PATH=$PATH:$AAPT_HOME  
# HomeBrew
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
export PATH="/usr/local/bin:$PATH"
export PATH="/usr/local/sbin:$PATH"
# HomeBrew END
```

### NodeJs
[下载地址](https://nodejs.org/en/)

```
D:\work\nodejs\;
D:\work\nodejs\node_global;
```

#### 设置 cache global
在nodejs的安装目录下，新建node_global和node_cache两个文件夹

```
npm config set cache "...\nodejs\node_cache"
```

#### 设置global和cache
```
npm config set prefix "...\nodejs\node_global"
```  
#### 安装VUE
```
npm install -g cnpm --registry=https://registry.npm.taobao.org 
cnpm install vue -g
cnpm install vue-cli -g
```




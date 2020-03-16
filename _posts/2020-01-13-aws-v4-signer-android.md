---
layout:	post
title:	"[Android]	AWS V4 Signatures"
date:	2020-01-13
---
An OkHttp interceptor for signing requests with AWSv4 signatures

[官方文档地址](https://docs.aws.amazon.com/zh_cn/general/latest/gr/sigv4_signing.html)

### 拦截器
[项目地址](https://github.com/ngLiaXL/AWS4Signer)

### 保存key到so文件中
-	加密Key保存在so文件中
-	在NDK中通过反射Java API 校验签名
-	在NDK中通过反射解密Key

-	生成.h 文件
```
\app\src\main\java>javah -jni com.ngliaxl.signer.Keys
```


```
\app\src\main\jni
	Android.mk
	Application.mk
	Keys.cpp
	com_ngliaxl_signer_Keys.h
```

-	Android.mk
```
LOCAL_PATH:=$(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE := Keys
LOCAL_SRC_FILES := Keys.cpp
include $(BUILD_SHARED_LIBRARY)
```

-	Application.mk
```
APP_ABI := all
```

-	Keys.cpp

-	com_ngliaxl_signer_Keys.h

-  编译
```
\app\src\main\jni>ndk-build
```


	


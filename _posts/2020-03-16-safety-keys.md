---
layout:	post
title:	"[Android]	SafetyKeys"
date:	2020-03-16
---
一些私密信息加密保存到so文件中，通过简单的反射机制解密信息防止so文件的盗用。

###	新建 SafetyKeys.java
```
package com.ngliaxl.safetykeys;

public class SafetyKeys {

    static {
        System.loadLibrary("SafetyKeys");
    }
    public native String nativeAccessKey();

}
```

### 新建DES.java 解密类
```

import android.util.Base64;

import java.io.IOException;

import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;


public class DES {


    public static String encrypt(String str, String key) {
        String encrypt = null;
        try {
            encrypt = jdkBase64String(encrypt2(str, key));
        } catch (Exception e) {
            e.printStackTrace();
        }
        return encrypt;
    }

    public static String decrypt(String str, String key) {
        String decrypt = null;
        try {
            decrypt = decrypt2(jdkBase64Decoder(str), key);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return decrypt;
    }


    /**
     * 使用DES对字符串加密
     *
     * @param str utf8编码的字符串
     * @param key 密钥（56位，7字节）
     */
    public static byte[] encrypt2(String str, String key) throws Exception {
        if (str == null || key == null) {
            return null;
        }
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(key.getBytes("utf-8"), "DES"));
        byte[] bytes = cipher.doFinal(str.getBytes("utf-8"));
        return bytes;
    }

    /**
     * 使用DES对数据解密
     *
     * @param bytes utf8编码的二进制数据
     * @param key   密钥（16字节）
     * @return 解密结果
     * @throws Exception
     */
    public static String decrypt2(byte[] bytes, String key) throws Exception {
        if (bytes == null || key == null)
            return null;
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");
        cipher.init(Cipher.DECRYPT_MODE, new SecretKeySpec(key.getBytes("utf-8"), "DES"));
        bytes = cipher.doFinal(bytes);
        return new String(bytes, "utf-8");
    }

    /**
     * 使用base64解决乱码
     *
     * @param secretKey 加密后的字节码
     */
    public static String jdkBase64String(byte[] secretKey) {
        return Base64.encodeToString(secretKey, Base64.DEFAULT);
    }

    /**
     * 使用jdk的base64 解密字符串 返回为null表示解密失败
     *
     * @throws IOException
     */
    public static byte[] jdkBase64Decoder(String str) throws IOException {
        return Base64.decode(str, Base64.DEFAULT);
    }

}
```

###	生成.h

your project path\app\src\main\java>`javah -jni com.ngliaxl.safetykeys.SafetyKeys`
```
com_ngliaxl_safetykeys_SafetyKeys.h
```
your project path\app\src\main\ 新建`jni`文件夹
将`com_ngliaxl_safetykeys_SafetyKeys.h`复制到`jni`

### 新建.cpp

`jni`文件夹下新建SafetyKeys.cpp 

```
#include <jni.h>
#include <com_ngliaxl_safetykeys_SafetyKeys.h>

extern "C" {
/*
 * Class:     com_ngliaxl_safetykeys_SafetyKeys
 * Method:    nativeAccessKey
 * Signature: ()Ljava/lang/String;
 */
JNIEXPORT jstring JNICALL Java_com_ngliaxl_safetykeys_SafetyKeys_nativeAccessKey
  (JNIEnv * env, jobject obj){
 jclass des_class = env->FindClass("com/ngliaxl/safetykeys/DES");
    if (env->ExceptionCheck()) {
        env->ExceptionClear();
      return NULL;
    }

    jmethodID decrypt_id = env->GetStaticMethodID(des_class, "decrypt", "(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;");
    if (env->ExceptionCheck()) {
        env->ExceptionClear();
      return NULL;
    }
    jstring access_key = env->NewStringUTF("urBJqAZ7vwf3KZT8UY7zTeDlQMT4hQRR");
    jstring des_key = env->NewStringUTF("nullnull");

    jstring decrypt_key = (jstring) env->CallStaticObjectMethod(des_class, decrypt_id, access_key,des_key);
    if (env->ExceptionCheck()) {
        env->ExceptionClear();
      return NULL;
    }
    return decrypt_key;

  }

}
```

### 新建 Android.mk Application.mk
Android.mk
```
LOCAL_PATH:=$(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE := SafetyKeys
LOCAL_SRC_FILES := SafetyKeys.cpp
LOCAL_LDLIBS := -llog
include $(BUILD_SHARED_LIBRARY)
```
Application.mk
```
APP_ABI := all
```
###
your project path\app\src\main\jni>`ndk-build`



	


---
layout: post
title:  "build.gradle配置多渠道包"
date:   2018-06-13 11:53:27 +0800
categories: android
---

```
android {
    compileSdkVersion 27
    defaultConfig {
        // ...
    }
    signingConfigs {
        debug {
            // No debug config
        }
        // gradlew assembleRelease
        release {
            storeFile file('Your Keystore path')
            storePassword "Your store password"
            keyAlias "Your alias name"
            keyPassword "Your key password"
        }
    }
    buildTypes {
        debug {
            minifyEnabled false
            signingConfig signingConfigs.release
        }
  
    }
    flavorDimensions "default"
    productFlavors {
        baidu {dimension "default"}
        yyb {dimension "default"}
    }
 
 
    productFlavors.all { flavor ->
        flavor.manifestPlaceholders = [PBOX_CHANNEL_VALUE: name]
    }
 
 
    applicationVariants.all { variant ->
        variant.outputs.all {
            def apkName = 'xxxx' + '-' + variant.versionName
            if (!variant.flavorName.isEmpty()) {
                apkName += ('-' + variant.flavorName)
            }
            outputFileName = apkName + '.apk'
        }
    }

}
```

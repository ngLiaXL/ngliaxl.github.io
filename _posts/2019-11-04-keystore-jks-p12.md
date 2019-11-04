---
layout:	post
title:	"[Android]	Convert a JKS Keystore to a PKCS12 (.p12) format"
date:	2019-11-04
---

The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore -destkeystore -deststoretype pkcs12"


```
keytool -importkeystore -srckeystore [MY_KEYSTORE.jks] -destkeystore [MY_FILE.p12] -srcstoretype JKS -deststoretype PKCS12 -deststorepass [PASSWORD_PKCS12]
keytool -importkeystore -srckeystore test.jks -destkeystore test.p12 -srcstoretype JKS -deststoretype PKCS12 -deststorepass 123123
```
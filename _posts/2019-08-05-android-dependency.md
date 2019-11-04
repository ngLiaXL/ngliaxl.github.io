---
layout:	post
title:	"[Android]	Display Dependency Tree"
date:	2019-08-05
---
###	使用命令直接查看	

`gradle :app:dependencies --configuration releaseCompileClasspath`	

`gradle :app:dependencyInsight --dependency 'your particular dependency(or dependencies)'  --configuration releaseCompileClasspath`	

### 项目gradle文件中增加 DependencyReportTask task	

```	```
subprojects{
    task DRT(type : DependencyReportTask){
    }
}
```
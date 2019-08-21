---
layout: post
title:  "Source Code"
date:   2014-04-25 11:53:27 +0800
categories: android
---


### 1、位运算运用-权限
```
public class Permission {

    public static final int READ = 1 << 0; // 0001

    public static final int WRITE = 1 << 1; // 0010

    private int permission;

    /**
     * 设置权限
     */
    public void set(int permission) {
        this.permission = permission;
    }

    /**
     * 添加权限
     */
    public void add(int permission) {
        this.permission |= permission;
    }

    /**
     * 删除权限
     */
    public void cancel(int permission) {
        this.permission &= ~permission;
    }

    /**
     * 是否有此权限
     */
    public boolean allow(int permission) {
        return (this.permission & permission) == permission;
    }

    /**
     * 是否没有此权限
     */
    public boolean notAllow(int permission) {
        return (this.permission & permission) == 0;
    }

    /**
     * 是否只拥有此权限
     */
    public boolean onlyAllow(int permission) {
        return this.permission == permission;
    }


}
```

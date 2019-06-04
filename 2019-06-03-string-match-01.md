---
layout: post
title:  "字符串模糊匹配-朴素/KMP算法"
date:   2019-06-3
---


### 朴素
有字符串AABCDEF

输入： AAF
输出： true

输入： AFE
输出： false

```
    public static boolean match(String target, String input) {
        if (input == null || "".equals(input)
                || target == null || "".equals(target)) return false;
        if (target.length() < input.length()) return false;

        int i = 0, j = 0;
        while (i < target.length() && j < input.length()) {
            if (target.charAt(i) == input.charAt(j)) {
                i++;
                j++;
            } else {
                i++; // 不相等主串向后移动
            }
        }
        if (j >= input.length()) {
            return true;
        }
        return false;
    }
```

输入： AAF
输出： false

输入： ABC
输出： true
```
    public static boolean match2(String target, String input) {
        if (input == null || "".equals(input)
                || target == null || "".equals(target)) return false;
        if (target.length() < input.length()) return false;

        int i = 0, j = 0;
        while (i < target.length() && j < input.length()) {
            if (target.charAt(i) == input.charAt(j)) {
                i++;
                j++;
            } else {
                i = i-j+1;// 当前位置-匹配成功个数 + 1 = 上次开始匹配的下一个字符
                j = 0;

            }
        }
        if (j >= input.length()) {
            return true;
        }
        return false;
    }
```


### KMP
...TODO











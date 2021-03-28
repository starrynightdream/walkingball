---
title: c++编码规范小记
date: 2021-03-28 20:38:27
tags:
- 技术
    - c++
thumbnail:
   /walkingball/images/14a0966889e255_1_post.jpg
---

# 待补充，作记录用

## 防卫式声明

    #ifndef __NAME__
    #define __NAME__

    code

    #endif //__NAME__

## 构造方法使用  ：特性 提高性能

    A(int a): B(a) { }

先写这么多
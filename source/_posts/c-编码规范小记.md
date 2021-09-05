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

## 参数与返回值使用引用
注意放回值使用引用的时候，引用不会因为是局部变量而销毁。
对参数，则请思考是否会改变，是否应该改变，并应情况添加const等修饰。

    A& func (const A& a)
    {
        return a;
    }


## 不会发生变更的参数和方法添加const
如果使用者使用const的对象传入参数/调用方法，却发现报了不可以改变的错误，这就不合逻辑

当然使用const_cast 也是个不错的想法

    // 一个不进行任何修改的的函数
    void func (const ClassName name) const {}

## + - * / 等运算的重载可写成全局
避免冲突，在全局重载又在类内部重载会产生二义性。

## 头文件和实现文件

[大佬的文章，这里做一个记录][head_and_cpp]

[head_and_cpp]: https://www.cnblogs.com/ider/archive/2011/06/30/what_is_in_cpp_header_and_implementation_file.html
先写这么多
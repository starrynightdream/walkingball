---
title: md写法测试与记录
date: 2021-04-17 18:32:39
tags:
- test
thumbnail:
   /walkingball/images/17a638ccc65014_1_post.jpg
---
# 原意是记录一下书写方法

但拿来主义认为先测试使用，之后再整理会比较好

## 基本

![image](/walkingball/images/17a638ccc65014_1_post.jpg)

## 数学公式测试书写与记录

$$ x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} \tag{1.1} $$

$ \tan 50 $

$ \log_x{y} $

$ x^{y^z} = (1+e^x)^{-2xy^w} $

$ x\div y$

$ {x} \over {y} $
$ \frac {x}{y} $

$ \sum_{x=0}^{y} x^2 $

$ \cdots $

$ \cdots $

$ \lim_{n\rightarrow+\infty} ax^n $
$ \lim\limits_{n\to0} {\sin{x} \over x }$ = 1

$ \prod_{x=1}^{y} ax^n $

$ \frac{du}{dt} and \frac{d^2 u}{dx^2} $

$ \vec{a}$

$ \int_x^{x^2} y^4 {\rm d} x $

---------------

## 流程图

graph TD
    A[Start] --> B{Is it?};
    B-->|Yes|C[OK];
    C-->D[Rethink];
    D-->B;
    B-->|No|E[End];
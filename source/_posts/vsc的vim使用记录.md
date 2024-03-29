---
title: vsc的vim使用记录
date: 2021-06-12 23:43:35
tags:
- 技术
   + vim
thumbnail: /walkingball/images/vim_magic.jpg
---
# 特别声明
虽然是特指了Visio studio code 的vim插件，但其实其同真正的vi，vim差别也不是很大。因此做一个快速入门，便捷查找的笔记在这。

顺便一提这里在不定长挖坑

# 使用介绍
根据功能划分vim的操作，分别归类于各个标题之下。希望快速入门则将各个主题的前部分同之后的概念部分过一遍。查找命令则根据分类查找。

强烈建议边开vim边操作。（万一我错了呢 =w=

## linux vi/vim 一图流
这里的是面向linux的，和本文vsc的vim插件可能有些许不同。

![img][vim_pict]

## 正文
### 概念
vim 编辑器存在三种模式，编辑，普通和命令，在这里不细说

vim 的使用是逻辑化的。

### 光标移动
#### 上下左右
k j h l 分别为上下左右
w 为移动到下一个字，即移动至断开后下一个开头 
W 为不认同标点为断开的 w
b 为回退到上一个字，即移动至开头
B 为不认同标点为断开的 b

#### 其他
' 为移动光标，可配合寄存器使用。'' 这样连按则是快速回到上一次位置。
m 为记录光标的位置。配合寄存器使用。
M 为回到屏幕正中行处。

### 屏幕移动
这是光标不动，将屏幕滑动到相应位置
zz 将屏幕滑动，使得光标在屏幕正中
zt 将屏幕滑动，使得光标在屏幕顶部
zb 将屏幕滑动，使得光标在屏幕最下

### 编辑
#### 进入编辑模式
o 光标下方新开一行  O 光标上方新开一行
a 在光标后方追加    A 该行后方追加
i 在光标之前追加    I 改行之前追加
s 替换当前光标字符并开始追加 S 替换当前光标该行并开始追加
J 为将下一行快速移动到这行行末，就是把行尾的换行删了。

### 命令
#### 数字 + 命令
重复对应次数的命令

#### 寄存器
寄存器是一个概念，比如录制宏的时候，使用a作为寄存器，就是在a这个位置记录一串操作。
```
q a // 指定 a 为寄存器
[操作]
q // 结束
```
当然，每种操作的寄存器是可以重复使用不会互相覆盖的。

#### 宏录制重复操作

q 为进入录制状态与结束录制。后加寄存器
@ 为运行寄存器中的脚本。后加寄存器。如 @@ 连按则意味执行上次执行的寄存器里的脚本。

### 参考
在学习过程中遇到许多，有些记下来了，有些不小心漏了。。 =w=

[a-z的学习法](https://0xzx.com/202001070446443282.html/amp)
[菜鸟也是王道了，一图流也是它的](https://www.runoob.com/linux/linux-vim.html)



[vim_pict]:/walkingball/innerPict/vim/vi-vim-cheat-sheet-sch.gif
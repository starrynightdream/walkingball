---
title: unity使用VSCode
date: 2021-03-05 23:41:07
tags:
- 技术
    - unity
    - 环境配置
thumbnail: /walkingball/images/65265727_p0.png
---
# 有官方的步骤
[官方的文档在这](https://code.visualstudio.com/docs/other/unity)
这里的环境是： win10 + unity2019，但估计差别不大。

首先是设置unity
1. Unity Edit-> Preferences-> External Tools
2. External Script Editor 设置为vsc

之后安装下列插件：
1. C#
2. Debugger for Unity
3. Unity Tools
4. Unity Code Snippets
5. Unity Snippets

若结束安装后依旧没有成功显示代码提示/补全，则可尝试查看 generate Unity Preferences 中 .csproj files for是否勾选local package。当然也可以尝试全勾上。再点击下方generate...后重新打开项目查看。
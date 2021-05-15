---
title: socket.io错误略记
date: 2021-05-15 11:09:33
tags:
    - 技术
        - socket.io
        - 错误
thumbnail: /walkingball/images/1d39f0c5f25e64_1_post.jpg
---

# npm 下载错误
个人感觉比较关键的信息是：
- npm安装ws 或 socket.io 运行到最后突然停住。应该是下载正常但运行安装不正常。
- node-gyp rebuild 命令错误
- bufferutil 报错
- npm安装express正常
- npm安装发生错误后下载的部分会被删除

个人查了一下，有许多的方法但并没有解决问题。但是使用cnpm下载虽然也会出现错误，但是并不会删除下载的结果

也就是说，用cnpm下载后运作正常了。使用这个办法运行了socket.io内提供的示例成功。

    个人版本说明
    cnpm@6.1.1
    npm@6.14.11
    node@14.6.0
---
title: c++函数笔记
date: 2021-03-21 10:24:58
tags:
- 技术
    - c++
    - 笔记
thumbnail: /walkingball/images/1a9356aae46c04_1_post.jpg
---
自己稍加整理

# ASCII 重点字符
- '0' = 48
- 'A' = 65
- 'a' = 97

# 字符串转数字
atoi() 和 strtol()是c有的

    string str = "16s";
    int a = atoi(str.c_str());
    int b = strtol(str.c_str(), nullptr, 10);

    // a = 16, b = 16
    // 无异常抛出

stoi c++的

    int c = stoi("123");    
    // c = 123
    int k = stoi("12a");
    // 抛出异常

stringstream 功能强大

    stringstream ss;
    int ans; string tar = "123";
    ss << tar; ss >> ans;
    // ans = 123;
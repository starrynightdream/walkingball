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
主要作用是速览而非具体的功能介绍，真正十分在意应该前往进行细节查找

# ASCII 重点字符
- '0' = 48
- 'A' = 65
- 'a' = 97

# 字符串转数字
atoi() 和 strtol()是c有的

```c++
    string str = "16s";
    int a = atoi(str.c_str());
    int b = strtol(str.c_str(), nullptr, 10);

    // a = 16, b = 16
    // 无异常抛出
```

stoi c++的

```c++
    int c = stoi("123");    
    // c = 123
    int k = stoi("12a");
    // 抛出异常
```

stringstream 功能强大

```c++
    // 头文件 <sstream>
    stringstream ss;
    int ans; string tar = "123";
    ss << tar; ss >> ans;
    // ans = 123;
```

# string 用法记录
构造函数
```c++
    string str; // 空字符串

    string str1('a', 9); //重复的9个a

    string str2("hello world"); // "hello world" 字符串

    string str3("abcd", 3); // "abc"

    string str4("abcde", 1, 2); // "bc"

    string str5(str1); //"aaaaaaaaa"
```

c_str() 获取字符串等价的 char*

```c++
    string a = "hello world";
    const char* a_to_p = a.c_str();
```

char* char[] 转string 可直接进行赋值
```c++
    const char* pszName = "liitdar";
    char pszCamp[] = "alliance";
    strName = pszName;
    strCamp = pszCamp;
```

字符串比较 compare
```c++
    string a = "a"; string b = "b";
    int ans = a.compare(b); // 0 为相等
```
# algorithm

## partial_soct
局部排序
对从第一个参数到第三个参数范围内进行查询，并完成第一个到第二个参数之间的排序
第四个参数为自定义的判断函数

```c++
partial_sort(vec.begin(),vec.begin() + 5, vec.end());
```

## nth_element
会使在 参数一 到 参数三 范围内的第 参数二 个小的值放在 参数二 位置上
类似将小于 参数二 的在左边 大的在右边

```c++
nth_element(a + 1, a + 4, a + 8);
```

# STL操作

## emplace_back
与 push_back 一致向尾部添加一个元素，不同的是它会直接在插入时调用构造而不是先调用生成一个临时对象在调用移动构造函数。

```c++
elections.emplace_back(arg1, arg2...);
```

# STL容器

## unordered_map<T1, T2>
无序表，底层为哈希，逻辑存储键值对。存在默认值。
```c++
unordered_map<string, int> c_set;
```
---
title: c++的边学边记
date: 2021-09-05 13:36:59
tags:
- 技术
    + c++
thumbnail: /walkingball/images/86907186_p1.jpg
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

- [c++从头再学。按章节记录在意的知识点](#c从头再学按章节记录在意的知识点)
  - [未归类知识点](#未归类知识点)
  - [第 2 章 简单程序设计](#第-2-章-简单程序设计)

<!-- /code_chunk_output -->

# c++从头再学。按章节记录在意的知识点

## 未归类知识点
### volatile 关键字
指示变量易变，程序应该每次使用时都去地址处读取。
当一个变量使用频繁的时候，编译器可能会进行优化，将其放置在寄存器中。但是如果这个变量是由其它程序进行修改的，可能就会使得这个变量不能同步修改。
它也会阻止编译器进行常量合并
因此建议在使用由其它程序修改的变量时，使用 $ volatile $ 关键字

[参考文章][volatile]

## 第 2 章 简单程序设计

### 四种强制转换
四者均采用 $cast-name<typename>(express)$ 的形式调用
#### const_cast (常量转换)
- 编译时处理
- 可用于基本类型转换
    + 常量指针或引用转换为非常量指针或引用，指向原对象
```c++
    using namespace std;

    void Printer (int* val,string seperator = "\n")
    {
        cout << val<< seperator;
    }

    int main(void) 
    {	
        const int consatant = 20;
        //Printer(consatant);//Error: invalid conversion from 'int' to 'int*'
        Printer(const_cast<int *>(&consatant));
        
        return 0;
    }
```
#### dynamic_cast (动态转换)
- 运行时处理
- 不用于基本数据转换
- 可对类进行父子转换
    + 基类有虚函数方编译通过
    + 可向下转换返回子类指针，否则未NULL
    + 向上转换正常。
```c++
    dynamic_cast<int>(b);
    // 可参照后文static_cast的转换
```
#### reinterpret_cast (重解释)
- 编译时处理
- 不能去除const， volatile 属性
- 仅拷贝比特位
```c++
    unsigned short Hash(void * p)
    {
        unsigned int val = \
         reinterpret_cast<unsigned int>(p);
        return (unsigned short) val;
    }

    int main ()
    {
        int a[20];
        for (int i=0; i<20; ++i)
            cout<< Hash(a + i) <<endl;
    }
```
#### static_cast (静态转换)
- 编译时处理
- 可用于基本类型转换
    + 不能去除const , volitale 属性
- 可对类进行父子转换
    + 向下转换不保证安全
    + 向上转换正常。
- 基础类型和对象的转换
    + 通过调用 $ 构造函数 $ 与 $ 类型转换函数实现 $
```c++
    class Parents
    {
    public:
        virtual ~Parents(){}
        /*codes here*/
    };

    class Children : public Parents
    {
        /*codes here*/
    };

    int main() 
    {	
        Children * daughter = new Children();
        Parents * mother = static_cast<Parents*> (daughter); //right, cast with polymorphism
        
        Parents * father = new Parents();
        Children * son = static_cast<Children*> (father); //no error, but not safe
    }
```

```c++
    int main(void) 
    {
        Ape a;
        Human h = static_cast<Human> (a); // using promtion constructor

        Programmer p;
        p = static_cast<Programmer> (h); // using  Programmer-cast operaotor

        //Ape a2;
        //a2 = static_cast<Ape> (p); //Error, assignment operator should be used directly

        return 0;
    }
```

### 简单IO格式控制
| 操作符名 | 含义 |
| - | - |
| dec | 数值采用10进制表示 |
| hex | 数值采用16进制表示 |
| oct | 数值采用8进制表示 |
| ws | 提取空白符 |
| endl | 插入换行符，并刷新流 |
| ends | 插入空字符 |
| setsprecision(int) | 设置浮点数的小数位数(包括小数点) |
| setw(int) | 设置域宽 |

### C++ 特点
1. 兼容C
2. 支持面向对象

### 常量
1. 整型常量
2. 实型常量
3. 字符常量
4. 字符串常量
5. 布尔常量

### 变量
变量在使用之前需要首先声明其类型和名称

### 符号常量
符号常量在使用之前一定要首先声明

### 表达式
> - 一个常量或标识对象的标识符是一个最简单的表达式，其值是常数或对象的值。
> - 一个表达式的值可以用来参与其它操作，即用作其它运算符的操作数，这就形成了更复杂的表达式。
> - 包括在括号中的表达式仍是一个表达式，其类型和值与未加括号时的表达式相同。

### 算术运算符
> 由算术运算符、操作数和括号构成的表达式称为算术表达式。

----------

[volatile]: https://blog.csdn.net/ydar95/article/details/69822540
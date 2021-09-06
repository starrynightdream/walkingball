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
  - [第 3 章 函数](#第-3-章-函数)
  - [第 4 章 类与对象](#第-4-章-类与对象)

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

## 第 3 章 函数
- 在面向对象的程序设计中，函数是功能抽象的基本单位。
- 主调函数：调用其它函数的函数。
- 被调函数：被其它函数调用的函数

### 内联函数

> 内联函数不是在调用时发生控制转移，而是在编译时将函数体嵌入在每一个调用处

> inline关键字仅表示要求，编译器不一定会将其编译为内联函数，而没有inline的函数也可能被编译为内联。
> - 有些函数一定不可以被编译为内联（如对自身存在引用的
> - 结构简单、语句少的容易被编译为内联。

> 内联与宏的区别
> - 内联函数在运行时可调试，而宏不可以
> - 内联函数可以访问类的成员变量，而宏不可以
> - 在类中声明同时定义的简单的函数，会自动转换为内联函数
> - 编译器会对内联函数的参数类型做安全检查
```c++
// 宏要带一个括号
#defind max(a,b) a>b?a:b 
max(a,b) +1;// a>b?a:b+1 有错误, 因此宏请带括号
```

### 默认形参值
- 不能在一个函数的多个声明中对同一个参数的默认值重复定义。
```c++
int add (int x=5, int y=3);
int main(){...}
int add (int x/*=5*/, int y/*=3*/){
    // 不可以再定义，但可以用注释告知。
}
```

### 函数重载
> 两个以上的函数，具有相同的函数名，但是形参的个数或者类型不同，编译器根据实参和形参的类型及个数的最佳匹配，自动确定调用哪一个函数。

### 运行栈
- esp寄存器记录栈顶地址。 即 `栈指针`
- ebp寄存器记录函数刚被调用时栈指针的位置。即`帧指针`
    + 局部变量和形参通过`帧指针`定位

## 第 4 章 类与对象 

### 抽象
> - 面向对象方法中的抽象，是指对具体问题（对象）进行概括，抽出一类对象的公共性质并加以描述的过程。
> - 对一个问题的抽象应该包括：
>    + 数据抽象 (功能抽象)
>    + 行为抽象 (代码抽象)

### 封装
> 封装就是将抽象得到的数据和行为（或功能）相组合，形成一个有机整体，也就是将数据与操作数据的函数代码进行有机的结合，形成`类`，其中的数据和函数都是类的成员。

### 继承
### 多态
> - 多态性是指一段程序能够处理多种类型对象的能力。
> - 在C++中，这种多态可以通过：强制多态、类型参数化多态、包含多态4中形式来实现。

### 类和对象
> - 在面向对象程序设计中，程序模块是由类构成的。
> - 类是对逻辑上相关的函数与数据的封装，它是对问题的抽象描述。

### 类成员的访问控制
- 公有类型 public:
    + 公有类型定义了类的外部接口
- 私有类型 private:
    + 私有成员只能被本类的成员函数访问，来自类外部的任何访问都是非法的。
- 保护类型 protected:
    + 继承过程中对产生的新类影响不同。

### 类的成员函数
#### 内联成员函数
> 隐式声明：将函数体写在类体内。
```c++
class A{
public: 
    void func(){
        // 这是一个内联成员函数
    }
}
```
> 显示声明： 使用`inline`关键字
```c++
inline void A::func(){
    // 这是一个内联成员函数
}
```

### 构造函数和析构函数
- 在定义对象的时候进行的数据成员设置，称为对象初始化

#### 构造函数
> 构造函数的作用就是：
> 在对象被创建时利用特定的值构造对象，将对象初始化为一个特定的状态
> - 构造函数在对象被创建的时候将被自动调用

#### 复制构造函数
> - 其形参是本类的对象的引用。
> - 其作用是使用一个已经存在的对象(由复制构造函数的参数指定)，去初始化同类的一个新对象。
> - 隐含的复制构造函数的功能：把初始值对象的每个数据成员的值都复制到新建立的对象中。 

#### 析构函数
> - 用来完成对象被删除前的一些清理工作
> - 析构函数是在对象的生存周期即将结束的时刻被自动调用的。

---------

[volatile]: https://blog.csdn.net/ydar95/article/details/69822540
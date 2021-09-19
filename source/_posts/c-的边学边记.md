---
title: c++的边学边记
date: 2021-09-05 13:36:59
tags:
- 学习
    + c++
thumbnail: /walkingball/images/86907186_p1.jpg
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

- [c++从头再学。按章节记录在意的知识点](#c从头再学按章节记录在意的知识点)
  - [曾经的测试](#曾经的测试)
  - [未归类知识点](#未归类知识点)
  - [第 1 章 绪论](#第-1-章-绪论)
  - [第 2 章 简单程序设计](#第-2-章-简单程序设计)
  - [第 3 章 函数](#第-3-章-函数)
  - [第 4 章 类与对象](#第-4-章-类与对象)
  - [第 5 章 数据共享与保护](#第-5-章-数据共享与保护)
  - [第 6 章 数组、指针与字符串](#第-6-章-数组-指针与字符串)
  - [第 7 章 继承与派生](#第-7-章-继承与派生)
  - [第 8 章 多态性](#第-8-章-多态性)
  - [第 9 章 群体类和群体数据的组织](#第-9-章-群体类和群体数据的组织)
  - [第 10 章 泛型程序设计与C++标准模板库](#第-10-章-泛型程序设计与c标准模板库)
  - [第 11 章 流类库与输入输出](#第-11-章-流类库与输入输出)
  - [第 12 章 异常处理](#第-12-章-异常处理)

<!-- /code_chunk_output -->

# c++从头再学。按章节记录在意的知识点

## 曾经的测试
```c++
// b为 char， 在经过自加与加数字之后依旧是字符类型。

cout<<(typeid(b)).name()<<endl;
cout<<(typeid(b++)).name()<<endl;
// c
// c
```


## 未归类知识点
### explict 防止隐式转换
```c++
// 可用在构造函数与类型转换函数
explict ClassName():i(1){}

explict operator int(){
    return 1;
}
```
实际上类型转换函数和构造函数有同等效力
工程中一般不使用类型转换函数，写类ToType()的函数
### volatile 关键字
指示变量易变，程序应该每次使用时都去地址处读取。
当一个变量使用频繁的时候，编译器可能会进行优化，将其放置在寄存器中。但是如果这个变量是由其它程序进行修改的，可能就会使得这个变量不能同步修改。
它也会阻止编译器进行常量合并
因此建议在使用由其它程序修改的变量时，使用 $ volatile $ 关键字

[参考文章][volatile]

### `NULL` 与 `nullptr`
C++支持的NULL在C的宏定义为
```c
#define NULL ((void *)0)
```
因为void * 是不允许隐式转换的，因此编译器在处理NULL时会这样：
```c++
#ifdef __cplusplus
#define NULL 0
#else
#define NULL ((void *)0)
#endif
```
虽然这样切合了需求。但是将`NULL`作为0是有可能引起歧义的（比如重载了 `int` 和 `void*` 的函数，在使用`NULL`时会调用 `int` 的版本）
因此 c++11 引入了 `nullptr` 关键字进行区分。

[参考文章][NULL_nullptr]

### GCC 编译步骤
1. 预处理 （预编译）
```
gcc -E target.cpp -o target.i   // c语言用
g++ -E target.cpp -o target.i // g++是gcc的c++编译器
cpp target.cpp>target.i // cpp是预编译器
```
- 将所有#define删除，并且展开所有的宏定义
- 处理所有的条件预编译指令，如#if #ifdef  #undef  #ifndef  #endif #elif
- 递归处理 #include ,插入包含的文件
- 删除所有注释
- 添加行号和文件名标识，以便于编译时产生的错误警告能显示行号
> 注意其保留#pragma编译器指令

2. 编译
```
gcc  -S  target.i   -o  target.s
g++  -S  target.i   -o  target.s
```
词法分析、语法分析、语义分析及优化后生成汇编代码

3. 汇编
```
gcc  -c  target.s  -o  target.o
as  target.s -o  target.o
```
生成对应的机器代码

4. 链接
链接动态库和静态库

[参考文章][GCC_Step]

### 聚合类特殊初始化
当一个类：
- 所有成员都是public
- 没有定义构造函数
- 没有类内初始值
- 没有基类，也没有虚函数

就称为聚合类。可以通过特殊的花括号语法初始化
```c++
class Test{
public:
    int a;
    string b;
}
int main(){
    Test test = {12, "123"};
    // 可以这样初始化一个类
}
```

按类中的声明顺序依次赋值，不足则靠后的值不赋值，多则报错。

### 占位参数
```c++
// 形如此
void func(int a, int){
    // 后面的值函数内无法应用
    // 但调用函数时必须提供。
    // 也可以为占位参数提供默认值
    // (int a, int =19)
}

// 例
void f1(int a, int b, int){
    // 方案一
}
void f1(int a, int b, char){
    // 方案二
}

int main(){
    f1(1,2,1); // 方案一
    f1(1,2,'c'); // 方案二
}
```
顺便一提貌似后置++ 就是有一个占位参数，借此区别于前置++

## 第 1 章 绪论
#### 机器语言与汇编语言
软件包括了计算机运行所需的各种程序及其有关的文档资料
程序是指令的集合
指令是计算机可以识别的命令
指令系统是一台计算机硬件系统能够识别的所有指令的集合
机器语言是由计算机硬件系统可以识别的二进制指令组成的语言
汇编语言将机器指令映射为一些可以被人读懂的助记符

#### 高级语言
它屏蔽了机器的细节，提高了语言的抽象层次，程序中可以采用具有一定含义的数据命名和容易理解的执行语句。

#### 面向对象的语言
问题域：开发软件是为了解决某些问题，这些问题所涉及的业务范围称为该软件的问题域。
面向对象的编程语言将客观事物看作具有属性和行为的对象，通过抽象找出同一类对象的共同属性和行为，形成类。

> 结构化程序设计思路：
> 自顶向下、逐步求精
> 其程序结构是按功能划分为若个基本模块，这些模块形成树状结构
> 各模块之间的关系尽可能简单，在功能上相对独立。
> 每一模块内部均是由顺序、选择和循环3种基本结构组成
> 其模块化实现的具体方法是使用子程序

> 将数据即对数据的操作方法放在一起，作为一个对象。
> 同类型对象抽离其共性，形成类。
> 类中的大多数数据，只能用本类的方法进行处理。
> 类通过一个简单的外部接口与外界发生关系，对象之间通过消息进行通信。

对象：
系统中用来描述客观事物的一个实体，它是用来构成系统的一个基本单位。
对象由一组属性和行为构成。

类：
具有相同属性和服务的一组对象的集合。
分类所依据的原则是抽象。

封装：
把对象的属性和服务结合成一个独立的系统单位，并尽可能隐蔽对象的内部细节。
封装是面向对象方法的一个重要原则

继承：
特殊类的对象拥有其一般类的全部属性与服务，称作特殊类对一般类的继承

多态性：
一般类中定义的属性或行为，被特殊类继承之后，可以具有不同的数据类型或表现出不同的行为。

面向对象的软件工程：
面向对象方法在软件工程领域的全面应用
- 面向对象的分析OOA
- 面向对象的设计OOD
- 面向对象的编程OOP
- 面向对象的测试OOT
- 面向对象的软件维护OOSM

### 信息的表示与存储
- 信息
    + 控制信息
        * 指令
        * 控制字
    + 数据信息
        * 数值信息
            - 定点数
            - 浮点数
        * 非数值信息
            - 字符数据
            - 逻辑数据

bit 1
Byte 8
Word 取决计算机，常见8、16、32

K = 1024B
M = 1024K
G = 1024M

原码
0 1 正负
反码
取反 正负
补码
负数 取反加1

### 程序开发基本概念
目标程序：源程序翻译加工生成的程序。（机器语言、汇编、其它）
翻译程序：用于翻译得到目标程序的程序。有三种：
- 汇编程序：汇编到机器语言
- 编译程序：高级到其它。
- 解释程序：一句一句。不产生目标程序。


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
应当用于临时为未添加`const`的函数提供调用便利，而非修改`const`值。
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
        unsigned int val = reinterpret_cast<unsigned int>(p);
        return (unsigned short) val;
    }

    int main ()
    {
        int a[20];
        for (int i=0; i<20; ++i)
            cout<< Hash(a + i) <<endl;
    }
```
它应该用在如：已知指针类型情况下转换为正确的指针。

> 细节
> 如P489的提示一般
> c++标准未限制 `reinterpret_cast` 转换指针的行为，但是其实现往往是仅拷贝比特位的。

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
    - 十进制
    - 八进制
    - 十六进制
    - 长整型
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

### 类的组合
#### 组合
- 类的组合就是一个类内嵌其它类对象作为成员的情况。
    + 它们之间的关系是一种包含和被包含的关系。
- 当创建类的对象时，如果这个类具有内嵌对象成员，那么各个内嵌对象将首先被自动创建。
    + 在创建对象时初始化基本类型数据成员和内嵌对象成员。

> - 内嵌对象的构造函数调用次序为其组合类的定义中出现的次序。与初始化列表中出现的顺序无关。
> - 析构函数的调用执行顺序与构造函数相反

### 结构体与联合体 (struct and union)
#### 结构体
> **类与结构体唯一区别**
> - 结构体默认访问权限为公共，类为私有。
#### 联合体
**联合体数据成员共享一组内存**

### 扩展
#### 位域
- C++未规定具体位域打包发生。由编辑器决定
- 支持位域的成员：bool、char、int、enum
- 时间换空间。因为打包解包需要时间。

```c++
#include <iostream>
using namespace std;
...// Level 的定义。含4个值。可用两bit存储
enum Grade {A, B, C, D} // 同理
class Student {
    ... //code there in P138
private:
    unsigned number:27; // 占27比特
    Level level: 2; // 占2比特
    Grade grade: 2; // 占2比特
}
... // code there
```
#### 显式临时对象
```c++
ClassName(ClassBasic(param), ClassBasic(param)); // 表达式结束对象被销毁
// 有以下等价写法。 P140
ClassName((ClassBasic)param, (ClassBasic)param);
ClassName(static_cast<ClassBasic>(param), static_cast<ClassBasic>(param));
```
对于等价写法，实际上就是根据构造函数创建了类型转换函数。若想避免，使用 `explicit` 关键字修饰构造函数。 (构造函数默认使用 `implicit` 关键字修饰)
```c++
class String{
public:
    explicit String(int n) {};
    String(const char *p) {};
};
```

#### 参数 和 返回值 传递对象
> **参数情况**
> - 调用复制构造函数，创建一个新类在参数部分。
> - 在某些情况（如传入临时对象）编译器会进行优化（传入临时对象则会在构造临时对象时就将其创建在被调函数的参数部分。）

> **返回值情况**
> *对如今大多数编译器而言*
> 先创建临时变量以保证返回值的空间，将地址告知被调函数，被调函数在地址处初始化对象。主调函数再将初始化的临时对象赋给目标值。
> 有优化为主调函数只分配空间，被调函数在空间上初始化类。

#### 拷贝构造函数 和 = 运算符
可以使用 `delete` 关键字阻止自动生成的 拷贝构造函数 和`=`运算符
```c++
class Person { // 这样类就不能使用值传递创建以及被赋值创建
public:
    Person(const Person& p) = delete;
    // disable Person p(other); and func(Person p);
    Person& operator=(const Person& p) = delete;
    // disable p = other;
};
```
- 拷贝构造函数必须以引用传参
- 调用的是拷贝构造函数还是赋值运算符，主要是看是否有新的对象实例产生。
    + 如果产生了新的对象实例，那调用的就是拷贝构造函数
        * 值方式传参
        * 值方式返回值
        * 使用一个对象初始化另一个对象
    + 如果没有，那就是对已有的对象赋值，调用的是赋值运算符。
        * a = b 等

## 第 5 章 数据共享与保护

### 作用域 
 **作用域是一个标识符正文中有效的区域**

#### 函数原型作用域
**在函数原型声明时形参的作用范围就是函数原型作用域**

#### 局部作用域
> - 形参列表中的形参： 从声明 到 函数体结束
> - 函数体内声明变量：从声明 到 所在块大括号结束
> - 访问类内部变量：
>    + 无同名局部变量，直接访问
>    + x.m 或 X::m 。x 是对象 X 是类。后者是访问静态变量。
>    + ptr->m。ptr为指针

#### 命名空间作用域
```c++
namespace name {
    // code there
}
namespaceName::operator // 使用其它命名空间内容
using namespaceName::operator // 引入一个操作
using namespace namespaceName; // 引入命名空间
```
命名空间可嵌套
```c++
namespace A{
    namespace B{
        operator;
    }
}
A::B::operator; // 使用
```
匿名空间常用于屏蔽不希望暴露给其它源文件的标识符。
```c++
namespace {
    // 不会暴露的内容
}
```
> **具有命名空间作用域的变量也称为全局变量**

#### 可见性
> 程序运行到某一点，能够引用到的标识符，就是该处可见的标识符
> - 声明在前，引用在后
> - 同一作用域，不能声明同名标识符
> - 不互相包含的作用域不互相影响
> - 内外同名标识符外层不可见

### 对象的生存期
#### 静态生存期
**如果对象的生存期于程序运行期相同，则称它具有静态生存期**

#### 动态生存期
**局部生存期对象诞生于声明点，结束于声明所在的块执行完毕之时**

### 类的静态成员
#### 静态数据成员
> - 使用`static`关键字
> - 类属性是描述类的所有对象共同特征的一个数据项，对于任何对象实例，它的属性值是相同的。
> - 静态数据成员具有静态生存期
> - `类名::标识符`
#### 静态函数成员
> 静态成员函数可以直接访问该类的静态数据和函数成员。而访问非静态成员，必须通过对象名。

### 类的友元
> **友元机制提供了类的成员函数与其它类的成员函数或一般函数之间进行数据共享的机制**

#### 友元函数
类中用`friend`修饰的非成员函数
```c++
// P160
class A{
public:
    friend float dist(A a1, A a2); // 友元函数声明
    ...// code there 
}

float dist(A a1, A a2) {
    a1.operator; // 可以直接访问
    ... // code there
}
```
#### 友元类
若A类为B类的友元，则A类的所有成员函数都是B的友元函数
```c++
class B{
    ...
    friend class A; // 声明友元类
}
```
- 友元关系不能传递
- 友元关系是单向的
- 友元关系不能继承

### 共享数据的保护

#### 常对象
**常对象必须初始化，而且不能被更新**

#### 用const修饰类成员
#### 常引用
```c++
func(const className &p);
```

#### 多文件结构和预处理命令
##### 文件结构一般是分为
- 类定义
- 类实现
- 主函数

#### 外部变量与外部函数
##### 外部变量
`extend` 关键字修饰。可以在定义它的源文件以外的文件使用。
```c++
// 源文件1
int i=3;
void next();
int main(){
    i++; next();
    return 0;
}

void next() {
    i++; other();
}
// 源文件2
extern int i;
void other(){
    i++;
}
```
##### 外部函数
非成员函数，都可以在不同的编译单元中被使用。只要在调用之前进行引用性声明（即声明函数原型）即可。

##### 将变量和函数限制在编译单元内
> 现在推荐使用匿名空间变量。

#### 标准 C++ 库
> - 输入输出类
> - 容器类与ADT（抽象数据类型）
> - 存储管理类
> - 算法
> - 错误处理
> - 运行环境支持

#### 编译预处理
##### #include
```c++
#include <文件名> //标准方式，在系统目录的include子目录下搜索
#include "文件名" //在当前目录下搜索，再用标准方式
```

##### #define #undef
define 定义
undef 取消 define 的定义

##### 条件编译指令
```c++
# if 常量表达式
    code there
# elif 常量表达式
    code there
# else
    code there
# endif
```

##### defined

#### 代码的编译链接与执行过程
##### 编译
由 .cpp 产生 .o

## 第 6 章 数组、指针与字符串
### 数组
> 数组是具有一定顺序关系的若干对象的集合体，组成数组的对象称为该数组的元素。
> - 每个元素有n个下标的数组叫n维数组

声明：
- 确定数组的名称
- 确定数组元素的类型
- 确定数组的结构（包括数组维数，每一维数的大小)

*数组在内存中是顺序、连续存储的*
*数组的初始化是在声明数组时给部分或全部元素赋初值*
*数组名传参，传递的是地址*
*数组名传参，传递的是地址*

### 指针
*指针变量是用于存放内存单元地址的*

- `*` 表解析
- `&` 表取址

*数组名称实际上就是一个不能被赋值的指针，即指针常量*

> **指向常量的指针**
> ```c++
> const int* p1= &a; // 不能修改p1指向的内容
> ```
> **指针类型的常量**
> ```c++
> int* const p1= &a; // 不能修改p1
> ```
>

> *指针加减n为指向前后n个长度的元素*
> *0专用于表示空指针，也就是一个不指向任何有效地址的指针*

#### 指针数组
```c++
int * pLine[3] = {p1, p2, p3};
pLine[1][1]; // 然后这样用
```

#### 指针型函数
数据类型 (* 函数指针名) (形参表)
```c++
// 因为写法比较繁琐，一般使用typedef简化
typedef int (* DoubleIntFunc)(double); 
DoubleIntFunc funcPtr; // 一个指向参数为一个double，返回int 的函数指针
```

### 字符串
string 类

### 扩展
*引用底层是指针*
#### 指针的安全隐患及其应对方案
> **地址安全性**
> - 指针算术运算的用途，一定要限制在获得同个数组内其它元素指针上
> - 并注意通过首元素进行运算。
> - 或直接使用封装的数组 （如 `vector` ）

> **类型安全性**
> - reinterpret_cast 保证：
>   + A类型的 p 转换为 B类型的 q 再转回 A类型的r后有 ` p == r ` 成立
> - 但是对于void的转换使用 static_cast 会更有保证

> **堆对象管理**
> - 用`new`创建的对象用`delete`删除
> - 最好由哪个对象创建，就由哪个对象删除。
> - 对于使用函数创建需要的对象的情况，则需要对接口进行约定（写好注释），由调用者进行删除。
> - 或使用共享指针 如 Boost库, c++11新标准

> **const_cast的应用**
> 不应该用于出尔反尔地去除 `const` 修改变量。而应该为未保证不修改但确实不修改的函数提供临时的便利。

## 第 7 章 继承与派生
### 类的继承与派生
> 类的继承，是新的类从已有类那里得到已有的特性。
> 从已有类产生新类的过程就是类的派生

一个派生类，可以同时有多个基类，这种情况称为多继承
一个派生类只有一个直接积累的情况称为单继承。
直接参与派生的为直接基类
间接参与派生（基类的基类）的为间接基类
派生类成员是指基类以外新增的数据和成员函数

#### 派生过程
1. 吸收基类成员
2. 改造基类成员
    - 同名函数会隐藏基类的函数（包括重载的参数不同的函数）
3. 添加新成员

#### 访问控制
继承方式规定了如何访问从基类继承的成员

| 继承方式 | 公有成员 | 保护成员 | 私有成员 |
|-|-|-|-|
| 公有继承| 公有 | 保护 | 不可直接访问|
| 保护继承| 保护 | 保护 | 不可直接访问|
| 私有继承| 私有 | 私有 | 不可直接访问|

### 类型兼容规则
*是指在需要基类对象的地方，都可以用公有派生的对象来替代*
> 替代之后只能使用基类继承的成员。

### 派生类的构造和析构函数
#### 构造函数
*构造派生类对象时，就要初始化基类和新增成员*
- 需要调用基类待形参的构造函数时，派生类就必须声明构造函数

#### 复制构造函数
```c++
// 设D 为 B 的派生类
D::D(const D &v) :B(v){...}
// 这是系统生成的默认复制构造函数
```
#### 析构函数
- 析构函数的执行顺序和构造函数相反

### 派生类成员的标识与访问
1. 不可访问成员
2. 私有成员
3. 保护成员
4. 公有成员

#### 作用域分辨符
`::`
可以通过指定作用域访问被隐藏的基类成员。
- 菱形继承情况下，子类使用不同直接基类的同名成员需要使用作用域辨识符
    + 虽然函数只有一个副本，但是调用函数需要将调用对象作为隐藏参数传入，因此还是需要通过辨识符指定传入哪一个直接基类。

#### 虚基类
> 将共同的基类设置为虚基类，这是从不同路径继承的同名数据在内存只有一个副本，函数名也只有一个映射。

> 在继承时添加 `virtual` 实现
```c++
// 例 p277
class B{...};
class B1: virtual public B{...};
class B2: virtual public B{...};
class D: public B1, public B2{...};
// 此时D中仅有一份B的数据
```

#### 构造顺序
1. 如果该类有直接或间接虚基类，则**先执行虚基类的构造函数**
2. 如果该类有其它基类，则按照它们在**继承声明列表中出现的次序**，分别执行它们的构造函数，但构造过程中，**不再执行它们的虚基类的构造函数**。
3. 按照在类的**定义中出现的次序**，对派生类中新增的成员对象进行初始化。对于类类型的成员，如果出现在构造函数初始化列表中，则使用其中指定的参数执行构造函数，若无，则执行默认。对于基本数据类型，如果出现在构造函数初始化列表中，则使用其中的值赋以初值，否则什么都不做。
4. 执行构造函数的**函数体**
### 扩展
#### 组合与继承
选择组合，更多是 “A 有一个 B”
选择继承，更多是 “B 是一种 A”
#### 派生类对象内存布局
> **核心**
> 派生类对象的内存布局满足:一个基类指针，无论指向基类对象还是派生类对象，都用相同的步骤访问基类中定义的数据成员

> P 299

1. 单继承

| 指针 | 内存  |
|---|---|
| 基类指针和类指针 -> | Base数据成员  |
| | 派生数据成员  |

2. 多继承

| 指针 | 内存  |
|---|---|
| A基类指针和类指针 -> | A基类数据成员 |
| B基类指针 -> | B基类数据成员  |
| | 派生数据成员  |

> 此时对于派生类转B基类时，需要加上偏移值。如果派生类的指针值为0(`NULL`)，则转换也返回0

3. 虚拟继承
```c++
class B{...}
class B1: virtual public B{...}
class B2: virtual public B{...}
class D: public B1, B2{...};
// 其是为了解决虚拟继承中，重复基类造成空间冗余的问题
// 虚拟继承将保证重复的 B 基类在 D中只有一个
```

| 指针 | 内存  | 指向 |
|---|---|---|
| B1基类指针和D指针 -> | B的指针 | 末尾的B对象 |
|  | B1基类数据成员 |
| B2基类指针 -> | B的指针 | 末尾的B对象 | 
|  | B2基类数据成员  |
| B基类指针 -> | B数据成员  |
| | 派生数据成员  |

> 注意：
> 可以经过以上示例看出，指针类型转换不止是复制地址

当涉及 `void *` 指针的转换时，是不会进行偏移量的加减的。
所以
```c++
D * pd = new D();
void* pv = pd;
B* pb = static_cast<B*>(pv);
// 这里的指针可能会出现错误，因为没有增删偏移量。
// 如果没有偏移量改变就无所谓。
```

派生类的 构造函数/析构函数 不会继承基类的。

## 第 8 章 多态性
### 概述
同样的消息被不同类型的对象接收时导致不同的行为。

#### 类型
- 重载多态
    + 如函数重载，运算符重载等
- 强制多态
    + 强制转换变元类型等。（`int` `float` 加法时 `int` 被转换为 `float`
- 包含多态
    + 类族中定义于不同类中的同名成员函数的多态行为（主要通过虚函数实现。
- 参数多态
    + 类模板。

前二者为专用多态，后二者为通用多态

#### 实现
从实现角度分：
- 编译时多态
- 运行时多态

> 绑定： 确定操作的具体对象的过程。
> 绑定是指计算机程序自身彼此关联的过程（即联系标识符名和一个存储地址）
> - 静态绑定: 绑定工作在编译链接阶段完成
> - 动态绑定：绑定工作在程序运行阶段完成

### 运算符重载
> 对已有的运算符赋予多重含义，使同一个运算符作用于不同类型的数据时导致不同的行为。

#### 规则
- 运算符基本可以重载，但只能重载C++已有的。
- 运算符的优先级和结合性不会改变
- 运算符重载是针对新类型数据的实际需要，对原有运算符进行适当改造。
    + 一般而言：
    + 重载功能与原有功能类似
    + 不能改变操作对象个数
    + 至少有一个操作对象是自定义类型

不能重载的运算符
- `.` 类属关系
- `.*` 成员指针
- `::` 作用域分辨符（因为它的操作对象是类型）
- `?:` 三目运算符

重载代码
```c++
返回类型 operator + (形参表)
{
    函数体
}
```

|操作符| 一般函数  | 成员函数  |
|---|---|---|
| o1 B o2 | type operator B(o1, o2)  | type o1::operator B (o2) |
| B o1 | type operator B(o1)  | type o1::operator B () |

> 后置 `++` / `--` 有一个 int 形参，但不起作用，仅区别前后置;

### 虚函数
#### 一般虚函数成员
由 `virtual` 修饰
其声明只能在类定义中的函数原型声明中，不能在成员函数实现中。

过程多态条件
1. 满足赋值兼容规则
2. 声明虚函数
3. 由成员函数调用或通过指针引用来访问虚函数

（如果没有显示声明）判断是否为虚函数：(需要都满足)
- 是否与基类的虚函数有相同的**名称**
- 是否与基类的虚函数有相同的**参数个数**及相同的对应**参数类型**
- 是否与基类的虚函数有相同的**返回值**或者满足赋值兼容规则的指针、引用型的返回值

若判断为虚函数则:
- 覆盖基类的虚函数
- 隐藏基类中同名函数的所有其它重载形式

> 注意： 可以通过辨识符调用被隐藏的函数。 

> 对象切片： 用派生类的对象复制构造基类对象的行为。
> 因为此时派生的部分不会被复制，仅有基类部分拷贝。

> c++ 不能声明虚构造函数

因为使用基类指针 `delete` 对象时，会调用基类的析构函数。因此将虚构函数声明为虚函数是通常操作。

```c++
virtual ~B(); // 虚析构函数
virtual type name (params) = 0; // 纯虚析构函数
```

声明为纯虚析构函数之后，基类就可以不再给出函数的实现部分。(类接口)

#### 抽象类
指带有纯虚析构函数的类是抽象类。
抽象类不能实例化。

### 扩展
#### 多态类型和非多态类型
C++的类类型分两类
- 多态类型
    + 指有虚函数的类类型
    + 其析构函数设置为虚函数。
- 非多态类型
    + 不是多态类型的类型
    + 对其的公有继承，应当慎重，而且一般没有太大必要

#### 运行时类型识别
dynamic_cast
1. dynamic_cast 转换，会先进行检查
    - 如果指针转换不合理，则得到空指针
    - 如果引用转换不合理，则抛出异常
2. 不能用于非多态类型，因为其没有保存转换信息。

typeid
```c++
#include <typeinfo>
// typeid (express)

// Base *b;
typeid (b); // Base *
const type_info info2 = typeid (*b);// Base
info2 == typeid(Base); //true
```
#### 深入虚函数
虚表：由虚函数指针构成的表。类共用。派生类新增的虚函数在后面。
虚表指针：指向虚表首地址的指针。

> 事实上对象的运行时类型信息需要通过虚表访问，其用于支持运行时类型识别。
> 因此虚表不仅有虚函数指针

## 第 9 章 群体类和群体数据的组织
自定义类型是由多个基本类型或自定义类型的元素组成的，称之为**群体数据**
群体类则封装群体数据与相关特殊操作

> 群体可分两种
> 1. 线性群体：元素按位置排列有序
> 2. 非线性群体：不用位置顺序标识元素

### 函数模板和类模板
参数化多态性，就是将程序所处理的对象的类型参数化，使得一段程序可以用于处理多种不同类型的对象。
#### 函数模板
```c++
template<模板参数表>
类型名 函数名(参数表)
{
    函数体的定义
}
```
模板参数表可接收由`,`隔开的
1. `class`(`typename`)标识符，代表类型。
```c++
template <class T>
```
2. 类型说明符，接收一个类型说明符规定的常量。
```c++
template <int Size>
```
3. trmplate<参数表> class 标识符，接收一个类模板作为参数。
```c++
template <class con = allocon<c>>
```

当类型参数确定后，编译器以函数模板为样板，生成一个函数。过程称为函数模板的实例化，生成函数称为实例。

> *函数模板与函数的区别*
> 1. 函数模板本身在编译时不会生成任何代码，只有由模板生成的实例会生成目标代码。
> 2. 被多个源文件引用的函数模板，应当连同函数体一同放在头文件中，而不能像普通函数那样只将声明放在头文件中。
> 3. 函数指针只能指向模板的实例，而不能指向模板本身。

#### 类模板
使用类模板使用户可以为类定义一种模式，使得类中的某些数据成员、某些成员函数的参数、返回值或局部变量能取任意类型。

```c++
template<模板参数表>
class 类名
{
    类成员声明
}
// 在类模板以外定义其成员函数
template<模板参数表>
类型名 类名<模板参数表标识符列表>::函数名 (参数表)
// 建立对象
模板名<模板参数表>对象名1,...,对象名1;
```
类模板本身不是类，只有被其它代码引用时模板从根据引用的需要生成具体的类。

### 线性群体
#### 概念
对可直接访问的线性群体，我们可以直接访问群体中的任何一个元素
对顺序访问的线性群体，只能按元素的排列顺序从头开始依次访问各个元素。
<!--(索引访问搁置-->
#### 直接访问的 Array 的实现 
P 355
1. 深浅复制
2. `=` `[]`需要返回对象的引用。因为它们有作为左值的情况。
3. 自定义类型转换。
```c++
// 重载了Array 向 T*类指针的转换
template<class T>
Array<T>::operator T*() {
    return list;
}
template<class T>
Array<T>::operator const T*() const{
    return list;
}
```

> tip:
> 这里是第二种方法。第一种是之前提及的使用构造函数的方式。

#### 顺序访问的 链表类
P366

#### 栈类
P369
#### 队列类
P376

### 群体数据的组织
#### 插入排序 
P 378
#### 选择排序 
P 379
#### 交换排序 （最简单的交换排序是冒泡排序）
P 380
#### 顺序查找
P 381
#### 折半查找
P 381

### 扩展
#### 模板实例化机制
##### 对模板与模板实例关系的辨析
1. 模板函数不是函数，模板类不是类。而对应的实例是模板/类。
2. 不同实例之间没有关系，既不是友元也不是派生。
3. 模板函数有时发现没有指定类型参数，因为C++会根据实参类型进行类型推断，以确定使用哪种模板函数。

##### 隐含实例化
自动按需进行的模板实例化。（对应类型的函数被调用导致的实例化）

##### 多文件结构中的组织
模板成员函数的定义部分也应该放在头文件中。
> **Warn!**
> 当一个函数的定义出现在头文件的时候，多个文件对其的使用就会分别产生一个代码导致链接冲突。
> ```c++
> // 示例
> // head.h
> void funcThere(){
    > //do nothing
> }
> // willInclude.cpp
> #include "head.h"
> // main.cpp
> #include "head.h"
> void main(){
    > funcThere():
> }
> 
> // 此时运行
> // g++ willInclude.cpp main.cpp -o cppRun
> // 会报出多个定义的错误
> ```
> 但是模板函数的隐含实例化导致的冲突是被允许的。编译器对其作了特殊处理。

> Warn!
> 若在头文件中仅提供模板的声明而没有实现。则：
> 源程序在头文件中找到定义，
> 源程序根据参数需求寻找特化定类型后的函数，发现没有。
> 决定生成
> 模板呢？找不到。于是报未定义错误。
> 因为在声明与定义模板的时候，是不会产生任何函数的代码的，仅有在被需要的时候才会按需生成。这导致所有可能需要生成模板的函数都需要知道模板的定义。


##### 显式实例化
```c++
// template 实例化目标声明
template void output<int>(const int * arr); // 例
// 因为有自动类型推断，所以参数省略
template void output(const int * arr); 
```
> 这样就可以分离声明和定义。因为这使得代码在
> 当然这样就需要穷举需要的类型。

#### 为模板定义特殊实现
##### 模板的特化 (全特化)
定义：C++允许程序员为一个函数模板或类模板在某些特定参数下提供特殊的定义。
```c++
// from P389
// 通过下面的写法为Stack提供bool的特定版本
template<> // 此处为固定格式，仅作区别无意义
class Stack<bool, 32> {
    ... 
}
// 类外成员函数的定义方法
bool Stack<bool, 32>::pop() {
    assert(!isEmpty()); 
    ... // code there
}
// 因为对类特化而非对成员函数。因此指定成员函数无需使用
// template<> 告知为特化
```

##### 类模板的偏特化
上述特化，同时限定住了类型和大小，而只打算限定类型时则需要借助偏特化
```c++
// from P390
// 通过下面的写法为Stack提供bool的特定版本
template<int SIZE> 
class Stack<bool, SIZE> {
    ... 
}
// 类外成员函数的定义方法
template<int SIZE> 
bool Stack<bool, SIZE>::pop() {
    assert(!isEmpty()); 
    ... // code there
}
```
> 特化结果是一个普通的类。
> 偏特化的结果仍然是一个模板

其它应用:可限定类的取值范围
```c++
// 指定T为指针时使用该偏特化
template<class T>class<T *>{...};
// 指定T为const指针时使用该偏特化
template<class T>class<const T *>{...};
// 因为第二类比第一类严格，因此在二者都符合情况下会选择第二类。
```

##### 函数模板的重载
> C++不允许将函数模板偏特化，但可以重载。效果与偏特化类似。

```c++
// P 392 例
template<class T>
T myMax(T a, T b) {
    return (a>b)?a:b;
}
// 重载
template<class T*>
T* myMax(T* a, T* b) {
    return (*a>*b)?a:b;
}
// 同理，因为第二类比第一类严格，因此在二者都符合情况下会选择第二类。
```

#### 模板元编程
把运行时计算，放到编译完成。
##### 例：阶乘编译计算
```c++
// P393
template<unsigned N>
struct Factorial {
    enum {VALUE=N* Factorial<N-1>::VALUE};
};
template<>
struct Factorial<0>{
    enum {VALUE=1};
};
```
> 如此，在需要使用 n! 时就可以使用模板了，并且会在编译的过程中就计算出来。
```c++
const int M = 6;
int array[Factorial<M>::VALUE];
```

##### 例：乘方计算
同理，不做说明
```c++
// P 394
template<unsigned N>
struct Power {
    template<class T>
    struct T value(T x) {
        return v *Power<N-1>::value(x);
    }
}

template<>
struct Power<1>{
    template<class T>
    struct T value(T x) {
        return v;
    }
}

// 辅助函数模板
template<unsigned N, class T>
inline T power(T v) {
    return Power<N>::value(N);
}
```

## 第 10 章 泛型程序设计与C++标准模板库
### 泛型程序设计及STL结构。
> 我们用**概念** (concept)来描述泛型程序设计中作为参数的数据类型所需具备的功能。
> - 这里的概念是泛型程序设计中的一个术语
> - 它的内涵是这些功能
> - 它的外延是具备这些功能的所有数据类型。
> 
> 具备一个概念所需要功能的数据类型称为这一概念的一个**模型**。
> 
> 对于不同的两个概念A和B，如果A所需求的所有功能也是概念B所需求的所有功能（即概念B一定是概念A的模型），那么就说概念B是概念A的子概念。
>
#### STL 简介
标准模板库
1. 容器
    - 容纳、包含一组元素的对象。
    - 向量 vector ，双端队列 deque，列表 list，集合 set，多重集合 multiset，映射 map，多重映射 multimap。
    - 分为基本的 顺序容器(线性相关)、关联容器(快速提取)
2. 迭代器
    - 提供了顺序访问容器中每个元素的方法。
3. 函数对象
    - 一个行为类似函数的对象。（重载`()`运算符）
    - 包含头文件 `<functional>`
4. 算法
    - 最重要的特性：统一性，并且可以广泛用于不同的对象和内置的数据类型。

### 迭代器
#### 输入流迭代器，输出流迭代器
1. 输入流迭代器
用来从一个输入流中连续地输入某种类型的数据，它是一个类模板。

> STL 设计非常灵活，往往有多个模板参数但多有默认参数。

2. 输出流迭代器 
用来从一个输出流中连续地输出某种类型的数据，它也是一个类模板。

> 输入流迭代器，输出流迭代器可以看作一个适配器
> 适配器用于为已有的对象提供新的接口，本身一般不提供新功能，只为了改变对象的接口存在。

#### 迭代器的分类
0. 所有迭代器都支持
- ++p // 指向下一个元素，返回p1自身的引用
- p++ // 指向下一个元素，但返回类型是不确定的

1. 输入迭代器支持
- p1 == p2
- p1 != p2
- *p1
- p1->m
- *p1++

> 注意：p1 == p2 不保证 ++p1 == ++p2

2. 输出迭代器支持
- *p1 = t
- *p1++ = t

> 注意：连续自增没有写入与连续写入都是未定义行为。

3. 向前迭代器
- *p1
- p1++
4. 双向迭代器
- --p1
- p1--
5. 随机访问迭代器
- p1+=n
- p1-=n
- p1+n与n+p1
- p1-n
- p1[n]

#### 迭代器的区间
[p1, p2) 表示，不包括p2.
p1 == p2 表示空区间

#### 迭代器的辅助函数
advance 前进或后退多个元素
```c++
template <class InputIterator, class Distance>
void advance(InputIterator& iter, Distance n);
```
distance 计算两个迭代器之间的距离
```c++
template <class InputIterator>
void advance(InputIterator& first, InputIterator& last)
// 须有 last >= first 成立
```

### 容器
#### 容器的基本功能与分类
- S s1
- s1 op s2
- s1.begin()
- s1.end()
- s1.clear()
- s1.empty()
- s1.size()
- s1.swap(s2)

> 有些操作之间效果是一致的，但是未必有相同的效率。


- S::iterator 普通迭代器，指向元素类型为T
- S::const_iterator 常量迭代器，元素类型为 const T 因此只能访问元素而不能通过迭代器修改

P411 :
容器 -> 可逆容器 -> 随机访问容器

> tip:
> 一般容器的 begin 和 end 函数提供向前迭代器。
> 事实上STL提供的都至少是可逆容器，所以基本可以双向遍历。

逆向迭代可以用：

- S::reverse_iterator 普通迭代器，指向元素类型为T
- S::const_reverse_iterator 常量迭代器，元素类型为 const T 因此只能访问元素而不能通过迭代器修改

> 实际上逆向迭代器都可以直接构造一个方向相反的逆向迭代器。
> ```c++
> S::reserve_iterator(p1);
> ```
>

STL 中各容器头文件和所属概念
| 容器名  | 中文名  | 头文件  | 所属概念  |
|---|---|---|---|
|  vector | 向量      | \<vector\>  | 随机访问容器，顺序容器  |
| deque   | 双端队列  | \<deque\>   | 随机访问容器，顺序容器 |
| list    | 列表  | \<list  \>  | 可逆容器，顺序容器  |
| set     |  集合 | \<set   \>  | 可逆容器，关联容器  |
| multiset| 多重集合  | \<multit\>  | 可逆容器，关联容器  |
| map     | 映射  | \<map   \>  | 可逆容器，关联容器  |
| multimap| 多重映射  | \<multimap\>  | 可逆容器，关联容器  |

#### 顺序容器
1. 其基本功能
vector deque list
    - 构造函数
        + S s(n, t); n 个 t
        + S s(n); n 个 T()
        + S s(q1, q2); 用[q1, q2) 区间的数据构造
    - 赋值函数
        + s.assign(n, t);
        + s.assign(n);
        + s.assign(q1, q2); 
    - 元素的插入
        + s.insert(p1, t);
        + s.insert(p1, n, t);
        + s.insert(p1, q1, q2);
    - 元素的删除
        + s1.erase(p1);
        + s1.erase(p1, p2);
    - 改变容器的大小
        + s1.resize(n); 减少删尾部，增多尾部补T()
    - 首尾元素的直接访问
        + s.front(); 头
        + s.back(); 尾
    - 在容器尾部插入、删除元素
        + s.push_back(t)
        + s.pop_back()
    - 在容器头部插入、删除元素
        + s.push_front(t)
        + s.pop_front()
> vector 不属于前插顺序容器，但也可以通过 `inster` `erase` 函数实现前插，但效率较低。

2. 各自特点
> **向量 vector**
> 随机访问，尾高效插入，其它位置效率都不高
> 为避免动态增删频繁申请空间，其容器容量一般大于容器大小，额外的有如下函数：
> - s.capacity() 返回s的容量
> - s.reserve(n) 如果当前容量大于等于n则什么都不做，否则扩大s的容量，使得s的容量不小于n
> > 在大量插入数据前，可用reserve提前申请空间。
> > 注意
> > 增删vector的内容会使得它所有迭代器失效。
> > (更具体的说)：
> > 增：
> >     - 如果引起了空间分配，则一切失效。
> >     - 否则，在插入位置之后的迭代器、指针、引用会失效
> > 删：
> >     - 其删除位置之后的迭代器、指针、引用会失效
>
> 技巧
> 因为删除元素不会使得空间被释放，所以可以通过
> ```c++
> vector<T> (s.begin(), s.end()).swap(s);
> vector<T> ().swap(s); // 我觉得这样写才对吧。。。
> ```
> 释放空间

> **双端队列 deque** P418 <samll>有结构图</small>
> 随机访问，但效率不及向量，首尾高效插入
> 底层有一个个固定大小的数组，和一个记录所有数组首地址的索引数组
> 两端加入元素，不会使指针、引用失效，会使迭代器失效
> 两端删除元素，被删除元素的迭代器、指针引用失效，但不会影响其它元素。

> **列表 list**
> 不能随机访问，但可以高效在任意位置增删。
> 任何增删都不会使任何已有元素的迭代器、引用、指针失效。
> 其额外支持接合（splice）操作。（从一处删除加到另一处）
> - s1.splice(p, s2) 将s2列表的所有元素插入到s1p-1和p之间，s2情空
> - s1.splice(p, s2, q1) 将s2列表的q1指向的元素插入到s1p-1和p之间，并从s2移除
> - s1.splice(p, s2, q1, q2) 将s2列表的[q1, q2)指向的元素插入到s1p-1和p之间，并从s2移除

对比
- 大量随机访问，主要尾部插入： 向量
- 少量随机访问，两端插入： 双端队列
- 基本无随机访问，任意位置插入： 列表
P 422 详情

3. 插入迭代器
它是一种适配器，使用它可以通过输出迭代器的接口来向指定元素的指定位置插入元素。

- template<class FrontInsertionSequence>class front_insert_iterator; 只适用于前插顺序容器。
- template<class Sequence>class back_insert_iterator; 所有容器
- template<class Sequence>class insert_iterator; 所有容器

其可以通过构造函数创建，但一般通过如下辅助函数
```c++
template<class FrontInsertionSequence>
front_insert_iterator<FrontInsertionSequence> front_inserter (FrontInsertionSequence& s);

template<class Sequence>
back_insert_iterator<Sequence> back_inserter (Sequence& s);

template<class Sequence, class Iterator>
insert_iterator<Sequence> inserter (Sequence& s, Iterator i);
```
因为其可以自动推断类型，无需特地指定。

例：从标准输入得到的整数插入容器s的末尾
```c++
copy(istream_iterator<int>(cin), istream_iterator<int>(), back_inserter(s));
```

4. 顺序容器的适配器 （裁剪操作的封装）
> **栈和队列**
> ```c++
> template<class T, class Sequence=deque<T>> class Stack; // 可以使用任一顺序容器
> template<class T, class FrontInsertionSequence=deque<T>> class queue; // 限定为前插顺序容器
> ```
> 
> 栈和队列共同支持
> - s1 op s2
> - s.size()
> - s.empty()
> - s.push(t)
> - s.pop()
> 
> 栈支持
> - s.top()
> 
> 队列支持
> - s.front()
> - s.back()

> **优先队列**
> 支持 size empty push pop top
> 与栈、队列不同之处：
> - 不支持比较
> - 元素要支持`<`运算符

#### 关联容器
1. 分类和基本功能
    - 其每个元素都有一个键（key）
    - 其中元素按键的取值升序排列

> tip： 关联容器在内部使用了平衡二叉树对数据进行维护。

| 类型 | 简单关联容器(本身为键) | 二元关联容器(键是本身的一部分) |
|---|---|---|
| 单重关联容器(键唯一) | 集合 (set) | 映射 (map) |
| 多重关联容器(键不唯一) | 多重集合 (multiset) | 多重映射 (multimap) |

关联容器的键需要可以进行 `<` 比较大小

> **`<` 运算的细节**
> C++标准规定，`<`必须构成严格弱序关系，即：
> - 非自反性：对任何x `x < x` 返回 `false`
> - `<` 传递性， x < y , y < z 均为 true ，则 x < z 为 true
> - `==` 传递性：把 x == y 定义为 !(x < y) && !(y < x)，则x == y , y == z 均为 true ，则 x == z 为 true

单重和多重关联容器的区别
1. 构造函数
    ```c++
    S s(q1, q2)
    ```
    单：[q1, q2)出现相同键时，只有第一个会被加入
    多：[q1, q2)无条件加入
2. 元素的插入
    ```c++
    s.insert(t)
    // 将 t 插入 s 中
    ```
    单：键不存在时成功，返回`pair<S::iterator, bool>`。成功返回插入键的迭代器和true， 否则返回键一致元素的迭代器和false
    多：返回已插入元素的迭代器
    ```c++
    s.insert(p1, t)
    // p1 是一个提示位置，如果该位置距离目标位置很近，则提高效率。即便不准确也不影响函数的正确性
    ```
    单：当t不存在于s中才插入，返回t键相同元素的迭代器
    多：总是插入成功，返回插入元素的迭代器
    ```c++
    s.insert(q1, q2)
    // 对区间内每个元素分别执行s.insert(t)
    ```
3. 元素的删除
    ```c++
    s.earse(p1); // 删除p1指向元素
    ```
    ```c++
    s.earse(p1, p2); // 删除[p1, p2)区间元素
    ```
    ```c++
    s.earse(k); // 删除所有键为k的元素，返回被删除元素的个数
    ```
4. 基于键的查找和计数
    ```c++
    s.find(k) // 找任意一个键为 k 的元素，没有返回 s.end()
    ```
    ```c++
    s.lower_bound(k) // s中第一个键值不小于k的元素迭代器
    ```
    ```c++
    s.upper_bound(k) // s中第一个键值大于k的元素迭代器
    ```
    ```c++
    s.equal_range(k) 
    // 得到一个用 pair<S::iterator, S::iterator> 表示的区间，刚好包含所有键为k的元素。
    // 即 p1 == s.lower_bound(k)
    // 且 p2 == s.upper_bound(k)
    ```
    ```c++
    s.count(k) // k元素的个数
    ```

**集合 set**
存储有限不重复的元素。

**映射 map**
- 集合数据本身是键，映射是键-值的二元组。
- 常用于查键的附加数据很像字典
-
tip：
因为需要插入的数据类型为pair，每次指定都比较繁琐，因此常使用 make_pair 辅助函数构建
它是`<utility>`中定义的一个专用于辅助二元组构造的函数模板。
```c++
template<class T1, class T2>
pair<T1, T2>make_pair(T1 v1, T2 v2){return pair<T1, T2>(v1, v2);}

// 使用
m.insert(make_pair("C++", 2));
```
> 在使用 [] 寻址时，如果map中不存在该键，则会插入一个值为V()的元素。最后都返回对应键的引用。

**多重集合 multiset 多重映射 multimap**
它不支持`[]`了，因为键值不再一一对应。
一般使用 equal_range 进行范围限定，以查找目标。
底层一般是平衡树，故元素也需要支持 `<` 比较

#### 函数对象基本概念及分类
是一个行为类似函数的对象，它可以不需要参数，也可以带有若干参数，其功能是获取一个值或改变操作的状态。
就是普通的函数和任何重载了调用运算符 `()` 的类的对象

P436 常用函数对象的六种分类

> 1. 产生器 一元函数 二元函数
> 具有 0、1、2 个传入参数的函数对象
> 2. 一元谓词，二元谓词
> 返回值为 bool，并具有一个、两个参数


P 439 的标准谓词
tip： 其实大小判断均依赖 `<`，相等判断均依赖 `==`

> 3. 关联类型
函数对象可以获取由基类继承来的 参数类型信息 和 返回类型信息。称为关联类型的特征信息。

#### 函数适配器
将一种函数对象转化为另一种符合要求的还是担心。
可分为四类
- 绑定适配器
- 组合适配器
- 指针函数适配器
- 成员函数适配器
<small>P 441 详细介绍</small>
因为声明特定类型过于繁琐，一般采用辅助函数。
<small>P 442 辅助函数介绍</small>


#### 算法
算法本身就是一种函数模板
STL几乎所有算法的头文件都是`<algorithm>`
分类
- 非可变序列的算法，不直接修改所操作容器内容 P449
- 可变序列的算法，会改变所操纵容器的内容:
    + 如：复制(copy) 生成(generate) 删除(remove) 替换(replace) 倒序(reverse) 旋转(rotate) 交换(swap) 变换(transform) 分割(partition) 去重(unique) 填充(fill) 洗牌(shuffle)

- 排序和搜索算法，排序和合并算法，二分查找以及有序序列的集合操作算法等
    + sort 要求两端指针为随机迭代器类型。因为sort具体实现使用了快速排序。
- 通用数组算法，较少。
    + P459

### 扩展
#### swap
swap 有一个通用模板，但是对于某些特定的实现是不够高效的，因此往往会去特化其实现。
```c++
// 通用 ver 这个也在std下，不详述
template<class T>
void swap(T &a, T &b)
{
    T temp = a;
    a = b;
    b = temp;
}
// 特化ver, 这个特地表明在std下
// 是为了体现对于swap的特化需要在std命名空间下进行。
namespace std {
    template<class T>
    void swap(vector<T> &a, vector<T> &b)
    {
        a.swap(b);
    }
}
// 因为vector访问底层修改指针会更快，
// 因此先定义一个成员函数swap，(为了访问内部变量)
// 再通过模板的swap调用，就可以兼顾统一与效率。
```
> 注：
> swap 是定义在 std 命名空间内的，因此对其的特化也要放在std命名空间内。

> Warn：
> 对自己的模板而非切实的类进行 swap 的特殊化不像对类提供一般简单，因为无法偏特化一个函数模板，只能重载它。
> 而且向c++保留命名空间std内部定义新的类、函数或模板会产生不确定的行为，对已有的进行特化则允许。
> 你在std外部为其定义函数模板一般不会被其它STL算法调用

#### STL 组件的类型特征与STL 扩展
所谓类型特征，一般都是用`typedef`定义在类或结构体内的数据类型。
标准的STL组件都提供了完整的类型特征，因此它们可以很好地协同工作。
在为STL编写扩展组件时，也应当为其定义相关的类型特征。
1. 利用函数对象的类型特征实现函数适配器
```c++
template<class BinaryFunction>
class binder1st:public unary_function<typename BinaryFunction::second_argument_type, typename BinaryFunction::result_type> {
    protected:
        BinaryFunction op;
        typename BinaryFunction::first_argument_type value;
    public:
        // 构造函数
        binder1st(const BinaryFunction& op,
        const typename BinaryFunction::first_argument_type& value): op(op), value(value){ }
        // () 重载
        typename Operation::result_type
        operator()(const typename BinaryFunction::secode_argument_type& x) const { return op(value, x);}
}
// P470
// 调用辅助函数
template<class BinaryFunction, class T>
binder1st<BinaryFunction>bind1st(const Operation& op, cosnt T& x){
    return binder1st<BinaryFunction>(op, x);
}
```
> 细节：
> 此处使用了大量的typename，此处用于强调后跟着的标识符表示的是一个数据类型。
> c++标准规定，在模板中引用的、依赖于模板参数的数据类型，必须用typename修饰，否则标识符就不被解析为一个数据类型。

2. 迭代器的类型特征

ptrdiff_t 与 size_t 类似，与指针有相同字节数的整数类型。但ptrdiff_t 有符号。

定义自己的迭代器时，无须分别定义各个类型特征，只要继承 iterator 类即可。
P471 有头文件。
```c++
// <iterator>头文件
template<class Iterator>struct iterator_traits{
    typedef typename Iterator::difference_type difference_type;
    typedef typename Iterator::value_type value_type;
    typedef typename Iterator::pointer pointer;
    typedef typename Iterator::reference reference;
    typedef typename Iterator::iterator_category iterator_category;
}
// 为数组特化
template<class T>struct iterator_traits<T*>{
    typedef ptrdiff_t difference_type;
    typedef T value_type;
    typedef T * pointer;
    typedef T &reference;
    typedef random_access_iterator_tag iterator_category;
}
// 为常数数组特化
template<class T>struct iterator_traits<const T*>{
    typedef ptrdiff_t difference_type;
    typedef T value_type;
    typedef const T * pointer;
    typedef const T &reference;
    typedef random_access_iterator_tag iterator_category;
}
```

3. 利用类型特征实现算法。
```c++
// P474
template<class InputIterator, class OutputIterator>
void mySort(InputIterator first, InputIterator last, OutputIterator result) {
    // 此处根据传入的InputIterator类型推断调用本函数
    // 将IpuutIterator内记录的value_type用作初始化
    // 这样vector的初始化就无需指定类型。
    // 故无需特地告知输入的类型
    vector<typename iterator_traits<InputIterator>::value_type> s(first, last);
    sort(s.begin(), s.end());
    copy(s.begin(), s.end(), result);
}
```
```c++
// 根据不同迭代器类型执行不同
// 向后移动 n 位
// 的示例
// 出入迭代器ver
template<class InputIterator, class Distance>
inline void__advance_helper(InputIterator& i, Distance n, input_iterator_tag) {
    while (n-- !=0) ++i;
}
// 随机访问迭代器ver
template<class InputIterator, class Distance>
inline void__advance_helper(InputIterator& i, Distance n,random_access_iterator_tag) {
    i+=n;
}
// 辅助函数
template<class InputIterator, class Distance>
inline void advance(InputIterator& i, Distance n) {
    __advance_helper(i, n,
        typename iterator_traits<InputIterator>::iterator_category());
}
```

> 细节
> 因为__advance_helper函数是 inline 的，所以该函数是容易被编辑器优化，所以看似比 advance 多使用一个参数，但不会占用额外的空间。

#### Boost 简介
一个很有用的第三方库。
比如 lambda c++ 图像处理库 通配符等等等等。

## 第 11 章 流类库与输入输出
> - 流是一种抽象，它负责在数据的生产者和数据的消费者之间建立联系，并管理数据的流动。
> - 程序将流对象看作是文件对象的化身。
> - 读 - 提取
> - 写 - 插入

### 输出流
三个常用ostream (顺带一提还有一个常用的istream是cin)
- cout 标准输出流
- cerr 标准错误输出流，没有缓冲，发送给它的内容立即被输出。
- clog 类似cerr，但是有缓冲，缓冲区满时被输出。

> **关于缓冲**
> 当一个输出流没有缓冲的时候，就会每接收到一个字符就执行一次刷新。
> 如输出"之乎者也"，就会刷新4次面板。
> 但是如果在内存不足的情况下，没有缓冲就显得尤为重要，因为你没有多余的空间暂存你的信息。
> 也因此，cerr没有缓冲，cout有，clog则是补正cerr的不足而出现的。
> [参考][inner_cout_cerr_clog]

> **关于区别**
> 虽然三者都默认输出到屏幕，但除了缓冲区外，标准输出和标准错误输出还是有区别的。
在运行程序时，可以使用 `>` 对标准输出进行重定向，此时cout输出的内容被重定向，但cerr和clog则依旧打印在了屏幕上。
> 你也可以通过 `2>` 来对标准错误重定向
```
target.exe > info.txt // 这样二者就区别开了
// err 会打印在屏幕
// info.txt
cout则会在文件中
```

ofstream 磁盘文件输出


```c++
ofstream myFile;
myFile.open("fn");
//or
ofstream myFile("fn");
//or
ofstream myFile;
myFile.open("f1n");
myFile.close();
myFile.open("f2n");
myFile.close();
```

插入运算符 `<<`
可以同预先定义的操纵符一起工作，控制输出。
> 具体查看P484

- 控宽 width setw
- 填空 fill
- 对齐 P486
- 精度
- 进制

#### 文件输出流成员函数
与操纵符等价的成员函数
执行非格式化写操作的成员函数
其它修改流状态且不同于操纵符或插入运算符的成员函数

1. open(filename, 操纵符)
    - 操纵符可以使用 `|` 组合使用
2. close()
    - 输出流析构函数会关闭未关闭的文件
3. put
    - cout.put('a') 等价 cout<<"a";
4. write
    - 二进制写入。空字符不停止
5. seekp tellp
    - 记录一个指针指向文件内部的位置，使得对文件可以随机访问。
    - seekp 为设置， tellp为获取
    - [参考][inner_seekp_tellp]
6. 错误处理 P490

#### 二进制输出文件
不同操作系统结束符不一样，
所以使用二进制输入文件可以规避换行符的转换。
```c++
ofstream os("fn.txt", ios_base::out|ios_base::binary);
```

#### 字符输出流
ostringstream
- 特有函数 str() 返回string对象

### 输入流


- istream
- ifstream
- istringstream

你可以使用`<`重定向标准输入，这样cin可以从文件中获取数据

ifstream 类似于 ofstream

提取运算符 `>>`
#### 相关函数
P493
- get
- getline
- read
- seekg
- tellg

#### 字符串输入流
从一个字符串中读取数据
```c++
string str = ...
istringstream is(str);
// 使用示例
T v;
is>>v;
```

### 输入输出流
fstream 磁盘文件的输入输出
stringstream 字符串输入输出

### 扩展
#### 宽字符、宽字符串与宽流
1. 其缺陷
`char` 取值为 0~255，但对于汉字就不够了。
所以汉字使用连续两个`char`表示。但有些不足：
- size 返回的长度是字符数两倍
- substr 切割会得到原字符串不存在的汉字
    + 同理可以使用 find 得到原本不存在的字符
```c++
int main() {
    string s = "这是一个中文字符串";
    cout<<s.size()<<endl;
    cout<<s.substr(3,2)<<endl;
    cout<<s.find("且")<<endl;
}
// 18
// (GBK编码环境下) 且 (是的第二个字符和一的第一个字符)
// 3 //然而原来并没有'且'
```

2. 宽字符与宽字符串
关键字：`wchar_t` 为宽字符。声明宽字符要在单引号前加L
```c++
wchar_t c = L'宽';
```
`wstring` 是宽字符串类，除了使用`wchar_t`以外和`string`没有区别

3. 宽流
然而，cout输出单位是char，与wchar_t并不兼容。
所以就有特殊一类的输入输出流。基本就是在一般的输入输出流前加入w即可：wistream、wifstream、wofstream、wstringstream等。

> 实际上`wstring`和宽流和一般的字符串和流都是相同模板，不过普通的是以`char`为模板参数，而加了w前缀意味使用`wchar_t`最为模板参数

当然这也使得编码出现隐患："ABCD" "甲乙丙丁"前者占4字节，后者占8字节，但使得它们在程序的表现长度都是4。读取几个字节确定一个字符就要通过编码方案。

*关于设定*
流的imbue成员函数可以接受locale型的参数设定本地化配置。
```c++
locale loc(".936"); //936对应GBK
wcout.imbue(loc);
wcout<<L"剑阁峥嵘而崔巍"<<endl;
```

#### 对象的串行化
将普通类型、复合类型（类、结构体）写入文件的过程。

将对象的连续内存空间的内容写入文件，虽简便但有几个问题。
- 存在指针，则指向的对象没法保存。
- 对象内部有其它复合类型，可能导致直接恢复不适用。
- 对象除了包含数据，还有一系列行为，但该方法使得行为无法触发。

特地为特定的复合类型书写load save方法则会使得扩展性不好。

于是有 boost 的 Serialization 库可以使用

## 第 12 章 异常处理
异常处理：由于环境条件和用户操作的正确性是没有百分之百的保障的，所以在设计程序之初，就要考虑各种意外并给予恰当的处理。

异常处理程序的任务：程序运行有些错误可以预料但不可避免，就要力争允许用户排除环境错误，继续运行程序，至少提供提示信息。

### C++异常处理的实现
关键字： `try` `throw` `catch`
#### 语法
```c++
try{
    throw express;
} catch (){ // 可以不指明形参名，不过就无法访问错误类型

} catch (...){ // 如果是 ... 则处理所有类型

}
```
异常匹配标准： (会按catch语句的次序由前到后依次匹配)
1. catch 子句中声明的异常类型就是抛出异常对象的类型或引用
2. catch 子句中声明的异常类型是抛出异常对象的类型的公共基类或其引用
3. 抛出的异常类型和catch子句中声明的异常类型皆为指针类型，且前者到后者可隐式转换。
```c++
#incldue <iostream>
using namespace std;
int divide(int x, int y){
    if (y==0)
        throw x; // 抛出 int
    return x/y;
}

int main(){
    try{
        divide(7, 0);
    } catch (int e) { // 抛出匹配int
        cout<<e<<endl;
    }
    return 0;
}

// 7
```

#### 异常声明接口
为函数使用者快速了解该函数会抛出什么函数。
```c++
// 意味抛出A，B，C，D及其子类型。
void func() throw(A,B,C,D);
// 不声明为可抛出任意类型
void func();
// 空声明意味不抛出任何类型的错误
void func() throw();
```

> 如果一个函数抛出了它的异常接口不允许抛出的异常时，会调用 unexpected 函数。其默认行为是调用 terminate 中止程序。
> 用户可以自定义 unexpected 以替换默认的。

### 异常处理的构造与析构
如果匹配的错误是值类型，则复制抛出的异常对象
如果匹配的错误是引用类型，则指向抛出的异常对象

> 异常处理的真正作用，是析构所有异常抛出前构造的局部对象。

栈的解旋：异常抛出后，从进入`catch`对应的`try`块起，到抛出前这段时间在栈上构造的所有对象(未析构的)都会被自动析构，析构顺序与构造顺序相反。

### 标准程序库异常处理
Exception为基类
有一个what() 函数用于获取错误信息
```c++
virtual const char* what() const throw();
```
P518 规定的异常与含义

> 习惯：
> 一些语言强制自能抛出某类的派生类。
> C++也可以使用 Exception 如此实践。
> 这样会比较方便

### 扩展
#### 异常安全性问题
```c++
// 这是异常不安全的
template<class T, int SIZE>
void Stack<T, SIZE>::push(const T &item){
    assert(!isFull());
    list[++top]=item;
}
// 当item的赋值运算过程中发生了错误抛出
// top已经自加，但栈顶没有赋值。

// 这是异常安全的
template<class T, int SIZE>
void Stack<T, SIZE>::push(const T &item){
    assert(!isFull());
    list[top+1]=item;
    top++;
}
```
异常安全的函数，在发生异常的时候不会泄露任何自由，也不会使任何对象陷入非法状态。

编写异常安全函数的基石是肯定不会抛出异常的函数。
- 基本数据类型的绝大部分操作（除0例外）
- 指针赋值、算术运算、比较运算
- STL的很多容器和算法在模板参数定义的相关操作不抛出异常的情况下不抛出异常
- STL的swap不抛出异常
```c++
// 这是异常安全的
void reverse(vector<SomeClass> &s){
    vector<SomeClass> t; //即便这两句发生异常，也不会影响外部
    copy(s.rbegin(), s.rend(), back_inserter(t));
    s.swap(t); // 只有这句改变状态，但它不会抛出异常
}
```
> 像这种先计算，再用swap交换的方式，是编写异常安全函数的通用技巧。
> 因此用户使用自定义类型对swap进行特化时也应该保证不抛出任何错误。

> 尽量保证析构函数不会抛出异常。
> 因为异常抛出对象会被析构，若析构抛出异常则异常机制无法继续工作。
> 实际上，析构函数大多数只为了释放空间，只要逻辑没有错误，delete 和 delete [] 就不会抛出错误。析构函数也就不会抛出错误。

#### 避免异常发生时的资源泄露
以动态内存（堆）为例。
实际上涉及：文件、互斥锁、条件变量、套接字、管道等

```c++
void SF(int n){
    int *s = new int [n];
    ...//code there
    if (...){
        delete[] s;
        throw someException();
    }
    ...//code there
    try {
        someOtherFunction();
    } catch(...){
        delete[] s;
        throw;
    }
    ...//code there
    delete[] s;
}
// 这样就有很多资源释放的语句。
```
因为在栈上的对象是无需在意资源释放的，异常会自动调用其析构函数。因此一个解决方法是将资源包装起来，作为栈上的一个对象。

```c++
void SF(int n){
    vector<int>s(n);
    ...//code there
    if (...){
        throw someException();
    }
    ...//code there
    someOtherFunction(); // 这里抛出异常，则s的空间也会释放
    ...//code there
}
```

有些对象必须使用new生成，毕竟new要更灵活。则可以使用C++提供的
智能指针：auto_ptr 头文件 memory
如： `auto_ptr<int>`为指向`int`的智能指针

智能指针指向一个对象，则对象的析构任务由智能指针负责。
智能指针在析构时，会自动对它所关联的指针执行delete。

```c++
X* get() const throw();
```
可以使用get获取关联的指针，但智能指针的* -> 均被重载，可以直接使用

```c++
X* reset(X* p=0) throw();
```
可以使用reset重置指向的指针，但注意如果指向一个不同的指针则之前的指针会被执行delete操作

```c++
X* release() throw();
```
可以使用release 使关联指针变为空，然后返回原关联指针而不执行delete。不打算让智能指针负责一个指针的删除可以用它。


```c++
auto_ptr<SomeClass>ap2(ap1);
// 等价于
auto_ptr<SomeClass>ap2(ap1.release());

ap2 = ap1;
// 等价于
ap2.reset(ap1.release());
```
可以使用一个智能指针 *复制构造* 另一个智能指针。此时会交接关联对象。这是为了保证至多一个智能指针管理同一个指针

综上，将auto_ptr定义在栈上，就可以很方便地删除堆对象，在发生异常时也无需做特别处理。

> 习惯：
> 工程中一般将刚获取的指针直接构造auto_ptr对象，而不经过中间变量。因为这会混淆堆对象删除责任的归属。

> tip
> 除了auto_ptr还有其它的智能指针，如shared_ptr。

---------

[volatile]: https://blog.csdn.net/ydar95/article/details/69822540
[NULL_nullptr]: https://blog.csdn.net/reasonyuanrobot/article/details/100022574
[GCC_Step]: https://blog.csdn.net/gt1025814447/article/details/80442673


[inner_cout_cerr_clog]: https://blog.csdn.net/bsmmaoshenbo/article/details/50778068
[inner_seekp_tellp]: https://blog.csdn.net/mafuli007/article/details/7314917



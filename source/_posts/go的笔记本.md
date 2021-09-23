---
title: go的笔记本
date: 2021-09-23 14:32:56
tags:
- 技术
    + golang
thumbnail: /walkingball/images/0075ecec-71f8-4692-a590-ab5b81593df8.jpg
---
# go的笔记本，先学，有空整理
## 基本

`package` 指定某一源文件属于某个包，需要放在第一行
```go
```
`import` 导入包
`const` 声明常量   `var` 声明变量
```go
var a int = 1
var (
    aa = 1
    ab = "123"
)

const b = 1
const {
    ba = 1
    bb = "345"
}
```
`:=` 简短声明，左侧至少有一个未声明变量，会有自动类型推断
`func`
```go
func functionName(paramName ptype, paramName ptype) (returnType1, returnType2){
    body
}
// 参数列表和返回类型非必须，置空意为无参/无返回
// 参数类型一致可以在最后加类型
func add(a, b int) int{
    return a+b
}

// 返回多个变量
func t()(int, int){
    return 1, 2
}
func t()(time, res int){
    time = 1
    res = 2
    return //必须
}
```
`_` 表示任何类型的任何只读值，既不能读取值也不能读取它的类型。主要用于占位


## 命令
```bash
go run 文件名 // 用于运行脚本
go env // 查看go的相关变量
```

## 解惑，疑惑
### 关于 const 的无类型作用
```go
var a1 = 1
// var b int64 = a // 报错
var b1 int64 = int64(a)

const a2 = 1
var b2 int64 = a
var b3 int32 = a
var b4 = a // 均可 
```
可以看出`const`的无类型使得在赋值的时候可以选取合适的类型
因为`const`唯有在需要类型的时候才会在默认类型中选取合适的返回
所以，就可以这样使用
```go
const a = 5
// 都会自动解释类型
var test1 int = a
var test2 int64 = a
var test3 float64 = a
var test4 complex128 = a
```

### 关于 go install 运行代码失败
```
version is required when current directory is not in a module
    Try 'go install firstProgram@latest' to install the latest version
```
翻译：当前目录不在模块中时需要版本
但是即便加上了@latest也没用
```
go install: firstProgram@latest: malformed module path "firstProgram": missing dot in first path element
```
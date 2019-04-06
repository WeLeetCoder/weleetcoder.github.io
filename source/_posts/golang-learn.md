---
title: go语言学习
date: 2019-04-05 16:22:39
tags: [go]
categories: [技术学习]
top: true
---

## GO 基础学习

### 变量

Go语言定义变量有好几种方式。

使用 `var`关键字是GO最基本定义变量的一种方式，与其他静态类型不一样的是`变量类型是写在变量之后的`，就像这样：

```go
var variable type // var 是声明一个变量，这个是定义一个名字叫variable的变量，类型为type
```

下面是几种变量定义的方式：

```go
package main

// 定义包内变量 并且在包内都可以使用
var variableDefine int 

// 同样是定义包内变量，不用指定变量类型
var helloWorld = "hello,world!" 

// 在一个组内定义变量，可以用一个var定义多个不同类型变量
var (
    a string
    b int
    c interface{}
)

func main() {
    
    // 使用var同时初始化多个变量，变量类型自动推断，这种方式十分舒服
    var v1, v2, v3 = 2, 3.1415, "hello"

    // 一个var只能指定一种变量类型
    var va1, va2, va3 string = "va1", "va2", "va3" 
}
```

还有一种方式，只能够在函数内使用，在函数外部使用会导致编译不通过，一般定义全局变量使用`var`。

```go
package main

// 错误！这种定义方式不能够使用在此处
varable := "hello,world"
func main() {
    
    // 正确！在函数内部，才能够使用这种便利的方式定义变量，不需要指定变量类型
    variable := "hello,world"
    
    // 这是同时定义多个不同的类型的变量
    var1, var2, var3 := "var1", 2, 3.14
}
```

`_`（下划线，不需要定义）是一个特殊的变量，go语言所有定义的变量都必须使用，而有的变量是不需要的，例如`for range` 只想要 value 而不要 index，就可以使用下划线来将其丢弃。

如果定义了一个变量，没有被使用，那么将会报错

```go
package main

func main() {
    var i int
}
```

就像下面这样，定义了一个变量 i 却没有使用

```bash
# command-line-arguments
src\gd\client.go:12:2: i declared and not used
```

如果像下面这样：

```go
package main

func main() {
    var i int
    _ = i
}
```

这样就不会报错了，因为，将i赋值给`_`变量，会将i丢弃，这样i也使用了，就不会报错。





## 常量



常量，也就是在程序编译阶段就确定下来的值，而程序在运行时无法改变该值。在go语言中，可以定义为常量的类型有：布尔型、rune型、整数型、浮点型、复数型还有字符型，rune、integer、floating-point,还有complex统称为数值常量。

> There are *boolean constants*, *rune constants*, *integer constants*, *floating-point constants*, *complex constants*, and *string constants*. Rune, integer, floating-point, and complex constants are collectively called *numeric constants*.



常量的定义方法：

```go
const constantName = value

// 如果有需要，也可以明确指定数据类型
const Pi float32 = 3.1415926


const Pi = 3.1415926 
const MaxNum int= 10000
const prefix = "db_"
```





## 内置基础类型

在GO语言中，内置了丰富的基础类型。

#### Boolean (布尔型)

布尔型的类型为 `bool` ，值是`true` 或 `false` ，默认零值为`false`。

```go
package main
var isActive bool           //声明全局变量
var enabled, disabled = true, false   // 忽略类型的声明
func main() {
    var avilable bool                // 一般声明
    valid := false                   // 简短声明
    available := true 				 // 赋值操作
}
```



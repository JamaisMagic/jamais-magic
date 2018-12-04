---
title: "golang experience 01"
description: "golang experience 01"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - golang
---

应一个学霸妹子兼学习积极分子的要求，写一写最近看golang的些少总结。

这个只是第一部分，后面分几部分不好说，要视乎进度，目前先做一些简单的记录。

由于水平有限，如有错误或理解不到位的地方，麻烦轻拍。

## 简介

golang是一款开源的、编译型的、静态类型的语言。

<a target="_blank" href="https://golang.org/">官网地址</a> （官网好像会被墙哦，毕竟Google家的，Angular的官网也是被墙的，呵呵。）

设计目标大概是用来代替C++，所以需要做到性能高而简单易用。至于怎么做到的，不是我现在想关心的，我现在只想关心怎样用啫。

如果说做一个东西为什么要用go，很大可能是因为它的高性能而且简单。

## 下载安装

从官网下载安装即可，教程已经很清晰了，直接下载分发包安装即可。<a target="_blank" href="https://golang.org/dl/">Downloads</a>，<a target="_blank" href="https://golang.org/doc/install">Getting Started</a>

下载完可以测试一下

```bash
go version
```

## 可以开始写Hello World了

首先建好目录，workspace/demo1/src 用来放源码，workspace/demo1/bin 用来放编译后的应用程序，workspace/demo1/pkg 用来放第三方包等。

然后可以新建文件如 workspace/demo1/src/hello/hello.go。贴上代码

```golang
package main

import "fmt"

func main() {
  fmt.Println("Hello, go.")
}
```

可以看到，go首先要声明包，然后导入依赖的包库等，至少有个main函数，代码就是从main函数开始执行。

执行命令， go run workspace/demo1/src/hello/hello.go 可以看见输出 Hello, go.

## 变量声明

go声明变量使用var关键字，形式如var variable type = value

```golang
package main

import "fmt"

func main() {
    var count int
    var count2 int = 2
    var count3 = 3
    var count4, count5 = 4, 5
    var count6, count7 int
    var (
        count8  = "8"
        count9  int
        count10 = 10
    )

    count11, count12 := "11", 12

    fmt.Println(count, count2, count3, count4, count5, count6,
        count7, count8, count9, count10, count11, count12)
}
```

声明时，值可以忽略，编译器会自动赋予默认值，如果有值，类型也可以忽略，编译器会自动推断类型，以该值的类型为类型，但是不能同时忽略类型和值，否则会报语法错误。

一个var关键字同时声明多个变量，只能是同一类型的，如果想声明多个不同类型的，可以使用小括号方式声明。

还有一种:=的短式声明方法，可以声明一个变量，也可以多个，但是这种声明方法不能忽略值。另外这种声明变量的方法，必须至少有一个新变量。

## golang类型

go有3种基本类型：bool，string，number，其中number又可以分为下面十几种。

* int8, int16, int32, int64, int
* uint8, uint16, uint32, uint64, uint
* float32, float64
* complex64, complex128
* byte
* rune

值得注意的是golang原生支持复数，可以通过complex函数或者形如z := 1 + 2i 进行计算。

```golang
package main

import "fmt"

func main() {
    var z = complex(1, 2)
    var z2 = 1 + 2i

    fmt.Println(z, z2)
}
```

golang不支持隐式类型转换，需要显式转换。

```golang
package main

import (
    "fmt"
    "strconv"
)

func main() {
    var num1 = 5
    var float1 = 5.1
    var str1 = "666"
    var sum = float64(num1) + float1
    var concat1 = strconv.Itoa(num1) + str1

    fmt.Println(sum, concat1)
}
```

## 常量

常量用const关键字声明，不可重复赋值，这个应该都知道。同时常量的值需要在编译期确定，因此不能把函数返回值赋值给常量。

## 函数

声明函数用func关键字。

```golang
func functionname(parametername type) returntype {  

}
```

其中，参数和返回类型可以忽略。

函数可以返回多个值，用逗号隔开即可。

```golang
package main

import (
    "fmt"
)

func cal(a, b int)(int, int) {
    var result1 = a + b
    var result2 = a * b
    return result1, result2
}

func main() {
    result1, result2 := cal(2, 3)
    fmt.Println(result1, result2)
}
```

go支持一种叫named return value的返回方式，大概就是再指定参数类型的时候可以同时指定返回值的名称，函数体里面可以不必要显示返回该值，函数运行完毕后会自动返回命名返回值。

```golang
package main

import (
    "fmt"
)

func named(firstName, lastName string)(fullName string) {
    fullName = firstName + " " + lastName
    return
}

func main() {
    fullName := named("Jamais", "Ng")
    fmt.Println(fullName)
}
```

golang可以是用空白标记（blank identifier）接收任意类型的返回值，可以用于在多个返回值里忽略自己不需要的值。

golang的函数声明关键字func既简单又明了，不像js和php用完整的单词，也不像python用莫名其妙的def。
golang的参数相对来说比较简单，不像python那么多花样（如命名参数，默认参数，位置参数，关键字参数等）
和python一样可以返回多个值。
golang还有一个独特的地方是可以事先对返回值命名，由函数自动返回这些值。

```golang
package main

import (
    "fmt"
)

func cal(a, b int)(int, int) {
    var result1 = a + b
    var result2 = a * b
    return result1, result2
}

func main() {
    result3, _ := cal(4 ,5)
    fmt.Println(result3)
}
```

## 包（package）

对包的概念应该都不陌生了。

go每个文件都要package关键字声明这个文件属于哪个package。

如需要倒入外部包，需要用import关键字导入。

自己写的包，对外暴露变量或函数，这些变量和函数的名字要以大写字母开头。

每个包都可以有一个或多个init函数，这个init函数没有返回类型也没有参数，而且不能显式调用，由程序自动调用。

init函数的调用顺序，首先，包级别的变量先初始化，然后各个init函数以它在编译器出现的顺序执行。

如果导入了其他包，则其他包的init函数会先执行，包括比当前文件的包级别的变量初始化要早。

一个包无论被import多少次，只会初始化一次。

在import的报名掐年加空白表肌肤（blank identifier）可以导入包而不使用而编译器不会报错。

```golang
import (
    "fmt"
    _ "anotherPkg/anotherPkg"
)
```

golang的访问控制比较简单，相当于只有Java的public和private，没有protected和package级别的访问控制。
而且也不需要关键字，只用字母大小写做区分。这一点和python有点像，python用下划线表示私有，但不同的是，python的下划线是约定俗成的，
而golang的是语言层面决定的。

未完待续，持续更新～

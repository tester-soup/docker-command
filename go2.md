---
title: "Go（二）变量&常量"
date: 2023-03-07T13:34:58+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---

<!-- TOC -->

- [变量](#%E5%8F%98%E9%87%8F)
    - [变量声明](#%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E)
    - [变量初始化](#%E5%8F%98%E9%87%8F%E5%88%9D%E5%A7%8B%E5%8C%96)
- [常量](#%E5%B8%B8%E9%87%8F)
    - [常量定义](#%E5%B8%B8%E9%87%8F%E5%AE%9A%E4%B9%89)
    - [iota](#iota)
- [值类型和引用类型](#%E5%80%BC%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B)
- [Tips](#tips)

<!-- /TOC -->
## 变量
### 变量声明

```golang
var name string
```
<!--more-->
批量声明
```golang
var(
    name string
    age int
    b bool
)
```

### 变量初始化
```golang
var name string = "hahah"
```

类型推断
```golang
var name = "haha"
var age = 18
```
批量声明
```golang
var name,age,b = "haha",123,true
```
短变量声明   
```golang
age := 123
```



## 常量
### 常量定义
`const Name [type] = value`
```golang
const PI float32 = 3.14
```
常量一旦被定义，无法被修改

### iota
可被修改的常量编辑器

```golang
const (
    a1 = iota //0
    a2 = iota //1
    a3 = iota //2
    a4        //3
    a5        //4
)
fmt.Println(a1, a2, a3, a4, a5)
```
跳过某个值

```golang
const (
    a1 = iota  //0
    _          //iota+=1
    a3 = iota  //2
    a4 = "ggg" // iota+=1
    a5 = iota  //4
)
fmt.Println(a1, a3, a4, a5)
```


## 值类型和引用类型
  - 值类型：int、float、bool、string、数组、结构体
    - 变量直接指向内存中存在的值，内存在栈上分配
    - =号赋值为 值拷贝
  - 引用类型：指针、slice切片、map、管道chan、interface 
    - 引用类型存储都是内存地址，内存在堆上分配


## Tips
- 变量不能重复声明
- :=短变量声明，只能放在函数内部，不能放在函数外部用于声明全局变量
- 变量声明了，但没有使用，编译会报错
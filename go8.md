---
title: "Go（八）struct结构体"
date: 2023-03-09T18:17:15+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [结构体定义](#%E7%BB%93%E6%9E%84%E4%BD%93%E5%AE%9A%E4%B9%89)
- [结构体的内存布局](#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80)
- [结构体的比较](#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E6%AF%94%E8%BE%83)
- [结构体的转换](#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E8%BD%AC%E6%8D%A2)
- [结构体的嵌套和匿名成员](#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E5%B5%8C%E5%A5%97%E5%92%8C%E5%8C%BF%E5%90%8D%E6%88%90%E5%91%98)
- [Tips](#tips)

<!-- /TOC -->

## 结构体定义
- 结构体是自定义数据，代表一类事物
```go
type 结构体名称 struct{
    字段1 type
    字段2 type
}
```
<!--more-->
- 结构体字段 = 属性
- 字段可以是基本数据类型、数组、引用类型
- 结构体是值类型
```go
type Cat struct{
    Name string
    Age int
}
```
结构体声明
```go
type Cat struct {
    Name string
    Age  int
}

var cat1 Cat
cat1.Name = "miaomiao"
cat1.Age = 1

cat2 := Cat{"haha", 2}

var cat3 *Cat = new(Cat)
(*cat3).Name = "xixi"
cat3.Name = "xixihaha" //go程序设计者为了方便，cat3.Name 等同于（*cat3).Name

var cat4 *Cat = &Cat{}
cat4.Name = "heihei"
(*cat4).Age = 4
```
- 后两种声明方式返回结构体指针。
- go编译器底层对cat3.Name做了转化，等同于（*cat3).Name

## 结构体的内存布局
- 结构体的所有字段在内存中是连续的  
- 参考[https://www.liwenzhou.com/posts/Go/struct-memory-layout/](https://www.liwenzhou.com/posts/Go/struct-memory-layout/)

## 结构体的比较
- 如果结构体的全部成员都是可以进行比较的，那么结构体也是可以进行比较的

## 结构体的转换
- 结构体和其他类型进行转换时，需要有完全相同的字段（名字、个数、类型）

## 结构体的嵌套和匿名成员
- 只声明数据类型，不指名成员名字，称为匿名成员
- 匿名成员数据是类型必须是命名的类型或者指向一个命名类型的指针。
```go
type Point struct {
    X, Y int
}

type Circle struct {
    Point
    Radius int
}

type wheel struct {
    Circle
    Spokes int
}
var w wheel
w.X = 1  // 匿名属性直接访问，等同于 w.Circle.Point.X = 1
w.Y = 2  // 匿名属性直接访问，等同于 w.Circle.Point.Y = 2
w.Radius = 3
w.Spokes = 4
fmt.Println(w)
```
-  结构体字面值暂没有表示简短匿名的语法，以下不能编译通过
```go
w = Wheel{8, 8, 5, 20}                       // compile error: unknown fields
w = Wheel{X: 8, Y: 8, Radius: 5, Spokes: 20} // compile error: unknown fields
```
- 以下遵循形状类声明时的结构
```go
w := Wheel{Circle{Point{8, 8}, 10}, 20}
fmt.Println(w)

w2 := Wheel{
    Circle: Circle{
        Point:  Point{1, 2},
        Radius: 5,
    },
    Spokes: 8,
}
fmt.Println(w2)
```

## Tips
- 结构体类型的零值，是每个成员都是零值。
- 空结构体是不占空间的
- 结构体的变量可以进行取地址&c.Name
- 结构体进行type重新定义相当于取别名，golang认为是新的数据类型，但是可以互相强制转换
- 结构体变量名以大写字母开头，为公有变量，可以被外部引用。小写为私有变量。
- 结构体的每个字段可以写上一个tag，该tag可以通过反射机制获取。常见如果序列化和反序列化
```go
type Cat struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
cat := Cat{"haha", 2}
fmt.Println(cat)

jsonStr, err := json.Marshal(cat)
if err != nil {
    fmt.Println("json错误", err)
}
fmt.Println("jsonStr", string(jsonStr))
//jsonStr {"name":"haha","age":2}
```
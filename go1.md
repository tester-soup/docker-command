---
title: "Go（一）特点&命名"
date: 2023-03-07T13:31:58+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [go语言诞生](#go%E8%AF%AD%E8%A8%80%E8%AF%9E%E7%94%9F)
- [go语言的特点](#go%E8%AF%AD%E8%A8%80%E7%9A%84%E7%89%B9%E7%82%B9)
- [下载安装](#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85)
- [配置环境](#%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83)
- [go工具](#go%E5%B7%A5%E5%85%B7)
- [关键字](#%E5%85%B3%E9%94%AE%E5%AD%97)
- [命名规范](#%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
- [仪式感--第一个go程序](#%E4%BB%AA%E5%BC%8F%E6%84%9F--%E7%AC%AC%E4%B8%80%E4%B8%AAgo%E7%A8%8B%E5%BA%8F)
- [其他小tips](#%E5%85%B6%E4%BB%96%E5%B0%8Ftips)
- [编辑器](#%E7%BC%96%E8%BE%91%E5%99%A8)

<!-- /TOC -->

## go语言诞生
- 核心开发团队：Ken Thompson、Rob Pike、Robert Griesemer
- 诞生原因：
  - 计算机硬件频繁更新，性能提高很快。目前主流语言明显落后于硬件，不能合理使用**多核CPU**的优势提升软件性能
  - 软件系统复杂程度越来越高，维护成本越来越高，目前缺少一个足够简洁高效的编程语言。
  - 企业运行维护很多c/c++的项目，虽然速度快，编译速度却很慢，同时存在内存泄露等一系列问题。
<!--more-->
## go语言的特点
- 引入包概念。一个文件属于一个包，而不能单独存在。
- 天然支持并发
- 垃圾回收机制，内存自动回收
- 管道机制，实现不同的goroute之前相互通信
- 函数可以返回多个值
- 切片slice、延时执行defer
  

## 下载安装
略
[详情见官网](https://go.dev/doc/install)

## 配置环境
```shell
go env set GO111MODULE = "on"
go env set GOPROXY = "http://goproxy.cn"
```
<!-- goroot
gopath 高版本不依赖gopath，使用go mod来管理项目 -->

## go工具 
```
go run xxx.go     #执行程序
go build xxx.go   #编译程序
go clean          #清除对象文件和混存文件
go env            #查看环境信息
go test           #编译且安装包和依赖
go tool           #运行特定的go tool
go list           #查看包和模块
go version        #查看go版本
go get            #下载安装包和依赖
go install        #编译安装包和依赖
```

## 关键字
```
break      default       func     interface   select
case       defer         go       map         struct
chan       else          goto     package     switch
const      fallthrough   if       range       type
continue   for           import   return      var
```
预定义名字
```
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

内建函数: make len cap new append copy close delete
          complex real imag
          panic recover
```

## 命名规范
- 包名：一般和目录保持一致，小写单词，不使用下划线和混合大小写。
- 文件名：消协单词，使用下划线分隔单词
- 结构体名：驼峰命名，首字母根据访问控制大写或小写
- 接口名：同结构体。单个函数结果名以“er”作为后缀
- 变量名：同上。如appService变量类型为bool类型，则名称应以has、is、can或者allow开头
- 常量名：均需使用大写字母组成，并使用下划线分词

## 仪式感--第一个go程序
创建文件夹 hello
```shell
mkdir hello
cd hello
go mod init
```
创建文件 helloworld.go
```golang
package main

import "fmt"

func main() {
	fmt.Println("Hello world!")
}

```
执行 helloworld.go

```golang
go run helloworld.go
```
输出
```shell
[Running] go run "/Users/huangzeshan/go/src/hello/helloworld.go"
Hello world!
```

## 其他小tips
- go语言严格区分大小写
- 入口函数为main()
- go语言不需要在语句或者声明的末尾添加分号，除非一行上有多条语句。编译器会主动把特定符号后的换行符转换为分号
- go编译器是一行行行进行编译的，因此遗憾只能写一条语句，不能写多条语句
- import的包或者定义的变量没有使用到，代码编译不通过


## 编辑器
- 此处选择 **Vscode**  
- 插件安装：
  - Code Runner
  - Go

- 快捷键  
  ```  
  注释：   
    单行注释：[ctrl+k,ctrl+c] 或 ctrl+/ 
    取消单行注释：[ctrl+k,ctrl+u] (按下ctrl不放，再按k + u)   
    多行注释：[alt+shift+A]  
    多行注释：/**    

  折叠代码：   
    ctrl + k + 0-9 (0是完全折叠)   
    ctrl + shift + [ 折叠鼠标所在代码段     
    ctrl + shift + ] 展开鼠标所在代码段    
  
  展开代码： ctrl + k + j (完全展开代码)   
  显示/隐藏左侧目录栏 ctrl + b   
  控制台终端显示与隐藏：ctrl + ~      
  复制当前行：shift + alt +up/down  
  删除当前行：shift + ctrl + k   s

  全局替换：ctrl + shift + h  
  当前文件替换：ctrl + h    
  全局查找文件：ctrl + shift + f  
  
  新建一个窗口 : ctrl + shift + n    
  拆分编辑器 : ctrl + 1/2/3 .  
  切换窗口 : ctrl + shift + left/right    
  切换全屏 : F11    
  
  代码格式化：shift + alt +f 
    ```

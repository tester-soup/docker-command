---
title: "Go（五）程序流程控制"
date: 2023-03-09T18:14:37+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [if](#if)
- [switch](#switch)
- [for](#for)
- [break](#break)
- [continue](#continue)
- [跳转语句goto](#%E8%B7%B3%E8%BD%AC%E8%AF%AD%E5%8F%A5goto)
- [跳转语句return](#%E8%B7%B3%E8%BD%AC%E8%AF%AD%E5%8F%A5return)
- [练习](#%E7%BB%83%E4%B9%A0)
    - [打印金字塔](#%E6%89%93%E5%8D%B0%E9%87%91%E5%AD%97%E5%A1%94)
    - [打印空心的金字塔](#%E6%89%93%E5%8D%B0%E7%A9%BA%E5%BF%83%E7%9A%84%E9%87%91%E5%AD%97%E5%A1%94)
    - [打印乘法表](#%E6%89%93%E5%8D%B0%E4%B9%98%E6%B3%95%E8%A1%A8)
    - [登录验证实现，有三次机会，如果用户名为“earchou”，密码为“123”，提示登录成功，否则提示还有几次机会。](#%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81%E5%AE%9E%E7%8E%B0%E6%9C%89%E4%B8%89%E6%AC%A1%E6%9C%BA%E4%BC%9A%E5%A6%82%E6%9E%9C%E7%94%A8%E6%88%B7%E5%90%8D%E4%B8%BAearchou%E5%AF%86%E7%A0%81%E4%B8%BA123%E6%8F%90%E7%A4%BA%E7%99%BB%E5%BD%95%E6%88%90%E5%8A%9F%E5%90%A6%E5%88%99%E6%8F%90%E7%A4%BA%E8%BF%98%E6%9C%89%E5%87%A0%E6%AC%A1%E6%9C%BA%E4%BC%9A)

<!-- /TOC -->

## if 
单分支
```
if 条件表达式{
    执行代码块
}
```
双分支
```
if 条件表达式{
    执行代码块
}else{
    执行代码块
}
```
<!--more-->
多分支
```
if 条件表达式1{
    执行代码块
}else if 条件表达式2{
    执行代码块
}else{

}
```

## switch
```
switch 表达式{
case 表达式1,表达式2,...:
    语句块1
case 表达式3,表达式4,...:
    语句块2
default:
    语句块
}
```
- case可以有多个，逗号间隔，但不能重复
- default不是必须的
- case语句块不需要写break
- case按顺序执行
- case后的个个表达式的值类型，必须和switch的表达式数据一致
- switch后也可以不带表达式，累屎if--else分支来使用
    ```golang
    switch {
    case score >= 90:
        fmt.Println("优秀")
    case score >= 70 && score < 90:
        fmt.Println("良好")
    case score >= 60 && score <= 70:
        fmt.Println("及格")
    default:
        fmt.Println("不及格")
    }
    ```
- fallthrough穿透（只能穿透一层）。如果case语句块后增加fallthrough，则会继续执行下一个case。
    ```golang
    var n int = 10
    switch n {
    case 10:
        fmt.Println("ok1")
        fallthrough
    case 20:
        fmt.Println("ok2")
    case 30:
        fmt.Println("ok3")
    }
    //ok1
    //ok2
    ```
- Type Switch：判断某个interface变量中实际指向的变量类型
    ```golang
    var x interface{}
    var y = 10.0
    x = y

    switch i := x.(type) {
    case nil:
        fmt.Printf("x的类型：%T\n", i)
    case int:
        fmt.Println("x的类型：int")
    case float32:
        fmt.Println("x的类型：float32")
    case float64:
        fmt.Println("x的类型：float64")
    case string:
        fmt.Println("x的类型：string")
    case bool:
        fmt.Println("x的类型：bool")
    default:
        fmt.Println("未知类型")
    }
    ```

## for
```
for 循环变量初始化;循环条件;循环条件变量迭代{
    循环操作
}
```
```golang
for i:=0;i<10;i++{
    fmt.Println(i)
}
```
省略循环变量和迭代
```golang
j := 1
for j <= 10 {
	fmt.Println("hello")
	j++
}
```
全部省略（死循环，需要与break搭配使用）
```golang
j := 1
for {
	fmt.Println("hello")
	j++
	if j > 10 {
		break
	}
}
```
for-range遍历字符串
```golang
var str string = "hello,world"
for _, r := range str {
	fmt.Printf("%c\n", r)
}
```

## break
break语句用于终止某个语句的执行，中断当前for循环或switch语句
```
{
    ...
    break
    ...
}
```
多层嵌套中，可以通过标签，指明要终止哪一层
```golang
lable1:
	for i := 0; i < 10; i++ {
		for j := 0; j < 10; j++ {
			if j == 2 {
				break lable1
			}
			fmt.Println("j=", j)
		}
	}
```

## continue
continue语句用于结束本次循环，继续执行下一次循环
```
{
    ...
    continue
    ...
}
``` 
嵌套循环中，可以通过标签指定要跳过哪一层循环
```golang
lable1:
	for i := 0; i < 5; i++ {
		for j := 0; j < 5; j++ {
			if j == 2 {
				fmt.Println("")
				continue lable1

			}
			fmt.Printf("j=%d ", j)
		}

	}
```
输出
```shell
j=0 j=1 
j=0 j=1 
j=0 j=1 
j=0 j=1 
j=0 j=1
```
## 跳转语句goto
goto语句可以无条件跳转到程序中指定的行。
一般不主张使用goto，避免程序混乱。
```
goto label
...
label:statement
```
例子
```golang
	var n int = 3
	fmt.Println("哒哒哒...")
	if n > 2 {
		goto lable1
	}

	fmt.Println("没有经过goto语句")
lable1:
	fmt.Println("触发goto-->跳转到了label1")
```
输出
```shell
哒哒哒...
触发goto-->跳转到了label1
```

## 跳转语句return
终止函数，不再执行return后面的代码。
详情见**函数**章节
## 练习

### 打印金字塔
```
    *
   ***
  *****
 *******
*********
```
```golang
var rows int = 5
for i := 1; i <= rows; i++ {
	for k := 1; k <= rows-i; k++ {
		fmt.Print(" ")
	}
	for j := 1; j <= 2*i-1; j++ {
		fmt.Print("*")
	}
	fmt.Println("")
}
```
分析：   
空字符规律：行数 - 第几列    
*字符规律：2 * 行数 - 1
### 打印空心的金字塔
```
    *
   * *
  *   *
 *     *
*********
```
```golang
var rows int = 5
for i := 1; i <= rows; i++ {
	for k := 1; k <= rows-i; k++ {
		fmt.Print(" ")
	}
	for j := 1; j <= 2*i-1; j++ {
		if i == rows || j == 1 || j == 2*i-1 {
			fmt.Print("*")
		} else {
			fmt.Print(" ")
		}
	}
	fmt.Println("")
}
```
分析：    
首行、尾行、每层的第一个和最后一个打印*，其他都是空字符串
### 打印乘法表
```
1*1=1 
1*2=2 2*2=4 
1*3=3 2*3=6 3*3=9 
1*4=4 2*4=8 3*4=12 4*4=16 
1*5=5 2*5=10 3*5=15 4*5=20 5*5=25 
1*6=6 2*6=12 3*6=18 4*6=24 5*6=30 6*6=36 
1*7=7 2*7=14 3*7=21 4*7=28 5*7=35 6*7=42 7*7=49 
1*8=8 2*8=16 3*8=24 4*8=32 5*8=40 6*8=48 7*8=56 8*8=64 
1*9=9 2*9=18 3*9=27 4*9=36 5*9=45 6*9=54 7*9=63 8*9=72 9*9=81 
```

```golang
for i := 1; i <= 9; i++ {
	for j := 1; j <= i; j++ {
		fmt.Printf("%d*%d=%d ", j, i, i*j)
	}
	fmt.Println("")
}
```
### 登录验证实现，有三次机会，如果用户名为“earchou”，密码为“123”，提示登录成功，否则提示还有几次机会。
```golang
var username string
var password string
var login_count int = 3

for {
    fmt.Println("请输入用户名：")
    fmt.Scanln(&username)
    fmt.Println("请输入密码")
    fmt.Scanln(&password)
    
    if username == "earchou" && password == "123" {
        fmt.Println("登录成功")
        break
    }
    login_count--

    fmt.Printf("登录失败，还有%d次登录机会\n", login_count)
    if login_count == 0 {
        break
    }
}
```
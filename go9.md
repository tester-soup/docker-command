---
title: "Go（九）方法"
date: 2023-03-09T18:17:22+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---

<!-- TOC -->

- [方法定义和声明](#%E6%96%B9%E6%B3%95%E5%AE%9A%E4%B9%89%E5%92%8C%E5%A3%B0%E6%98%8E)
- [例子](#%E4%BE%8B%E5%AD%90)
- [练习](#%E7%BB%83%E4%B9%A0)
    - [定义计算器结构体Calcuator，实现加减乘除](#%E5%AE%9A%E4%B9%89%E8%AE%A1%E7%AE%97%E5%99%A8%E7%BB%93%E6%9E%84%E4%BD%93calcuator%E5%AE%9E%E7%8E%B0%E5%8A%A0%E5%87%8F%E4%B9%98%E9%99%A4)
    - [编写方法，使给定的二维数组（3*3）转置。](#%E7%BC%96%E5%86%99%E6%96%B9%E6%B3%95%E4%BD%BF%E7%BB%99%E5%AE%9A%E7%9A%84%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%8433%E8%BD%AC%E7%BD%AE)
    - [阅读以下代码，写出打印结果](#%E9%98%85%E8%AF%BB%E4%BB%A5%E4%B8%8B%E4%BB%A3%E7%A0%81%E5%86%99%E5%87%BA%E6%89%93%E5%8D%B0%E7%BB%93%E6%9E%9C)

<!-- /TOC -->
## 方法定义和声明
```
func (recevier type)methodName(参数列表)(返回值列表){
    方法体
    return
}

```
作用在指定的数据类型上的，和指定数据类型绑定。因此自定义类型都可以有方法。
<!--more-->
```go
type A struct {
	Num int
}

func (a A) test() {
	fmt.Println(a.Num)
}

func main() {
	var a A
	a.Num = 23
	a.test()
}

```

## 例子
```go
type Person struct {
	Name string
	age  int
}

func (p Person) GetAge() {
	fmt.Printf("%v的年龄为：%d\n", p.Name, p.age)
}

func (p *Person) SetAge(age int) {
	p.age = age
}

func (p Person) eat(food string) {
	fmt.Printf("%v在吃%v...\n", p.Name, food)
}
func main() {
	var p Person
	p.Name = "xixi"
	p.SetAge(18)
	p.GetAge()
	p.eat("好吃的")

}

```
输出
```powershell
xixi的年龄为：18
xixi在吃好吃的...
```
- 方法名大写字母开头为公有方法，外部可以调用。小写为私有，外部无法调用。
- 方法接收者也是值拷贝。如需修改原来的值，需传指针类型。


## 练习
### 定义计算器结构体Calcuator，实现加减乘除
```go
type Calcuator struct {
	a, b float64
}

func (c *Calcuator) cal(s string) float64 {
	res := 0.0
	switch s {
	case "+":
		res = c.a + c.b
	case "-":
		res = c.a - c.b
	case "*":
		res = c.a * c.b
	case "/":
		res = c.a / c.b
	default:
		fmt.Printf("符号[ %v ]输入错误 \n", s)
	}
	return res
}

func (c *Calcuator) getSum() float64 {
	return c.a + c.b
}

func main() {
	var c Calcuator
	c.a = 1
	c.b = 23
	s := "/"
	fmt.Printf("%v%v%v=%v\n", c.a, s, c.b, c.cal(s))

}
```
### 编写方法，使给定的二维数组（3*3）转置。
```dotnetcli
1 2 3       1 4 7
4 5 6  -->  2 5 8
7 8 9       3 6 9
```
```go
type Arr2D struct {
	arr [][]int
}

func (arr2D *Arr2D) change() {
	if len(arr2D.arr) != len(arr2D.arr[0]) {
		fmt.Println("请传入n*n的数组")
		return
	}
	//新建切片，以原来的切片长度为长度
	new_arr2D := make([][]int, len(arr2D.arr)) 

	for i := 0; i < len(arr2D.arr); i++ {
		for j := 0; j < len(arr2D.arr[i]); j++ {
			//切片需要先make，再使用，如果为nil则make
			if new_arr2D[j] == nil { 
				new_arr2D[j] = make([]int, len(arr2D.arr[i]))
			}
			//交换n*n切片的坐标
			new_arr2D[j][i] = arr2D.arr[i][j] 
		}

	}
	//赋值给原来的切片
	arr2D.arr = new_arr2D
}
func main() {
	var a Arr2D
	a.arr = [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	a.change()
	fmt.Println(a)
```

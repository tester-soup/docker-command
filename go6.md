---
title: "Go（六）数组&切片"
date: 2023-03-09T18:15:30+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [数组](#%E6%95%B0%E7%BB%84)
    - [数组定义](#%E6%95%B0%E7%BB%84%E5%AE%9A%E4%B9%89)
    - [数组的使用](#%E6%95%B0%E7%BB%84%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [数组的比较](#%E6%95%B0%E7%BB%84%E7%9A%84%E6%AF%94%E8%BE%83)
    - [数组的内存布局](#%E6%95%B0%E7%BB%84%E7%9A%84%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80)
    - [Tips](#tips)
    - [练习](#%E7%BB%83%E4%B9%A0)
        - [创建一个byte类型的26个元素的数组，分别放置‘A’-‘Z’。使用for循环访问所有元素并打印。](#%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAbyte%E7%B1%BB%E5%9E%8B%E7%9A%8426%E4%B8%AA%E5%85%83%E7%B4%A0%E7%9A%84%E6%95%B0%E7%BB%84%E5%88%86%E5%88%AB%E6%94%BE%E7%BD%AEa-z%E4%BD%BF%E7%94%A8for%E5%BE%AA%E7%8E%AF%E8%AE%BF%E9%97%AE%E6%89%80%E6%9C%89%E5%85%83%E7%B4%A0%E5%B9%B6%E6%89%93%E5%8D%B0)
        - [求出数组最大值，及其下标](#%E6%B1%82%E5%87%BA%E6%95%B0%E7%BB%84%E6%9C%80%E5%A4%A7%E5%80%BC%E5%8F%8A%E5%85%B6%E4%B8%8B%E6%A0%87)
        - [随机生成5个数，并将其反转打印](#%E9%9A%8F%E6%9C%BA%E7%94%9F%E6%88%905%E4%B8%AA%E6%95%B0%E5%B9%B6%E5%B0%86%E5%85%B6%E5%8F%8D%E8%BD%AC%E6%89%93%E5%8D%B0)
- [切片](#%E5%88%87%E7%89%87)
    - [切片定义](#%E5%88%87%E7%89%87%E5%AE%9A%E4%B9%89)
    - [切片的访问、遍历](#%E5%88%87%E7%89%87%E7%9A%84%E8%AE%BF%E9%97%AE%E9%81%8D%E5%8E%86)
    - [切片表达式](#%E5%88%87%E7%89%87%E8%A1%A8%E8%BE%BE%E5%BC%8F)
    - [append动态追加](#append%E5%8A%A8%E6%80%81%E8%BF%BD%E5%8A%A0)
    - [从切片中删除元素](#%E4%BB%8E%E5%88%87%E7%89%87%E4%B8%AD%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
    - [切片的拷贝](#%E5%88%87%E7%89%87%E7%9A%84%E6%8B%B7%E8%B4%9D)
    - [string和byte](#string%E5%92%8Cbyte)
    - [切片扩容](#%E5%88%87%E7%89%87%E6%89%A9%E5%AE%B9)
    - [Tips](#tips)
    - [练习](#%E7%BB%83%E4%B9%A0)
        - [斐波那契数列](#%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97)

<!-- /TOC -->

## 数组
### 数组定义
```
var 数组[数组大小]数据类型
```
数组长度固定,可以由0个或多个元素组成
  ```golang
  var arr1 [3]int = [3]int{1,2,3}

  var arr2 = [3]int{1,2,3}
  
  //数组不确定长度，由...代替，长度由初始化的数据决定 
  var arr3 [...]int = [1,2,3,4,5]

  //指定索引值初始化;类型推断初始化
  arr4 := [...]int{1: 1, 3: 5} //[0,1,0,5]
  ```
<!--more-->
### 数组的使用
- 访问 `数组名[下标]`
- 遍历  
  for循环遍历
  ```golang
  var arr = [...]string{"a", "b", "c"}
  for i := 0; i < len(arr); i++ {
    fmt.Printf("index[ %d ] value[ %v ]\n", i, arr[i])
  }
  ```
  for-range遍历
  ```golang
  var arr = [...]string{"a", "b", "c"}
  for _, value := range arr {
    fmt.Printf("value[ %v ]\n", value)
  }
  ```
### 数组的比较
当两个数组的值和类型都相等，才相等
```golang
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
d := [2]int{2, 1}
fmt.Println(a == b, a == c, a == d) //true false false
```
### 数组的内存布局
- 数组的地址通过数组名来获取 &arr
- 首元素地址就是数组的首地址
- 数组个个元素地址间隔是依据数组的类型来决定的，如int -> 8个字节  
 

### Tips
- 数组是值传递，支持“==”，“！=”等操作符
- [n]\*T表示指针数组，\*[n]T表示数组指针
- 多维数组只有第一层可以使用...来让编译器推导数组长度
- 数组中的元素可以说任何数据类型，但不能混用
- 数组传参时，需要考虑数组长度   
  错误例子1
  ```golang
  func modifyArr(arr []int) {
    arr[0] = 100
    fmt.Println("arr:", arr)
  }
  func main() {
    var arr = [...]int{1, 2, 3}
    modifyArr(arr)

  }
  // cannot use arr (variable of type [3]int) as type []int in argument to modifyArr
  ```
  错误例子2
  ```golang
  func modifyArr(arr [4]int) {
    arr[0] = 100
    fmt.Println("arr:", arr)
  }
  func main() {
    var arr = [...]int{1, 2, 3}
    modifyArr(arr)

  }
  //cannot use arr (variable of type [3]int) as type [4]int in argument to modifyArr
  ```
  正确例子
  ```golang
  func modifyArr(arr [3]int) {
    arr[0] = 100
    fmt.Println("arr:", arr)
  }
  func main() {
    var arr = [...]int{1, 2, 3}
    modifyArr(arr)

  }
  //arr: [100 2 3]
  ```

### 练习
#### 创建一个byte类型的26个元素的数组，分别放置‘A’-‘Z’。使用for循环访问所有元素并打印。
```golang
var chars [26]byte
for i := 0; i < len(chars); i++ {
	chars[i] = 'A' + byte(i)
	fmt.Printf("chars %c \n", chars[i])
}
```
#### 求出数组最大值，及其下标
```golang
var arr = [6]int{1, 2, 3, 4, 5, 5}
var max int
var maxIndex int
for i, val := range arr {
  if val > max {
    max = val
    maxIndex = i
  }
}
fmt.Printf("max=%d,maxIndex=%d \n", max, maxIndex)
```
#### 随机生成5个数，并将其反转打印
```golang
var arr [5]int

rand.Seed(time.Now().UnixNano())
for i, _ := range arr {
  arr[i] = rand.Intn(100) //0 <=n<100
  fmt.Printf("arr[%d]=%v\n", i, arr[i])
}
for i := 0; i < len(arr)/2; i++ {
  arr[i], arr[len(arr)-1-i] = arr[len(arr)-1-i], arr[i]
}
fmt.Printf("交换后arr：%v", arr)
```
反转数组
```go
	for i, j := 0, len(arr)-1; i < j; i, j = i+1, j-1 {
		arr[i], arr[j] = arr[j], arr[i]
	}
```
## 切片
- 切片是引用类型
- 切片使用与数组类似
- 切片是一个动态数组
### 切片定义
`var 切片名 []类型`      
`var 切片名 []type = make([]type,len,cap)`   
len 大小；cap指切片容量（可选）。cap>=len
```golang
//1.定义切片，引用数组
var arr = [5]int{1, 2, 3, 4, 5}
var slice = arr[:3]

// 2.make创建切片
var slice2 []int = make([]int, 5, 10)
slice2 = []int{3, 2, 1}

//3. 定义切片，直接指定数组
var slice3 []int = []int{5, 6, 7}
```
1和3的方式创建切片有什么区别？
- 方式1，直接引用数组，数组是事先存在的，对程序员可见
- 方式2，make也会直接创建一个数组，由切片在底层维护，对程序员不可见。

### 切片的访问、遍历
同[数组](#数组的使用)

### 切片表达式  
`s[low:high]`或`s[low : high : max]`(完整表达式)
  - low,high表示索引范围(左包含，右不包含)
  - 切片长度=high-low
  - 索引在运行时超出范围，会发生运行时panic
  - 切片容量=max-low
  - 完整切片表达式中，仅low可省略，默认为0
  - 完整表达式必须满足 `0 <= low <= high <= max <= cap(a)`

### append动态追加
```golang
var slice []int = []int{1, 2, 3, 4, 5, 6, 7, 8}
var slice2 = slice[2:5]
fmt.Printf("slice2 %v\n", slice2) //  slice2 [3 4 5]
slice2 = append(slice2, 0)
fmt.Printf("slice2 %v\n",  slice2) // slice2 [3 4 5 0]
```
注：slice 和slice2指向的是同一个数据空间

### 从切片中删除元素
```go
a := []int{1, 2, 3, 4, 5, 6, 7, 8}
a = append(a[:2], a[3:]...)
fmt.Println(a) //[1 2 4 5 6 7 8]
```

### 切片的拷贝
```go
var slice []int = []int{1, 2, 3, 4, 5}
var slice2 = make([]int, 8, 10)
copy(slice2, slice)
fmt.Printf("slice2 %v \n", slice2) //slice2 [1 2 3 4 5 0 0 0] 
slice2[0] = 100
fmt.Printf("slice %v \nslice2 %v \n", slice, slice2)
// slice [1 2 3 4 5] 
// slice2 [100 2 3 4 5 0 0 0] 
```

### string和byte
- string的底层是一个byte数组，因此string也可以进行切片
- string是不可修改的，不能通过`str[0]='A'`修改字符串
- 如需修改，可以转换成byte或rune修改再 重写成string
```go
var str string = "hi,你好"
arr := []byte(str)
arr[0] = 'H'
str = string(arr)
fmt.Println(str) //Hi,你好

arr2 := []rune(str)
arr2[3] = '您'
str = string(arr2)
fmt.Println(str) //Hi,您好
```
### 切片扩容
```go
var s []int
for i := 0; i < 10; i++ {
  s = append(s, i)
  fmt.Printf("cap=%d, len=%d, s=%v\n", cap(s), len(s), s)
}
```
```dotnetcli
cap=1, len=1, s=[0]
cap=2, len=2, s=[0 1]
cap=4, len=3, s=[0 1 2]
cap=4, len=4, s=[0 1 2 3]
cap=8, len=5, s=[0 1 2 3 4]
cap=8, len=6, s=[0 1 2 3 4 5]
cap=8, len=7, s=[0 1 2 3 4 5 6]
cap=8, len=8, s=[0 1 2 3 4 5 6 7]
cap=16, len=9, s=[0 1 2 3 4 5 6 7 8]
cap=16, len=10, s=[0 1 2 3 4 5 6 7 8 9]
```


### Tips
- 判断切片是否为空，使用len(s)==0，而不是s==nil 
- 切片不能直接比较


### 练习
#### 斐波那契数列
```go
func main(){
	var n int = 10
	r := fbn(n)
	fmt.Println(r)

}
func fbn(n int) []uint64 {
	fbnslice := make([]uint64, n)
	fbnslice[0] = 1
	fbnslice[1] = 1
	for i := 2; i < n; i++ {
		fbnslice[i] = fbnslice[i-1] + fbnslice[i-2]
	}
	return fbnslice
}
```
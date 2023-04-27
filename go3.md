---
title: "Go（三）基础数据类型"
date: 2023-03-07T14:31:58+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [数据类型总览](#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E6%80%BB%E8%A7%88)
    - [数字类型](#%E6%95%B0%E5%AD%97%E7%B1%BB%E5%9E%8B)
    - [浮点型](#%E6%B5%AE%E7%82%B9%E5%9E%8B)
    - [其他数字类型](#%E5%85%B6%E4%BB%96%E6%95%B0%E5%AD%97%E7%B1%BB%E5%9E%8B)
- [bool类型](#bool%E7%B1%BB%E5%9E%8B)
- [字符串类型](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%B1%BB%E5%9E%8B)
- [数值型](#%E6%95%B0%E5%80%BC%E5%9E%8B)
    - [整型](#%E6%95%B4%E5%9E%8B)
    - [浮点型float（float32、float64）](#%E6%B5%AE%E7%82%B9%E5%9E%8Bfloatfloat32float64)
- [字符类型](#%E5%AD%97%E7%AC%A6%E7%B1%BB%E5%9E%8B)
- [基本数据转换](#%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2)
    - [Tv](#tv)
    - [基本类型转换成string](#%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E6%88%90string)
    - [string转成基本数据类型](#string%E8%BD%AC%E6%88%90%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)

<!-- /TOC -->

*详情看官网[https://go.dev/doc/effective_go#data](https://go.dev/doc/effective_go#data)*

## 数据类型总览
|序号|类型|描述|
|--|--|--|
|1|Bool|true or false|
|2|int| <li>int</li> <li>float32</li> <li>float64</li> |
|3|string|<li>Go 的字符串是由单个字节连接起来的。</li><li>go语言字符串使用Utf-8编码标识Unicode文本</li>|
|4|派生类|<li>指针（Pointer）</li><li>数组</li><li>结构体（struct）</li><li>通道（Channel）</li><li>函数</li><li>切片</li><li>接口（interface）</li><li>Map</li>|
<!--more-->
### 数字类型
|序号|类型|描述|
|--|--|--|
|1|uint8|无符号8位整型（0-255）|
|2|uint16|无符号8位整型（0-65535）|
|3|uint32|无符号8位整型（0-4294967295）|
|4|uint64|无符号8位整型（0-18446744073709551615）|
|5|int8|无符号8位整型（-128-127）|
|6|int16|无符号8位整型（-32768-32767）|
|7|uint32|无符号8位整型（-2147483648-2147483647）|
|8|uint64|无符号8位整型（-9223372036854775808-9223372036854775807）|

### 浮点型
|序号|类型|描述|
|--|--|--|
|1|float32| IEEE-754 32位浮点型数 |
|2|float64| IEEE-754 64位浮点型数|
|3|complex64| 32位实数和虚数|
|4|complex128| 64位实数和虚数 |

### 其他数字类型
|序号|类型|描述|
|--|--|--|
|1|byte|类似uint8，要存储字符时，选byte|
|2|rune| 类似int32,表示一个Unicode码|
|3|uint|无符号，32系统4个字节，64位系统8个字节|
|4|int|有符号，32位系统4个字节，64位系统8个字节|
|5|uinptr|无符号整型，用于存放一个指针|

> 参考[https://www.runoob.com/go/go-data-types.html](https://www.runoob.com/go/go-data-types.html) 

## bool类型
- 不能使用0 和非0表示bool类型
- 占一个字节
- 只有true和false
- 默认值 false
- bool值与数字的转换
    ```golang
    func itob(i int) bool { return i != 0 }
    ```
## 字符串类型
- **双引号**扩起来的内容，可以识别转义字符
- **反引号**扩起来，表示原生字符串，没有转义操作，可用于写正则，防攻击等。
- 使用多行字符串时，用**反引号**。
- 默认值`""`
- 字符串连接方式
  - +号
    ```golang
    s1 := "hello"
    s2 := " world"
    fmt.Println(s1 + s2)
    ```
  - 
    ```golang
    s1 := "hello"
    s2 := " world"
    fmt.Printf("%s %s", s1, s2)
    ```
  - 
    ```golang
    s1 := "hello"
    s2 := " world"
    s3 := strings.Join([]string{s1, s2}, " ")
    fmt.Println(s3)
    ```
  - 
      ```golang
    s1 := "hello"
    s2 := " world"
    var buffer bytes.Buffer
    buffer.WriteString(s1)
    buffer.WriteString(" ")
    buffer.WriteString(s2)
    fmt.Printf("buffer.String(): %v\n", buffer.String())
      ```
- 转义字符 
    ```
    \a      响铃
    \b      退格
    \f      换页
    \n      换行
    \r      回车
    \t      制表符
    \v      垂直制表符
    \'      单引号（只用在 '\'' 形式的rune符号面值中）
    \"      双引号（只用在 "..." 形式的字符串面值中）
    \\      反斜杠
    ```
- 字符串切片
    ```golang
    s := "hello world" 
	fmt.Println(s[5:]) //world
    ```
- 字符串索引
  - 索引操作s[i]返回第i个字节的字节值
  - i必须满足0 ≤ i< len(s)条件约束
  - 
    ```golang
    s := "hello world"
    fmt.Println(s[6]) //119
    ```
  
- 字符串常用函数    
  - `len()`字符串长度（返回字符串中的字节数，不是rune字符数）
    ```golang
    s := "hello world"
    s2 := "中文字符"
    fmt.Println(len(s))  //11
    fmt.Println(len(s2))  //12
    ```
  - `+`号或`fmt.Sprintf`  拼接字符串
  - `strings.Split()`  分割
  - `strings.contains()`  判断是否包含
  - `strings.HasPrefix()`,`strings.HasSuffix()`  前缀/后缀判断
  - `strings.Index()`,`strings.LastIndex()` 子串出现位置
  - `strings.Join(a[]string,sep string)` join操作




- 占位符
  - 普通占位符
    ```
    %v       相应的值
    %#v      函数名+相应的值
    %T       值的类型
    %%       %百分号
    ```
  - 布尔占位符
    ```
    %t       单词true或false
    ```
  - 整数占位符
    ```
    %b       二进制
    %c       相应的Unicode码点所表示的字符
    %d       十进制
    %o       八进制
    %q       单引号围绕的字符字面值
    %x       十六进制，a-f
    %X       十六进制，A-F
    %U       Unicode格式U+1234
    ```
  - 浮点数和复数组成部分
    ```
    %b        无效书部分，指数为2的科学计数法
    %e        科学记数法
    %E        科学计数法
    %f        有小数无指数
    %g        根据情况选择%e或%f，末尾无0的输出（10.2）
    %G        根据情况选择%E或%f，末尾无0的输出（10.2+2i）
    ```
  - 字符串
    ```
    %s        输出字符串表示（string类型或[]byte）
    %q        双引号围绕的字符串
    %x        十六进制，a-f，每个字节两个字符
    %X        十六进制，A-F，每个字节两个字符
    ```
   - 指针
        ```
        %p        输出十六进制地址，前缀0x
        ```

## 数值型
### 整型
- 具体见[数字类型](#数字类型)
- 默认值0
### 浮点型float（float32、float64）
- 默认值0
- 非数NaN值，表示无效的除法操作结果
  ```golang
  var z float64
  fmt.Println(z, z/z, 0/z)
  ```
- NaN和任何数都是不相等的
  ```golang
  nan := math.NaN()
  fmt.Println(nan == nan, nan < nan, nan > nan) // "false false false"
  ```
## 字符类型
- 单引号扩起来
- 本质上一个整数，输出为该字符对应的UTF-8编码
- 可以进行运算，相当于整数，因为他们都有对应的Unicode编码
- 给一个变量赋值数字，输出按格式化%c，会输出对应的Unicode编码  

go语言的字符有以下两种   
1. uint8类型，叫byte型，代表一个ASCII码字符。
2. rune类型，代表一个UTF-8字符。int32.
   
字符串遍历例子
  ```golang
  s := "hello你好"
  for i := 0; i < len(s); i++ { //byte
    fmt.Printf("%v(%c) ", s[i], s[i])
  }
  fmt.Println() 
  //104(h) 101(e) 108(l) 108(l) 111(o) 228(ä) 189(½) 160( ) 229(å) 165(¥) 189(½) 

  for _, r := range s { //rune
    fmt.Printf("%v(%c) ", r, r)
  }
  fmt.Println()
  // 104(h) 101(e) 108(l) 108(l) 111(o) 20320(你) 22909(好) 
  ``` 
  >参考[https://www.liwenzhou.com/posts/Go/datatype/](https://www.liwenzhou.com/posts/Go/datatype/)
  
## 基本数据转换
### T(v)
- 需要显示转换，不能自动转换 
- T为类型，v为值

### 基本类型转换成string
- fmt.Sprintf
  ```golang
  var num1 int = 99
  str = fmt.Sprintf("%d", num1)
  fmt.Printf("str type: %T, str=%q \n", str, str)
  ```
- strconv   
  *官网strconv库文档 [https://pkg.go.dev/strconv@go1.20.2](https://pkg.go.dev/strconv@go1.20.2)*
  ```golang
  var num1 int = 99
  s := strconv.FormatInt(int64(num1), 2) //转成二进制字符串
  fmt.Printf("type:%T , s=%q", s, s)     //type:string , s="1100011"

  s2 := strconv.FormatInt(int64(num1), 10) //转成10进制字符串
  fmt.Printf("type:%T , s=%q", s2, s2)     //type:string , s="99"%
  ```

### string转成基本数据类型
  ```golang
  var str string = "true"
  var b bool
  b, _ = strconv.ParseBool(str)
  fmt.Printf("type:%T , str=%v", b, b) //type:bool , str=true
  ```
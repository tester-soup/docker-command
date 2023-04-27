---
title: "Go（七）Map"
date: 2023-03-09T18:16:50+08:00
# draft: true
tags: ["golang"]
categories: ["notes"]
---
<!-- TOC -->

- [map定义](#map%E5%AE%9A%E4%B9%89)
- [map增删改查](#map%E5%A2%9E%E5%88%A0%E6%94%B9%E6%9F%A5)
    - [增加/修改](#%E5%A2%9E%E5%8A%A0%E4%BF%AE%E6%94%B9)
    - [删除](#%E5%88%A0%E9%99%A4)
    - [查找](#%E6%9F%A5%E6%89%BE)
- [map遍历](#map%E9%81%8D%E5%8E%86)
- [map切片](#map%E5%88%87%E7%89%87)
- [map排序](#map%E6%8E%92%E5%BA%8F)
- [Tips](#tips)

<!-- /TOC -->
## map定义
`map[Key]Value`
- key可以是bool、数字、string、指针、channel，也可以是包含这几种类型的接口、结构体、数组
- key必须是支持==运算的类型。不支持slice、map、function
- key避免使用浮点数。可能出现NaN或者浮点数不相等的情况。
- Value值没有限制 
- key不能重复
- key是无序的
<!--more-->
```go
var m1 map[string]string
m1 = make(map[string]string, 5)
m1["name1"] = "earhcou1"
m1["name2"] = "earchou2"
fmt.Println(m1)

m2 := make(map[string]string)
m2["test1"] = "哈哈哈"
m2["test2"] = "嘻嘻嘻"
fmt.Println(m2)

m3 := map[string]string{
    "key1": "value1",
    "key2": "value2",
}
fmt.Println(m3)
```
- map声明不会分配空间
- map使用之前需要用make分配空间

## map增删改查
### 增加/修改
`map[key] = value`    
如果key不存在则增加，key存在则修改value

### 删除
`delete(map, "key")`     
删除所有key，可以遍历key，逐个删除。或者make一个新的，让原来的变成垃圾。被gc收回。
```go
m3 := map[string]string{
    "key1": "value1",
    "key2": "value2",
}
fmt.Println(m3) //map[key1:value1 key2:value2]
m3 = make(map[string]string)
fmt.Println(m3)  //map[]
```
### 查找
`map[key]` 如果key值不存在，则返回该类型的0值
```go
m3 := map[string]string{
		"key1": "value1",
		"key2": "value2",
	}

val, ok := m3["key1"]
if ok {
    fmt.Printf("找到啦！value=%v\n", val)
} else {
    fmt.Printf("没找到。\n")
}
```
## map遍历

```go
m3 := map[string]string{
    "key1": "value1",
    "key2": "value2",
}

for key, value := range m3 {
    fmt.Printf("%v : %v\n", key, value)
}
```

## map切片
如果切片的类型是map，则称为slice of map，map切片，这样map个数就可以动态变化。  
`var m []map[string]string`

## map排序
- 按key值排序
- 先将key放入到切片中
- 对切片进行排序
- 遍历切片，按照key来输出map的值
```go
map1 := make(map[int]int)
map1[10] = 100
map1[11] = 110
map1[12] = 120
fmt.Println(map1)

var keys []int
for k, _ := range map1 {
    keys = append(keys, k)
}
sort.Ints(keys)
fmt.Println(keys)

for _, k := range keys {
    fmt.Printf("map1[%v]=%v\n", k, map1[k])
	}
```

## Tips
- map容量到达后，会自动扩容，不会发生panic
- map是引用类型
- map的value经常使用struct类型
- map的默认初始值为nil
- map中的元素不是一个变量，因此对他进行区地址 `&map[key]`
- map无法直接进行比较，除非是nil

    通过函数循环实现
    ```go
    func equal(x, y map[string]int) bool {
        if len(x) != len(y) {
            return false
        }
        for k, xv := range x {
            if yv, ok := y[k]; !ok || yv != xv {
                return false
            }
        }
        return true
    }
    ```




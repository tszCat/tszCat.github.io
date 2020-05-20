---
layout: post
title:  "[Golang]Slice的使用与原理"
date:   2020-04-08
categories: jekyll update
---

### 数组

数组(array)是计算机科学中非常重要的一种数据结构，它是相同数据类型元素的集合。

数组的大小是固定的，在golang中数组的长度是数组类型的一部分

```go
//虽然a和b都是int类型数组，但是由于长度不一样，所以它们不是同一类的数组
var a [2]int
var b [3]int
fmt.Println(reflect.TypeOf(a))	//[2]int
fmt.Println(reflect.TypeOf(b))	//[3]int
```



### 切片

同样作为一种数据结构，切片(slice)与数组存在关联，但是它们是两个不同的概念。切片是某个数组连续片段的描述符，这个数组通常被称为切片的底层数组(underlying array)。多个切片可以描述同一个底层数组的相同或者不同连续片段。

切片可以看作由三部分组成：

* 指向数组元素的指针
* 切片的长度(`len`)
* 容量（`cap`，从指向底层数组元素的位置到底层数组最后一个元素之间的元素个数）

代码表示如下：

```go
type sliceHeader struct{
    ZerothElement uintptr
    Length        int
    Capacity      int
}
```

![slice](/assets/slice.svg)

<center>底层数组与切片</center>



#### 初始化切片的三种方式

* **使用下标对已有的数组或切片进行切片**

```go
arr := [3]int{1,2,3}
sli := []int{4,5,6}

slice1 := arr[0:2] //slice1的底层数组为arr
slice2 := sli[0:2] //slice2与sli指向同一个底层数组
```



* 使用字面量进行初始化

```go
slice := []int{1,2,3}
```



* 使用关键字`make`初始化

golang内置函数`make()`可以用来初始化切片。

```go
func make([]T, len, cap) []T
```

`len`和`cap`分别用来指定slice的长度和容量，其中，`cap`是可选参数。由于上面讲到，切片是某个底层数组的连续片段，所以，使用`make()`初始化切片时会先分配一个新的底层数组，然后再创建一个切片指向这个底层数组数组，底层数组的长度等于`cap`。如果没有传入`cap`，则默认`cap`等于`len`，其效果相当于：

```go
func make([]T, len, len) []T
```





### Reference

* Donovan, Alan A. A, Kernighan, Brian W. The Go programming language[M]. China Machine Press, 2016.

* https://blog.golang.org/slices-intro 
* https://blog.golang.org/slices 
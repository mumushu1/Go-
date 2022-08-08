# 更多类型：struct、slice和映射

## 指针

- 指针保存了值的内存的地址
- *T是指向T的指针，零值为==nil==

```go
var p *int
```

- ==&操作符==会生成一个指向其操作数的指针

```go
i:=2
p=&i
```

- ==*操作符==表示指针指向的底层值
- 与C不同，Go没有指针运算

```go
func main (){
    i,j:=12,31
    p:=&i//p是指向i的指针
    fmt.Println(*p)
    *p=21//通过指针改变i的值
    
    p=&j//p指向j,因为在前面p已经声明过了，所以不用:=
    *p=*p/2//通过指针对j进行除法运算
    
}
```

## 结构体

一个结构体就是一组字段

```go
import "fmt"

type jiegou struct{
    X int 
    Y int 
}

func main (){
    fmt.Println(jiegou{1,2})
}
//输出结果为{1,2}
```

#### 结构体字段

结构体字段使用**点号**来访问

```go
type Vertex struct {
    x int
    y int
}

func main (){
    v:=Vertex{1,2}
    v.x=3
    fmt.Println(v.x)
}//输出：3
```

#### 结构体指针

- 结构体字段可以用结构体指针来访问
- 如果我们有一个指向结构体的指针p，通过（*p）.x就可以访问字段x，直接写成==p.x==也可以

```go
type Vertex struct {
    x int
    y int
}


func main (){
    v:=Vertex{1,2}
    p:=&v
    p.x=3
    fmt.Println(v)//结果为（3，2）
}
```

#### 结构体文法

结构体文法通过==直接列出字段的值==来新分配一个结构体

```go
type Vertex struct {
    x int
    y int
}
func main (){
    v1=Vertex{1,2}
    v2=Vertex{x:1}//默认y:0
    v3=Vertex{}//默认x:0,y:0
    p=&Vertex{1,2}//p是指向{1,2}的结构体指针
}

```

## 数组

[n]T表示n个T类型的数组

```
var a[10]int 
```

```go
var main (){
    var a[2]string
    a[0]="hello"
    a[1]="world"
    
    b:=[6]int{1,2,3,4,5,6}
}
```

## 切片

- 每个数组的大小都是固定的，而切片为数组提供==动态大小的、灵活的视角==，在实践中，切片比数组更常用。
- 类型[ ]T表示一个元素类型为T的切片
- 切片通过两个下标来定，一个**上界**一个**下界**，二者用**冒号**分离  ==a[low : high]==
- 它会选择一个半开区间，**包括第一个元素，但排除最后一个元素**
- 例：包含下标从1到3的切片

```go
a[1:4]
```

```go
func main (){
    b:=[6]int {1,2,3,4,5,6}
    
    var s[]int = b[1:4]//切片s包含b1-b3
}
```

#### 切片就像数组的引用

- 切片不存储数据，只是描述了**底层数组的一段**
- 更改切片的元素会修改其底层数组对应的元素

```go
func main (){
    names:=[4]string {
        "LiHua"
        "Mike"
        "Tom"
        "ha"
    }
    a:=names[0:2]
    a[0]="Jack"
    fmt.Pritln(names)//names[0]就改为了jack
    
}
```

#### 切片文法

- 切片文法类似于没有长度的数组文法
- 数组文法

```go
[3]bool{true,false,true}
```

- 切片文法

```go
[]bool{true,false,true}
```

```go
func main (){
    q:=[]int {1,2,3,4,5,5}
    p:=[]bool{true,false}
}
```

#### 切片的默认行为

- 在进行切片时，可以利用默认行为**忽略上下界**，切片的**下界默认值为0，上界则是该切片的长度**

```go
var a[10]int 
a[0:10]
a[:10]
a[0:]
a[:]是等价的
```


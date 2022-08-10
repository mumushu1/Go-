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

#### 切片的长度和容量

- 切片的长度：包含的元素个数
- 切片的容量：从第一个元素开始数，底层数组元素末尾的个数
- 用len(s)获取长度，cap(s)获取容量
- 可以通过**重新**切片扩展一个切片，给它提供足够容量

```go
func main (){
    s:=[]int {1,2,3,4,5,6}
    
    s=s[:0]//使其长度为0
    s=s[0:4]//重新切片扩展切片
    s=s[2:]//舍弃前两个值
}
```

#### nil切片

- 切片的零值是**nil**
- 含义：切片的长度和容量都为0，且没有底层数组

#### 用make创建切片

- 切片可以用内建函数**make**创建，这也是创建**动态数组**的方式

```go
a:=make([]int,2,2)//切片a的长度为2，容量为2
```

```go
func main (){
    a:=make([]int 5)
    printslice("a",a)b
    
    b:=make([]int,0,5)
    printslice("b",b)
    
    c:=b[0:2]
    printslice("c",c)
    
    d:=c[2:5]
}
```

#### 切片的切片

切片可以包含任何类型，包括切片

```go
func main(){
    a:=[][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
}
}
```

#### 向切片追加元素

使用内建函数append函数为切片追加新的元素

```go
func main (){
    var s[]int
    
    s=append(s,0)//添加一个空切片

    s=append(s,1)
    
    s=append(s,2,3,4)//可以一次性添加多个元素
}
```

#### Range

for循环的range形式可以**遍历切片或映射**

当用for循环遍历切片时，每次迭代都会返回**两个值**，第一个为**当前元素下标**，第二个为**该下标所对应元素的一份副本**

```go
var pow[]{1,2,3,4,5,6}
func main (){
    for i;v=range pow{
        fmt.Println("%d,%d",i,v)
}
}
```

可以将下标或值赋予_来忽略它

```go
for i, _ := range pow//只返回下标
for _, value := range pow//只返回值
```

如果只需要索引，忽略第二个变量即可

```go
for i := range pow
```

## 映射

- 映射将键映射到值
- 映射的零值为nil，nil映射没有键，也不会添加键
- make函数会**返回给定类型的映射**，并将其初始化备用

```go
type Vertex struct{
	Lat,Long float64
}

var m map[string] Vertex

func main (){
    m=make(map[string]Vertex)
    m["Bell Labs"]=Vertex{
       	40.2,-32 
    }
    fmt.Println(m["Bell Labs"])
}
```

#### 映射的文法

- 映射的文法和结构体类似，但**必须有键名**

```go
var m map[string]Vertex{
    "Bell Labs":Vertex{
        2,3
    }
    "Google":Vertex{
        2,4
}
}
```

- 若顶级类型只是一个类型名，在文法元素中可以省略

```go
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

#### 修改映射

- 在m中插入或修改元素

```go
m[key]=elem
```

- 获取元素

```go
elem=m[key]
```

- 删除元素

```go
delete(m,key)
```

- 检测某个键是否存在：通过双赋值，若key在m中，ok为true，否则为false。若key不在m，则elem是该映射元素类型的零值

```go
elem,ok=map[key]
```

```go
func main (){
    m:=make(map[string]int)
    
    m["answer"]=19
 	m["answer"]=121
    
    delete(m,"answer")
    
    v,ok:=m["answer"]

}
```

## 函数值

- 函数也是值，可以像其他值一样**传递**
- 函数值可以作为函数的**参数或返回值**

```go
func compute (fn func(float64,float64)float64)float64{
    return (fn(3,4))
}
```

#### 函数的闭包

- 闭包：能够读取**其他函数内部变量**的函数

- Go函数可以是一个闭包。闭包是一个**函数值**，它引用了函数体之外的变量，该函数可以访问并赋予其引用变量的值，即该函数被这些变量绑定在一起。

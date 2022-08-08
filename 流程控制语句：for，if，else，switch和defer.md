# 流程控制语句：for，if，else，switch和defer

## for

- Go只有一种循环结构：即for循环
- 基本的for循环由三部分组成，中间用分号隔开
  - 初始化语句：在第一次迭代前执行
  - 条件表达式：在每次迭代前求值
  - 后置语句：在每次迭代的结尾执行
- 一旦**条件表达句的布尔值为false**，循环终止
- 和c不同，Go的for循环不用小括号，但{ }是必须的

```go
func main (){
    sum:=0
    for i:=0;i<10;i++{
        sum+=i
}
    fmt Println(sum)
}
```

- **初始化语句**和**后置语句**是可选的

```go
sum:=1//初始化
for ;sum<100 ;{
    sum+=sum
}
```

- 上述语句可以省略分号，因为C中的while就是Go的for

```go
	sum := 1
	for sum < 1000 {
		sum += sum
	}
```

- 无线循环：如果**省略循环条件**，该循环就不会结束，因此无限循环可以写的很紧凑

```go
func main (){
    for{ }
}
```

## if

- 与for类型，不用小括号，大括号是必须的

```go
func sqrt (x float64)string{
    if x<0 {
        return 0
}
}
```

- if的简短语句：与for一样，if语句可以在条件表达式前执行一个简单语句（相当于for的初始化语句），该语句声明变量的作用域仅在if语句之内
- 上述的简单语句的作用域包含**if对应的else块**

- 练习：用牛顿法实现平方根函数

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
	z:=1.0
	for ;z<=10;z++{
		z -= (z*z - x) / (2*z)
		fmt.Println(z)
	}
	return z
}

func main() {
	fmt.Println(Sqrt(2))
}

```

## Switch

- switch 是编写一连串if-else的简便方法，它运行第一个值等于条件表达式的case语句
- Switch的case无需为常量，也不必为整数
- Go自动提供了每个case后面所需的**break**语句，除非以 `fallthrough` 语句结束，否则分支会自动终止。

```go
import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		fmt.Printf("%s.\n", os)
	}
}

```

#### switch 的求值顺序

switch的case语句从上到下依次执行，直到匹配成功停止

```go
switch i{
    case 0
    case f()
}//在i=0时，f()不会被调用
```

#### 没有条件的switch

- 没有条件的switch即switch true
- 这种形式可以将一长串if-then-else写的更加清晰

## defer

- 功能：将函数**推迟**到外层函数返回之后执行
- 推迟调用的函数其参数会立即求值，但**直到外层函数返回前**该函数都不会被调用

```go
func main (){
    defer fmt.Println("world")
    
    fmt.Println("hello")
}//输出结果为 hello world 
```

#### defer 栈

推迟的函数调用会被**压入一个栈中**。当外层函数返回时，被推迟的函数会按照**后进先出**的顺序调用




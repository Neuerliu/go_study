# 1.包、函数、变量
## 1.1 包
导入包

```go
import (
    "fmt"
    "math/cmplx"
) // 采用分块导入，使用以导入包的最后一个词为准
```

## 1.2 函数
函数示例

```go
func swap(x, y string) (string, string) {
    return y, x
} // 同样的类型可以合并
```

## 1.3 变量
变量一般采用下面这种形式初始化

```go
var a int // a = 0 
var a = 0 // a = 0，自动推断出类型
var c, java, python bool // 都是false

// 短赋值语句
c, java, python := false, false, false
```

<font style="color:rgb(0, 0, 0);">没有明确初始化的变量声明会被赋予对应类型的</font><font style="color:rgb(0, 0, 0);"> </font>**<font style="color:rgb(0, 0, 0);">零值</font>**<font style="color:rgb(0, 0, 0);">。</font>

<font style="color:rgb(0, 0, 0);">零值是：</font>

+ <font style="color:rgb(0, 0, 0);">数值类型为</font><font style="color:rgb(0, 0, 0);"> </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">0</font>`<font style="color:rgb(0, 0, 0);">，</font>
+ <font style="color:rgb(0, 0, 0);">布尔类型为</font><font style="color:rgb(0, 0, 0);"> </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">false</font>`<font style="color:rgb(0, 0, 0);">，</font>
+ <font style="color:rgb(0, 0, 0);">字符串为 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">""</font>`<font style="color:rgb(0, 0, 0);">（空字符串）。</font>

## 1.4 基本数据类型
Go的**基本数据类型**包括

```plain
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 表示一个 Unicode 码位

float32 float64

complex64 complex128
```

数据类型转换

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

i := 42
f := float64(i)
u := uint(f)
```

类型推断：<font style="color:rgb(0, 0, 0);">在声明一个变量而不指定其类型时（即使用不带类型的 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">:=</font>`<font style="color:rgb(0, 0, 0);"> 语法 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">var =</font>`<font style="color:rgb(0, 0, 0);"> 表达式语法），变量的类型会通过右值推断出来。</font>

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

常量采用`const`关键字，不能采用`:=`声明，常量可以在main函数外部声明，<font style="color:rgb(0, 0, 0);">数值常量是高精度的</font>**<font style="color:rgb(0, 0, 0);">值</font>**<font style="color:rgb(0, 0, 0);">，即使不是对应类型也可以用。</font>

# 2.循环、判断
## 2.1 for循环
```go
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

<font style="color:rgb(51, 51, 51);">for 是 Go 中的 while，去掉</font>`<font style="color:rgb(51, 51, 51);">;</font>`<font style="color:rgb(51, 51, 51);">和前后两个，只保留中间的判断条件就是while</font>

## <font style="color:rgb(51, 51, 51);">2.2 if判断</font>
```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}
```

## 2.3 switch分支
switch语句示例

```go
switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("macOS.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plan9, windows...
        fmt.Printf("%s.\n", os)
}
```

## 2.4 defer语句
<font style="color:rgb(0, 0, 0);">defer 语句会将函数推迟到外层函数返回之后执行。推迟调用的函数调用会被压入一个栈中。 当外层函数返回时，被推迟的调用会按照后进先出的顺序调用。</font>

<font style="color:rgb(0, 0, 0);">示例代码</font>

```go
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

# 3.指针、结构体、数组
## 3.1 指针
<font style="color:rgb(0, 0, 0);">指针保存了值的内存地址，类型 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">*T</font>`<font style="color:rgb(0, 0, 0);"> 是指向 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">T</font>`<font style="color:rgb(0, 0, 0);"> 类型值的指针，其零值为 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">nil</font>`<font style="color:rgb(0, 0, 0);">。</font>

:::info
<font style="color:rgb(0, 0, 0);">var p *int</font>

:::

<font style="color:rgb(0, 0, 0);">指针的操作</font>

```go
func main(){
    var p *int // 声明指针
    
    i := 10
    p = &i // 指针赋值
    fmt.Println(*p) // 访问指针对应的值

    *p = 12 // 通过指针修改值
}
```

## 3.2 结构体
<font style="color:rgb(0, 0, 0);">一个 结构体（</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">struct</font>`<font style="color:rgb(0, 0, 0);">）就是一组 字段（field）。</font>

<font style="color:rgb(0, 0, 0);">结构体示例，结构体字段可通过点号 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">.</font>`<font style="color:rgb(0, 0, 0);"> 来访问。</font>

```go
type Vertex struct {
	X int
	Y int
}

func main() {
    v := Vertex{1, 2}
	fmt.Println(v)
    x := v.X
    fmt.Println(x)
}
```

<font style="color:rgb(51, 51, 51);">结构体字面量：</font><font style="color:rgb(0, 0, 0);">使用 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">Name:</font>`<font style="color:rgb(0, 0, 0);"> 语法可以仅列出部分字段（字段名的顺序无关），示例如下：</font>

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予零值
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

## 3.3 数组
<font style="color:rgb(0, 0, 0);">类型 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">[n]T</font>`<font style="color:rgb(0, 0, 0);"> 表示一个数组，它拥有 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">n</font>`<font style="color:rgb(0, 0, 0);"> 个类型为 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 250, 250);">T</font>`<font style="color:rgb(0, 0, 0);"> 的值。例如</font>`<font style="color:rgb(0, 0, 0);">var a [10]int</font>`<font style="color:rgb(0, 0, 0);">。</font>

<font style="color:rgb(0, 0, 0);">数组示例</font>

```go
func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13} // 直接写出数组元素
	fmt.Println(primes)
}
```

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">数组切片</font>

<font style="color:rgb(0, 0, 0);">切片通过两个下标来界定，一个下界和一个上界，二者以冒号分隔，即</font>`<font style="color:rgb(0, 0, 0);">a[low : high]</font>`<font style="color:rgb(0, 0, 0);">，其中包括[low, high)。切片就像数组的引用，切片并不存储任何数据，修改切片中的元素会改变原始数组。</font>

<font style="color:rgb(0, 0, 0);">切片示例</font>

```go
func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```

<font style="color:rgb(0, 0, 0);"></font>

[<font style="color:rgb(0, 0, 0);">链接</font>](about:blank)<font style="color:rgb(0, 0, 0);"></font>

# <font style="color:rgb(0, 0, 0);">随手记</font>
+ `fmt.Sprint` 将字符串和变量拼接并返回拼接后的字符串


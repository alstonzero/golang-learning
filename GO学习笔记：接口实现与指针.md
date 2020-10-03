# GO学习笔记：接口实现与指针

Go不是面向对象的，因此在实现接口的方式上也没有implements关键字，而是通过实现接口（interface）所定义的方法，就表示某个结构体实现了该接口。

也正是因为Go不是面向对象的，Go中的结构体和类还有一点不同。Java中的类是包含有属性和方法的，在Go中，struct只定义了属性，方法是通过方法的接收者绑定到相应的结构体上的。

- 定义Usb接口

```go
type Usb interface {
	Start()
	Stop()
}
```

- Phone结构体实现Usb接口方法

```go
type Phone struct {
	name string
}

//让Phone实现Usb接口方法
func (p Phone) Start() {
	fmt.Println("手机开始工作")
}

func (p Phone) Stop() {
	fmt.Println("手机停止工作")
}
```

这段代码就是将Start()，Stop()方法绑定到Phone结构体上，使得Phone可以拥有Start(),Stop()方法。

看似比较简单，但是在实际过程中遇到了一个坑，就是方法的接收者。上面的例子中方法的接收者是一个**值类型**。



```go
//让Phone实现Usb接口方法
func (p *Phone) Start() {
	fmt.Println("手机开始工作")
}

func (p *Phone) Stop() {
	fmt.Println("手机停止工作")
}
```

如果这样写，方法的接收者就是一个**指针类型**了。[Go指南](https://tour.go-zh.org/methods/3)中对于这部分有一个简单的case，但是只说明了使用指针类型可以改变原对象的值，使用值类型是不能改变的。这个区别其实就是传值与传地址的区别，很好理解。

```go
package main

import (
	"fmt"
)

//define a interface
type Usb interface {
	Start()
	Stop()
}

type Phone struct {
	name string
}

//让Phone实现Usb接口方法
func (p *Phone) Start() {
	fmt.Println("手机开始工作")
}

func (p *Phone) Stop() {
	fmt.Println("手机停止工作")
}

func main() {
	var c1 Usb
	c1 = Camera{"Nikon"}
	c1.Start()
	c1.Stop()
}
```

上面的例子定义了一个接口Usb和它的实现Camera，在main函数中定义了一个变量c1类型是接口类型，当我使用`c1 = Camera{"Nikon"}`进行赋值时，会出现编译错误。

```go
cannot use Camera literal (type Camera) as type string in assignment
Process exiting with code: 2 signal: false
```

大致意思是说我的Camera没有实现Usb接口。

这个错的根源在于我`Start()`，`Stop()`方法的接收者是`*Camera`，是`Camera`的指针类型，Go会认为`Start()`,`Stop()`方法绑定在了`*Camera`上，而不是`Camera`，因此我的`Camera`并没有实现接口，实现接口的是`*Camera`这个指针类型。

因此如果我还想继续使用指针作为方法的接收者，就需要将`Camera`的指针赋值给`c1`就可以通过编译了。

```go
func main() {
	var c1 Usb
	c1 = &Camera{"Nikon"}
	c1.Start()
	c1.Stop()
}
```

```
Nikon Camera start working
Nikon Camera stop working
```


# Golang遗漏知识点

[TOC]

## struct结构体

### 

```go
package main
import(
    "fmt"
)

type Person struct{
    Name string
    Age int
}
```

### 创建结构体实例4种方式

- 方式1：直接声明

   `var person Person`

- 方式2：{}

  `var person Person = Person{}`

  {}里面也可以直接写进去字段的值

#### 返回结构体指针

- 方式3：&

  `var person *Person=new(Person)`

  由于p3是一个指针，因此使用标准的给字段赋值方式

  ```go
  var p3 *Person = new(Person)
  (*p3).Name = "smith"
  (*p3).Age = 30
  ```

  `(*p3).Name = "smith"`也可以写成`p3.Name = "smith"`

  ```go
  p3.Name = "smith"
  p3.Age = 30
  ```

  go的设计者为了程序员使用方便，在底层会把p3.Name处理成为(*p3).Name

- 方式4:{}

  `var person *Person = &Person{}`

  ```go
  var p4 *Person = &Person{}
  (*p4).Name = "scoot"
  (*p4).Age = 88
  ```

  - 和方式3一样，也可以写成`p4.Name = "scoot"`这种方式
  - 同样可以直接给字符赋值

  ```go
  var person *Person = &Person{"mary",60}
  ```

  


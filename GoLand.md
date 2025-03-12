运行 go 程序

```sh
#直接编译执行
go run hello.go
#编译后生成可执行文件
go build hello.go
```

go语法

```go
//必须有main包
package main
import "fmt"
//多个包
import (
	"fmt"
   	"time"
)
//A := 99  错误, := 只能用在函数里
func main(){
    var a int
    var b int = 100
    var c = "string"
    d := 3.14
    fmt.Println("type is %T", a)
    var x, y int= 1, 2
    var n, m = 0, "0"
    //多行变量初始化
    var (
        ii int = 100
        jj bool = true
    )
    //常量
    const ab int = 7
    //const 定义枚举
    const (
        AAA = 0
        BBB = -1
    )
    //iota 每次使用加1
    const (
        AAA = iota
        BBB = iota
    )
}
```

defer

```go
defer fmt.Println("end")
// 如果多个,就是以栈的顺序执行
```

关于Go导包

```go
import (
    _ "xxx/lib1" //匿名导入
    libA "xxx/lib2" //别名导入
    . "xxx/lib3" //直接使用
)
libA.init()
lib3Test()
```

数据结构使用

```go
// 数组
var arr [10] int
// 取长度
len(arr)
arr2 := [10]int{1,2,3,4}
// 遍历
for index,value := range arr2{
    fmt.Println("index=",index,"value=",value)
}
// 动态数组 slice (切片)
arr3 := []int{1,2,3}
// 固定长度的数组在传参时是值拷贝, 而且长度要一样, 而动态数组是引用类型
slice1 := []int{1,2,3}
slice2 := make([]int, 3, 5) // len=3,cap=5
var slice3 []int // silce3 == nil
// 打印切片
Printf("len=%d,slice=%v\n",len(slice1),slice1)
cap(slice2) //容量
slice1 = append(slice1,1000)
// map
var map1 map[string]string
make(map[string]string,10)
map1["one"]="java"
map1["two"]="c++"

map2 := make(map[int]string)
map2[1]="jvav"
map2[2]="c艹"

map3 := map[string] string{
    "one":"sb",
    "two":"666",
}
// 也能使用for range map1 遍历
// map的删除
delete(map1,"key")
map1["key"]="newValue"

// 类型别名
type myint int
// 结构体
type Book struct {
    id int
    name string
}
var b1 Book
b1.id = 1001
b1.name = "NEW WORLD"
// 直接传参不会改变原值
func changeBook(book *Book){
    // 参数为指针时可以改变原值
    book.id = 1000
}
// 面向对象语法
func (this Book) SetId(id int){
    this.id = id
}
// 初始化
b1 = Book{id:1,name:"NAT"}
// interface 断言
func intfaceTest(arg interface{}) {
	Println("测试函数", arg)
    value, OK := arg.(string)
    if !OK {
        Println("arg不是字符串")
    } else {
        Println("是字符串")
    }
}
```







go的标签,类似于java的注解,主要用于辅助反射功能

```go
type Book struct{
    Id int `id`
    Name string `book_name`
}
// 使用json解码
import (
    encoding/json
)
book := Book{1000,"ABOOK"}
jsonStr, err := json.Marshal(book)
if !err {
    Println("ERROR!")
    return
}
Println(jsonStr)
```


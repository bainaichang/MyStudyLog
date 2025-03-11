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


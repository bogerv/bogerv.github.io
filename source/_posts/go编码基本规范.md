---
title: Go 编码基本规范
date: 2017-10-13 13:29:20
categories: [go]
---
## 声明空的切片 (Slices)
相对于 `t := []string{}` 更推崇 `var t []string`

前者是 `non-nil`, 但是长度为0, 而后者声明了一个 `nil` 的 `slice`. 都能实现相应的功能 ( 函数 `len` 和 `cap` 都为 0 ), 但应该首选 `nil` `slice`

只有在特定的情况下, 选择 `non-nil` 但长度为 0 的 `slice`

* 编码 JSON 对象的时候
  > `nil slice` 编码为 `null`, 而 `[]string{}` 编码为 JSON 数组 `[]`

设计接口时, 不区分两者的区别, 可能导致微妙的编程错误

## Crypto Rand
即便使用一次, 也不要使用 `math/rand` 生成键.

使用 `crypto/rand` 的 Reader:
```go
import (
  "crypto/rand"
  // "encoding/base64"
  // "encoding/hex"
  "fmt"
)

func Key() string{
  buf := make([]byte, 16)
  _, err = rand.Read(buf)
  if err != nil {
    panic(err)
  }
  return fmt.Sprintf("%x", buf)
  // or hex.EncodeToString(buf)
  // or base64.StdEncoding.EncodeToString(buf)
}
```

## Imports
避免重命名导入从而避免名称冲突; 如果发生冲突, 最好重命名本地或特殊项目的导入包.

导入时, 应组织分组, 之间添加空行. 标准库的包总在第一组内.
```go
package main

import (
  "fmt"
  "hash/adler32"
  "os"

  "appengine/foo"
  "appengine/user"

  "code.google.com/p/x/y"
  "github.com/foo/bar"
)
```

## Import 三种方式
1. 点操作
    `import( . "fmt")`
    > 这个点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名，也就是前面你调用的 `fmt.Println("hello world")` 可以省略的写成 `Println("hello world")`
2. 别名操作 ( 别名操作顾名思义可以把包命名成另一个用起来容易记忆的名字 )
    > `import( f "fmt" )`   别名操作调用包函数时前缀变成了重命名的前缀，即 `f.Println("hello world")`
3. _操作   这个操作经常是让很多人费解的一个操作符，请看下面这个import
    ```go
    import (
      "database/sql"
      _ "github.com/ziutek/mymysql/godrv"
    )
    ```
    > `_` 操作其实只是引入该包. 当导入一个包时, 它所有的 `init()` 函数就会被执行，但有些时候并非真的需要使用这些包, 仅仅是希望它的 `init()` 函数被执行而已. 这个时候就可以使用_操作引用该包了. 即使用_操作引用包是无法通过包名来调用包中的导出函数, 而是只是为了简单的调用其init函数(). 

## Import Dot
`import .` 的形式可以用于测试场景, 由于循环依赖, 不能成为被测试包的一部分
```go
package foo_test

import (
	"bar/testutil" // 同样也 import "foo"
	. "foo"
)
```
这种情况下, 由于 `bar/testtuil` 包中导入了 `foo`, 所以 `foo_test` 不能再次包含 `foo` 这个包. 所以我们使用 `import .` 的形式. 除了这种情况外, 不要使用 `import .`. 它让程序更难阅读.

## 接口

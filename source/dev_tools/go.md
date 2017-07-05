* 安装 go
* 添加 GOPATH 这个系统变量变量

    ![](/assets/dev_tools/go_env.png)
* 安装插件
    ```go
    go get -u -v github.com/nsf/gocode
    go get -u -v github.com/rogpeppe/godef
    go get -u -v github.com/golang/lint/golint
    go get -u -v github.com/lukehoban/go-find-references
    go get -u -v github.com/lukehoban/go-outline
    go get -u -v sourcegraph.com/sqs/goreturns
    go get -u -v golang.org/x/tools/cmd/gorename
    go get -u -v github.com/tpng/gopkgs
    go get -u -v github.com/newhook/go-symbols
    ```
    安装完成后, 在 GOPATH 指定的目录中 src 文件夹下会多出几个文件夹, 即插件的文件夹
* 测试

    用 vscode 打开 GOPATH, 在根目录创建测试文件 test.go

    输入相应代码
    ```go
    package main

    import "fmt"

    func main(){
        fmt.Println("fsdsfsd")
    }
    ```

    运行 `go run test.go`
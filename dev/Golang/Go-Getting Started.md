## Go-Getting Started

[Getting Started](https://golang.org/doc/install)



## 安装Go

**下载安装包**

```bash
wget https://dl.google.com/go/go1.14.2.linux-amd64.tar.gz
```

**解压安装包到/usr/local目录**

```bash
tar -C /usr/local -xzf go1.14.2.linux-amd64.tar.gz
```


**添加环境变量**

```shell
export PATH=$PATH:/usr/local/go/bin
```

**测试是否安装成功**

```bash
touch hello.go
```

```go
package main
import "fmt"
func main() {
        fmt.Printf("hello, world\n")
}
```

**使用go工具编译**

```bash
go build hello.go
```

**生成可执行文件hello，并运行**

```bash
>./hello
hello, world
```

### 其他模块开启

```bash
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
go env -w GOROOT=/usr/local/go
go env -w GOPATH=/usr/local/go/bin
```



```
go get 
```


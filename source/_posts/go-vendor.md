---
title: golang 使用 vendor 包管理工具
tags: [go, vendor]
categories: [技术学习]
date: 2019-04-06 00:28:37
top: true
---



### vendor 目录介绍

Go 从 1.5 release 版本之后，vendor 目录也被添加到了 Go 的依赖目录查找中了，Go 1.6 之前，需要手动设置环境变量`GO15VENDOREXPERIMENT=1` ，Go 才能够查找 `vendor` 目录。Go 从 1.6 以后就不需要设置了（在 Go 中无论如何，都需要配置 GOPATH和GOROOT）。

查找顺序如下所示：

1. 当前目录下的 vendor 目录
2. 依次向上一级一级查找 vendor 目录
3. 然后是 GOROOT
4. 最后才是 GOPATH



#### 为什么需要使用 vendor？

vendor 目录允许不同的项目使用不同版本的依赖包，这做到了工程的隔离。

#### govendor status types

| code           | description                                  |
| -------------- | -------------------------------------------- |
| +local（l）    | 在项目中的包                                 |
| +external（e） | 引用的包在GOPATH之下，但是没有在vendor目录下 |
| +vendor（v）   | 包在vendor目录中，已经被govendor管理         |
| +std（s）      | 标注库中的包                                 |
| +excluded（x） | 引用的包被排除在GOPATH，没有在当前目录下     |
| +unused（u）   | 这个包在vendor目录下，但是未使用             |
| +missing（m）  | 有代码应用了这个包，但是找不到这个引用包     |
| +program（p）  | 这个包是包含 main 包的目录                   |
| +outside       | 外部包和缺失的包                             |
| +all           | 所有的包                                     |

以上的`status types` 会在执行 `govendor list` 会列出所有的包，并且能够看到项目中所有包的情况 

### govendor 使用

| command | description                                                  |
| ------- | ------------------------------------------------------------ |
| init    | 初始化，创建 vendor 目录和 vendor.json 文件                  |
| list    | 列出项目中所有的 包，包括自己的包                            |
| add     | 添加包到 vendor 目录，从 `$GOPATH` 中添加依赖包，会加到 `vendor.json` 中 |
| update  | 从 `$GOPATH` 更新包到 vendor 目录                            |
| remove  | 从 vendor 管理中删除依赖包                                   |
| status  | 列出所有缺失、过期、和修改过的包                             |
| fetch   | 从远程仓库中添加或更新一个包到 vendor 目录                   |
| sync    | 根据 vendor.json 记录的信息，从远程仓库中拉取包              |
| get     | 类似于 `go get`， 拉取依赖包到 vendor 目录下                 |
| license | 列出某个包的许可证                                           |
| shell   | 运行一个shell，可以执行以上命令                              |

> go tool commands that are wrapped:
>           "+status" package selection may be used with them
>         fmt, build, install, clean, test, vet, generate, tool

其他可以使用 `govendor --help` 来查看更多，更详细的命令，也可以通过 <https://github.com/kardianos/govendor>，来查看信息。

> 注意：项目目录（工作空间）必须在 `$GOPATH/src` 目录下，govendor 是用于管理项目依赖包的，如果 `$GOPATH` 下没有依赖包，则需要先下载。 



```
cd "my project in GOPATH"
govendor init

# 将在 $GOPATH 下却没有在当前目录（vendor）下，加入到 vendor 目录。
govendor add +external

# govendor 会分析.go 为后缀的文件，分析出依赖包的状态
govendor list 

# 列出指定的包在哪被使用了
govendor list -v fmt

# 从远程仓库中获取指定版本的包，也可以不指定版本，默认会获取最新版本
govendor fetch github.com/gin-gonic/gin@v1.3

# 格式化本地的包，并返回格式化了哪些包
govendor fmt +local

# build 本地的包
govendor install +local

# 只测试本地的包
govendor test +local

# 移除被 vendor 所管理的包
govendor remove +v 
```

```
$ govendor list
pl  github.com/golearn/project
 l  github.com/golearn/project/tt
  m github.com/gin
```

前面是是状态类型，`pl` 代表的意思是：这个包是本地的包，并且包含了main包，也就是可以执行的包。`m` 代表的是这个包被引用了，但是找不到这个包。

```
$ govendor list +<status types>

$ govendor list +std
 s  fmt
 s  net
 s  net/http
```

这个会列出项目中用到的标准库包；使用`govendor list +<status types>` 可以更精确的分析项目中依赖包的状态。



#### govendor 报错

```
$ govendor init
Error: FindFirstFile C:\Users\ZAKI\Code\go\src\src: The system cannot find the file specified.
```

原因：有多个GOPATH，没有指定GOPATH，导致报错。

解决方案：只能够删除掉不使用的GOPATH，才能够初始化成功。

```
$ echo %GOPATH%
C:\Users\ZAKI\Code\go;C:\Users\ZAKI\Code\go\src
$ govendor init
Error: FindFirstFile C:\Users\ZAKI\Code\go\src\src: The system cannot find the file specified.
```

```
$ echo %GOPATH%
C:\Users\ZAKI\Code\go

$ govendor init
```

当只有一个 GOPATH 的时候，创建vendor就成功了





### go mod

在Go 1.11 中，官方加入了实验性的包管理工具（package management tool），称之为Modules。

其目的可能是：

* 统一已有的第三方包管理工具
* 移除在 Go 1.5 版本引入的 vendor
* 移除 `$GOPATH`



#### go mod 指令介绍

| command  | description                                          |
| -------- | ---------------------------------------------------- |
| download | 将依赖全部下载到本地，位置为 `$GOPATH/pkg/mod/cache` |
| edit     | 编辑 go.mod 例如锁定某个版本的依赖                   |
| graph    | 列出项目中那个部分使用了某个依赖                     |
| init     | 初始化创建 `go.mod`                                  |
| tidy     | 增加缺少的依赖，移除未使用的依赖                     |
| vendor   | 将所有的 go.mod 依赖全部存储在 `/vendor` 目录下      |
| verify   | 验证本地依赖依然是预期的内容，就是符合` go.sum` 的   |
| why      | 解释摸个依赖为何存在于 `go.mod` 中，为什么需要它     |

在 `$GOPATH` 中使用 go mod 依然按照现有的 vendor 机制，除非修改环境变量 `GO111MODULE` 为 `on`，来强制开启它。



接下来我们就在`$GOPATH` 以外演示一下 go modules

```
$ go mod init test
go: creating new go.mod: module test

$ ls
go.mod
```

会看见目录下多了一个 `go.mod` 文件，这个文件就是用来保存 go 的依赖的

```
$ cat go.mod
module test
```

下面来试着写一个简单的 hello world

```go
// main.go
package main

import (
	"fmt"
    "github.com/gofrs/uuid"
)

func main() {
  	uuid, _ := uuid.NewV4()
  	fmt.Println("hello world", uuid)
}
```

这个hello world 中包含了外部包，首先确认，我们的 GOPATH 下没有这个包，然后我们开始吧。

```
$ go run main.go
go: finding github.com/gofrs/uuid v3.2.0+incompatible
go: downloading github.com/gofrs/uuid v3.2.0+incompatible
hello world 0ffb8f8d-5019-407c-bfb5-f85fe64508f5
```

go 自己去下载了第三方的包，并且保存到了 `$GOPATH/pkg/mod/cache` 目录下了；并且 ·`go.mod` 文件会记录下这个依赖

```
$ cat go.mod
module test

require github.com/gofrs/uuid v3.2.0+incompatible // indirect
```

每次使用 go 的子命令，像 `go build`、`go run`、`go get`、`go test` 的时候，都会检查整个项目目录的依赖，并且自动更新到 `go.mod` 下。

### 从已有的 vendor 转移到 go modules

非常简单，只需要在项目目录下直接执行 `go mod init `即可

go 会自动去读取 `glide.yaml` 或是 `vendor/vendor.json` 并生成一个 `go.mod`。

可以试试：

```
$ go mod tidy
```

整个命令会帮助你移除没有使用的依赖。
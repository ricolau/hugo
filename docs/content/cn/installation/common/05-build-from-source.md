## 使用源代码安装 Build from source 

要从源代码构建Hugo的扩展版，您必须：
1. 已安装 [Git]
2. 已安装 [Go] 1.19 或者更新的版本
3. 安装一个C 语言编译器, 比如 [GCC] 或者[Clang]
4. 参考 [Go documentation] 文档中描述的内容来更新系统变量中的 `PATH`  值

> 安装目录由`GOPATH`和 `GOBIN` 环境变量控制。如果设置了 `GOBIN` ，则二进制文件将安装到该目录中。如果设置了`GOPATH`，二进制文件将安装到`GOPATH`列表中第一个目录的bin子目录中。否则，二进制文件将安装到默认`GOPATH`（`$HOME/go`或`%USERPROFILE%\go`）的bin子目录中。

开始build安装包并测试:

```sh
CGO_ENABLED=1 go install -tags extended github.com/gohugoio/hugo@latest
hugo version
```

[Clang]: https://clang.llvm.org/
[GCC]: https://gcc.gnu.org/
[Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
[Go documentation]: https://go.dev/doc/code#Command
[Go]: https://go.dev/doc/install

# go mod

go modules是golang 1.11新加的特性。Modules官方定义为：

> 模块是相关Go包的集合，modules是源代码交换和版本控制的单元。go命令直接支持使用modules, 包括记录和解析对其他模块的依赖性。modules替换旧的基于GOPATH的方法来指定在给定构建中使用那些源文件。

## go mod 命令

golang 提供了go mod命令来管理包

go mod有以下命令：

| 命令     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| download | download modules to local cache(下载依赖包)                  |
| edit     | edit go.mod from tools or scripts(编辑go.mod)                |
| graph    | print module requirement graph(打印模块依赖)                 |
| init     | initialize new modules in current directory(在当前目录初始化mod) |
| tidy     | add missing and remove unused modules(拉取缺少的模块、移除不用的模块) |
| vendor   | make vendored copy of dependencies(将依赖复制到vendor下)     |
| verify   | verify dependencies have expected content(验证依赖是否正确)  |
| why      | explain why packages or modules are needed(解释为什么需要依赖) |

## 常用命令

```shell
# GO111MODULE=on 以后，下载的模块内容会缓存在 $GOPATH/pkg/mod 目录中.要是想清理该缓存，执行下面命令
go clean --modcache
```

## go get 升级

- go get -u 将会升级到最新的次要版本或修订版本(x.y.z, z是修订版本号， y是次要版本号)
- go get -u=patch将会升级到最新的修订版本
- go get package@version将会升级到指定的版本号version
- go get 如果有版本的更改，那么go.mod文件也会更改

## 参考资料
[Using Go Modules](https://blog.golang.org/using-go-modules)

[go mod 使用](https://juejin.im/post/5c8e503a6fb9a070d878184a)

[Go 1.11 Modules](https://github.com/golang/go/wiki/Modules)

[go proxy](https://github.com/goproxy/goproxy.cn)

[第 61 期 Go Modules、Go Module Proxy 和 goproxy.cn](https://github.com/developer-learning/night-reading-go/issues/468)
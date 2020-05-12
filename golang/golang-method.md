# [如何写出优雅的 Go 语言代码](https://draveness.me/golang-101/)

## 代码规范

[Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

### 辅助工具

尽量自动化一切能够自动化的步骤，让工程师审查真正重要逻辑和设计

### [goimports](https://godoc.org/golang.org/x/tools/cmd/goimports)

Go语言官方提供的工具，它能够为我们自动格式化Go语言代码并对所有引入的包进行管理，包括自动增删依赖的包引用，将依赖包按字母序排序并分类。

### golint

常用的静态检查工具，作为官方提供的工具，它在可定制化上有着非常差的支持。

- 在基础库或者框架中使用golint进行静态检查

- 在其他的项目中使用可定制化的golangci-lint来进行静态检查。

在基础库和框架中施加强限制对于整体的代码质量有着更大的收益。

### 自动化

在项目中引入这些工具就一定要在代码的 CI 流程中加入对应的自动化检查。

## 最佳实践

### 目录结构

#### /pkg

目录中存放的就是项目中可以被外界应用使用的代码库。

#### 私有代码

私有代码推荐放到```/internal```目录中。

![Goland Internal app and pkg](https://img.draveness.me/golang-internal-app-and-pkg.png)

#### /src

在 Go 语言的项目最不应该有的目录结构其实就是``` /src```了

#### /cmd

```/cmd``` 目录中存储的都是当前项目中的可执行文件，该目录下的每一个子目录都应该包含我们希望有的可执行文件，如果我们的项目是一个 ```grpc``` 服务的话，可能在 ```/cmd/server/main.go``` 中就包含了启动服务进程的代码，编译后生成的可执行文件就是 ```server```。

#### /api

```/api``` 目录中存放的就是当前项目对外提供的各种不同类型的 API 接口定义文件了，其中可能包含类似``` /api/protobuf-spec```、```/api/thrift-spec```或者 ```/api/http-spec``` 的目录，这些目录中包含了当前项目对外提供的和依赖的所有 API 文件

#### Makefile

最后要介绍的 Makefile 文件也非常值得被关注，在任何一个项目中都会存在一些需要运行的脚本，这些脚本文件应该被放到``` /scripts``` 目录中并由 Makefile 触发，将这些经常需要运行的命令固化成脚本减少『祖传命令』的出现。

### 模块拆分

 #### 按层拆分

典型的MVC架构

#### 按职责拆分

Go 语言项目中的每一个文件目录都代表着一个独立的命名空间。最根本原因：

- Go 语言对同一个项目中不同目录的命名空间做了隔离，整个项目中定义的类和方法并不是在同一个命名空间下的，这也就需要工程师自己维护不同包之间的依赖关系；
- 按照职责垂直拆分的方式在单体服务遇到瓶颈时非常容易对微服务进行拆分，我们可以直接将一个负责独立功能的 package 拆出去，对这部分性能热点单独进行扩容；

### 显示调用

 Go 语言社区对于显式的初始化、方法调用和错误处理非常推崇。

#### init

- init函数减少使用
- init 方法使用做一些简单、轻量的前置条件判断

#### error

error最佳实践：

- 使用 error 实现错误处理 — 尽管这看起来非常啰嗦；
- 将错误抛给上层处理 — 对于一个方法是否需要返回 error 也需要我们仔细地思考，向上抛出错误时可以通过 errors.Wrap 携带一些额外的信息方便上层进行判断；
- 处理所有可能返回的错误 — 所有可能返回错误的地方最终一定会返回错误，考虑全面才能帮助我们构建更加健壮的项目；

### 面向接口

```go
package post

type Service interface {
    ListPosts() ([]*Post, error)
}

type service struct {
    conn *grpc.ClientConn
}

func NewService(conn *grpc.ClientConn) Service {
    return &service{
        conn: conn,
    }
}

func (s *service) ListPosts() ([]*Post, error) {
    posts, err := s.conn.ListPosts(...)
    if err != nil {
        return []*Post{}, err
    }
    
    return posts, nil
}
```

- 通过接口 Service 暴露对外的 ListPosts 方法；
- 使用 NewService 函数初始化 Service 接口的实现并通过私有的结构体 service 持有 grpc 连接；
- ListPosts 不再依赖全局变量，而是依赖接口体 service 持有的连接；

当我们使用上述方法组织代码之后，其实就对不同模块的依赖进行了解耦，也正遵循了软件设计中经常被提到的一句话 — 『依赖接口，不要依赖实现』，也就是面向接口编程。

### 单元测试

#### 可测试

1. 尽可能减少目标方法的依赖，让目标方法只依赖必要的模块；
2. 依赖的模块也应该非常容易地进行 Mock；

#### 接口



#### 函数简单



#### 组织方式



#### Test

#### Suite



#### BDD



#### Mock方法

#### SQL

#### HTTP

#### 猴子补丁

#### 断言






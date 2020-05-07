# TOP命令参数详解

## 进程信息

| 列名  | 含义                                                      |
| ----- | --------------------------------------------------------- |
| PID   | 进程ID                                                    |
| PPID  | 父进程ID                                                  |
| RUSER | Real User Name                                            |
| UID   | 进程所有者的用户ID                                        |
| USER  | 进程所有者的用户名                                        |
| GROUP | 进程所有者的组名                                          |
| %CPU  | 上次更新到现在的CPU时间占用百分比                         |
| %MEM  | 进程使用的物理内存百分比                                  |
| VIRT  | 进程使用的虚拟内存总量，单位KB。VIRT=SWAP+RES             |
| RES   | 进程使用的，未被换出的物理内存大小，单位Kb。RES=CODE+DATA |
| CODE  | 可执行代码占用的物理内存大小，单位Kb                      |
| DATA  | 可执行代码外的部分(数据段+栈)占用的物理内存大小，单位Kb   |
| SHR   | 共享内存大小，单位Kb                                      |



## reference

[linux TOP命令各参数详解](https://juejin.im/post/5e16b0645188253aac0cd19a)


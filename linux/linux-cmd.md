# grep

grep有用参数：
- -A num, --after-context=num: 在结果中同时输出匹配行之后的num行
- -B num, --before-context=num: 在结果中同时输出匹配行之前的num行
- -i, --ignore-case: 忽略大小写
- -n, --line-number: 显示行号
- -R,-r, --recursive: 递归搜索子目录
- -v, --invert-match: 输出没有匹配的行

# [grep 多个关键字与和或操作](https://blog.csdn.net/mmbbz/article/details/51035401)

#### 或操作

```shell
grep -E '123|abc' filename  // 找出文件（filename）中包含123或者包含abc的行
egrep '123|abc' filename    // 用egrep同样可以实现
awk '/123|abc/' filename   // awk 的实现方式
```

#### 与操作

```shell
grep pattern1 files | grep pattern2 //显示既匹配 pattern1 又匹配 pattern2 的行。
```

#### 其他操作

```shell
grep -i pattern files   //不区分大小写地搜索。默认情况区分大小写，
grep -l pattern files   //只列出匹配的文件名，
grep -L pattern files   //列出不匹配的文件名，
grep -w pattern files  //只匹配整个单词，而不是字符串的一部分（如匹配‘magic’，而不是‘magical’），
grep -C number pattern files //匹配的上下文分别显示[number]行，
```

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


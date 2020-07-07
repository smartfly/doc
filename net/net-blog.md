

# [tcpdump简明教程](https://github.com/mylxsw/growing-up/blob/master/doc/tcpdump%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B.md)

```shell
sudo tcpdump -i eth1 'host 10.xx.xx.xx' -w test.cap
```

​		抓eth1网卡，源IP或目的IP为```10.xx.xx.xx```的数据。

# I/O模型

### 阻塞IO

### 非阻塞IO

### 多路复用IO

### 信号驱动IO

### 异步IO



## 参考资料

[网络轮询器](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-netpoller/)

[Java NIO：浅析I/O模型](https://www.cnblogs.com/dolphin0520/p/3916526.html)
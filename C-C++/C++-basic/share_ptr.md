# [智能指针shared_ptr的用法](https://www.cnblogs.com/jiayayao/p/6128877.html)

C++11 提供了三种智能指针：std::shared_ptr, std::unique_ptr, std::weak_ptr, 使用时需添加头文件<memory>

shared_ptr使用引用计数，每一个shared_ptr的拷贝都指向相同的内存。每使用他一次，内部的引用计数加1，每析构一次，内部的引用计数减1，减为0时，删除所指向的堆内存。shared_ptr内部的引用计数是安全的，但是对象的读取需要加锁。


# Redis持久化(durability)

持久化就是把内存的数据写到磁盘中去，防止服务宕机了内存数据丢失

Redis提供了两种持久化方式：RDB(default)和AOF


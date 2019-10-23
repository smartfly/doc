# MySQL 5.7 json类型中文乱码问题

使用dataX进行mysql之间数据同步时候，出现json类型字段中中文是乱码。通过排查，确定由于mysql驱动jdbc版本导致的。根据版本信息查阅[dataX](https://github.com/alibaba/DataX)使用的驱动版本号：

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.34</version>
</dependency>
```

查资料才发现要jdbc 5.1.40以上才行，更新依赖可以解决。

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.40</version>
</dependency>
```

针对dataX问题，只需要到plugin下面对应的mysql reader/writer下面libs下面的```mysql-connector-java-5.1.34.jar```替换成```mysql-connector-java-5.1.40.jar```，可以完美解决该问题。
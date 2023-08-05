# 《MYSQL是怎么运行的》笔记

## 0. Innodb_ruby使用指北

[innodb_ruby wiki](https://github.com/jeremycole/innodb_ruby/wiki)

[innodb_ruby打印解析](https://juejin.cn/post/6844903844107780103)

## 1. 初识MYSQL

### 1.1 查看mysql数据目录

> mac端无法执行`which`和`whereis`命令查看mysql安装目录

```cmd
mysql> show global variables like "%datadir%";
+---------------+-------------------------------------------------+
| Variable_name | Value                                           |
+---------------+-------------------------------------------------+
| datadir       | /usr/local/mysql-5.7.20-macos10.12-x86_64/data/ |
+---------------+-------------------------------------------------+
1 row in set (0.00 sec)
```

### 1.2 windows注册服务

```cmd
"mysqld的完整路径" --install
```

```cmd
net start MYSQL
net stop MYSQL
```

### 1.3 进程间通信

> 1. TCP/IP
> 2. windows的命名管道和共享内存
> 3. UNIX域套接字

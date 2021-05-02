- [MySQL连接方式](#mysql连接方式)
  - [默认使用到本地服务器的Unix套接字文件(socket方式)登录](#默认使用到本地服务器的unix套接字文件socket方式登录)
  - [显式指定到本地服务器的Unix套接字文件(socket方式)登录](#显式指定到本地服务器的unix套接字文件socket方式登录)
  - [使用到本地或远程服务器的TCP/IP传输协议(TCP/IP方式)登录](#使用到本地或远程服务器的tcpip传输协议tcpip方式登录)
- [其他相关命令](#其他相关命令)
  - [查看连接方式是TCP/IP方式还是socket方式](#查看连接方式是tcpip方式还是socket方式)
  - [查看socket文件路径](#查看socket文件路径)
  - [查看my.cnf文件加载顺序，后面的配置会覆盖前面的配置](#查看mycnf文件加载顺序后面的配置会覆盖前面的配置)

### MySQL连接方式

#### 默认使用到本地服务器的Unix套接字文件(socket方式)登录

```shell
[root@app01 software]# mysql -uqiqi -p
```

#### 显式指定到本地服务器的Unix套接字文件(socket方式)登录

```shell
[root@app01 software]# mysql -uqiqi -p -S /data/mysql/mysql_3306/tmp/mysql_3306.sock
```

#### 使用到本地或远程服务器的TCP/IP传输协议(TCP/IP方式)登录

* 本地TCP/IP方式登录

```shell
[root@app01 software]# mysql -h127.0.0.1 -uqiqi -p
```

* 远程TCP/IP方式登录

```shell
[root@app01 software]# mysql -h192.168.21.21 -uqiqi -p
```

### 其他相关命令

#### 查看连接方式是TCP/IP方式还是socket方式

```mysql
mysql> status;
```

#### 查看socket文件路径

* 第一种方式

```mysql
mysql> SELECT @@socket;
```

* 第二种方式

```shell
[root@app01 software]# /usr/local/mysql/bin/mysqladmin -uroot -p variables | grep socket
```

#### 查看my.cnf文件加载顺序，后面的配置会覆盖前面的配置

```shell
[root@app01 software]# mysql --verbose --help | grep my.cnf
```

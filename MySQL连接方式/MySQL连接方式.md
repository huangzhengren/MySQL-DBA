- [MySQL连接方式](#mysql连接方式)
  - [默认使用到本地服务器的Unix套接字文件(socket方式)登录](#默认使用到本地服务器的unix套接字文件socket方式登录)
  - [显式指定到本地服务器的Unix套接字文件(socket方式)登录](#显式指定到本地服务器的unix套接字文件socket方式登录)
  - [使用到本地或远程服务器的TCP/IP传输协议(TCP/IP方式)登录](#使用到本地或远程服务器的tcpip传输协议tcpip方式登录)

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

```mysql
[root@app01 software]# mysql -h127.0.0.1 -uqiqi -p
```

* 远程TCP/IP方式登录

```mysql
[root@app01 software]# mysql -h192.168.21.21 -uqiqi -p
```




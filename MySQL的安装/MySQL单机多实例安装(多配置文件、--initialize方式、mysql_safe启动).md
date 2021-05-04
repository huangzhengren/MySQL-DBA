- [MySQL单机多实例安装(多配置文件、--initialize方式、mysql_safe启动)](#mysql单机多实例安装多配置文件--initialize方式mysql_safe启动)
  - [查看CentOS版本](#查看centos版本)
  - [卸载与mariadb、mysql相关的rpm包](#卸载与mariadbmysql相关的rpm包)
  - [删除my.cnf、mariadb、mysql相关文件和目录](#删除mycnfmariadbmysql相关文件和目录)
  - [安装wget和libaio库](#安装wget和libaio库)
  - [下载通用二进制包，然后解压到/usr/local/mysql下并建立符号链接](#下载通用二进制包然后解压到usrlocalmysql下并建立符号链接)
  - [配置MySQL环境变量](#配置mysql环境变量)
  - [创建用于存放MySQL数据、日志和临时文件的目录](#创建用于存放mysql数据日志和临时文件的目录)
  - [创建mysql组，创建mysql用户并加入到mysql组，并且禁止使用mysql登录服务器](#创建mysql组创建mysql用户并加入到mysql组并且禁止使用mysql登录服务器)
  - [将相关目录及文件的所有者和所属组都修改为mysql](#将相关目录及文件的所有者和所属组都修改为mysql)
  - [编辑配置文件/data/mysql/mysql_3307/mysql_3307.cnf](#编辑配置文件datamysqlmysql_3307mysql_3307cnf)
  - [编辑配置文件/data/mysql/mysql_3308/mysql_3308.cnf](#编辑配置文件datamysqlmysql_3308mysql_3308cnf)
  - [编辑配置文件/data/mysql/mysql_3309/mysql_3309.cnf](#编辑配置文件datamysqlmysql_3309mysql_3309cnf)
  - [初始化MySQL(3307、3308、3309)](#初始化mysql330733083309)
  - [启用SSL(3307、3308、3309)](#启用ssl330733083309)
  - [启动mysqld服务(3307、3308、3309)](#启动mysqld服务330733083309)
  - [使用初始化生成的密码登录MySQL(3307)，并重置密码为qiqi](#使用初始化生成的密码登录mysql3307并重置密码为qiqi)
  - [使用初始化生成的密码登录MySQL(3308)，并重置密码为qiqi](#使用初始化生成的密码登录mysql3308并重置密码为qiqi)
  - [使用初始化生成的密码登录MySQL(3309)，并重置密码为qiqi](#使用初始化生成的密码登录mysql3309并重置密码为qiqi)
  - [使用qiqi密码登录MySQL(3307、3308、3309)，查看对应的端口、socket文件和server_id](#使用qiqi密码登录mysql330733083309查看对应的端口socket文件和server_id)
- [登录对应MySQL实例方式](#登录对应mysql实例方式)

### MySQL单机多实例安装(多配置文件、--initialize方式、mysql_safe启动)

#### 查看CentOS版本

```shell
[root@db01 software]# cat /etc/redhat-release
```

#### 卸载与mariadb、mysql相关的rpm包

```shell
[root@db01 software]# rpm -qa | grep -Ei 'mariadb|mysql' | xargs rpm -ev --nodeps
```

#### 删除my.cnf、mariadb、mysql相关文件和目录

* 该命令非常危险，请根据需要进行删除

```shell
[root@db01 software]# find / -name mariadb | xargs rm -rf
[root@db01 software]# find / -name mysql | xargs rm -rf
[root@db01 software]# find / -name my.cnf | xargs rm -rf
```

#### 安装wget和libaio库

```shell
[root@db01 software]# yum -y install wget libaio
```

#### 下载通用二进制包，然后解压到/usr/local/mysql下并建立符号链接

```shell
[root@db01 software]# wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.32-linux-glibc2.12-x86_64.tar.gz
[root@db01 software]# tar -zxvf mysql-5.7.32-linux-glibc2.12-x86_64.tar.gz -C /usr/local
[root@db01 software]# ln -sv /usr/local/mysql-5.7.32-linux-glibc2.12-x86_64 /usr/local/mysql
```

#### 配置MySQL环境变量

```shell
[root@db01 software]# echo 'export PATH=$PATH:/usr/local/mysql/bin' >> /etc/profile
[root@db01 software]# source /etc/profile
```

#### 创建用于存放MySQL数据、日志和临时文件的目录

```shell
[root@db01 software]# mkdir -p /data/mysql/mysql_{3307,3308,3309}/{data,logs,tmp}
```

#### 创建mysql组，创建mysql用户并加入到mysql组，并且禁止使用mysql登录服务器

```shell
[root@db01 software]# groupadd mysql
[root@db01 software]# adduser -r -g mysql -s /bin/false mysql
```

#### 将相关目录及文件的所有者和所属组都修改为mysql

```shell
[root@db01 software]# chown -R mysql:mysql /usr/local/mysql /usr/local/mysql-5.7.32-linux-glibc2.12-x86_64 /data
```

#### 编辑配置文件/data/mysql/mysql_3307/mysql_3307.cnf

```shell
[root@db01 software]# cat > /data/mysql/mysql_3307/mysql_3307.cnf << EOF
[mysqld]
user=mysql
port=3307

basedir=/usr/local/mysql
datadir=/data/mysql/mysql_3307/data
socket=/data/mysql/mysql_3307/tmp/mysql_3307.sock

log_error=/data/mysql/mysql_3307/logs/error.log
log_error_verbosity=3

server_id=2
log-bin=/data/mysql/mysql_3307/logs/mysql-3307.bin
log-bin-index=/data/mysql/mysql_3307/logs/mysql-3307.index


[client]
socket=/data/mysql/mysql_3307/tmp/mysql_3307.sock
EOF
```

#### 编辑配置文件/data/mysql/mysql_3308/mysql_3308.cnf

```shell
[root@db01 software]# cat > /data/mysql/mysql_3308/mysql_3308.cnf << EOF
[mysqld]
user=mysql
port=3308

basedir=/usr/local/mysql
datadir=/data/mysql/mysql_3308/data
socket=/data/mysql/mysql_3308/tmp/mysql_3308.sock

log_error=/data/mysql/mysql_3308/logs/error.log
log_error_verbosity=3

server_id=3
log-bin=/data/mysql/mysql_3308/logs/mysql-3308.bin
log-bin-index=/data/mysql/mysql_3308/logs/mysql-3308.index


[client]
socket=/data/mysql/mysql_3308/tmp/mysql_3308.sock
EOF
```

#### 编辑配置文件/data/mysql/mysql_3309/mysql_3309.cnf

```shell
[root@db01 software]# cat > /data/mysql/mysql_3309/mysql_3309.cnf << EOF
[mysqld]
user=mysql
port=3309

basedir=/usr/local/mysql
datadir=/data/mysql/mysql_3309/data
socket=/data/mysql/mysql_3309/tmp/mysql_3309.sock

log_error=/data/mysql/mysql_3309/logs/error.log
log_error_verbosity=3

server_id=4
log-bin=/data/mysql/mysql_3309/logs/mysql-3309.bin
log-bin-index=/data/mysql/mysql_3309/logs/mysql-3309.index


[client]
socket=/data/mysql/mysql_3309/tmp/mysql_3309.sock
EOF
```

#### 初始化MySQL(3307、3308、3309)

```shell
[root@db01 software]# /usr/local/mysql/bin/mysqld --defaults-file=/data/mysql/mysql_3307/mysql_3307.cnf --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3307/data
[root@db01 software]# /usr/local/mysql/bin/mysqld --defaults-file=/data/mysql/mysql_3308/mysql_3308.cnf --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3308/data
[root@db01 software]# /usr/local/mysql/bin/mysqld --defaults-file=/data/mysql/mysql_3309/mysql_3309.cnf --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3309/data
```

#### 启用SSL(3307、3308、3309)

```shell
[root@db01 software]# /usr/local/mysql/bin/mysql_ssl_rsa_setup --datadir=/data/mysql/mysql_3307/data
[root@db01 software]# /usr/local/mysql/bin/mysql_ssl_rsa_setup --datadir=/data/mysql/mysql_3308/data
[root@db01 software]# /usr/local/mysql/bin/mysql_ssl_rsa_setup --datadir=/data/mysql/mysql_3309/data
```

#### 启动mysqld服务(3307、3308、3309)

```shell
[root@db01 software]# /usr/local/mysql/bin/mysqld_safe --defaults-file=/data/mysql/mysql_3307/mysql_3307.cnf --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3307/data &
[root@db01 software]# /usr/local/mysql/bin/mysqld_safe --defaults-file=/data/mysql/mysql_3308/mysql_3308.cnf --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3308/data &
[root@db01 software]# /usr/local/mysql/bin/mysqld_safe --defaults-file=/data/mysql/mysql_3309/mysql_3309.cnf --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3309/data &
```

#### 使用初始化生成的密码登录MySQL(3307)，并重置密码为qiqi

```shell
[root@db01 software]# grep 'A temporary password' /data/mysql/mysql_3307/logs/error.log | awk -F ': ' '{print $2}'
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3307/tmp/mysql_3307.sock --connect-expired-password -Bse "ALTER USER 'root'@'localhost' IDENTIFIED BY 'qiqi';"
```

#### 使用初始化生成的密码登录MySQL(3308)，并重置密码为qiqi

```shell
[root@db01 software]# grep 'A temporary password' /data/mysql/mysql_3308/logs/error.log | awk -F ': ' '{print $2}'
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3308/tmp/mysql_3308.sock --connect-expired-password -Bse "ALTER USER 'root'@'localhost' IDENTIFIED BY 'qiqi';"
```

#### 使用初始化生成的密码登录MySQL(3309)，并重置密码为qiqi

```shell
[root@db01 software]# grep 'A temporary password' /data/mysql/mysql_3309/logs/error.log | awk -F ': ' '{print $2}'
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3309/tmp/mysql_3309.sock --connect-expired-password -Bse "ALTER USER 'root'@'localhost' IDENTIFIED BY 'qiqi';"
```

#### 使用qiqi密码登录MySQL(3307、3308、3309)，查看对应的端口、socket文件和server_id

```shell
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3307/tmp/mysql_3307.sock -Bse "SELECT @@port; SELECT @@socket; SELECT @@server_id;"
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3308/tmp/mysql_3308.sock -Bse "SELECT @@port; SELECT @@socket; SELECT @@server_id;"
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3309/tmp/mysql_3309.sock -Bse "SELECT @@port; SELECT @@socket; SELECT @@server_id;"
```

### 登录对应MySQL实例方式

* 使用socket文件登录

```shell
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3307/tmp/mysql_3307.sock
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3308/tmp/mysql_3308.sock
[root@db01 software]# mysql -uroot -p -S /data/mysql/mysql_3309/tmp/mysql_3309.sock
```

* 使用TCP/IP方式登录

```shell
[root@db01 software]# mysql -h127.0.0.1 -P3307 -uroot -p
[root@db01 software]# mysql -h127.0.0.1 -P3308 -uroot -p
[root@db01 software]# mysql -h127.0.0.1 -P3309 -uroot -p
```
- [使用通用二进制在CentOS7.9上安装MySQL5.7.32(--initialize方式)](#使用通用二进制在centos79上安装mysql5732--initialize方式)
  - [查看CentOS版本](#查看centos版本)
  - [卸载与mariadb、mysql相关的rpm包](#卸载与mariadbmysql相关的rpm包)
  - [删除my.cnf、mariadb、mysql相关文件和目录](#删除mycnfmariadbmysql相关文件和目录)
  - [安装wget和libaio库](#安装wget和libaio库)
  - [下载通用二进制包，然后解压到/usr/local/mysql下并建立符号链接](#下载通用二进制包然后解压到usrlocalmysql下并建立符号链接)
  - [配置MySQL环境变量](#配置mysql环境变量)
  - [创建用于存放MySQL数据、日志和临时文件的目录](#创建用于存放mysql数据日志和临时文件的目录)
  - [创建mysql组，创建mysql用户并加入到mysql组，并且禁止使用mysql登录服务器](#创建mysql组创建mysql用户并加入到mysql组并且禁止使用mysql登录服务器)
  - [将相关目录及文件的所有者和所属组都修改为mysql](#将相关目录及文件的所有者和所属组都修改为mysql)
  - [编辑配置文件/etc/my.cnf](#编辑配置文件etcmycnf)
  - [初始化MySQL](#初始化mysql)
  - [启用SSL](#启用ssl)
  - [复制启动脚本到/etc/rc.d/init.d目录下，并为启动脚本赋予执行权限](#复制启动脚本到etcrcdinitd目录下并为启动脚本赋予执行权限)
  - [将mysqld加入系统服务，并设置运行级别为2345](#将mysqld加入系统服务并设置运行级别为2345)
  - [启动mysqld服务，并查看mysqld服务状态](#启动mysqld服务并查看mysqld服务状态)
  - [使用初始化生成的密码登录MySQL](#使用初始化生成的密码登录mysql)
  - [使用ALTER USER语句重置密码](#使用alter-user语句重置密码)

### 使用通用二进制在CentOS7.9上安装MySQL5.7.32(--initialize方式)

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
[root@db01 software]# mkdir -pv /data/mysql/mysql_3306/{data,logs,tmp}
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

#### 编辑配置文件/etc/my.cnf

```shell
[root@db01 software]# cat > /etc/my.cnf << EOF
[mysqld]
user=mysql
server_id=1
basedir=/usr/local/mysql
datadir=/data/mysql/mysql_3306/data
socket=/data/mysql/mysql_3306/tmp/mysql_3306.sock

log_error=/data/mysql/mysql_3306/logs/error.log
log_error_verbosity=3


[client]
socket=/data/mysql/mysql_3306/tmp/mysql_3306.sock
EOF
```

#### 初始化MySQL

```shell
[root@db01 software]# /usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3306/data
```

#### 启用SSL

```shell
[root@db01 software]# /usr/local/mysql/bin/mysql_ssl_rsa_setup --datadir=/data/mysql/mysql_3306/data
```

#### 复制启动脚本到/etc/rc.d/init.d目录下，并为启动脚本赋予执行权限

```shell
[root@db01 software]# cp -rv /usr/local/mysql/support-files/mysql.server /etc/rc.d/init.d/mysqld
[root@db01 software]# chmod u+x /etc/rc.d/init.d/mysqld 
```

#### 将mysqld加入系统服务，并设置运行级别为2345

```shell
[root@db01 software]# chkconfig --add mysqld
[root@db01 software]# chkconfig --level 2345 mysqld on
```

#### 启动mysqld服务，并查看mysqld服务状态

```shell
[root@db01 software]# systemctl start mysqld.service
[root@db01 software]# systemctl status mysqld.service
```

#### 使用初始化生成的密码登录MySQL

```shell
[root@db01 software]# grep 'A temporary password' /data/mysql/mysql_3306/logs/error.log | awk -F ': ' '{print $2}'
[root@db01 software]# mysql -uroot -p
```

#### 使用ALTER USER语句重置密码

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'qiqi';
```


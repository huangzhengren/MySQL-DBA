- [MySQL启动和关闭的方法](#mysql启动和关闭的方法)
  - [MySQL启动方法](#mysql启动方法)
    - [mysqld方式启动](#mysqld方式启动)
    - [mysqld_safe方式启动](#mysqld_safe方式启动)
    - [mysqld指定配置文件启动，--defaults-file参数必须位于最前面](#mysqld指定配置文件启动--defaults-file参数必须位于最前面)
    - [mysqld_safe指定配置文件启动，--defaults-file参数必须位于最前面](#mysqld_safe指定配置文件启动--defaults-file参数必须位于最前面)
    - [Sysvinit方式启动](#sysvinit方式启动)
    - [Sysvinit带参数方式启动，跳过授权表，跳过网络连接](#sysvinit带参数方式启动跳过授权表跳过网络连接)
    - [Systemd方式启动](#systemd方式启动)
    - [通过启动脚本方式启动](#通过启动脚本方式启动)
  - [MySQL关闭方法](#mysql关闭方法)
    - [Sysvinit方式关闭](#sysvinit方式关闭)
    - [Systemd方式关闭](#systemd方式关闭)
    - [通过mysqladmin关闭](#通过mysqladmin关闭)
    - [通过启动脚本方式关闭](#通过启动脚本方式关闭)
    - [登录到mysql服务器关闭](#登录到mysql服务器关闭)
    - [通过mysql命令行直接执行SQL语句关闭](#通过mysql命令行直接执行sql语句关闭)
  - [其他相关命令](#其他相关命令)
    - [Sysvinit方式重启](#sysvinit方式重启)
    - [Sysvinit方式查看状态](#sysvinit方式查看状态)
    - [Systemd方式重启](#systemd方式重启)
    - [Systemd方式查看状态](#systemd方式查看状态)
    - [通过启动脚本方式重启](#通过启动脚本方式重启)
    - [通过启动脚本方式查看状态](#通过启动脚本方式查看状态)

### MySQL启动和关闭的方法

#### MySQL启动方法

##### mysqld方式启动

```shell
[root@app01 software]# /usr/local/mysql/bin/mysqld --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3306/data &
```

##### mysqld_safe方式启动

```shell
[root@app01 software]# /usr/local/mysql/bin/mysqld_safe --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3306/data &
```

##### mysqld指定配置文件启动，--defaults-file参数必须位于最前面

```shell
[root@app01 software]# /usr/local/mysql/bin/mysqld --defaults-file=/etc/my.cnf --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3306/data &
```

##### mysqld_safe指定配置文件启动，--defaults-file参数必须位于最前面

```shell
[root@app01 software]# /usr/local/mysql/bin/mysqld_safe --defaults-file=/etc/my.cnf --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql/mysql_3306/data &
```

##### Sysvinit方式启动

```shell
[root@app01 software]# service mysqld start
```

##### Sysvinit带参数方式启动，跳过授权表，跳过网络连接

```shell
[root@app01 software]# service mysqld start --skip-grant-tables --skip-networking
```

##### Systemd方式启动

```shell
root@app01 software]# systemctl start mysqld.service
```

##### 通过启动脚本方式启动

* 第一种方法

```shell
[root@app01 software]# /etc/init.d/mysqld start
```

* 第二种方法

```shell
[root@app01 software]# /etc/rc.d/init.d/mysqld start
```

#### MySQL关闭方法

##### Sysvinit方式关闭

```shell
[root@app01 software]# service mysqld stop
```

##### Systemd方式关闭

```shell
[root@app01 software]# systemctl stop mysqld.service
```

##### 通过mysqladmin关闭

```mysql
[root@app01 software]# /usr/local/mysql/bin/mysqladmin -uroot -p shutdown
```

##### 通过启动脚本方式关闭

* 第一种方法

```shell
[root@app01 software]# /etc/init.d/mysqld stop
```

* 第二种方法

```shell
[root@app01 software]# /etc/rc.d/init.d/mysqld stop
```

##### 登录到mysql服务器关闭

* 登录到mysql服务器

```shell
[root@app01 software]# mysql -uroot -p
```

* 执行SHUTDOWN命令

```mysql
mysql> SHUTDOWN;
```

##### 通过mysql命令行直接执行SQL语句关闭

```mysql
[root@app01 software]# mysql -uroot -p -Bse "SHUTDOWN;"
```

#### 其他相关命令

##### Sysvinit方式重启

```shell
[root@app01 software]# service mysqld restart
```

##### Sysvinit方式查看状态

```shell
[root@app01 software]# service mysqld status
```

##### Systemd方式重启

```shell
[root@app01 software]# systemctl restart mysqld.service
```

##### Systemd方式查看状态

```shell
[root@app01 software]# systemctl status mysqld.service
```

##### 通过启动脚本方式重启

* 第一种方法

```shell
[root@app01 software]# /etc/init.d/mysqld restart
```

* 第二种方法

```shell
[root@app01 software]# /etc/rc.d/init.d/mysqld restart
```

##### 通过启动脚本方式查看状态

* 第一种方法

```shell
[root@app01 software]# /etc/init.d/mysqld status
```

* 第二种方法

```shell
[root@app01 software]# /etc/rc.d/init.d/mysqld status
```
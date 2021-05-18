### 服务器SQL模式

#### 什么是服务器SQL模式

```
服务器SQL模式定义了MySQL应该支持什么样的SQL语法，以及应该执行什么样的数据验证检查。这使得在不同的环境中使用MySQL以及与其他数据库服务器一起使用MySQL变得更加容易。MySQL服务器将这些模式分别应用于不同的客户端。
```

#### 安装MySQL 5.7时，默认的服务器SQL模式

```
MySQL5.7默认的SQL模式包括:ONLY_FULL_GROUP_BY、STRICT_TRANS_TABLES、NO_ZERO_IN_DATE、NO_ZERO_DATE、ERROR_FOR_DIVISION_BY_ZERO、NO_AUTO_CREATE_USER、NO_ENGINE_SUBSTITUTION。
```


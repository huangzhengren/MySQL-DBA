### DDL(数据定义语言)

#### 概念

* 一组用于操作数据库本身而不是单个表行的SQL语句。包括CREATE、ALTER和DROP语句的所有形式。还包括TRUNCATE语句，因为它的工作方式不同于DELETE FROM table_name语句，尽管最终效果是相似的。
* DDL语句自动提交当前事务;不能回滚。
* InnoDB在线DDL特性增强了CREATE INDEX、DROP INDEX和许多类型ALTER TABLE操作的性能。另外，每个表的InnoDB文件设置会影响DROP TABLE和TRUNCATE TABLE操作的行为。


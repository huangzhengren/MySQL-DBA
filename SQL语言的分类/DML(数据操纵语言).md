#### DML(数据操纵语言)

#### 概念

* DML数据操纵语言，一组用于执行INSERT、UPDATE和DELETE操作的SQL语句。
* SELECT语句有时被认为是一个DML语句，因为SELECT…FOR UPDATE表单与INSERT、UPDATE和DELETE一样需要考虑锁定问题。
* InnoDB表的DML语句是在事务的上下文中操作的，因此它们的效果可以作为单个单元提交或回滚。


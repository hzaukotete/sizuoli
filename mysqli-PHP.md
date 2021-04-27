# MySqlli-PHP
### MySqli

### 创建mysqli对象
`$mysql = new mysqli_connect($server,$username,$password,$dbname,$port)`
### 设置字符集
`$mysql -> set_charset('utf-8');`
### 编写sql语句并且执行
sql语句可以时dml语句，也可以是dql语句；

注：个人使用dql语句，所以接下来的指令结果都按dql语句情况分析
```
$sql='select * from table';
$res = $mysql -> query($sql);
```
### 将SELECT的结果显示在页面，取数据共有四种方式
`fetch_assoc(),fetch_row(),fetch_object(),fetch_array()`

具体区别见<https://blog.csdn.net/weixin_30418341/article/details/96566057>
```
while($row = $res -> fetch_object()){
	var_dump($row);
}
```
### 释放结果集，关闭连接
*为什么要释放结果？*

*在PHP中释放结果或多或少可以告诉服务器和PHP都删除查询返回的结果集.这是一种释放内存的方法,尤其是当您有许多查询或返回大结果集的查询时.*
```
$res->free();
$mysql->close();
```
### 到目前为止基本上可以实现些基础功能
```
<?php
    $server='9.134.33.80';
    $username='root';
    $password='zYjns!4789WF';
    $dbname='midas_sync';
    $port=3306;
    $con=mysqli_connect($server,$username,$password,$dbname,$port);
    if(!$con)
    {
        die("error".mysqli.mysqli_connect_error());
    }
    $sql='select * from t_table_conf;';
    $res=$con->query($sql);
    echo '<pre>';
    while($row = $res -> fetch_assoc())
    {
        var_dump($row);
    }
    $res->free();
    $con->close();
?>
```
### mysqli的事务处理
* mysql事务

	1. MySQL事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你既需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务。
	2. 在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。
	3. 事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。
	4. 事务用来管理 insert,update,delete 语句
	5. 在 MySQL 命令行的默认设置下，事务都是自动提交的，即执行 SQL 语句后就会马上执行 COMMIT 操作。因此要显式地开启一个事务务须使用命令 BEGIN 或 START TRANSACTION，或者执行命令 SET AUTOCOMMIT=0，用来禁止使用当前会话的自动提交。
* 代码

开启事务
```
$mysql -> query('start transaction');
$mysql -> query('set autocommit = false');
$mysql -> begin_transaction();
```
提交事务

`$mysql->commit();`

进行数据回滚

`$mysql->rollback();`

### 批量执行dml语句
### 批量执行dql语句+读数据问题未解决？？？
```
$sql='sql1;sql2;sql3';
$res=$mysql->multi_query($sql);
```


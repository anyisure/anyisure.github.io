---
title: mysql运维
categories: 数据库
tags:
  - mysql
  - 数据库
abbrlink: ad09ad43
date: 2025-03-19 11:19:56
---

## 常见操作

### 清空表数据
#### 1）清空表数据：truncate
sql命令：`truncate table 表名1,表名2...`

注意：
- truncate会删除表中的所有数据、释放空间，但是保留表结构
- truncate删除操作立即生效，原数据不放到rollback segment中，不能rollback，操作不触发trigger
- truncate删除数据后不写服务器log，整体删除速度快

#### 2）删除表：drop
sql命令：`drop table if exists 表名;`

注意：
- drop会删除整个表，包括表结构和数据，释放空间
- 立即执行，执行速度最快
- 不可回滚

#### 3）删除/清空表数据：delete
sql命令：`delete from 表名;`

注意：
- 删除表中数据而不删除表结构，也不释放空间
- delete可以删除一行、多行、乃至整张表
- 每次删除一行，都在事务日志中为所删除的每行记录一项，可回滚

## binlog日志

> binlog是Mysql sever层维护的一种二进制日志，与innodb引擎中的redo/undo log是完全不同的日志；其主要是用来记录对mysql数据更新或潜在发生更新的SQL语句，记录了所有的DDL和DML(除了数据查询语句)语句，并以事务的形式保存在磁盘中，还包含语句所执行的消耗的时间，MySQL的二进制日志是事务安全型的。


作用主要有：

- 复制：MySQL Replication在Master端开启binlog，Master把它的二进制日志传递给slaves并回放来达到master-slave数据一致的目的
- 数据恢复：通过mysqlbinlog工具恢复数据
- 增量备份

二进制日志包括两类文件：
- 二进制日志索引文件（文件名后缀为.index）用于记录所有的二进制文件，
- 二进制日志文件（文件名后缀为.00000*）记录数据库所有的DDL和DML(除了数据查询语句)语句事件。

### 日志管理
#### 开启binlog
修改配置文件 `my.cnf`:

```.cnf
log-bin=mysql-bin
log-bin-index=mysql-bin.index
```
- 这里的 `log-bin` 是指以后生成各 Binlog 文件的前缀，比如上述使用mysql-bin，那么文件就将会是mysql-bin.000001、master-bin.000002 等。

- `log-bin-index` 则指 binlog index 文件的名称，这里我们设置为mysql-bin.index，可以不配置。

#### 基础命令
在`mysql命令行窗口`执行：
```bash
## 查看配置
show variables like '%log_bin%';  

## 查看binlog文件列表(当binlog日志写满(binlog大小max_binlog_size，默认1G),或者数据库重启会生产新文件)
show binary logs;

## 查看日志状态(显示正在写入的二进制文件，及当前position)
show master status;

## 刷新日志(生成一个新编号的binlog日志文件)
flush logs;

## 重置(清空)所有binlog日志
reset master;

```

#### 查看日志内容
> 1）mysqlbinlog查看日志

在MySQL目录下执行命令：
```bash
# 查看日志内容
mysqlbinlog mysql-bin.000001
# 转化为utf8格式
mysqlbinlog mysql-bin.000001 > mysql-bin.000001.txt
# 将日志中行的base64格式转换成sql
mysqlbinlog --base64-output=decode-rows -v mysql-bin.000001 > mysql-bin.000001.txt
```

> 2）show binlog events查看binlog日志

在`mysql命令行窗口`执行：
```bash
# 查询第一个(最早)的binlog日志：
show binlog events; 

# 指定查询 mysql-bin.000021 这个文件：
show binlog events in 'mysql-bin.000021';

# 指定查询 mysql-bin.000021 这个文件，从pos点:8224开始查起：
show binlog events in 'mysql-bin.000021' from 8224;

# 指定查询 mysql-bin.000021 这个文件，从pos点:8224开始查起，查询10条
show binlog events in 'mysql-bin.000021' from 8224 limit 10;

# 指定查询 mysql-bin.000021 这个文件，从pos点:8224开始查起，偏移2行，查询10条
show binlog events in 'mysql-bin.000021' from 8224 limit 2,10;
```

### 数据恢复

#### 备份
```bash
# 1.先完全备份，并生成完成备份之后的新日志
/usr/local/mysql/bin/mysqldump  -h127.0.0.1 -p3306 -uroot -p密码 -lF -B test >/backup/test.dump

# 2.查看新的日志文件名称，最下面一个假定是mysql-bin.000005
mysql> show binary logs;
```

#### 恢复
 
恢复步骤：
```bash
# 1.刷新日志生成新文件，保证要还原的日志mysql-bin.000005不再变化
mysql> flush logs;
# 2.查看要还原日志mysql-bin.000005的内容，假定发现数据破坏在2513行之后
mysql> show binlog events in 'mysql-bin.000005';
# 3.先进行完全备份恢复
mysql  -h127.0.0.1 -p3306 -uroot -p密码 -v </backup/test.dump
# 4.通过binlog日志恢复，恢复到2513行
bin\mysqlbinlog    --stop-position=2513    data\mysql-bin.000005 | bin\mysql  -h127.0.0.1 -p3306 -uroot -p密码  数据库名
```

binlog日志恢复命令解析：
```bash
mysqlbinlog mysql-bin.0000xx | mysql -u用户名 -p密码 数据库名
```
- 常用选项：
    - `--start-position=953`                   起始pos点
    - `--stop-position=1437`                   结束pos点
    - `--start-datetime="2024-01-29 13:00:00"` 起始时间点
    - `--stop-datetime="2024-01-30 13:00:00"`  结束时间点
    - `--database=test`                     指定只恢复test数据库
​    
- 不常用选项：    
    - `-u --user=name`              连接到远程主机的用户名
    - `-p --password[=name]`        连接到远程主机的密码
    - `-h --host=name`              从远程主机上获取binlog日志
    - `--read-from-remote-server`   从某个MySQL服务器上读取binlog日志
 
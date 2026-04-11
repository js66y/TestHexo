---
title: PostgreSQL
date: 2026-04-08T12:06:40+08:00
tags:
---

接下来PostgreSQL很有用...
<!-- more -->
SQL语言：
- DDL数据库定义（表，字段的类型DROP、CREATE、ALTER）
- DML数据库操作（插入，更新INSERT、 UPDATE、 DELETE）
- DQL数据库查询（SELECT）
- DCL数据库控制语言（GRANT、REVOKE、COMMIT、ROLLBACK）

数据库访问(定义的规范，不是指定实现)
- ODBC
- JDBC(Java)
- ADO.NET(.NET)
- PDO(PHP)

在linux中安装后会创建一个用户，在用户下使用psql能够直接连接到数据库
连接数据库后可以使用`\l`命令或者在外部使用`psql -l`命令
配置文件：data下面的postgresql.conf和pg_hba.conf[默认不支持远程连接，只有通过配置文件才能配置]
`\help`或`\h`数据库级别命令->`\help create user`
`\?`查看到服务级别的命令
权限修改`alter`,`grant`

双引号表示关键字，单引号表示字符串
类型转换方式`<T> ""`和`""::T`和`CAST("" as T)`

decimal、numeric作为浮点的类型

```SQL
mysql中没有序列
创建序列：create sequence laozhang.table_id_seq
查询下一个：nextval('laozhang.table_id_seq')
查询当前：currval('laozhang.table_id_seq')
-- 主要应用在主键的自增上
create table xxx(
id int8 default nextval(laozheng.table_id_seq))
-- 或者更简单的
create table laozhang.yyy(
  id bigserial,
  name varchar(16)
);
-- 支持的有smallserial,serial,bigserial三种
-- 通过serial创建的序列删除表后序列不会被删除？(我不确定...)
```

整数类型操作
- `^幂；|/平方根；@绝对值；&与；|或；<<左移；>>右移；pir()；ound()；floor()；ceil()`
字符串类型操作
- `character,character varying,text`
其他数据类型（先不看了...）

事务命令
```begin;commit;rollback```
保存点（针对大事务不好控制的问题，通过先内存后磁盘）
```savepoint;rollback to savepoint```


并发事务会出现的问题:
1. 脏读:读到了其他事务未提交的数据。(必须避免这种情况)
2. 不可重复读:同一事务中，多次查询同一数据，结果不一致，因为其他事务修改造成的。(一些业务中这种不可重复读不是问题)
3. 幻读:同一事务中，多次查询同一数据，因为其他事务对数据进行了增删吗，导致出现了一些问题。(一些业务中这种幻读不是问题)

隔离级别
- READ UNCOMMITTED: 读未提交(啥用没用，并且PGSQL没有，提供了只是为了完整性)
- READCOMMITTED: 读已提交，可以解决脏读(PGSQL默认隔离级别)
- REPEATABLE READ: 可重复读，可以解决脏读和不可重复读(MySQL默认是这个隔离级别，PGSQL也提供了，但是设置为可重复读，效果还是串行化)
- SERIALIZABLE: 串行化，啥都能解决(锁,效率慢)

多版本并发控制MVCC：
1. 比如你要查询一行数据，但是这行数据正在被修改，事务还没提交，如果此时对这行数据加锁，会导致其他的读操作阻塞，需要等待。如果采用PostgreSQL，他的内部会针对这一行数据保存多个版本，如果数据正在被写入，包就保存之前的数据版本。让读操作去查询之前的版本，不需要阻塞。等写操作的事务提交了，读操作才能查看到最新的数据。这几个及时可以确保读写操作没有冲突，这个就是MVCC的主要特点。
2. 在操作之前，先了解一下PGSQL中，每张表都会自带两个字段：
    - xmin(当前版本号): 给当前事务分配的数据版本。如果有其他事务做了写操作，并且提交事务了，就给xmin分配新的版本。
    - xmax(系统最新版本号): 当前事务没有存在新版本，xmax就是0。如果有其他事务做了写操作，未提交事务，将写操作的版本放到xmax中。提交事务后，xmax会分配到xmin中，然后xmax归0。

表锁：
- ACCESS SHARE:共享锁(读锁),读读操作不阻塞，但是不允许出现写操作并行
- ACCESS EXCLUSIVE(默认):互斥锁(写锁),无论什么操作进来，都阻塞。
行锁
- for update
- for share

数据迁移pgloader:https://pgloader.readthedocs.io/en/latest/index.html

主从：
PostgreSQL自身只支持简单的主从，没有主从自动切换，仿照类似Nginx的效果一样，采用keepalived的形式，在主节点宕机后，通过脚本的执行完成主从切换。
1. 查看主从信息
    - 查看从节点是否有t1表
    - 从节点无法完成写操作，他是只读模式
    - 主节点添加一行数据，从节点再查询，可以看到最新的数据
    - 查看节点`主查从select * from pg_stat_replication；从查主select * from pg_stat_wal_receiver`
2. 主从切换（主节点only拥有一下两个文件）
    - standby.signal文件，这个是从节点开启备份
    - postgresql.auto.conf文件，这个从节点指定主节点的地址信息
    - pg_ctl prompte(需要配置，更改为主节点)
    - pg_rewind(基于归档日志的对比)
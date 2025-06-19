# [0063. 自增变量的持久化](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0063.%20%E8%87%AA%E5%A2%9E%E5%8F%98%E9%87%8F%E7%9A%84%E6%8C%81%E4%B9%85%E5%8C%96)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 自增变量持久化](#2--自增变量持久化)

<!-- endregion:toc -->

## 1. 📝 概述

- 在 MySQL 8.0 之前，自增主键 AUTO_INCREMENT 的值如果大于 `max(primary key)+1`，在 MySQL 重启后，会重置 `AUTO_INCREMENT=max(primary key)+1`，这种现象在某些情况下会导致业务主键冲突或者其他难以发现的问题。
- **在 MySQL 5.7 系统中，对于自增主键的分配规则，是由 `InnoDB` 数据字典内部一个计数器来决定的，而该计数器只在内存中维护，并不会持久化到磁盘中**。

```sql
-- 当数据库重启时，该计数器会通过下面这种方式初始化。
SELECT MAX(ai_col) FROM table_name FOR UPDATE;
```

- **MySQL 8.0 将自增主键的计数器持久化到重做日志中。每次计数器发生改变，都会将其写入重做日志中。如果数据库重启，InnoDB 会根据重做日志中的信息来初始化计数器的内存值。为了尽量减小对系统性能的影响，计数器写入到重做日志时并不会马上刷新数据库系统**。

## 2. 💻 自增变量持久化

```sql
-- 创建的数据表中包含自增主键的 id 字段
CREATE TABLE teest1(id INT AUTO_INCREMENT PRIMARY KEY);

-- 插入 4 个空值
INSERT INTO teest1 VALUES ((0), (0), (0), (0));

-- 查询数据表 test1 中的数据
SELECT * FROM teest1;
-- 结果如下：
-- +----+
-- | id |
-- +----+
-- |  1 |
-- |  2 |
-- |  3 |
-- |  4 |
-- +----+

-- 删除 id 为 4 的记录
DELETE FROM teest1 WHERE id = 4;

-- 再次插入一个空值
INSERT INTO teest1 VALUES ((0));

-- 查询数据表 test1 中的数据
SELECT * FROM teest1;
-- 结果如下：
-- +----+
-- | id |
-- +----+
-- |  1 |
-- |  2 |
-- |  3 |
-- |  5 |
-- +----+

-- 从结果可以看出，虽然删除了 id 为 4 的记录，但是再次插入空值时，并没有重用被删除的 4，而是分配了 5。

-- 删除 id 为 5 的记录
DELETE FROM teest1 WHERE id = 5;

-- 重启数据库，重新插入一个空值。
INSERT INTO teest1 VALUES ((0));

-- 查询数据表 test1 中的数据
SELECT * FROM teest1;

-- MySQL 8.0 之前输出结果如下：
-- +----+
-- | id |
-- +----+
-- |  1 |
-- |  2 |
-- |  3 |
-- |  4 |
-- +----+

-- 从结果可以看出，新插入的 0 值分配的是 4，按照重启前的操作逻辑，此处应该分配 6。
-- 出现上述结果的主要原因是自增主键没有持久化。

-- MySQL 8.0 输出结果如下：
-- +----+
-- | id |
-- +----+
-- |  1 |
-- |  2 |
-- |  3 |
-- |  6 |
-- +----+

-- 从结果可以看出，自增变量已经持久化了。
```

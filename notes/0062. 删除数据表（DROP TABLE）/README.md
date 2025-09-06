# [0062. 删除数据表（DROP TABLE）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0062.%20%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE%E8%A1%A8%EF%BC%88DROP%20TABLE%EF%BC%89)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 删除没有被关联的表](#2--删除没有被关联的表)
- [3. 💻 删除被其他表关联的主表](#3--删除被其他表关联的主表)

<!-- endregion:toc -->

## 1. 📝 概述

- 删除数据表就是将数据库中已经存在的表从数据库中删除。
- 在删除表的同时，表的定义和表中所有的数据均会被删除。
- 在进行删除操作前，最好对表中的数据做一个备份，以免造成无法挽回的后果。

## 2. 💻 删除没有被关联的表

- 在 MySQL 中，使用 DROP TABLE 可以一次删除一个或多个没有被其他表关联的数据表。
- 基本语法：

```sql
DROP TABLE [IF EXISTS] 表1, 表2, ... 表n;
```

- `IF EXISTS`
  - 如果要删除的表不存在，MySQL 会提示错误：Unknown table '表名'
  - 参数“IF EXISTS”用于在删除前判断删除的表是否存在
  - 如果表存在，就执行删除操作，这样可以避免报错。
- 示例：删除顾客表和订单表

```sql
DROP TABLE IF EXISTS customers, orders;
```

## 3. 💻 删除被其他表关联的主表

- 在数据表之间存在外键关联的情况下，如果直接删除父表，结果会显示失败，原因是直接删除将破坏表的参照完整性。
- 如果必须要删除，可以先删除与它关联的子表，再删除父表，只是这样就同时删除了两个表中的数据。
- 有的情况下可能要保留子表，这时若要单独删除父表，只需将关联的表的外键约束条件取消，然后就可以删除父表了。

```sql
-- 1. 创建主表和子表并添加外键约束
-- 创建主表 `users`
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- 创建子表 `orders`，并与 `users` 表建立外键关联
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    CONSTRAINT fk_orders_user
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- ------------------------------------------------

-- 2. 尝试直接删除主表 users（会失败）
DROP TABLE users;
-- ❌ 报错：因为 `orders` 表依赖于 `users` 表的主键，无法直接删除。

-- ------------------------------------------------

-- 3. 解除外键约束后删除主表
-- 先解除外键约束
ALTER TABLE orders
DROP FOREIGN KEY fk_orders_user;

-- fk_orders_user
-- 假如在第一步定义表结构的时候
-- 并没有给外键约束显式命名
-- 不知道约束的名称是什么
-- 那么可以先通过 `SHOW CREATE TABLE orders` 命令查询外键约束的名称

-- ------------------------------------------------

-- 4. 再次尝试删除主表 users
DROP TABLE users;
-- ✅ 成功：此时可以删除主表

-- ------------------------------------------------

-- 如果需要保留子表结构但清理数据，可选择清空主表数据而非删除表
TRUNCATE TABLE users; -- 清空主表数据但保留表结构
-- TRUNCATE 操作将清除所有数据，但仍保留表结构，适用于需要保留外键关系的情况
```

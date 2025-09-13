# [0059. 查看表基本结构（DESCRIBE）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0059.%20%E6%9F%A5%E7%9C%8B%E8%A1%A8%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84%EF%BC%88DESCRIBE%EF%BC%89)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 📒 基本语法](#2--基本语法)
- [3. 💻 `DESCRIBE` 和 `DESC`](#3--describe-和-desc)

<!-- endregion:toc -->

## 1. 🫧 评价

- `DESCRIBE` 和 `DESC`
  - 在数据库开发和调试过程中，经常需要快速查看某个表的字段定义及其结构信息。
  - MySQL 提供了 `DESCRIBE` 和 `DESC` 命令，用于展示表的基本结构，包括字段名、数据类型、是否允许为空、是否为主键、默认值等。
- `DESCRIBE` / `DESC` 命令的作用

| 作用 | 说明 |
| --- | --- |
| **快速查看表结构** | 显示字段名、数据类型、是否为空、是否主键、默认值等信息。 |
| **辅助开发与调试** | 在编写 SQL 查询或设计关联表时，帮助理解现有表的字段定义。 |
| **验证建表语句** | 可以确认创建表时设置的约束（如 `NOT NULL`, `DEFAULT`, `AUTO_INCREMENT`）是否生效。 |

- 其他数据库（非 MySQL）中的等价命令

| 数据库 | 查看表结构命令 | 示例 |
| --- | --- | --- |
| **PostgreSQL** | `\d 表名` | `\d users` |
| **SQL Server** | `sp_help 表名` | `EXEC sp_help users` |
| **Oracle** | `DESC 表名` 或 `DESCRIBE 表名` | `DESC employees` |
| **SQLite** | `PRAGMA table_info(表名);` | `PRAGMA table_info(users);` |

## 2. 📒 基本语法

```sql
DESCRIBE 表名;
DESC 表名; -- DESCRIBE 可简写为 DESC
```

## 3. 💻 `DESCRIBE` 和 `DESC`

```sql
-- 假设我们有如下用户表：
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 执行以下命令查看表结构：
DESCRIBE users;

-- 输出结果类似如下：
-- | Field      | Type         | Null | Key  | Default           | Extra              |
-- |------------|--------------|------|------|-------------------|--------------------|
-- | id         | int          | NO   | PRI  | NULL              | auto_increment     |
-- | username   | varchar(50)  | NO   |      | NULL              |                    |
-- | email      | varchar(100) | YES  | UNI  | NULL              |                    |
-- | created_at | timestamp    | YES  |      | CURRENT_TIMESTAMP | DEFAULT_GENERATED  |
```

- 输出字段含义简介

| 字段名 | 含义说明 |
| --- | --- |
| `Field` | 字段名称 |
| `Type` | 数据类型（如 `INT`, `VARCHAR`, `TIMESTAMP` 等） |
| `Null` | 表示该列是否可以存储 `NULL` 值。（`YES` / `NO`） |
| `Key` | 表示该列是否已编制索引。（如 `PRI` 表示主键，`UNI` 表示唯一索引，`MUL` 表示在列中某个给定值允许出现多次） |
| `Default` | 表示该列是否有默认值，有的话指定值是多少。（若未设置则为 `NULL`） |
| `Extra` | 表示可以获取的与给定列有关的附加信息。（如 `auto_increment`, `on update CURRENT_TIMESTAMP` 等） |

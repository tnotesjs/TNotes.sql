# [0060. 查看表详细结构（SHOW CREATE TABLE）](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0060.%20%E6%9F%A5%E7%9C%8B%E8%A1%A8%E8%AF%A6%E7%BB%86%E7%BB%93%E6%9E%84%EF%BC%88SHOW%20CREATE%20TABLE%EF%BC%89)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 📒 基本语法](#2--基本语法)
- [3. 💻 示例说明](#3--示例说明)

<!-- endregion:toc -->

## 1. 📝 概述

- `SHOW CREATE TABLE`
  - 在数据库开发中，除了使用 `DESCRIBE`、`DESC` 查看表的基本字段信息外，有时还需要了解表的完整定义，包括字符集、排序规则、索引、约束、分区信息以及建表语句本身。
  - MySQL 提供了 `SHOW CREATE TABLE` 命令可查看完整的建表语句及表结构细节。
- `\G` 参数
  - `G` - 表示 `Grid`、`vertical Grid layout`、`Vertical Output Mode`（垂直输出模式）
  - 如果不加 `\G` 参数，显示的结果可能非常混乱，加上参数 `\G` 之后，可使显示结果更加直观，易于查看。
  - 每个字段或属性单独一行显示，便于阅读长语句。
  - `\G` 不是 SQL 语句的一部分，而是 MySQL 命令行客户端的一个格式化指令。
- `SHOW CREATE TABLE` 命令的作用

| 作用 | 说明 |
| --- | --- |
| **查看完整的建表语句** | 返回创建该表所使用的完整 SQL 语句。 |
| **获取表结构细节** | 包括字符集、排序规则、引擎类型、索引、约束等信息。 |
| **辅助迁移或复制表结构** | 可直接复制该语句用于在其他数据库或环境中重建相同结构的表。 |

- 使用场景

| 场景             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| **结构复现**     | 直接复制输出的建表语句，在其他环境快速重建相同结构的表。 |
| **调试建表语句** | 验证实际生效的建表语句，检查是否有隐式设置被自动添加。   |
| **迁移与备份**   | 在做数据库迁移或导出时，常配合 `mysqldump` 使用。        |
| **权限审计**     | 查看表是否存在敏感配置或不符合规范的定义。               |

- 其他数据库中的等价操作

| 数据库 | 查看详细建表语句的方法 | 示例 |
| --- | --- | --- |
| **PostgreSQL** | 使用 `\d+ 表名` 或查询系统表 `pg_views` / `pg_matviews` | `\d+ users` |
| **SQL Server** | 使用 `sp_help` 和 `sp_helptext` 联合查询 | `EXEC sp_helptext 'users'` |
| **Oracle** | 查询 `DBMS_METADATA.GET_DDL` 函数 | `SELECT DBMS_METADATA.GET_DDL('TABLE','USERS') FROM DUAL;` |
| **SQLite** | 查询 `sqlite_master` 系统表 | `SELECT sql FROM sqlite_master WHERE name='users';` |

## 2. 📒 基本语法

```sql
SHOW CREATE TABLE <表名\G>;
```

## 3. 💻 示例说明

```sql
-- 假设我们有如下用户表：
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- 执行以下命令查看详细结构：
-- SHOW CREATE TABLE users; -- 默认输出格式（横向），输出结果可能不太适合阅读
-- SHOW CREATE TABLE users\G;

SHOW CREATE TABLE users\G;
-- 输出结果类似如下：
-- *************************** 1. row ***************************
--        Table: users
-- CREATE TABLE `users` (
--   `id` int NOT NULL AUTO_INCREMENT,
--   `username` varchar(50) NOT NULL,
--   `email` varchar(100) DEFAULT NULL,
--   `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
--   PRIMARY KEY `id`,
--   UNIQUE KEY `email` (`email`)
-- ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci
```

- 输出内容详解

| 内容           | 说明                                            |
| -------------- | ----------------------------------------------- |
| `CREATE TABLE` | 完整的建表语句，可用于重建该表                  |
| 字段定义       | 包括字段名、数据类型、是否为空、默认值等        |
| 约束信息       | 主键（PRIMARY KEY）、唯一性约束（UNIQUE KEY）等 |
| 存储引擎       | 如 `ENGINE=InnoDB`                              |
| 字符集         | 如 `DEFAULT CHARSET=utf8mb4`                    |
| 排序规则       | 如 `COLLATE=utf8mb4_unicode_ci`                 |
| 自动增长       | 若存在 `AUTO_INCREMENT`，也会显示               |

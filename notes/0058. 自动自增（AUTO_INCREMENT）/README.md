# [0058. 自动自增（AUTO_INCREMENT）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0058.%20%E8%87%AA%E5%8A%A8%E8%87%AA%E5%A2%9E%EF%BC%88AUTO_INCREMENT%EF%BC%89)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 📒 基本语法](#2--基本语法)
- [3. 💻 创建表时设置 `AUTO_INCREMENT`](#3--创建表时设置-auto_increment)

<!-- endregion:toc -->

## 1. 🫧 评价

- `AUTO_INCREMENT`
  - 在数据库应用中，经常希望在每次插入新记录时，系统自动生成字段的主键值，可以通过为表主键添加 `AUTO_INCREMENT` 关键字来实现。
    - `AUTO_INCREMENT` 是 MySQL 中的一种自动增长机制。
    - 通常用于主键字段，使得每次插入新记录时，系统自动为该字段生成唯一的递增值。
  - 默认情况下，`AUTO_INCREMENT` 从 `1` 开始，每新增一条记录自动加 `1`。
  - 一个表中只能有一个字段使用 `AUTO_INCREMENT` 约束，且该字段必须是主键或包含在主键中。
  - `AUTO_INCREMENT` 约束的字段可以是任何整数类型（`TINYINT`、`SMALLIN`、`INT`、`BIGINT` 等）​。
- `AUTO_INCREMENT` 的作用

| 作用             | 说明                                   |
| ---------------- | -------------------------------------- |
| **简化主键管理** | 不需要手动指定主键值，数据库自动分配。 |
| **保证唯一性**   | 生成的值不会重复，适合用作唯一标识。   |
| **提高开发效率** | 减少业务逻辑中对主键赋值的处理。       |

- 注意事项与限制

| 注意事项 | 说明 |
| --- | --- |
| **仅适用于整数类型** | `AUTO_INCREMENT` 只能用于整数类型字段（如 `INT`, `BIGINT`）。 |
| **必须为主键或主键组成部分** | 非主键字段不能直接使用 `AUTO_INCREMENT`。 |
| **一张表只能有一个自增字段** | 不支持多个字段同时使用 `AUTO_INCREMENT`。 |
| **删除记录不影响自增序列** | 删除某条记录后，其 `id` 不会被复用。 |
| **性能优化建议** | 在高并发插入场景下，`AUTO_INCREMENT` 能有效减少锁冲突。 |
| **迁移兼容性注意** | 其他数据库（如 PostgreSQL）没有 `AUTO_INCREMENT`，而是使用 `SERIAL` 或 `IDENTITY` 实现类似功能。 |

## 2. 📒 基本语法

```sql {2}
CREATE TABLE 表名 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    字段名 数据类型,
    ...
);
```

## 3. 💻 创建表时设置 `AUTO_INCREMENT`

```sql {2}
CREATE TABLE Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
-- id 列被定义为自增主键，默认从 1 开始，每次插入自动加 1。

-- 插入数据时的行为
INSERT INTO Users (name) VALUES ('Alice');
INSERT INTO Users (name) VALUES ('Bob');
-- 插入时不需指定 id 字段，系统会自动分配：
-- 第一条记录 `id = 1`
-- 第二条记录 `id = 2`

-- 修改自增字段的起始值
-- 如果显式插入了 `id` 值，则后续自增值将基于最大值继续递增。
-- 可以手动设置 `AUTO_INCREMENT` 的起始值：
ALTER TABLE Users AUTO_INCREMENT = 100;
-- 下次插入记录时，id 将从 100 开始递增
```

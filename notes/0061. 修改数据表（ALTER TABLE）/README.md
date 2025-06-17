# [0061. 修改数据表（ALTER TABLE）](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0061.%20%E4%BF%AE%E6%94%B9%E6%95%B0%E6%8D%AE%E8%A1%A8%EF%BC%88ALTER%20TABLE%EF%BC%89)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 `RENAME` 修改表名](#2--rename-修改表名)
- [3. 💻 `MODIFY` 修改字段的数据类型](#3--modify-修改字段的数据类型)
- [4. 💻 `CHANGE` 同时修改字段名和数据类型](#4--change-同时修改字段名和数据类型)
- [5. 💻 `ADD COLUMN` 添加字段](#5--add-column-添加字段)
- [6. 💻 `DROP COLUMN` 删除字段](#6--drop-column-删除字段)
- [7. 💻 `MODIFY` 调整字段排序](#7--modify-调整字段排序)
- [8. 💻 `ENGINE` 修改表的存储引擎](#8--engine-修改表的存储引擎)
- [9. 💻 `FOREIGN KEY ... REFERENCES ...` 添加外键约束](#9--foreign-key--references--添加外键约束)
- [10. 💻 `DROP FOREIGN KEY ...` 删除外键约束](#10--drop-foreign-key--删除外键约束)

<!-- endregion:toc -->

## 1. 📝 概述

- `ALTER TABLE`
  - 在应用开发过程中，随着业务需求的变化，有时需要对已经存在的数据表进行结构上的调整。
  - MySQL 提供了 `ALTER TABLE` 命令用于修改数据表的结构。
  - 常用的修改表的操作有修改表名、修改字段数据类型或字段名、增加和删除字段、修改字段的排列位置、更改表的存储引擎、删除表的外键约束等。
- 修改表的一些常见操作

| 操作 | 描述 | SQL 示例 |
| --- | --- | --- |
| 修改表名 | 更改现有表的名称 | `ALTER TABLE old_name RENAME TO new_name;` |
| 修改字段类型 | 更改某个字段的数据类型 | `ALTER TABLE table MODIFY column type;` |
| 修改字段名 | 更改字段的名称 | `ALTER TABLE table CHANGE old_col new_col type;` |
| 添加字段 | 向表中新增一个或多个字段 | `ALTER TABLE table ADD COLUMN col type;` |
| 删除字段 | 移除不再使用的字段 | `ALTER TABLE table DROP COLUMN col;` |
| 调整字段位置 | 控制字段在表中的排列顺序 | `ALTER TABLE table MODIFY col type AFTER other_col;` |
| 修改存储引擎 | 更改表所使用的存储引擎（如 MyISAM、InnoDB） | `ALTER TABLE table ENGINE=engine;` |
| 添加外键 | 添加外键约束 | `ALTER TABLE child ADD CONSTRAINT fk_name FOREIGN KEY(col) REFERENCES parent(col);` |
| 删除外键 | 删除外键约束 | `ALTER TABLE table DROP FOREIGN KEY fk_name;` |

- 查看表结构
  - 在修改的 sql 执行之后，可以通过 `DESC <表名>;`、`SHOW CREATE TABLE <表名/G>;` 命令查看表结构是否发生变化。

## 2. 💻 `RENAME` 修改表名

```sql
ALTER TABLE 旧表名
RENAME TO 新表名;

-- 示例：
-- 假设现在需要将表 `users` 的表名改为 `members`。
ALTER TABLE users
RENAME TO members;

-- 修改表名不会对表结构有任何影响。
```

## 3. 💻 `MODIFY` 修改字段的数据类型

```sql
ALTER TABLE 表名
MODIFY 字段名 新数据类型 [约束条件];

-- 示例：
-- 假设 `email` 字段的数据类型为 `VARCHAR(100)` 现在需要将其改为 `VARCHAR(150)`。
ALTER TABLE users
MODIFY email VARCHAR(150);

-- 注意：
-- 修改字段类型可能会影响已有数据，需确保兼容性。
```

## 4. 💻 `CHANGE` 同时修改字段名和数据类型

```sql
ALTER TABLE 表名
CHANGE 旧字段名 新字段名 数据类型 [约束条件];

-- 示例：
-- 假设 users 表中有 username 字段，数据类型是 `VARCHAR(50)`
-- 现在需要修改为 user_name，并将其数据类型改为 `VARCHAR(100)`
ALTER TABLE users
CHANGE username user_name VARCHAR(100);
```

## 5. 💻 `ADD COLUMN` 添加字段

```sql
ALTER TABLE 表名
ADD COLUMN 字段名 数据类型 [约束条件] [位置];

-- 示例 1：添加字段到末尾（默认）
ALTER TABLE users
ADD COLUMN age INT;

-- 示例 2：添加字段到最前
ALTER TABLE users
ADD COLUMN id_card VARCHAR(18) FIRST;

-- 示例 3：添加字段到某一列之后
ALTER TABLE users
ADD COLUMN gender VARCHAR(10) AFTER username;
```

## 6. 💻 `DROP COLUMN` 删除字段

```sql
ALTER TABLE 表名
DROP COLUMN 字段名;

-- 示例：
-- 假设现在需要删除 `age` 字段。
ALTER TABLE users
DROP COLUMN age;

-- 注意：
-- 删除字段将永久移除该字段及其数据，请谨慎操作。
```

## 7. 💻 `MODIFY` 调整字段排序

```sql
-- 使用 `MODIFY` 或 `CHANGE` 配合 `AFTER` 子句可修改字段的排列位置
ALTER TABLE 表名
MODIFY 字段名 数据类型 FIRST|AFTER  目标字段名;

-- 示例：
ALTER TABLE users
MODIFY gender VARCHAR(10) AFTER id;
-- 将 `gender` 字段移动到 id 字段之后。

-- 注意：调整字段位置时，必须重新指定该字段的数据类型。
-- MySQL 的 MODIFY 操作不仅用于调整字段顺序，还会检查字段的数据类型是否与原定义一致。
-- 即使不修改数据类型，也需在语句中显式声明原有的数据类型，否则会报错。
```

## 8. 💻 `ENGINE` 修改表的存储引擎

```sql
ALTER TABLE 表名
ENGINE = 引擎名;

-- 示例：
ALTER TABLE users
ENGINE = InnoDB;
-- 将表 `users` 的存储引擎改为 `InnoDB`。
-- 常见引擎：MyISAM、InnoDB、MEMORY、CSV
```

## 9. 💻 `FOREIGN KEY ... REFERENCES ...` 添加外键约束

```sql
-- 添加外键约束
ALTER TABLE 从表
ADD CONSTRAINT 外键名
FOREIGN KEY (从表字段) REFERENCES 主表(主表字段);

-- 示例：
ALTER TABLE orders
ADD CONSTRAINT fk_user_id
FOREIGN KEY (user_id) REFERENCES users(id);
```

## 10. 💻 `DROP FOREIGN KEY ...` 删除外键约束

```sql
-- 删除外键约束
ALTER TABLE 表名
DROP FOREIGN KEY 外键名;

-- 示例：
ALTER TABLE orders
DROP FOREIGN KEY fk_user_id;
-- 删除外键后，关联字段仍然存在，但不会再有外键约束检查。
```

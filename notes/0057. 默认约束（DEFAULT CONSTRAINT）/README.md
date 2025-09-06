# [0057. 默认约束（DEFAULT CONSTRAINT）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0057.%20%E9%BB%98%E8%AE%A4%E7%BA%A6%E6%9D%9F%EF%BC%88DEFAULT%20CONSTRAINT%EF%BC%89)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 📒 基本语法](#2--基本语法)
- [3. 💻 创建表时添加 `DEFAULT` 约束](#3--创建表时添加-default-约束)
- [4. 💻 修改已有表添加 `DEFAULT` 约束](#4--修改已有表添加-default-约束)
- [5. 💻 删除 `DEFAULT` 约束](#5--删除-default-约束)

<!-- endregion:toc -->

## 1. 📝 概述

- 默认约束（Default Constraint）
  - 默认约束用于指定某列在未提供值时所使用的默认值。
  - 当插入新记录时没有为该字段显式赋值，数据库将自动使用默认值填充该字段。
    - 如男性同学较多，性别就可以默认为‘男’。如果插入一条新的记录时没有为这个字段赋值，那么系统会自动为这个字段赋值为‘男’。
  - 常用于简化数据插入操作，减少业务代码中对默认值的处理。
- `DEFAULT` 约束的作用

| 作用 | 说明 |
| --- | --- |
| **简化插入操作** | 插入数据时无需手动填写某些固定或常用值。 |
| **增强数据完整性** | 防止字段出现空值（NULL），尤其适用于非空字段设置默认值。 |
| **统一业务逻辑** | 保证某些字段在缺失输入时仍能保持合理的默认状态，如性别、状态、启用标志等。 |

## 2. 📒 基本语法

```sql
CREATE TABLE 表名 (
    字段名 数据类型 DEFAULT 默认值,
    ...
);

-- 也可以在已有表中修改字段添加默认值：
ALTER TABLE 表名
ALTER COLUMN 字段名 SET DEFAULT 默认值;

-- 删除默认值约束：
ALTER TABLE 表名
ALTER COLUMN 字段名 DROP DEFAULT;

-- 注意
-- 不同数据库语法略有差异
-- 例如 SQL Server 使用 `ADD CONSTRAINT` 定义命名默认约束。
```

## 3. 💻 创建表时添加 `DEFAULT` 约束

```sql {3}
CREATE TABLE Users (
    id INT PRIMARY KEY,
    gender VARCHAR(10) DEFAULT '男'
);
-- `gender` 列被定义为默认值 '男'。
-- 如果插入数据时不指定 gender 的值，则系统会自动将其设为 '男'。
```

## 4. 💻 修改已有表添加 `DEFAULT` 约束

```sql
-- 假设已经存在如下表：
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    status VARCHAR(20)
);

-- 添加默认值约束
ALTER TABLE Employees
ALTER COLUMN status SET DEFAULT '在职';
-- 将 `status` 列设置默认值为 '在职'
```

## 5. 💻 删除 `DEFAULT` 约束

```sql
-- 删除 status 列的默认值约束
ALTER TABLE Employees
ALTER COLUMN status DROP DEFAULT;
-- 该语句移除了 `status` 列的默认值设置
```

# [0055. 唯一性约束（UNIQUE CONSTRAINT）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0055.%20%E5%94%AF%E4%B8%80%E6%80%A7%E7%BA%A6%E6%9D%9F%EF%BC%88UNIQUE%20CONSTRAINT%EF%BC%89)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 📒 `PRIMARY KEY` vs. `UNIQUE`](#2--primary-key-vs-unique)
- [3. 📒 基本语法](#3--基本语法)
- [4. 💻 创建表时添加 `UNIQUE` 约束](#4--创建表时添加-unique-约束)
- [5. 💻 创建表时对多列组合应用 `UNIQUE` 约束](#5--创建表时对多列组合应用-unique-约束)
- [6. 💻 向已有表添加 `UNIQUE` 约束](#6--向已有表添加-unique-约束)
- [7. 💻 删除已有表中指定字段的 `UNIQUE` 约束](#7--删除已有表中指定字段的-unique-约束)

<!-- endregion:toc -->

## 1. 🫧 评价

- UNIQUE
  - 唯一性约束（Unique Constraint）要求该列唯一，允许为空，但只能出现一个空值。
  - 唯一约束可以确保一列或者几列不出现重复值。
  - 一个表中可以有多个字段声明为 UNIQUE。
- 作用
  - **保证数据唯一性**：如用户名、邮箱、手机号等字段要求不重复。
  - **辅助主键机制**：当主键不是唯一标识时，可以使用唯一约束来增强数据完整性。
    - 主键可能只是用于技术层面的唯一标识（如自增 ID），而不是业务上的唯一标识。
    - 某些字段在业务逻辑上需要保持唯一，但它们不是主键。
    - 主键虽然能保证数据库记录的唯一性，但在业务逻辑中，某些字段也需要唯一性保障，这时就需要用 UNIQUE 约束来补充。
  - **支持索引创建**：数据库会自动为设置了 `UNIQUE` 的列创建唯一性索引。
- 应用场景
  - 用户名、邮箱、手机号等唯一标识字段
  - 订单编号、身份证号、车牌号等业务唯一字段
  - 联合唯一约束用于复合业务场景（如：学生 + 课程 的唯一性）
- 注意事项
  - 唯一约束对大小写是否敏感取决于数据库及字符集配置（如 MySQL 默认不区分大小写）。
  - 使用联合唯一约束时应合理设计字段顺序。
  - 删除唯一约束需通过 `DROP INDEX` 操作，而非直接删除约束语句。

## 2. 📒 `PRIMARY KEY` vs. `UNIQUE`

| 特性             | 主键（PRIMARY KEY） | 唯一性约束（UNIQUE） |
| ---------------- | ------------------- | -------------------- |
| 是否允许 NULL    | 不允许              | 允许一个 NULL        |
| 一张表有几个     | 仅一个              | 多个                 |
| 是否自动创建索引 | 是                  | 是                   |
| 是否可重复       | 否                  | 否                   |

## 3. 📒 基本语法

```sql {2,11}
CREATE TABLE 表名 (
    字段名 数据类型 UNIQUE,
    ...
);

-- 也可以为多列设置联合唯一约束：

CREATE TABLE 表名 (
    字段1 数据类型,
    字段2 数据类型,
    UNIQUE (字段1, 字段2)
);
```

## 4. 💻 创建表时添加 `UNIQUE` 约束

```sql {3}
CREATE TABLE Users (
    id INT PRIMARY KEY,
    email VARCHAR(255) UNIQUE
);
-- `email` 列被定义为 `UNIQUE`，以确保所有用户的电子邮件地址是唯一的。
```

## 5. 💻 创建表时对多列组合应用 `UNIQUE` 约束

```sql {5}
CREATE TABLE Reservations (
    reservation_id INT PRIMARY KEY,
    room_number INT,
    reserved_date DATE,
    UNIQUE (room_number, reserved_date)
);
-- UNIQUE (room_number, reserved_date)
-- 作用：将 `UNIQUE` 约束应用于房间号 `room_number` 和预定日期 `reserved_date` 的组合。
-- 含义：以防止同一房间在同一天被多次预订。
```

## 6. 💻 向已有表添加 `UNIQUE` 约束

```sql {10}
-- 假如已经创建了如下 Employees 表
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100)
);

ALTER TABLE Employees
ADD CONSTRAINT unique_email UNIQUE (email);
-- 这将向 `Employees` 表中的 `email` 列添加 `UNIQUE` 约束。

-- ALTER TABLE Employees
-- 表示要修改现有的表结构，表名为 Employees。

-- ADD CONSTRAINT unique_email
-- 添加一个约束，并将其命名为 unique_email。
-- 使用 CONSTRAINT 关键字可以为这个约束指定一个名称，方便以后引用或删除。

-- UNIQUE (email)
-- 定义该约束为唯一性约束，适用于 email 列。
-- 这意味着 email 列中的值必须在整个表中保持唯一，不能有两个相同的电子邮件地址。
```

## 7. 💻 删除已有表中指定字段的 `UNIQUE` 约束

```sql {10-11}
-- 假如已经创建了如下 Employees 表
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    CONSTRAINT unique_email UNIQUE (email)
);

ALTER TABLE Employees
DROP CONSTRAINT unique_email;
-- 该语句删除了名为 `unique_email` 的 `UNIQUE` 约束。
```

# [0016. 主键约束（PRIMARY KEY CONSTRAINT）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0016.%20%E4%B8%BB%E9%94%AE%E7%BA%A6%E6%9D%9F%EF%BC%88PRIMARY%20KEY%20CONSTRAINT%EF%BC%89)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 单字段主键（自增 ID）](#2--单字段主键自增-id)
- [3. 💻 多字段联合主键](#3--多字段联合主键)
- [4. 💻 修改现有表，添加、删除主键](#4--修改现有表添加删除主键)
- [5. 💻 外键关联](#5--外键关联)

<!-- endregion:toc -->

## 1. 📝 概述

- 主键
  - 主键，又称主码，是表中 **一列或多列的组合**，用于唯一标识表中的每条记录。
  - 主键能够唯一地标识表中的一条记录，可以结合外键来定义不同数据表之间的关系，并且可以加快数据库查询的速度。
  - 主键和记录之间的关系如同身份证和人之间的关系，它们之间是 **一一对应** 的。
  - 优先使用 `AUTO_INCREMENT` 整数主键（如 `BIGINT`），兼顾存储效率与扩展性。
- 主键的类型
  - 单字段主键：适用于简单表结构（如用户表用 `user_id` 作为主键）。
  - 多字段联合主键：当单一列无法保证唯一性时使用（如订单明细表需 `订单 ID + 商品 ID` 组合）。

| 类型 | 说明 |
| --- | --- |
| **单字段主键** | 仅使用一列作为主键（如 `id INT PRIMARY KEY`）。 |
| **多字段联合主键** | 通过多个列的组合确保唯一性（如 `PRIMARY KEY (order_id, product_id)`）。 |

- 主键的作用
  - **主键约束（Primary Key Constraint）**
    - **`NOT NULL + UNIQUE`**
    - 主键约束要求主键列的数据唯一，并且不允许为空。
  - **唯一标识记录**：确保每条数据可通过主键精准定位。
  - **支持关系定义**：结合外键（`FOREIGN KEY`）建立跨表关联（如订单表与用户表）。
  - **加速查询**：数据库自动为主键创建索引，大幅提升检索效率。
    - 主键自动创建聚簇索引（InnoDB），`SELECT * FROM users WHERE user_id = 5` 极快
    - 避免用长字符串（如 UUID）作主键，会降低索引效率
- 定义主键的基本语法

| 主键定义方式               | 语法示例                             |
| -------------------------- | ------------------------------------ |
| 在定义列的同时指定主键     | `<字段名> <数据类型> PRIMARY KEY`    |
| 在定义完所有列之后指定主键 | `PRIMARY KEY (<字段名>)`             |
| 多字段联合主键             | `PRIMARY KEY (<字段名1>, <字段名2>)` |

- 最佳实践
  - 主键无业务含义。
  - 优先使用 `AUTO_INCREMENT` 整数主键（如 `BIGINT`），兼顾存储效率与扩展性。
  - 主键一旦定义，其值不可重复且不可修改（如身份证号不会改变）。
  - 设计时应选择 **稳定、无业务含义** 的列（如自增 ID），避免后续变更风险。
  - 业务需求变化通过扩展新字段实现。
  - 主键必须变更时，通过事务保证数据一致性。

## 2. 💻 单字段主键（自增 ID）

```sql {3}
-- 创建用户表（user_id 作为主键）
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,  -- 自增主键
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);

-- 插入数据（主键自动生成）
INSERT INTO users (username, email)
VALUES ('Alice', 'alice@example.com');

-- ❌ 错误示例（主键为空）
INSERT INTO users (user_id, username) VALUES (NULL, 'Bob');
-- 报错：Column 'user_id' cannot be null

-- ❌ 直接更新主键通常被禁止
UPDATE users SET user_id = 100 WHERE username = 'Alice';
-- 这属于危险操作，会破坏数据完整性。
-- 若目标值 100 已存在，将触发 Duplicate entry 错误（违反唯一性约束）。
```

## 3. 💻 多字段联合主键

```sql {6}
-- 创建订单明细表（订单ID+商品ID作为联合主键）
CREATE TABLE order_details (
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT,
    PRIMARY KEY (order_id, product_id)  -- 联合主键
);

-- 插入合法数据
INSERT INTO order_details VALUES (1001, 201, 2);
INSERT INTO order_details VALUES (1001, 202, 1); -- ✅ 允许：不同商品ID

-- 非法插入（违反主键约束）
INSERT INTO order_details VALUES (1001, 201, 3); -- ❌ 错误：重复主键 (1001,201)
```

## 4. 💻 修改现有表，添加、删除主键

```sql
-- 为已存在的员工表添加主键
ALTER TABLE employees
ADD PRIMARY KEY (employee_id);

-- 删除主键约束（谨慎操作！）
ALTER TABLE employees
DROP PRIMARY KEY;
```

## 5. 💻 外键关联

```sql {12}
-- 主表（主键）
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);

-- 从表（外键引用主键）
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)  -- 外键约束
);
```

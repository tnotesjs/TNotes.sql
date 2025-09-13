# [0054. 非空约束（NOT NULL CONSTRAINT）](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0054.%20%E9%9D%9E%E7%A9%BA%E7%BA%A6%E6%9D%9F%EF%BC%88NOT%20NULL%20CONSTRAINT%EF%BC%89)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 📒 非空约束基本语法](#2--非空约束基本语法)
- [3. 💻 非空约束](#3--非空约束)
- [4. 💻 默认值](#4--默认值)

<!-- endregion:toc -->

## 1. 🫧 评价

- 非空约束（Not Null Constraint）
  - 非空约束（`NOT NULL`）是数据库中用于限制字段值不能为空的一种约束。
  - 一旦为某个字段添加了 `NOT NULL` 约束，那么在插入或更新数据时，该字段 **必须包含一个明确的值**，不能为 `NULL`。
  - 对于使用了非空约束的字段，如果用户在添加数据时没有指定值，数据库系统会报错。
  - 一张表可有多个非空约束。
- 作用
  - **确保数据完整性**：防止某些关键字段（如用户名、手机号、订单号等）遗漏数据。
  - **提高数据可靠性**：保证重要信息始终存在，便于后续查询和业务逻辑处理。
- 应用场景
  - 用户注册信息中的必填项（如用户名、密码）
  - 订单表中的下单时间、客户 ID
  - 商品表中的价格、库存数量等关键字段

## 2. 📒 非空约束基本语法

- 约束指定字段为非空，只需要在定义列的时候加上 `NOT NULL` 即可。

```sql {2}
CREATE TABLE 表名 (
    字段名 数据类型 NOT NULL,
    ...
);
```

- 对于已存在的表，可以在修改表结构时添加或删除非空约束。

```sql
-- 修改字段为非空
ALTER TABLE 表名
MODIFY 字段名 数据类型 NOT NULL;
-- 注意：修改已有字段为 `NOT NULL` 时，需确保当前数据中该字段无 `NULL` 值，否则操作会失败。

-- 修改字段允许为空
ALTER TABLE 表名
MODIFY 字段名 数据类型 NULL;
```

## 3. 💻 非空约束

```sql {3,5}
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL,  -- 用户名不能为空
    email VARCHAR(100),             -- 邮箱可以为空
    phone VARCHAR(20) NOT NULL      -- 手机号不能为空
);

-- 插入数据时，`username` 和 `phone` 必须有值；
-- 如果未提供这两个字段的值，数据库会抛出错误：

INSERT INTO users (email) VALUES ('test@example.com');
-- ❌ 错误：Column 'username' cannot be null
```

## 4. 💻 默认值

- `NOT NULL` 约束通常应配合默认值（`DEFAULT`）使用，避免插入失败。

```sql
-- 添加默认值的例子
CREATE TABLE settings (
    user_id INT NOT NULL,
    theme VARCHAR(50) DEFAULT 'light'
);
```

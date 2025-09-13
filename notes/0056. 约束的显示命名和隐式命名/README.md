# [0056. 约束的显示命名和隐式命名](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0056.%20%E7%BA%A6%E6%9D%9F%E7%9A%84%E6%98%BE%E7%A4%BA%E5%91%BD%E5%90%8D%E5%92%8C%E9%9A%90%E5%BC%8F%E5%91%BD%E5%90%8D)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 💻 `UNIQUE` - 显示命名 vs. 隐式命名](#2--unique---显示命名-vs-隐式命名)
- [3. 💻 `FOREIGN KEY` - 显示命名 vs. 隐式命名](#3--foreign-key---显示命名-vs-隐式命名)

<!-- endregion:toc -->

## 1. 🫧 评价

- 显示命名：使用 `CONSTRAINT` 来指定约束的名称。
- 隐式命名：未使用 `CONSTRAINT` 来指定约束的名称。

## 2. 💻 `UNIQUE` - 显示命名 vs. 隐式命名

::: code-group

```sql [显示命名] {6}
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    CONSTRAINT unique_email UNIQUE (email) -- 显示命名
);

-- 去除 email 的唯一性约束
ALTER TABLE Employees
DROP CONSTRAINT unique_email;
-- 该语句删除了名为 `unique_email` 的 `UNIQUE` 约束。
```

```sql [隐式命名] {5}
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE -- 隐式命名
);

-- 去除 email 的唯一性约束
ALTER TABLE Employees
email NULL;
```

:::

| 对比点 | 显示命名 | 隐式命名 |
| --- | --- | --- |
| **`UNIQUE` 约束命名** | 使用了 `CONSTRAINT unique_email UNIQUE (email)` 显式命名约束 | 使用的是隐式命名方式 `email VARCHAR(100) UNIQUE`，没有指定约束名称 |
| **可维护性** | 由于有明确的约束名称 `unique_email`，可以通过名称直接删除或修改该约束，便于维护 | 没有指定名称，删除或修改时需要依赖数据库自动生成的名称，不够直观，且不同数据库行为可能不一致 |
| **语法规范性** | 标准、清晰，推荐用于生产环境 | 简洁但缺乏控制，适用于快速原型开发或测试环境 |
| **删除约束** | 可使用 `ALTER TABLE Employees DROP CONSTRAINT unique_email;` 直接删除 | 需要知道数据库自动生成的约束名才能删除，通常需查询系统表或信息模式 |

- 如果你需要**精确控制和维护约束**，请使用第一组的**显式命名方式**。
- 如果只是**临时测试或原型开发**，可以使用第二组的简洁写法，但要注意其局限性。

## 3. 💻 `FOREIGN KEY` - 显示命名 vs. 隐式命名

::: code-group

```sql [显示命名] {12,17}
-- 创建主表
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- 创建子表并显式命名外键约束
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    CONSTRAINT fk_orders_user FOREIGN KEY (user_id) REFERENCES Users(id)
);

-- 删除外键约束
ALTER TABLE Orders
DROP FOREIGN KEY fk_orders_user;
```

```sql [隐式命名] {10,15,19}
-- 创建主表
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- 创建子表，使用隐式命名方式定义外键
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT REFERENCES Users(id), -- 隐式命名外键
    order_date DATE
);

-- 查询外键名称（必须通过系统表或 SHOW CREATE TABLE）
SHOW CREATE TABLE Orders;

-- 假设查询结果显示外键名为 orders_ibfk_1，则删除如下：
ALTER TABLE Orders
DROP FOREIGN KEY orders_ibfk_1;
```

:::

- **🤔 删除外键约束的时候一定要知道约束名称才能删除吗？是不是必须要先查询名称，然后再删除呢？（背景：如果没有显式指定约束名称）**
  - 🤖 是的，**在没有显式指定外键约束名称的情况下，删除外键约束确实必须先查询其自动生成的名称，然后才能执行删除操作**。
  - MySQL 要求使用 `ALTER TABLE ... DROP FOREIGN KEY fk_name;` 删除外键时，必须明确提供外键约束的名称。
  - 如果你建表时没有使用 `CONSTRAINT` 显式命名外键，MySQL 会自动为其生成一个默认名称（如 表名 `_ibfk_` 序号），例如：`orders_ibfk_1`, `employees_ibfk_2` 等等。这些名称不是固定的，会根据数据库中已有对象动态变化。

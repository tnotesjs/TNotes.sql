# [0056. 约束的显示命名和隐式命名](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0056.%20%E7%BA%A6%E6%9D%9F%E7%9A%84%E6%98%BE%E7%A4%BA%E5%91%BD%E5%90%8D%E5%92%8C%E9%9A%90%E5%BC%8F%E5%91%BD%E5%90%8D)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 显示命名 vs. 隐式命名](#2--显示命名-vs-隐式命名)

<!-- endregion:toc -->

## 1. 📝 概述

- 显示命名：使用 `CONSTRAINT` 来指定约束的名称。
- 隐式命名：未使用 `CONSTRAINT` 来指定约束的名称。

## 2. 💻 显示命名 vs. 隐式命名

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

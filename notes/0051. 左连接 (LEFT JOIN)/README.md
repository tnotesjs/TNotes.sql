# [0051. 左连接 (LEFT JOIN)](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0051.%20%E5%B7%A6%E8%BF%9E%E6%8E%A5%20(LEFT%20JOIN))

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 LEFT JOIN](#2--left-join)

<!-- endregion:toc -->

## 1. 📝 概述

- **左连接（LEFT JOIN）**
  - 左连接（LEFT JOIN），也称为左外连接（LEFT OUTER JOIN），是 SQL 中用于从两个表中提取数据的一种连接方式。
  - 与内连接不同，**左连接会保留左表中的所有记录**，即使右表中没有匹配的行。
  - 通过 `LEFT JOIN`，可以确保不会遗漏左表中的任何一条记录，同时灵活地关联其他表的信息。
- **作用**：`LEFT JOIN` 会根据指定的连接条件将两个表进行连接。
- **结果集**：
  - 包含左表中的所有行。
  - 如果右表中存在匹配的行，则返回匹配的数据。
  - 如果右表中没有匹配的行，则右表对应的列值为 `NULL`。
- **语法结构**

```sql
SELECT 列名列表
FROM 表1
LEFT JOIN 表2
ON 表1.列名 = 表2.列名;
```

- **特点**
  - **保留左表的所有记录**：无论右表是否有匹配的行。
  - **右表无匹配时填充 NULL**：如果右表中没有满足条件的记录，对应字段为 `NULL`。
  - **适合统计分析**：常用于需要展示所有主表数据，并关联其他表信息的场景。
- **使用场景**
  - 获取所有用户及其订单信息（即使某些用户没有订单）。
  - 查询商品目录和库存信息，显示所有商品，包括缺货商品。
  - 数据完整性检查：找出在右表中缺失的数据。

## 2. 💻 LEFT JOIN

```sql {4-6,23-25}
-- 表 `Customers`
-- | CustomerID | CustomerName |
-- | ---------- | ------------ |
-- | 1          | Alice        |
-- | 2          | Bob          |
-- | 3          | Charlie      |

-- 表 `Orders`
-- | OrderID | CustomerID | OrderAmount |
-- | ------- | ---------- | ----------- |
-- | 101     | 1          | 100         |
-- | 102     | 2          | 200         |
-- | 103     | 4          | 150         |

SELECT Customers.CustomerName, Orders.OrderAmount
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;

-- 结果集：
-- | CustomerName | OrderAmount |
-- | ------------ | ----------- |
-- | Alice        | 100         |
-- | Bob          | 200         |
-- | Charlie      | NULL        |

-- 左表 `Customers` 的所有客户都会被返回，即使他们没有订单（如 `Charlie`）。
-- 对于没有订单的客户，`OrderAmount` 显示为 `NULL`。
```

# [0052. 右连接 (RIGHT JOIN)](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0052.%20%E5%8F%B3%E8%BF%9E%E6%8E%A5%20(RIGHT%20JOIN))

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 RIGHT JOIN](#2--right-join)

<!-- endregion:toc -->

## 1. 📝 概述

- **右连接（RIGHT JOIN）**
  - 右连接（RIGHT JOIN），也称为右外连接（RIGHT OUTER JOIN），是 SQL 中的一种连接操作，它与左连接（LEFT JOIN）相对。
  - **右连接保留右表中的所有记录**，即使左表中没有匹配的行。如果左表中没有匹配的记录，则对应的列值为 `NULL`。
  - 通过 `RIGHT JOIN`，可以确保不会遗漏右表中的任何一条记录，同时灵活地关联其他表的信息。
- **作用**：根据指定的连接条件将两个表进行连接。
- **结果集**：
  - 包含右表中的所有行。
  - 如果左表中有匹配的行，则返回匹配的数据。
  - 如果左表中没有匹配的行，则左表对应的列值为 `NULL`。
- **语法结构**

```sql
SELECT 列名列表
FROM 表1
RIGHT JOIN 表2
ON 表1.列名 = 表2.列名;
```

- **特点**
  - **保留右表的所有记录**：无论左表是否有匹配的行。
  - **左表无匹配时填充 NULL**：如果左表中没有满足条件的记录，对应字段为 `NULL`。
  - **适合反向统计分析**：常用于需要展示所有从表数据，并关联主表信息的场景。
- **使用场景**
  - 获取所有订单及其用户信息（即使某些订单的用户不存在于用户表中）。
  - 查询日志记录及其关联的用户行为数据（确保所有日志都被保留）。
  - 数据一致性检查：找出在左表中缺失的关联数据。

## 2. 💻 RIGHT JOIN

```sql {11-13,23-25}
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
RIGHT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;

-- 结果集：
-- | CustomerName | OrderAmount |
-- | ------------ | ----------- |
-- | Alice        | 100         |
-- | Bob          | 200         |
-- | NULL         | 150         |

-- 右表 `Orders` 的所有订单都会被返回，即使某些订单的客户在 `Customers` 表中不存在（如 `CustomerID=4`）。
-- 对于这些订单，`CustomerName` 显示为 `NULL`。
```

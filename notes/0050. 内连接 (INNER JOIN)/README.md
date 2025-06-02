# [0050. 内连接 (INNER JOIN)](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0050.%20%E5%86%85%E8%BF%9E%E6%8E%A5%20(INNER%20JOIN))

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 内连接](#2--内连接)

<!-- endregion:toc -->

## 1. 📝 概述

- **内连接（INNER JOIN）**
  - 内连接（INNER JOIN）是 SQL 中最常用的一种连接操作，它用于从两个或多个表中提取 **满足连接条件的匹配行**。
  - 只有当连接的表之间存在匹配的数据时，这些行才会出现在结果集中。
- **作用**
  - `INNER JOIN` 会根据指定的连接条件，将两个表中的数据进行匹配。
- **结果集**
  - 只包含那些在连接的两个表中都有匹配值的记录。
- **等值连接**
  - 内连接也被称为“等值连接”（如果使用等号作为连接条件）。
- **语法结构**

```sql
SELECT 列名列表
FROM 表1
INNER JOIN 表2
ON 表1.列名 = 表2.列名;
```

- **特点**
  - **仅返回匹配的行**：如果某一行在其中一个表中没有对应的匹配项，则不会出现在结果中。
  - **避免 NULL 值干扰**：因为不包含未匹配的行，所以在处理时不需要考虑 NULL 值。
  - **性能较好**：由于只返回匹配的记录，通常比外连接更高效。
- **使用场景**
  - 当你只需要获取两个表中 **有对应关系的数据** 时。
  - 例如：
    - 获取用户及其订单信息。
    - 查询产品与库存的对应关系。
    - 分析日志与用户行为的关联数据。

## 2. 💻 内连接

```sql {4,5,11,12,24,25}
-- 顾客表 `Customers`
-- | CustomerID | CustomerName |
-- | ---------- | ------------ |
-- | 1          | Alice        |
-- | 2          | Bob          |
-- | 3          | Charlie      |

-- 订单表 `Orders`
-- | OrderID | CustomerID | OrderAmount |
-- | ------- | ---------- | ----------- |
-- | 101     | 1          | 100         |
-- | 102     | 2          | 200         |
-- | 103     | 4          | 150         |

SELECT Customers.CustomerName, Orders.OrderAmount
FROM Customers
INNER JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;

-- 结果集：

-- | CustomerName | OrderAmount |
-- | ------------ | ----------- |
-- | Alice        | 100         |
-- | Bob          | 200         |

-- 只有 `CustomerID` 在两个表中都存在的记录才会被返回。
-- 上述的顾客 `Charlie` 没有匹配到对应的订单，所以不会出现在结果中；
-- 上述的订单 `OrderID=103` 没有匹配到对应的客户，因此也不会被包含在结果集中；
```

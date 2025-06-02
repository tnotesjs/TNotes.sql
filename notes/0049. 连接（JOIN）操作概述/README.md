# [0049. 连接（JOIN）操作概述](https://github.com/Tdahuyou/TNotes.sql/tree/main/notes/0049.%20%E8%BF%9E%E6%8E%A5%EF%BC%88JOIN%EF%BC%89%E6%93%8D%E4%BD%9C%E6%A6%82%E8%BF%B0)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)

<!-- endregion:toc -->

## 1. 📝 概述

- 在 SQL 中，连接（JOIN）操作用于 **根据某些条件将多个表中的行组合在一起**。
- 常见的连接类型包括 **左连接（LEFT JOIN）**、**右连接（RIGHT JOIN）** 和 **内连接（INNER JOIN）**。

| 连接类型     | 返回的结果                  |
| ------------ | --------------------------- |
| `INNER JOIN` | 两个表中匹配的行            |
| `LEFT JOIN`  | 左表所有行，右表匹配或 NULL |
| `RIGHT JOIN` | 右表所有行，左表匹配或 NULL |

::: code-group

```sql [INNER JOIN]
-- 只返回两个表中满足连接条件的匹配行。
-- 如果某一行在其中一个表中没有匹配，则不会出现在结果中。

SELECT *
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

```sql [LEFT JOIN]
-- 返回左表中的所有行，即使右表中没有匹配的行。
-- 左表的所有记录都会被保留。
-- 对于右表中没有匹配的情况，结果中右表的列值为 NULL。

SELECT *
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

```sql [RIGHT JOIN]
-- 返回右表中的所有行，即使左表中没有匹配的行。
-- 右表的所有记录都会被保留。
-- 对于左表中没有匹配的情况，结果中左表的列值为 NULL。

SELECT *
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

:::

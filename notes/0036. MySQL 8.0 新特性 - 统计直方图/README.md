# [0036. MySQL 8.0 新特性 - 统计直方图](https://github.com/Tdahuyou/TNotes.mysql/tree/main/notes/0036.%20MySQL%208.0%20%E6%96%B0%E7%89%B9%E6%80%A7%20-%20%E7%BB%9F%E8%AE%A1%E7%9B%B4%E6%96%B9%E5%9B%BE)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 📒 什么是窗口函数？](#2--什么是窗口函数)
- [3. 📒 窗口函数的基本语法](#3--窗口函数的基本语法)
- [4. 📒 窗口函数与聚合函数的区别](#4--窗口函数与聚合函数的区别)
- [5. 📒 常见窗口函数分类](#5--常见窗口函数分类)
  - [5.1. ✅ 排名类函数](#51--排名类函数)
  - [5.2. ✅ 分布类函数](#52--分布类函数)
  - [5.3. ✅ 聚合类函数（配合 OVER 使用）](#53--聚合类函数配合-over-使用)
- [6. 📒 示例说明（排名类）](#6--示例说明排名类)
- [7. 📒 示例说明（聚合类）](#7--示例说明聚合类)
- [8. 📊 功能对比表（MySQL 5.x vs 8.0）](#8--功能对比表mysql-5x-vs-80)
- [9. ✅ 总结一句话：](#9--总结一句话)

<!-- endregion:toc -->

## 1. 📝 概述

- **MySQL 8.0 引入了对窗口函数（Window Functions）的支持**，这是其在分析型查询能力上的重大增强。
- 窗口函数类似于传统的聚合函数（如 `SUM()`、`COUNT()`），但不会将多行数据合并为一行，而是**为每一行返回一个计算结果**。
- 这使得我们可以在不丢失原始行的前提下进行分组统计、排名、累计等操作。

## 2. 📒 什么是窗口函数？

- **窗口函数（Window Function）**
  - 是一种可以对“一组相关行”进行计算的函数
  - 在不改变原始行结构的前提下，为每行返回一个基于该“窗口”的计算值
- 相比传统聚合函数：
  - 聚合函数会把多行合并成一行
  - 窗口函数保留原始行，同时增加一列或多列用于展示计算结果

## 3. 📒 窗口函数的基本语法

```sql
function_name(...) OVER (
    [PARTITION BY partition_expression]
    [ORDER BY sort_expression]
    [window_frame_clause]
)
```

- `function_name(...)`：窗口函数名，如 `ROW_NUMBER()`, `RANK()`, `SUM()`
- `OVER(...)`：定义窗口范围
- `PARTITION BY`：类似 `GROUP BY`，用于划分窗口的数据集
- `ORDER BY`：指定窗口内数据排序方式
- `window_frame_clause`：定义窗口帧范围（可选）

## 4. 📒 窗口函数与聚合函数的区别

| 特性 | 聚合函数（如 SUM, COUNT） | 窗口函数 |
| --- | --- | --- |
| 输出行数 | 多行 → 合并为一行 | 多行 → 每行都输出一个结果 |
| 是否保留原数据行 | ❌ 不保留 | ✅ 保留 |
| 是否支持排序和窗口范围 | ❌ 不支持 | ✅ 支持 |
| 是否能结合 RANK 排名使用 | ❌ 不支持 | ✅ 支持 |

## 5. 📒 常见窗口函数分类

### 5.1. ✅ 排名类函数

- `ROW_NUMBER()`：为每一行分配唯一序号，无并列
- `RANK()`：有并列，后续序号跳跃
- `DENSE_RANK()`：有并列，序号连续
- `NTILE(n)`：将数据分为 n 个桶，并返回桶编号

### 5.2. ✅ 分布类函数

- `PERCENT_RANK()`：返回当前行在其分区中的相对排名
- `CUME_DIST()`：返回当前行在分区中累积分布比例

### 5.3. ✅ 聚合类函数（配合 OVER 使用）

- `SUM(...) OVER (...)`
- `AVG(...) OVER (...)`
- `MIN(...) OVER (...)`
- `MAX(...) OVER (...)`
- `COUNT(...) OVER (...)`

## 6. 📒 示例说明（排名类）

```sql
-- 查询员工薪资，并按部门分组，显示每个员工在部门内的薪资排名
SELECT
    department,
    name,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank
FROM employees;
```

> ✅ 可以清晰地看到每位员工在部门内的排名情况。

## 7. 📒 示例说明（聚合类）

```sql
-- 查询每个员工的工资及其所在部门的平均工资
SELECT
    department,
    name,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS avg_salary
FROM employees;
```

```sql
-- 查询员工薪资及前 N 行的累计总和（滑动窗口）
SELECT
    id,
    salary,
    SUM(salary) OVER (ORDER BY id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM sales;
```

> ✅ 可实现动态汇总、趋势分析、移动平均等复杂分析逻辑。

## 8. 📊 功能对比表（MySQL 5.x vs 8.0）

| 功能 | MySQL 5.x | MySQL 8.0 |
| --- | --- | --- |
| 支持窗口函数 | ❌ 不支持 | ✅ 支持 |
| 实现排名功能 | ❌ 需借助变量或子查询 | ✅ 支持 `ROW_NUMBER`, `RANK`, `DENSE_RANK` |
| 实现滑动窗口统计 | ❌ 需手动模拟 | ✅ 支持 `ROWS BETWEEN ...` 定义窗口帧 |
| 实现分组统计而不合并行 | ❌ 不支持 | ✅ 支持 `OVER(PARTITION BY...)` |
| 分析型查询能力 | ⚠️ 较弱 | ✅ 强大且灵活 |

## 9. ✅ 总结一句话：

> MySQL 8.0 引入窗口函数后，极大增强了数据库的分析能力，使你可以在不破坏原始数据行的情况下，完成复杂的排名、累计、分布等分析任务，是 OLAP 场景下不可或缺的强大工具。

如果你想了解如何结合 CTE 和窗口函数做更复杂的分析，或者想看实际业务场景案例，欢迎继续提问！

<!-- 统计直方图

- MySQL 8.0 实现了统计直方图。利用直方图，用户可以对一张表的一列做数据分布的统计，特别是针对没有索引的字段。这可以帮助查询优化器找到更优的执行计划。

统计直方图（Histogram Statistics）

- **用途**：帮助优化器更准确评估查询代价
- **应用场景**：对无索引字段进行分布统计
- **创建方式**：

```sql
ANALYZE TABLE table_name UPDATE HISTOGRAM ON column_name;
``` -->

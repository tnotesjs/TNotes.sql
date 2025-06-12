### 唯一性约束（UNIQUE Constraint）介绍【0055】

#### 1. **定义**

唯一性约束（`UNIQUE`）用于确保某列或多个列的组合值在表中是唯一的，即不允许出现重复值。与 `PRIMARY KEY` 不同的是，`UNIQUE` 约束允许字段值为 `NULL`，但只能有一个 `NULL` 值。

---

#### 2. **作用**

- **保证数据唯一性**：如用户名、邮箱、手机号等字段要求不重复。
- **辅助主键机制**：当主键不是唯一标识时，可以使用唯一约束来增强数据完整性。
- **支持索引创建**：数据库会自动为设置了 `UNIQUE` 的列创建唯一性索引。

---

#### 3. **基本语法**

```sql
CREATE TABLE 表名 (
    字段名 数据类型 UNIQUE,
    ...
);
```

也可以为多列设置联合唯一约束：

```sql
CREATE TABLE 表名 (
    字段1 数据类型,
    字段2 数据类型,
    UNIQUE (字段1, 字段2)
);
```

修改已有表添加或删除唯一约束：

```sql
-- 添加唯一约束
ALTER TABLE 表名
ADD UNIQUE (字段名);

-- 添加多字段联合唯一约束
ALTER TABLE 表名
ADD UNIQUE (字段1, 字段2);

-- 删除唯一约束
ALTER TABLE 表名
DROP INDEX 索引名;  -- 需要知道该唯一约束对应的索引名
```

---

#### 4. **示例说明**

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,  -- 用户名必须唯一且非空
    email VARCHAR(100) UNIQUE              -- 邮箱可为空，但若存在则必须唯一
);
```

尝试插入重复值将导致错误：

```sql
INSERT INTO users (username, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (username, email) VALUES ('Alice', 'alice2@example.com');
-- 错误：Duplicate entry 'Alice' for key 'username'
```

---

#### 5. **与主键的区别**

| 特性             | 主键（PRIMARY KEY） | 唯一性约束（UNIQUE） |
| ---------------- | ------------------- | -------------------- |
| 是否允许 NULL    | 不允许              | 允许一个 NULL        |
| 一张表有几个     | 仅一个              | 多个                 |
| 是否自动创建索引 | 是                  | 是                   |
| 是否可重复       | 否                  | 否                   |

---

#### 6. **应用场景**

- 用户名、邮箱、手机号等唯一标识字段
- 订单编号、身份证号、车牌号等业务唯一字段
- 联合唯一约束用于复合业务场景（如：学生 + 课程 的唯一性）

---

#### 7. **注意事项**

- 唯一约束对大小写是否敏感取决于数据库及字符集配置（如 MySQL 默认不区分大小写）。
- 使用联合唯一约束时应合理设计字段顺序。
- 删除唯一约束需通过 `DROP INDEX` 操作，而非直接删除约束语句。

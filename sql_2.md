# SQL查询的执行顺序

1. **FROM子句**：这是SQL查询的起点，数据库系统首先读取FROM子句中指定的表，如果有多个表，则进行笛卡尔积操作生成虚拟表VT1。
2. ON子句：接着，数据库系统会对虚拟表VT1应用ON子句中的条件，筛选出满足条件的行，生成虚拟表VT2。
3. JOIN操作：如果指定了JOIN类型（如INNER JOIN、LEFT JOIN等），系统会根据JOIN条件将相关表的行合并到虚拟表VT2中。
4. **WHERE子句**：然后，系统会在虚拟表VT2上应用WHERE子句中的过滤条件，生成虚拟表VT3。
5. **GROUP BY子句**：如果有GROUP BY子句，系统会根据指定的列对VT3中的行进行分组，生成虚拟表VT4。
6. 聚合函数：此时，系统会计算聚合函数（如COUNT、SUM、AVG等），并将结果添加到虚拟表VT4中。
7. HAVING子句：如果有HAVING子句，系统会在VT4上应用HAVING条件，筛选出满足条件的分组，生成虚拟表VT5。
8. **SELECT子句**：系统会根据SELECT子句中指定的列或表达式，从VT5中选择数据，生成虚拟表VT6。
9. DISTINCT关键字：如果使用了DISTINCT关键字，系统会从VT6中移除重复的行，生成虚拟表VT7。
10. **ORDER BY子句**：系统会根据ORDER BY子句中的条件对VT7中的行进行排序，生成最终的查询结果。
11. LIMIT子句：最后，如果有LIMIT子句，系统会从排序后的结果中选择指定数量的行返回。

示例

```sql
SELECT 学生姓名, AVG(数学成绩) AS 数学平均成绩
FROM 学生成绩表
GROUP BY 学生姓名
HAVING 数学平均成绩 > 75
ORDER BY 数学平均成绩 DESC
LIMIT 3;
```

在这个查询中，数据库系统首先从"学生成绩表"中读取数据（FROM），然后对每个学生的数学成绩进行平均计算（AVG），接着筛选出平均成绩大于75的学生（HAVING），最后按照数学平均成绩降序排序并返回前三名学生的姓名和成绩（ORDER BY和LIMIT）。

注意事项

在这个查询中，数据库系统首先从"学生成绩表"中读取数据（FROM），然后对每个学生的数学成绩进行平均计算（AVG），接着筛选出平均成绩大于75的学生（HAVING），最后按照数学平均成绩降序排序并返回前三名学生的姓名和成绩（ORDER BY和LIMIT）.

- **WHERE子句**不能使用聚合函数，因为它在数据分组之前执行。
- **SELECT子句**中的列别名可以在ORDER BY和HAVING子句中使用，但不能在WHERE子句中使用。
- **GROUP BY**后的查询结果每组只包含一行，因此不需要再使用DISTINCT进行去重。

# 笛卡尔积相关

![img](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)

| 类型                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| **INNER JOIN**      | 返回两个表中满足连接条件的记录（交集）。                     |
| **LEFT JOIN**       | 返回左表中的所有记录，即使右表中没有匹配的记录（保留左表）。 |
| **RIGHT JOIN**      | 返回右表中的所有记录，即使左表中没有匹配的记录（保留右表）。 |
| **FULL OUTER JOIN** | 返回两个表的并集，包含匹配和不匹配的记录。                   |
| **CROSS JOIN**      | 返回两个表的笛卡尔积，每条左表记录与每条右表记录进行组合。   |
| **SELF JOIN**       | 将一个表与自身连接。                                         |
| **NATURAL JOIN**    | 基于同名字段自动匹配连接的表。                               |

---

## 两个表格有相同列名的JOIN操作详解

当两个表格有相同列名时，我们需要特别注意列名的引用方式。下面我用具体的例子来演示这种情况下的各种JOIN操作。

### 1. 创建示例数据

创建两个都有`id`和`name`列的表格：

#### Users表

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);

INSERT INTO Users VALUES 
(1, 'Alice', 25),
(2, 'Bob', 30),
(3, 'Charlie', 28),
(4, 'David', 35);
```

#### Orders表

```sql
CREATE TABLE Orders (
    id INT PRIMARY KEY,
    user_id INT,
    name VARCHAR(50),  -- 与Users表有相同列名
    amount DECIMAL(10,2)
);

INSERT INTO Orders VALUES 
(1, 1, 'Laptop Order', 999.99),
(2, 2, 'Phone Order', 599.99),
(3, 1, 'Tablet Order', 399.99),
(4, 3, 'Monitor Order', 299.99),
(5, 5, 'Keyboard Order', 79.99);  -- user_id=5在Users表中不存在
```

### 2. 各表数据展示

#### Users表

| id   | name    | age  |
| ---- | ------- | ---- |
| 1    | Alice   | 25   |
| 2    | Bob     | 30   |
| 3    | Charlie | 28   |
| 4    | David   | 35   |

#### Orders表

| id   | user_id | name           | amount |
| ---- | ------- | -------------- | ------ |
| 1    | 1       | Laptop Order   | 999.99 |
| 2    | 2       | Phone Order    | 599.99 |
| 3    | 1       | Tablet Order   | 399.99 |
| 4    | 3       | Monitor Order  | 299.99 |
| 5    | 5       | Keyboard Order | 79.99  |

### 3. 内连接（INNER JOIN）

当有相同列名时，必须使用表别名来区分：

```sql
SELECT u.id AS user_id, u.name AS user_name, u.age,
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u
INNER JOIN Orders o ON u.id = o.user_id;
```

**结果表格：**
| user_id | user_name | age  | order_id | order_name    | amount |
| ------- | --------- | ---- | -------- | ------------- | ------ |
| 1       | Alice     | 25   | 1        | Laptop Order  | 999.99 |
| 1       | Alice     | 25   | 3        | Tablet Order  | 399.99 |
| 2       | Bob       | 30   | 2        | Phone Order   | 599.99 |
| 3       | Charlie   | 28   | 4        | Monitor Order | 299.99 |

**特点：**
- 只显示有订单的用户
- David没有订单，所以不显示
- user_id=5的订单没有对应的用户，所以不显示

### 4. 左连接（LEFT JOIN）

```sql
SELECT u.id AS user_id, u.name AS user_name, u.age,
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u
LEFT JOIN Orders o ON u.id = o.user_id;
```

**结果表格：**
| user_id | user_name | age  | order_id | order_name    | amount |
| ------- | --------- | ---- | -------- | ------------- | ------ |
| 1       | Alice     | 25   | 1        | Laptop Order  | 999.99 |
| 1       | Alice     | 25   | 3        | Tablet Order  | 399.99 |
| 2       | Bob       | 30   | 2        | Phone Order   | 599.99 |
| 3       | Charlie   | 28   | 4        | Monitor Order | 299.99 |
| 4       | David     | 35   | NULL     | NULL          | NULL   |

**特点：**
- 所有用户都显示，包括没有订单的David
- David的订单信息为NULL

### 5. 右连接（RIGHT JOIN）

```sql
SELECT u.id AS user_id, u.name AS user_name, u.age,
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u
RIGHT JOIN Orders o ON u.id = o.user_id;
```

**结果表格：**

| user_id | user_name | age  | order_id | order_name     | amount |
| ------- | --------- | ---- | -------- | -------------- | ------ |
| 1       | Alice     | 25   | 1        | Laptop Order   | 999.99 |
| 2       | Bob       | 30   | 2        | Phone Order    | 599.99 |
| 1       | Alice     | 25   | 3        | Tablet Order   | 399.99 |
| 3       | Charlie   | 28   | 4        | Monitor Order  | 299.99 |
| NULL    | NULL      | NULL | 5        | Keyboard Order | 79.99  |

**特点：**
- 所有订单都显示，包括没有对应用户的订单
- user_id=5的订单没有对应用户，用户信息为NULL

### 6. 全外连接（FULL OUTER JOIN）

MySQL中模拟全外连接：

```sql
SELECT u.id AS user_id, u.name AS user_name, u.age,
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u
LEFT JOIN Orders o ON u.id = o.user_id
UNION
SELECT u.id AS user_id, u.name AS user_name, u.age,
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u
RIGHT JOIN Orders o ON u.id = o.user_id;
```

**结果表格：**
| user_id | user_name | age  | order_id | order_name     | amount |
| ------- | --------- | ---- | -------- | -------------- | ------ |
| 1       | Alice     | 25   | 1        | Laptop Order   | 999.99 |
| 1       | Alice     | 25   | 3        | Tablet Order   | 399.99 |
| 2       | Bob       | 30   | 2        | Phone Order    | 599.99 |
| 3       | Charlie   | 28   | 4        | Monitor Order  | 299.99 |
| 4       | David     | 35   | NULL     | NULL           | NULL   |
| NULL    | NULL      | NULL | 5        | Keyboard Order | 79.99  |

**特点：**
- 所有用户和所有订单都显示
- 不匹配的地方用NULL填充

### 7. 笛卡尔积（CROSS JOIN）

```sql
SELECT u.id AS user_id, u.name AS user_name,
       o.id AS order_id, o.name AS order_name
FROM Users u
CROSS JOIN Orders o;
```

**结果表格（部分）：**
| user_id | user_name | order_id | order_name     |
| ------- | --------- | -------- | -------------- |
| 1       | Alice     | 1        | Laptop Order   |
| 1       | Alice     | 2        | Phone Order    |
| 1       | Alice     | 3        | Tablet Order   |
| 1       | Alice     | 4        | Monitor Order  |
| 1       | Alice     | 5        | Keyboard Order |
| 2       | Bob       | 1        | Laptop Order   |
| 2       | Bob       | 2        | Phone Order    |
| ...     | ...       | ...      | ...            |

**特点：**
- 行数 = 4 × 5 = 20行
- 每个用户与每个订单都进行了组合

### 8. 使用USING子句简化相同列名的JOIN

当JOIN条件基于相同列名时，可以使用USING子句简化：

```sql
-- 注意：这里假设Orders表的主键列名也是id，但实际是user_id
-- 为了演示USING，我们创建一个临时场景

CREATE TABLE Users2 (id INT, name VARCHAR(50));
CREATE TABLE Contacts (id INT, user_id INT, phone VARCHAR(15));

-- 使用USING的示例（当两个表有完全相同的列名用于JOIN时）
SELECT * FROM table1 JOIN table2 USING (common_column);
```

### 重要注意事项

1. **必须使用表别名**：当两个表有相同列名时，必须使用表别名或表名来限定列名
2. **使用有意义的别名**：给列起有意义的别名，提高查询结果的可读性
3. **SELECT * 的问题**：避免在JOIN查询中使用`SELECT *`，因为它会返回重复的列名，导致歧义

```sql
-- 错误的写法（会产生歧义）
SELECT * FROM Users u JOIN Orders o ON u.id = o.user_id;

-- 正确的写法
SELECT u.id AS user_id, u.name AS user_name, 
       o.id AS order_id, o.name AS order_name, o.amount
FROM Users u 
JOIN Orders o ON u.id = o.user_id;
```

## 总结

当两个表格有相同列名时，关键是要：
- 使用表别名明确指定列所属的表
- 为结果集中的列起有意义的别名
- 避免使用`SELECT *`，而是明确列出需要的列

这样可以确保查询结果清晰明确，避免列名冲突带来的问题。
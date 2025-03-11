---
title: MySQL基础知识
date: 2025-03-11 18:56:55
tags:
---

## 第二章: 查询语句

参考链接：[MySQL LIKE 用法与实例](https://www.sjkjc.com/mysql/like/)

### 1.**the  SELECT**

- 后跟表中的列，多个列用逗号隔开；
- 检查所有列，使用*;
- 分号 ； 表示语句的结束。

将SQL关键字大写是一个好的习惯。但是，SQL是不区分到小写的。

```
SELECT columns_list
FROM table_name;
```



### 2.**WHERE 子句**

WHERE 子句允许允许您为  SELECT   指定查询的条件。

语法如下：

```
SELECT
    *
FROM
    actor
WHERE
    last_name = 'ALLEN'

```



### 3.**AND、OR、NOT 运算符**

- AND:  双目逻辑运算符，只有当两个操作数都为真时，结果才返回真，否则返回假或者 `NULL`。   运算优先级高于OR

  ```
  a AND b
  ```

- OR: 双目逻辑运算符，两个操作数至少有一个为TRUE(1),结果都返回TRUE；如果一个操作数为null，另一个为0,则运算的结果也为NULL。

  ```
  SELECT 0 OR 0;
  ```

- NOT：逻辑运算符，表示执行否定操作。



### 4.IN运算符

- 在mysql中通过IN 运算符来判断一个值是否包含在一个子列表中。

  ```
  name IN ('Alice', 'Tim', 'Jack')
  相当于
  name = 'Alice' OR name = 'Tim' OR name = 'Jack'
  ```

  In运算符左侧的操作数的值是右侧操作数集合中的一个时，返回1。否则，返回0。



### 5.**BETWEEN 运算符**

- BETWEEN运算符确定一个值是否介于两个值之间。

```
expression BETWEEN min AND max
expression NOT BETWEEN min AND max
```



### 6.**LIKE运算符**

-  `LIKE` 运算符可以根据指定的模式过滤数据。`LIKE` 运算符一般用于模糊匹配字符数据

```
expression LIKE pattern
```

- %	匹配零个或多个任意字符
- _          匹配单个任意字符
- 如果需要匹配通配符，则需要使用  \  转义字符，如 \% 或者 \_。

```
以下 SQL 语句使用 LIKE 运算符匹配 first_name 以字符 P 开头的所有演员。比如： PARKER。

SELECT * FROM actor WHERE first_name LIKE 'P%';
```

### 7.**REGEXP运算符**

REGEXP是用于匹配正则表达式的操作符。它允许你基于正则表达式的规则对字符串进行模式匹配。

```
SELECT column_name
FROM table_name
WHERE column_name REGEXP 'pattern';

colum_name 是要匹配的字段   pattern 是要匹配的正则表达式
```

- REGEXP区分大小写



### 8.**IS NULL 运算符**

测试一个值是否为 NULL

```
expression IS NULL
expression IS NOT NULL

expression可以是一个字段名，也可以是一个表达式 或 值。IS NOT NULL 是 IS NULL 的否定写法
```

实例:

检查staff表中的 password 是否为空的演示

```
SELECT
    first_name, last_name, password
FROM
    staff
WHERE
    password IS NULL;
```



### 9.**Order by子句**

使用ORDER BY 子句来排序 SELECT 语句的结果集。

语法：

```
SELECT
   column1, column2, ...
FROM
   table_name
[WHERE clause]
ORDER BY
   column1 [ASC|DESC],
   column2 [ASC|DESC],
   ...;

可以指定一个字段或多个字段。ASC 表示升序，DESC表示降序.
```

实例;

使用 ORDER BY 子句按演员姓氏升序排列

```
SELECT
    actor_id, first_name, last_name
FROM
    actor
ORDER BY last_name;
```



### 10.**LIMIT 子句**

在mysql中，我们使用  LIMIT 来限定数据返回的的行数。

语法格式:

```
LIMIT [offset,] row_count;


offect 是偏移量的意思，可以理解为在返回的结果集上跳过的行数。
row_count  返回的行数。
```

示例:

返回film表中等级为 'G' 的片长最长的10 部影片。

```
SELECT *
FROM FILM
WHERE RATING = 'G'
ORDER BY LENGTH 
LIMIT 10
```



### **11.EXISTS**

是一个单目操作符，通常需要一个子句作为参数。常写在WHER子句中。

语法:

```
SELECT column_name
FROM table_name
WHERE EXISTS(subquery);

subquery 参数
```

示例：

查询 language 表的一些语种，该语种在 film 中存在相关语种的影片。

```
SELECT * 
FROM LANGUAGE
WHER EXIXTS(
	ESLECT * 
	FROM FILM
	WHERE flim.language_id = language.language_id
	);
```



### 12.**DISTINCT**

​	在SELECT的结果集中，可能会出现很多的重复行，可以使用DISTINCT去除重复行。

​	语法：

```
SELECT DISTINCT
	COLUMS_LIST
FROM TABLE_NAME

COLUMS_LIST:要查询的字段列表
```

示例：

姓氏重复是很正常的现象，我们使用DISTINCT 去除返回记过中重复的姓氏。

```
SELECT DISTINCT
	LAST_NAME
FROM ACTOR;
```



## 第三章：多表查询

### **1.内连接,在多张表格中检索数据。**

示例：

将 order_item表和 product 表连接，每笔订单都返回产品 id 和 名字，连同order_item表的数量和单价。

```
SELECT order_id,oi.product_id,p.product_id,quantity,unit_price
FROM order_item oi
JOIN product p ON oi.product_id = p.product_id;
```



### **2.跨数据库连接**

在当前的数据库中是用其他数据库中的表时，需要做数据库连接。

示例：

在 sql_inventory 数据库中连接 sql_store 数据库中的中的 order_item表

```
USE sql_inventory;

SELECT * 
FROM order_item oi
JOIN sql_inventory.products p 
ON oi.product_id = p.product_id;
```



### **3.自连接**

mysql中使用表连接自己，以重复获得表中的数据，主要用于表中有层级关系的字段，例如 -员工和领导是不同的两个字段。

语法;

```
SELECT a.column_name,b.column_name
FROM table_name a
JOIN table_name b
ON a.common_column = b.common_column;

a 和 b 是同一张表的不同别名。
common_column 是两个表(实际上是同一个表)。
```



### **4.多表连接**

使用关键字 JOIN 将多个表关联在一起。

基本语法：

```
SELECT columns
FROM table1
JOIN table2 ON table1.column_name = table2.column_name
JOIN table3 ON table2.column_name = table3.column_name
...
WHERE condition;
```

- ON:指定连接条件，即两个表是如何匹配的。
- WHERE: 可以进一步过滤结果



### **5.复合连接条件**

在mysql的操作中，通过两个或多个连接条件来连接两个或多个表。

基本语法：

```
SELECT columns
FROM table1
JOIN table2 
ON (condition1 AND condition2)
```



### **6.隐式连接语法**

在mysql中，隐式连接是指没有使用 JOIN 关键字来进行表的连接,而是在 WHER 语句中使用指定的连接条件来实现表的连接。

基本语法：

```
SELECT columns
FROM table1,table2
WHERE table1.column_name = table2.column_name;
```

在这个查询中 FROM table1,table2 会从 table1 和 table2中选择数据，而WHERE子句则是指定了连接条件。



### **7.外连接**

外连接是一种返回匹配两个表中的记录，并且还会返回一个表中没有匹配记录的行。主要分为两种类类型

- 左外连接 (LEFT JOIN)
- 右外连接 (RIGHT JOIN)

左外连接返回的是左表中所有的行，以及右表中所有的行。如果右表中没有与左便匹配的记录,则记录为NULL.

基本语法：

```
SELECT column
FROM table1
LEFT JOIN table2 
ON table1.column_name = table2.column_name;
```



### **8.多表外连接**

- 类似于多表内连接;
- **注意**：使用多个表进行外连接时，尽量使左外连接，方便理解意图。

```
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name
LEFT JOIN table3 ON table2.column_name = table3.column_name;
```



### **9.自外连接**

自外连接是指自己与自己进行外部连接，自外连接确保返回表中所有的记录。及即他们在连接下没有匹配的项目。

- 左自外连接
- 右自外连接

语法：

```
SELECT a.column1, a.column2, b.column1, b.column2
FROM table_name a
LEFT JOIN table_name b ON a.column = b.column;

左自外连接
```

```
SELECT a.column1, a.column2, b.column1, b.column2
FROM table_name a
RIGHT JOIN table_name b ON a.column = b.column;


右自外连接
```

示例:

**示例 1：员工与上级的关系（左自外连接）**

假设有一个 `employees` 表，记录了员工的信息。`employees` 表中有一个 `manager_id` 字段，表示该员工的上级是哪个员工。员工表结构如下：

| employee_id | name    | manager_id |
| ----------- | ------- | ---------- |
| 1           | Alice   | NULL       |
| 2           | Bob     | 1          |
| 3           | Charlie | 1          |
| 4           | David   | 2          |
| 5           | Eve     | 2          |

在这个表中，`manager_id` 是指向同一表的 `employee_id`，表示某个员工的上级。我们想查询所有员工以及他们的上级信息（如果有的话）。我们可以使用**左自外连接**，确保即使员工没有上级（如 Alice），也能在结果中显示。

查询：

```
SELECT e1.name AS employee_name,e2.name AS employee_name
FROM employee e1
LEFT JOIN employee e2 ON e1.manager_id = e2.manager.id;
```

**解释：**

- `e1` 和 `e2` 是 `employees` 表的两个别名，分别代表员工和员工的经理。
- 使用 `LEFT JOIN` 确保即使某个员工没有上级，也会出现在查询结果中。如果员工没有上级，则 `manager_name` 为 `NULL`。



### **10.USING 子句**

在mysql中,USING 关键字用于 简化JOIN 查询, 特别是两个表之间有相同的列名时。USING 使得sql查查询更加简介和清晰。

语法：

```
SELECT columns
FROM table_name1
JOIN table_name2 
USING (column_name)
```

- column_name 是在两个表中相同的列名，USING会将这两列作为连接条件。



### **11.自然连接**

natural join 是一种特殊的连接方式，会自动寻找两个表中具有相同列的列名进行连接。

- 自动连接：`NATURAL JOIN` 会自动寻找两个表中具有相同列名的列，并在这些列上进行连接。
- 只保留一个重复列：连接结果中，只会显示一次这些相同的列，而不是显示两个相同列名的列。

**语法：**

```
SELECT columns
FROM table1
NATURAL JOIN table2;
```

- table1   和  table2 是要连接的两个表。
- natural join 会自动识别 table1 和 table2 中相同列名的列，并自动连接。



### **12.交叉连接**

交叉连接(CROSS JOIN) shi SQL中的一种连接类型，他返回左表和右表的笛卡尔积。简单来说，就是返回左表中的每一行与右表中的每一行配对，返回所有可能的组合结果。

- **笛卡尔积**: 给的那个两个表，一个m行，一个n行，返回一个m*n行的结果。

  CROSS JOIN 的基本语法：

  ```
  SELECT column1,column2,...
  FROM table1
  CROSS JOIN table2;
  ```

    - table1 和 table2 是你希望两个交叉连接的表。
    - column1,column2是你从两个表中选取的列。

  示例：

  表employee:

  | id   | name  |
    | ---- | ----- |
  | 1    | Alice |
  | 2    | Bob   |

  表 departments:

  | id   | department_name |
    | ---- | --------------- |
  | 1    | HR              |
  | 2    | IT              |

  你想要生成一个所有员工和部门可能的组合。

  ```
  SELECT employee.name,departments.department_name
  FROM employee
  CROSS JOIN departments;
  ```

  结果：

  | name  | department_name |
    | ----- | --------------- |
  | Alice | HR              |
  | Alice | IT              |
  | BOb   | HR              |
  | Bob   | IT              |



### **13.联合**

在mysql 数据库中，联合(UNION) 用于将两个或多个SELECT 查询的结果合并到一个结果集合中。并去除重复的行。

- UNION : 将多个SELECT 查询的结果合并，并去除重复的行。
- UNION ALL :  将多个SELECT 查询的结果合并，不去出重复的行。

语法:

```
SELECT column1,column2,...
FROM table
UNOIN 
SELECT colum1,colum2,...
FROM table2;
```

- `SELECT column1, column2, ... FROM table1` 和 `SELECT column1, column2, ... FROM table2` 是两个独立的 `SELECT` 查询，它们将被合并到一个结果集中。

- 两个查询的列数和列类型必须兼容。如果列的数目不同，查询会返回错误。

- 默认情况下，`UNION` 会自动去除结果集中的重复行。如果想保留重复行，使用 `UNION ALL`。

示例：

`employees` 表：

| employee_id | name  |
| ----------- | ----- |
| 1           | Alice |
| 2           | Bob   |

`contractors` 表：

| contractor_id | name    |
| ------------- | ------- |
| 1             | Charlie |
| 2             | David   |

如果我们想查询所有人员（员工和承包商）的姓名，并将结果合并成一个列表，可以使用 `UNION`。

```
SELECT name FROM employee
UNION
SELECT name FROM employee
```

- 第一个 `SELECT` 查询从 `employees` 表中选择 `name` 列。

- 第二个 `SELECT` 查询从 `contractors` 表中选择 `name` 列。

- `UNION` 会合并这两个查询的结果，并去除重复的姓名。

查询结果:

| name    |
| ------- |
| Alice   |
| Bob     |
| Charlie |
| David   |



## 第四章：增删改查语句

### **1.列属性**

在mysql中，列 (或字段) 的属性定义了每个字段中的特征，包括数据类型，允许是否为空，是否具有唯一性，默认值等。常见属性如下：

1. 数据类型 (Data Type)

    - 整数类型：INT,TINYINT
    - 浮动小数类型:FLOAT,DOUBLE
    - 字符类型:CHAR,VCHAR
    - 日期/时间类型:DATE,DATETIME
    - 二进制类型: BINARY

2. 允许是否为空 (Null)



3. 默认值 (DEFAULT)

   可以给列设置默认值。当插入数据时，没有指定该列的值，则使用默认值。

   示例;

   ```
   CREATE TABLE example (
       id INT NOT NULL,
       created_at DATETIME DEFAULT CURRENT_TIMESTAMP  -- 默认当前时间
   );
   ```



4. 唯一性 (UNIQUE)

    - UNIQUE: 确保列中的每一行都是唯一的，即每行的该列值都不能被重复。

      示例:

      ```
      CREATE TABLE users (
          email VARCHAR(255) UNIQUE,   -- email 列中的值必须唯一
          name VARCHAR(255)
      );
      ```



5. 主键  (PRIMARY KEY)

   主键使一个或多多个列的组合，它唯一标识表中的每一行，具有 NOT NULL,和 UNIQUE 属性。

   示例;

   ```
   CREATE TABLE users (
       id INT PRIMARY KEY,    -- id 是主键
       name VARCHAR(255)
   );
   ```



6. 自增 (AUTO_INCREMENT)

   自增通常用于主键，他会为每一啊很难过自动生成一个唯一的数字，通常用于标识id。

    - 只适用于整数类型列
    - 每次插入新行时，自动为该列分配一个递增的值。

   示例:

   ```
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255)
   );
   
   ```



7. 索引 (INDEX)

    - INDEX:为某个列创建索引，
    - FULLTEXT:创建全文索引
    - SPATIRL: 用于空间数据类型的索引

   示例:

   ```
   CREATE TABLE orders (
       order_id INT PRIMARY KEY,
       user_id INT,
       FOREIGN KEY (user_id) REFERENCES users(id)  -- user_id 是外键，引用 users 表的 id 列
   );
   ```



8. 外键 (FOREIGN KEY)

   外键通常用于维护两个表之间关系的列，通常指向两一个表的主键或唯一键。

   示例：

   ```
   CREATE TABLE orders (
       order_id INT PRIMARY KEY,
       user_id INT,
       FOREIGN KEY (user_id) REFERENCES users(id)  -- user_id 是外键，引用 users 表的 id 列
   );
   
   ```



9. 检查约束 (CHECK)

    - CHECK:用于设置约束条件，确保列中的数据复合一定条件。

   示例;

   ```
   CREATE TABLE employees (
       id INT PRIMARY KEY,
       age INT,
       CHECK (age >= 18)  -- 限制 age 列的值必须大于或等于 18
   );
   ```



10. 列的默认顺序(COLLATE)

    定义了列的字符排序顺序( 如排序和比较字符串的方式 )

    示例:

    ```
    CREATE TABLE users (
        id INT PRIMARY KEY,
        name VARCHAR(255) COLLATE utf8_general_ci   -- 指定字符集排序规则
    );
    ```



11. 列的字符集（CHARACTER SET）

    - 指定列的字符集，用于定义该列存储字符数据时使用的编码。

      示例：

      ```
      CREATE TABLE users (
          id INT PRIMARY KEY,
          name VARCHAR(255) CHARACTER SET utf8mb4   -- 指定字符集为 utf8mb4
      );
      ```



12. 自动删除列 (ON UPDATE,ON DELETE)

    外键约束中，可以指定删除或更新表记录如何处理从表中的相关记录

示例：

```
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
    ON DELETE CASCADE   -- 当用户被删除时，相关订单记录也将删除
);
```



### **2.插入单行**(INSERT INTO )

语法;

```
INSERT INTO table_name(column1,column2,column3,...)
VALUES (value1,value2,value3,...)
```



### **3.插入多行**

语法:

```
INSERT INTO table_name(column1,column2,...)
VALUES  (value1,value2,..),
		(value3,value4,..),
		(value5,value6,..),
```



### **4.插入分层行(数据)**

**分层数据**指的是有父子关系的数据。例如，部门表中的 `公司` -> `技术部` -> `研发部`，其中 `技术部` 是 `公司` 的子部门，`研发部` 是 `技术部` 的子部门。

我们使用 **邻接列表模型** 来存储这种分层数据，通常通过一个 `parent_id` 列来表示父级关系。

- **邻接列表模型（Adjacency List Model）**

在这个模型中，每个记录有一个 `parent_id` 字段，表示它的父节点。如果 `parent_id` 是 `NULL`，则表示它是根节点。

示例：部门表

假设我们有一个部门表，其中 `id` 是部门的唯一标识，`parent_id` 用来表示该部门的父部门。

表结构:

```
CREAT TABLE departments(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VCHAR(255) NOT NULL,
	parent_id INT DEFAULT NULL,   --父部门的id, 如果是根部门， parent_id 为 NULL
	FOREIGN KEY (parent_id) REFERENCES departments(id)  --外键约束
 )
```

- **插入数据**

```
-- 插入根部门 '公司'
INSERT INTO departments (name, parent_id) VALUES ('公司', NULL);

-- 插入一级子部门 '技术部' 和 '市场部'，它们的父部门是 '公司' (parent_id = 1)
INSERT INTO departments (name, parent_id) VALUES ('技术部', 1);
INSERT INTO departments (name, parent_id) VALUES ('市场部', 1);

-- 插入二级子部门 '研发部' 和 '运维部'，它们的父部门是 '技术部' (parent_id = 2)
INSERT INTO departments (name, parent_id) VALUES ('研发部', 2);
INSERT INTO departments (name, parent_id) VALUES ('运维部', 2);
```

- **查询数据**

```
SELECT id, name, parent_id FROM departments ORDER BY parent_id, id;
```

- **解释**

    - `parent_id` 是关键字段，它表示当前部门的上级部门。

    - 根部门（如 `公司`）的 `parent_id` 为 `NULL`，表示没有父部门。

    - 子部门（如 `技术部`）的 `parent_id` 是其父部门的 `id`（例如，`技术部` 的父部门是 `公司`，所以 `parent_id` 为 `1`）。

​	**总结**

​		在 MySQL 中，我们常通过 **邻接列表模型** 来表示分层数据，这种方式简单易懂，插入数据时		只需要指定每个部门的父部门的 ID。



### **5.创建表复制**

- 只复制表结构

  语法：

  ```
  CREATE TABLE new_table LIKE old_table;
  
  new_table: 新表
  old_table: 旧表
  ```

  这种方法会创建一个空的新表，且列定义、索引、默认值等都会复制过去

- 复制表结构并插入数据

  1.插入所有数据

  ```
  CREATE TABLE new_table AS SELECT * FROM old_table;
  ```

  2.插入部分数据

```
CREATE TABLE new_table AS SELECT * FROM original_table WHERE age > 30;

//加入了条件  age > 30;
```



### **6.更新单行(UPDATE)**

- UPDATE 语句可以更新表中的一行或者多行数据，可以更新表中的一个或者多个字段(列)。

语法：

```
UPDATE [IGNORE] table_name
SET 
	column1_name = value1,
	column2_name = value2,
	...
[WHERE clause];
```

用法说明：

- - UPDATE` 关键字后指定要更新数据的表名。
- 使用 `SET` 子句设置字段的新值。多个字段使用逗号分隔。字段的值可以是普通的字面值，也可以是表达式运算，还可以是子查询。
- 使用 [`WHERE`](https://www.sjkjc.com/mysql/where/) 子句指定要更新的行。只有符合 `WHERE` 条件的行才会被更新。
- `WHERE` 子句是可选的。如果不指定 `WHERE` 子句，则更新表中的所有行。

`UPDATE` 语句中的 `WHERE` 子句非常重要。除非您特意，否则不要省略 `WHERE` 子句。

示例：

假设我们有一个 `users` 表，表结构如下：

```
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255),
    age INT
);
```

如果你想更新某一行数据（比如更新 `id = 1` 的用户的邮箱），可以使用 `UPDATE` 语句并加上 `WHERE` 子句来指定目标行：

```
UPDATE user
SET email = 'new.email@example.com'
WHERE id= 1；
```



### **7.更行多行**

类似于更行单行，仅仅是在条件上加了更多的选择。

示例：

```
UPDATE table_name
SET status = 'active'
WHERE age < 30

查找年龄小与30 的，将所在行的 status 字段改为 active
```



### **8.在Update中使用子查询**

你可以在UPDATE 语句中使用子查询来基于两一个表或同一个表的数据进行更新。

语法：

```
UPDATE table_name
SET column1 = (SELECT value FROM  other_table WHERE condition)
WHERE condition;
```



### **9.删除行(DELETE)**

语法:

```
DELETE FROM table_name
[WHERE clause]
[ORDER BY ...]
[LIMIT row_count]

DELETE FROM 后跟的是要从中删除数据的表。
WHERE 子句用来过滤需要删除的行。满足条件的行会被删除。
WHERE 子句是可选的。没有 WHERE 子句时，DELETE 语句将删除表中的所有行。
ORDER BY 子句用来指定删除行的顺序。它是可选的。
LIMIT 子句用来指定删除的最大行数。它是可选的。
DELETE 语句返回删除的行数
```

`DELETE` 语句中的 `WHERE` 子句非常重要。除非您特意，否则不要省略 `WHERE` 子句。它会导致删除表中的所有数据。



## **第五章：常用函数**

### **1.聚合函数Aggregate Function**

在mysql中，聚合函数 用于对一组值执行某种操作并返回一个单一的值。通常与GROUP BY子句一起使用，用于对数据进行分组后进行汇总操作。

**常见的聚合函数：**

**1).****COUNT(): 计算行数**

COUNT() 函数用于返回查询结果中的行数，或者在特定列中非NULL的值的个数。

**示例**：计算 `employees` 表中所有员工的数量。

```
SELECT COUNT(*) FROM employees;
```

**2).SUM(): 计算总和**

SUM() 函数用于返回指定列所有值的总和。

**示例**：计算 `employees` 表中所有员工的总工资。

```
SELECT SUM(salary) FROM employees;
```

**3).AVG():计算平均值**

AVG()函数返回指定的平均值，通常用于数值列。

**示例**：计算 `employees` 表中所有员工的平均工资。

```
SELECT AVG(aslary) FROM employees;
```

**4).MIN()返回最小值**

MIN() 函数返回指定列的最小值。

**示例**：获取 `employees` 表中最低的工资。

```
SELECT MIN(salary) FROM employees;
```

**5).MAX() 返回最大值**

MAX() 函数返回指定列的最大值。

**示例**：获取 `employees` 表中最高的工资。

```
SELECT MAX(salary) FROM employees; 
```



### **2.GROUP BY 子句**

用于将查询结果集按一个或多个列进行分组。通常与聚合函数一起使用。通常在SELECT查询中使用，目的是将数据按照特定的字段进行分类，并对每组数据进行聚合计算。

基本语法：

```
SELECT column1,column2,aggregate_function(column3)
FROM table_name
WHERE condition
GROUP BY column1,column2;
```

- column1, column2：用于分组的列，可以是一个或多个列。
- aggregate_function(column3)：用于计算分组后的数据的聚合值，常用的聚合函数有 COUNT()、SUM()、AVG()、MIN()、MAX() 等。
- WHERE：用于在分组之前过滤数据。
- GROUP BY：指定一个或多个列来分组数据。



### **3.HAVING 子句**

在MYSQL中，用于过滤分组(GROUP BY)的结果, WHERE子句用于过滤行数据。

基本语法：

```
SELECT column1,column2,aggregate_function(column3)
FROM table_name
WHERE condition
GROUP BY column1,column2
HAVING aggregate_function(column3) condition;
```

- GROUP BY:用于对数据进行分组
- HAVING:  用于过滤已经分组的数据，根据聚合函数的结果返回筛选后的数据。

示例：

假设你有一个sales表，包含以下字段

- `product_id`（商品ID）

- `sale_date`（销售日期）

- `quantity`（销售数量)

示例：计算每个商品的总销售数量，并仅显示销售数量大于 100 的商品。

```
SELECT product_id, SUM(quantity) AS total_sales
FROM sales
GROUP BY product_id
HAVAING SUM(quantity) > 100;
```

- **`GROUP BY product_id`**：首先根据商品ID进行分组。

- **`HAVING SUM(quantity) > 100`**：对每个分组的销售数量总和进行筛选，只返回销售总量大于 100 的商品。



### **4.ROLLUP 运算符**

用于分组汇总的功能

语法：

```
SELECT column1,column2,...AGGREGATE_FUNCTION(columnN)
FROM table_name
GROUP BY column1,column2,...ROLLUP(columnN);
```

关键点：

1. **分组**：`ROLLUP` 运算符会按列的层级进行多层次的分组。
2. **汇总行**：它会生成分组的汇总行，通常是将某些列的值置为 `NULL` 来表示汇总数据。
3. **聚合函数**：在汇总的过程中，通常会使用聚合函数，如 `SUM()`, `AVG()`, `COUNT()` 等来计算汇总结果。



## 第六章：子查询

### 1.编写复杂查询

在MySQL中，复杂查询通常涉及多个表的连接，子查询，聚合函数，条件过滤等。

- 多表连接查询
- 聚合函数
- 子查询

​	子查询可以在WHERE,FROM,SELECT,子句中使用。

示例：

以下是一个使用子查询的例子，我们想找出那些订单金额高于客户平均订单金额的订单。

```
SELECT 
		o.order_id,
		o.customer_name,
		o.order_amount
FROM 	
		orders o
WHERE 
		o.order_amount > (
			SELECT AVG(order_amount)
			FROM orders
			WHERE customer_id = o.customer_id
		);
```

。。。



### 2.子查询(Subqueries)

子查询是指 在一个查询的 SELECT ,FROM , WHERE 或 HAVING子句中嵌套另一个查询。可以返回一个或多个值。

1. ) **标量子查询**（Scalar Subquery）：返回一个单一的值。
2. ). **列子查询**（Column Subquery）：返回一列数据。
3. ) **行子查询**（Row Subquery）：返回一行数据。
4. ) **表子查询**（Table Subquery）：返回多个行和列的数据。
5. ).**相关子查询**（Correlated Subquery）：外部查询依赖子查询的结果。

示例：

查询 `orders` 表中金额最大的订单的订单 ID。

```
SELECT 
	order_id,
	order_amount
FROM 
	orders
WHERE 
	order_amount = (SELECT MAX(order_amount)FROM order);
```



### 3.IN运算符

IN 运算符通常运用在子查询中，目的是外部查询的字段 与 子查询返回的一组值进行比较。

基本语法：

```
SELECT column_name
FROM order_name
WHERE column_name IN (subquery)
```

示例：假设有两个表：

- `customers` 表（存储客户信息）：
    - `customer_id`（客户 ID）
    - `customer_name`（客户姓名）
- `orders` 表（存储订单信息）：
    - `order_id`（订单 ID）
    - `customer_id`（客户 ID，关联 `customers` 表）
    - `order_amount`（订单金额）

我们希望找到所有至少有一笔订单的客户

```
SELECT 
	customer_id,
	customer_name
FROM
	customers
WHERE customer_id IN (SELECT DISTINCT customer_id FROM order_id)
```



### 4.子查询 VS  连接

- 子查询：

    - 优点：适合用于一次性的获取某些结果值，用于过滤外部查询的结果值。
    - 缺点：对于一些复杂的子查询 (尤其嵌套查询)，性能可能较差，因为mysql 可能会对每一行执行难子查询，这将导致查询速度较慢。

- 连接：

    - 优点：在表和表之间有明确的关系时，可以提升查询性能和代码可读性。
    - 缺点:   在处理非常大的数据集时，可能会导致性能下降。

  | **特性**     | **连接 (JOIN)**                                              | **子查询 (Subquery)**                                        |
    | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | **执行方式** | 通过连接表进行匹配，通常是一次性操作。                       | 通过在外部查询中嵌套执行另一个查询，逐行或逐列处理。         |
  | **性能**     | 在处理大数据时，`JOIN` 通常性能较好，因为它通过优化器直接执行。 | 子查询可能会导致性能下降，尤其是当它被用于 `WHERE` 子句中时。 |
  | **可读性**   | 连接查询有时更直观，尤其是当表之间有明确关系时。             | 子查询在一些复杂的查询中可能更容易理解。                     |
  | **使用场景** | 如果需要从多个表中合并数据，`JOIN` 是更合适的选择。          | 如果需要从子集数据中提取信息，子查询更合适。                 |
  | **结果集**   | 连接查询直接返回联合数据。                                   | 子查询通常返回一个值或一组值，作为外部查询的一部分。         |



### 5.ALL关键字(THE ALL Keyword)

在MySQL中， all 关键字通常与 比较运算符配合使用。表示外部查询的   某个列的值   必须要与 子查询返回的所有值满足某个条件。

语法:

```
SELECT column_name(s)
FROM table_name
WHERE column_name operator ALL (subquery);

operator:  表示比较运算符  如  > , < ,  =  。。。
```

示例：

假设我们有一个 `employees` 表，记录了员工的 `salary` 和 `department_id`，以及一个 `departments` 表，记录了部门的信息。我们想查询那些工资 **高于** 部门 2 中 **所有员工** 的工资的员工

```
SELECT employee_id,name,salary
FROM employees
WHERE salary > ALL(SELECT salary FROM employees WHERE department_id = 2)
```

在这个查询中：

- 子查询 `(SELECT salary FROM employees WHERE department_id = 2)` 返回部门 2 中所有员工的工资。
- 外部查询 `WHERE salary > ALL (...)` 表示我们只选择那些工资 **大于** 部门 2 中 **所有员工的工资** 的员工。

如果外部查询中的某个 `salary` 不满足大于所有子查询返回值的条件，结果将不包含该员工。



### 6.ANY 关键字 (THE ANY Keyword)

与ALL关键字相似，ANY关键字 会比较 外部查询的值 只要与 子查询 的值有一个满足比较的结果。就会返回查询结果。

语法：

```
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY (subquery);
```

示例：

假设我们还是使用 `employees` 表来查询那些薪水 **大于** 部门 2 中 **任意一名员工的薪水** 的员工：

```
SELECT employee_id,name,salary
FROM employees
WHERE salary > ANY (SELECT salary FROM employee WHERE department_id = 2);
```

在这个查询中：

- 子查询 `(SELECT salary FROM employees WHERE department_id = 2)` 返回部门 2 中所有员工的工资。
- 外部查询 `WHERE salary > ANY (...)` 表示我们只选择那些工资 **大于部门 2 中任意一名员工的工资** 的员工。

    3. **`ALL` 和 `ANY` 的区别**

| 特性           | `ALL`                                                        | `ANY`                                                        |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **含义**       | 外部查询的值必须与子查询返回的 **所有值** 满足条件。         | 外部查询的值与子查询返回的 **任意一个值** 满足条件。         |
| **查询条件**   | 外部查询的值必须满足与子查询返回的 **每一个值** 的条件。     | 外部查询的值只需满足与子查询返回的 **至少一个值** 的条件。   |
| **例子**       | `salary > ALL (SELECT salary FROM employees WHERE department_id = 2)`：薪水必须大于部门 2 中所有员工的薪水。 | `salary > ANY (SELECT salary FROM employees WHERE department_id = 2)`：薪水必须大于部门 2 中任意一名员工的薪水。 |
| **常用运算符** | `=`, `>`, `<`, `>=`, `<=`, `!=`（通常是比较外部查询值与子查询返回的所有值） | `=`, `>`, `<`, `>=`, `<=`, `!=`（通常是比较外部查询值与子查询返回的至少一个值） |



### 7.相关子查询

mysql中，相关子查询是指在子查询中引用了外不查询的列。

语法：

```
SELECT column_name(s)
FROM table_name AS t1
WHERE column_name operator
	(SELECT column_name FROM table_name AS t2 WHERE t2.column_name = t1.column_name);
```

这里：

- 外部查询（`t1`）中的列会被引用在子查询（`t2`）中。
- 子查询将基于外部查询的每一行进行执行。

示例：

​	 查询薪水高于该部门所有员工薪水的员工

假设我们有两个表：

- `employees`（员工表）：包含 `employee_id`（员工ID）、`salary`（薪水）和 `department_id`（部门ID）。
- `departments`（部门表）：包含 `department_id`（部门ID）和 `department_name`（部门名称）。

```
SELECT e.employee_id,e.name,e.salary
FROM employees e
WHERE e.salary > ALL (SELECT e2.salary
					  FROM employees e2
					  WHERE e2.department_id = e.department_id);
```



### 8.EXISTS运算符

EXISTS通常用于判断子查询是否返回任何结果。EXISTS 运算符返回一个布尔值；

- **`TRUE`**：如果子查询返回至少一行数据（即子查询非空）。
- **`FALSE`**：如果子查询没有返回任何数据。

语法：

```
SELECT column(s)
FROM table_name 
WHERE EXISTS (subquery)
```

示例：

​	假设我们有两个表：

- `employees`（员工表）：包含 `employee_id`（员工ID）和 `department_id`（部门ID）。
- `departments`（部门表）：包含 `department_id`（部门ID）和 `department_name`（部门名称）。

现在我们想查询所有有员工的部门（即至少有一名员工的部门）。

```
SELECT department_name
FROM departments d
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.department_id)
```

在这个查询中：

- 外部查询从 `departments` 表中选取部门名称。

- 子查询

  ```
  (SELECT 1 FROM employees e WHERE e.department_id = d.department_id)
  ```

  检查每个部门是否有员工。

    - 如果该部门在 `employees` 表中有至少一个员工，`EXISTS` 返回 `TRUE`，外部查询返回该部门的 `department_name`。
    - 如果该部门没有员工，`EXISTS` 返回 `FALSE`，外部查询将不返回该部门。
    - 在子查询中，我们用 `SELECT 1` 是为了优化性能，因为我们不关心子查询返回的具体内容，返回 `1` 或任何常量即可。



### 9.SELECT  语句中的子查询

在 SELECT 子句中，子查询可以用来为每一个行计算一个动态值，或者作为为一个计算列提供额外的信息。

示例：

​	假设有两个表：

- `employees`（员工表）：包含 `employee_id`（员工ID）、`name`（员工姓名）、`department_id`（部门ID）和 `salary`（薪水）。
- `departments`（部门表）：包含 `department_id`（部门ID）和 `department_name`（部门名称）。

我们想查询所有员工的 `employee_id`、`name` 和薪水，并且还想获取每个部门的平均薪水：

```
SELECT 	e.employee_id,
		e.name,
		e.salary,
		( SELECT AVG(salary)
		  FROM employees e2
		  WHERE e2.department_id = e.department_id
		 )AS avg_department_salary
FROM employee e
```

在这个查询中：

- 外部查询从 `employees` 表中选择每个员工的 `employee_id`、`name` 和 `salary`。
- 子查询 `(SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e.department_id)` 在每一行计算该员工所在部门的平均薪水，并且返回该值作为 `avg_department_salary` 列。



### 10.FROM  中的子查询。

子查询可以出现在 FROM 子句中，作为一个虚拟表或者派生表。这种类型的子查询通常称为**派生表**。

示例:

​	假设有两个表：

- `employees`（员工表）：包含 `employee_id`（员工ID）、`name`（员工姓名）、`department_id`（部门ID）和 `salary`（薪水）。
- `departments`（部门表）：包含 `department_id`（部门ID）和 `department_name`（部门名称）。

我们希望查询每个部门的最大薪水，子查询作为 `FROM` 子句中的一个虚拟表，计算每个部门的最大薪水：

```
SELECT department_id,MAX(salary) AS max_salary
FROM (
		SELECT department_id,salary
		FROM employees 
		WHERE salary > 50000
	  ) AS high_salary_employees
GROUP BY department_id;
```

在这个查询中：

- 内部子查询 `(SELECT department_id, salary FROM employees WHERE salary > 50000)` 从 `employees` 表中筛选出薪水大于 50000 的员工，并将其作为一个派生表 `high_salary_employees`。
- 外部查询对这个派生表进行分组，并计算每个部门的最大薪水。



## 第十一章：事务

### 1.事务，| Transcations[事务和并发]

事务是指一组数据库操作，作为一个单元执行，要么全部成功，要么全部失败。

**事务的四个关键特性(ACID)：**

**1.原子性（Atomicity）：**

- ​	事务是一个不可分割的单位，事务中的操作要么全部完成，要么全部不做。如果事务中的任何一个操作失败，整个事务都会回滚，数据库会恢复到事务开始前的状态。

**2.一致性（Consistency）:**

- ​	事务必须使数据库从一个一致性状态转换到另一个一致性状态。事务开始前和结束后，数据库的完整性约束不应该被破坏。
- ​         例如：银行的账户余额应该始终大于零。

**3.隔离性（Isolation）：**

- 事务之间彼此不互相影响，数据库提供不同的隔离级别，已决定多个事务如何并发执行。
- 不同的隔离级别可以控制 “ 脏读 ”(读取未提交的数据)， “不可重复读” (同一事务内读取的结果不同)  和 “幻读”  (一个事务读取到的结果在另一个事务提交后发生变化) 等问题。

**4.持久性（Durability）**

- 一旦事务提交，它对数据库的所有更改是永久的，即使数据库发生崩溃或断电，已提交的数据也不会丢失。



### 2.创建事务

1. **开始事务**

   (START TRANSCATION 或 BEGIN 或 SET AUTOCOMMIT = 0 :)来启动一个事务，MySQL会自动记录从此刻开始的所有sql 操作，知道事务被提交或回复你。

   ```
   START TRANSCATION
   ```

2. **提交事务**

   (COMMIT) :将食物的所有操作保存到数据库，表示事务成功完成。

   ```
   COMMIT;
   ```

3. **回滚事务**

   取消当前事务的所有操作，回滚到事务开始前的状态。

   ```
   ROLLBACK;
   ```

4. **设置自动提交模式**

   (SET AUTOCOMMIT): MYSQL设置自动提交模式，（AUTOCOMMIT = 1)。在次模式下，每条sql语句都会立即生效，无法组合成一个事务。可以关闭自动提交来显式控制十五的开始和结束。

   ```
   SET AUTOCOMMIT = 0  //关闭自动提交
   SET AUTOCOMMIT = 1  //开启自动提交
   ```

### 3.并发和锁定

MySQL 事务的并发和锁定问题指的是多个事务在并发执行时，为了确保数据的一致性和完整性，如何避免并发操作导致的数据冲突或不一致的现象。在数据库管理系统中，多个事务可能会同时访问和修改相同的数据项，这就涉及到如何合理管理对数据的访问，以确保事务能够正确、安全地执行。



### 4.并发问题

在并发执行的事务中，常见的问题有以下几种：
**1.脏读（Dirty Read）**

​	一个事务读取了另一个事务尚未尚未提交的数据，如果是另一个事务最终回滚，那么这个事务督导的数据就变得无效，从而出现脏读的现象。

**2.不可重复读（Non-Repeatable Read）**

​	在同一个事务中，某个数据被读取多次，但在两次读取之间，这个数据被其他事务更改了，导致读取到不同的值。

**3.幻读（Phantom Read）**

​	在同一个事务中，某个查询条件下的数据在两次查询之间发生了变换，例如，在第一次查询时没有数据满足条件，但在第二次查询时其他事务插入了新的数据，导致查询结果发生了变化。

**4、脏写**

一个事务修改了另一个事务尚未提交的数据，导致数据不一致和潜在错误。



### 5.事务隔离级别

你可以在MySQl中设置事务的隔离级别，这决定了事务之间的并发行为(脏读，不可重复读，幻读。。。)。你可以在事务开始前及设置事务的隔离级别。

事务的四种隔离级别：

- READ UNCOMIITTED (读取未提交的数据)： 可能发生脏读
- READ COMMITTED (读取已提交的数据) ：避免脏读，但可能不发生重复度。
- REPEATABLE READ (可重复读)： 避免发生不可重复读，但可能发生幻读
- SERIALIZABLE (可串行化)：最严格的隔离级别，完全串行执行事务，避免所有并发问题。

示例：

```
SET TRANSCATION ISOLATION LEVEL REPEARTABLE READ;
```



### 6.读未提交的隔离级别

​	在mysql中，读未提交（READ UNCOMMITTED） 是最低的事务隔离机制。它允许事务读取其他事务尚未提交的的数据。

- **脏读**

  在READ UNCOMMITTED 隔离级别下，脏读是最主要的问题。

- **使用场景**

​	日志数据，监控数据：这些数据坑你会被用来实时查看，但不会要求准确无误。

​        临时分析或查询：一些分析系统可能不要求数据完全一致，但需要快速大量处理。



### 7.读已提交的隔离级别

此隔离级别保证了一个事务读取到其他事务已经提交的修改，因此避免了脏读。

特点：

- **无脏读**
- **不可重复读：**在同一事务中，如果同以数据被多次读取，可能会得到不同的值。因为其他事务可能修改并提交了数据。
- **幻读：**多个事务同时查询相同条件下的数据时，可能会在事务执行期间新增数据，导致查询结果不同。

引用场景：

​	对数据一致性比较高，但对数据的多次读取一致性要求不高，比如某些查询型应用。



### 8.可重复读隔离级别

着是MySQL的默认隔离级别。它保证了在同一个事务内，所有的读取结果都是一直的，防止了不可重复读的发生。

特点：

- **无脏读**
- **无不可重复读**
- **可能发生幻读：**在一个事务中，基于某些条件的查询结果可能会在查询期间发生变化，导致事务读取到不同的数据集。

引用场景：

​	数据一致性较高，但允许某些数据被并发时修改，适合金融，支付系统等对一致性要求严格的场景



### 9.序列化隔离级别

也称串行化( SERIALIZABLE),这是最高的隔离级别，避免了所有的并发问题。

特点：

​	**无脏读，无不可重复读，无幻读：**该隔离级别保证了事务之间的完全隔离，数据不会被其他事务所更改，读取的数据最终一致。

引用场景：

​	对数据要求一致性极高的查场景，例如银行系统的账户转行等事务操作。

**简单总结：**

比较：事务隔离级别的并发性和一致性

| 隔离级别             | 脏读 | 不可重复读 | 幻读 | 并发性 | 一致性 |
| -------------------- | ---- | ---------- | ---- | ------ | ------ |
| **READ UNCOMMITTED** | 是   | 是         | 是   | 最高   | 最低   |
| **READ COMMITTED**   | 否   | 是         | 是   | 较高   | 较低   |
| **REPEATABLE READ**  | 否   | 否         | 是   | 较低   | 较高   |
| **SERIALIZABLE**     | 否   | 否         | 否   | 最低   | 最高   |

**事务隔离级别的选择**

- **性能 vs 一致性**：隔离级别越低，系统的并发性越高，但数据的一致性越差。对于某些性能敏感且数据一致性要求较低的应用（如日志收集、某些缓存系统），可以选择较低的隔离级别。而对于数据一致性要求极高的应用（如银行系统），则应选择更高的隔离级别。
- **业务场景**：实际中，应用可以根据业务需求选择适当的隔离级别。某些应用可能允许偶尔出现不一致的数据，只要最终的数据最终是一致的；而一些需要严格确保数据一致性的应用，则需要使用较高的隔离级别。



### 10.死锁

​	在MySQL中，事务死锁是一种并发问题，通常发生在两个或多个事务相互等待对反释放资源（如锁），导致他们无法继续执行。这中情况通常需要数据库回滚其中一个事务来打破死锁。

**MySQL如何检测和处理死锁：**

​	MySQL会定期的检查师傅偶存在死锁，如果存在死锁，系统会自动 中止(回滚)其中一个事务，以打破死锁。

**死锁的处理机制：**

​	**1.死锁检测：**MySQL会检测是否存在循环等待的情况。如果检测到死锁，数据库会选择一个事务回滚。

​	**2.回滚事务：**MYSQL会选择一个事务回滚，通常是**最小代价的事务**，即回滚事务中已执行操作较少的那个事务。回滚的事务会收到一个 ERROR 1213(4001): Deadlock found when tring to get lock 错误。

​	**3.事务重试**：死锁被回滚后，应用层通常会捕获该错并重试该事务，直到事务成功提交。



## 第十二章：数据类型

### 1.介绍| Introduction [数据类型]

MySQL提供了多种数据类型来支持不同的应用需求。数据类型通常分为一下几类：

1. 数值类型（包括整数和浮点数）
2. 日期和时间类型
3. 字符串类型（包括字符，文本，二进制等）
4. 其他类型（如枚举，集合等）



### 2.字符串类型

- **定长度字符串（Fixed-length String Types）**

| 数据类型 | 存储大小  | 值范围                        | 示例    |
| -------- | --------- | ----------------------------- | ------- |
| CHAR(N)  | 固定N字节 | 固定字符的长度（最多255字符） | ’hello‘ |

CHAR 是定长类型，不足时会用空格填充。



- **变长字符类型（Variable   length  Strng Types ）**

| 数据类型   | 存储大小     | 值范围                         | 示例    |
| ---------- | ------------ | ------------------------------ | ------- |
| VARCHAR(N) | 最大N=1 字节 | 0到N 个字符（最多65525个字符） | ’hello‘ |

VARCHAR 存储变长字符串，能有效节省存储空间，但有一个最大长度限制 (N).



- **文本类型（Text Type）**

用于存储大容量的文本数据。

| 数据类型   | 存储大小          | 值的范围         | 示例          |
| ---------- | ----------------- | ---------------- | ------------- |
| TINYTEXT   | 255字节           | 0 到255个字符    | ’short text‘  |
| TEXT       | 65,535字节        | 0 到65535个字符  | ’long text‘   |
| MEDIUMTEXT | 16,777,215个字节  | 0到1677721个字符 | ’larger text‘ |
| LONGTEXT   | 4,294,967,295字节 | 0到4GB           | 'huge text'   |

TEXT 类型用于存储大量文本数据，适合存储长文章，评论等。



- **二进制数据类型（Binary Types）**

| 数据类型     | 存储大小    | 值的范围             | 示例     |
| ------------ | ----------- | -------------------- | -------- |
| BINARY(Y)    | 固定N 字节  | 固定长度的二进制数据 | 0x10     |
| VARBINARY(N) | 最大N+1字节 | 变长二进制数据       | 0xA1B2C3 |

BINARY  用于存储固定长度的二机制数据，VARBINARY  用于储存变长二进制数据。



### 3.整数类型

整数类型用于存储整数值。MYSQL 提供了不同大小的证书类型，以适应不同范围的需求。整数类型可以是 **有符号（SIGNED）**，或 **无符号(UNSIGNED)**

| 数据类型      | 存储大小 | 值的范围（有符号）                                      | 值的范围（无符号）              |
| ------------- | -------- | ------------------------------------------------------- | ------------------------------- |
| TINYINT       | 1字节    | -128 到 127                                             | 0 到255                         |
| SMALLINT      | 2字节    | -32，768到32，767                                       | 0到65，535                      |
| MEDIUMINT     | 3 字节   | -8,388,608 到 8,388,607                                 | 0 到 16,777,215                 |
| INT / INTEGER | 4 字节   | -2,147,483,648 到 2,147,483,647                         | 0 到 4,294,967,295              |
| BIGINT        | 8 字节   | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 | 0 到 18,446,744,073,709,551,615 |



### 4.定点数类型和浮点数类型

在mysql中定点数（Fixed-point） 类型主要用于存储需要精确表示的是小数，特别是财务计算，货币金融等场景。

**定点数类型：DECIMAL 和 NUMERIC**

- DECIMAL

用于存储精确的小数，并且指定小数点前后位数。

语法

```
DECIMAL(M,D)

M:表示总位数(总位数)，即数字的总位数（包括小书点后面所有的数字）。
D:表示小数点后面的位数（标度）.
M 的最大值为65
D 的最大值为30
```

- NUMERIC

NUMRMIC  与 DECIMAL DE 类型完全相同，在MySQL中，NUMRMIC  是  DECIMAL 的别名，所以他们的行为和用法是完全一样的。

**浮点数类型**

浮点数类型用于存储带有小数部分的数值

| 数据类型       | 存储大小 | 值的范围                                            |
| -------------- | -------- | --------------------------------------------------- |
| FLOAT          | 4字节    | -3.402823466E+38 到 3.402823466E+38                 |
| DOUBLE         | 8字节    | -1.7976931348623157E+308 到 1.7976931348623157E+308 |
| DECIMAL/NUMRIC | 可变长度 | -10^38 +1 到 10^38 -1（精确到指定小数位）           |



### 5.布尔类型

在MySQL中并没有单独的 布尔类型。MySQL中的 boolean 类型只是 TINYNT(1) 类型的一个别名，用于表示真 或 假（TRUE OR FALSE）。

- 0 表示  FALSE（假）
- 非零的值（通常是1 表示 真  TRUE）

尽管MySQL并没有原生的BOOLEAN数据类型，使用 BOOLEAN 作为列的定义是完全有效的，MySQL会将其是为 TINYINT(1) 类型.

示例：

```
CREATE TABLE user(
		usr_id INT PRINMARY KEY,
		is_active BOOLEAN   //时间会被解析为 TINYINT(1)
);
```



### 6.枚举和集合类型

- **枚举类型（ENUM）**

枚举类型是一个字符串对象，可以选择一组预定义的值之一。

| 数据类型                                | 存储大小    | 示例     |
| --------------------------------------- | ----------- | -------- |
| ENUM('value1','value',’value3‘，。。。) | 1 或 2 字节 | ’value1‘ |

通常用于列举有限的选项，适合存储固定的字段，例如性别，状态等。



- **集合类型(SET)**

SET类型允许选择多个或零个预定义的值。

| 数据类型                            | 存储大小 | 示例            |
| ----------------------------------- | -------- | --------------- |
| SET('value1','value2','value3',...) | 1或2字节 | ’value1,value2‘ |

SET 类型使用于存储一组可以选择的多个指的字段，如一个用户可以有多个角色。



### 7.日期和时间类型

MYSQL 提供了多种用于处理日期和时间的类型。

| 数据类型  | 存储大小 | 值范围                                         | 示例                  |
| --------- | -------- | ---------------------------------------------- | --------------------- |
| DATE      | 3字节    | '1000-01-01' 到 '9999-12-31'                   | '2024-11-14'          |
| DATETIME  | 8字节    | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' | '2024-11-14 12:30:00' |
| TIMESTAMP | 4字节    | '1970-01-01 00:00:01' 到 '2038-01-19 03:14:07' | '2024-11-14 12:30:00' |
| TIME      | 3字节    | '-838:59:59' 到 '838:59:59'                    | '12:30:00'            |
| YEAR      | 1字节    | 1901 到 2155                                   | '2024'                |

- DATETIME 和  TIMESTAMP 都存储日期和时间，但 DATETIME 是绝对时间，不受时区影响，而TIMESTAMP是基于 UTC 的，适用于需要处理时区的场景。
- TIME   用于存储时长和持续时间。



### 8.BLob 类型

在MySQL中，BLOB 是用于存储大量的二进制数据的一个数据类型。如图像，音频，视频等文件。

常见的blob类型

| 数据类型   | 存储大小                  | 存储范围             | 适用场景                 |
| ---------- | ------------------------- | -------------------- | ------------------------ |
| TINYBLOB   | 255字节                   | 0到255字节           | 用于存储较小的二进制数据 |
| BLOB       | 65,535 字节 (64 KB)       | 0 到 65,535 字节     | 存储中等大小的二进制数据 |
| MEDIUMBLOB | 16,777,215 字节 (16 MB)   | 0 到 16,777,215 字节 | 存储较大的二进制数据     |
| LONGBLOB   | 4,294,967,295 字节 (4 GB) | 0 到 4GB             | 存储非常大的二进制数据   |



### 9.JSON  类型

mysql 支持 JSON 类型，用于存储 JSON 格式的数据。

| 数据类型 | 存储大小 | 值得范围          | 示例                          |
| -------- | -------- | ----------------- | ----------------------------- |
| JSON     | 变长     | 有效的SON格式数据 | '{"name": "John", "age": 30}' |

JSON 类型提供了对 JSON  数据的存储，索引，查询和操作支持。



## **第十三章:主键和外键**

### 1.主键

主键是用于唯一标识表中每一行的约束，它确保表中的每一条记录都是唯一的。但不能包含 NULL。主键通常用于确保数据完整性，帮优化查询效率。每个表只能有一个主键，但可以由多个列组成。

**特点：**

1. **唯一性：** 主键中的值必须是唯一的，不允许重复。
2. **非空性**：主键列中的值不能为 NULL
3. **自动索引：**mysql 会为主键列创建唯一索引，帮助提高查询效率。
4. **单一主键：**每一个表只能有一个主键，但主键可以由多个列组成（ 称为 **复合主键**）。

- **创建主键:**

1.最常见的情况是为表中的一个列定义主键，通常是一个自增的整数数列，来唯一标识每一条记录。

```
CREATE TABLE employees (
	employee_id, INT NOT NULL,
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	PRIMARY KEY (employee_id)
);
```

- **添加主键约束**

如果表已存在并且你需要为某一列 或者 某一组合列添加主键 ，可以用 ALTER TABLE  语句。

添加单列主键:

```
ALTER TABLE employee ADD PRIMARY KEY (employee_id);
```

添加复合主键

```
ALTER TABLE employee ADD PRIMARY KEY (order_id,product_id);
```

主键的删除

```
ALTER TABLE employee DROP PRIMARY KEY;
```



### 2.外键

在MySQL中，外键（Foreign Key） 是一种约束，用于确保表和表之间数据的一致性和完整性。外键约束确保一个表中的字段只能出现在另一个表的主键或唯一键中，外键通常用于建立两个表的关系。

**外键的特点**

- 应用完整性: 外键用与确保数据的引用完整性，防止插入不合法的数据。
- 约束的关系：外键约束是通过一个表中拆创建指向另一个表主键或唯一键来实现的。
- 级联操作：外键支持级联更新和级联删除，允许在主表中更新或删除是自动更新从表的相关记录。

**创建外键：**

```
 CREATE TABLE cutomers(
 	customer_id INT AUTO_INCREEMENT PRIMARY KEY,
 	customer_name VARCHAR(100)
 );
 
 CREATE TABLE order(
 	order_id INT AUTO_INCREMENT PRIMARY KEY,
 	order_date DATE,
 	customer_id INT,
 	FOREIGN KEY (customer_id) REFERENCE customer(customer_id)
 );
```

在这个例子中，order表中额customer_id 是外键，它引用 customers 表中的cutomer_id 主键，



### 3.外键约束

外键约是指用来维护数据一致性和完整性的机制，它确保一个表中的字段（外键）只能包含另一个表中的主键或唯一键的值。外键约束通常用于建立表之间的关系，如一对一 或一对多。

**外键的级联操作：**

- CASCADE: 当父表中的记录被删除或更新时，从表中的的相关记录会自动删除或更新
- SET NULL: 当父表中的记录被删除或更新时，从表中的外键字段会被设置称 NULL。
- NO ACTION  或 RESTIRCT:  禁止对父表记录的更新或删除操作，除非从表中没有依赖记录。
- SET DEFAULT:  将外键列的值设置为默认值 (在 MYSQL 中较少使用)。

**示例：级联删除（ON DELETE CASCADE）**

```
CREATE TABLE customers(
	customer_id INT AUTO_INCREMENT PRIMARY KEY,
	customer_name VARCHAR(100)
);

CREATE TABLE orders(
	order_id,INT AUTO_INCREMENT PPRIMARY KEY,
	order_date DATE,
	customer_id INT,
	FOREIGN KEY (customer_id)
	REFERENCE customers(customer_id)
	ON DELETE CASCADE     //当附表中的记录被删除时，从表中的相关记录也会被删除。
 )
```

在这个例子中，如果删除 `customers` 表中的某个客户，`orders` 表中所有与该客户相关的订单记录都会被自动删除。



## 三大范式

### 1.第一范式 -- 原子性

第一范式要求数据库表中的每一个字段都是必须是原子性的，即每一个字段只能包含一个值，不允许包含集合，数组，列表等数据类型。

**例如：** 如果有一个 `students` 表，字段 "phone_numbers" 存储了多个电话号码，那么这违反了第一范式。应该将每个电话号码放入单独的记录，而不是存储多个电话号码。

**解决方法：** 把 "phone_numbers" 拆分成多个记录，每个记录只有一个电话号码。

示例：

```
CREATE TABLE student(
	student_id INT PRIMARY_KEY,
	name VARCHAR(100),
	phone_number VARCHAR(20)
);
```



### 2.第二范式 -- 消除部份依赖

第二范式在第一范式的基础上，要求非主属性对部分候选键的依赖。也就是说，表中的非主属性（非键字段）必须完全依赖于整个主键，而不仅是依赖于主键的一部分。

适用于 **复合主键** 的表 如果一个字段只依赖于主键的一部分，则违反了第二范式。

**例如：** 如果有一个包含复合主键的 `orders` 表，其中 `order_id` 和 `product_id` 共同组成主键，而 `product_name` 只依赖于 `product_id`，这就违反了第二范式，因为 `product_name` 只依赖于主键的一部分。

**解决方法：** 将 `product_name` 移到一个单独的表，专门存储 `product_id` 和 `product_name`，从而确保 `product_name` 仅依赖于 `product_id`。

示例：

```
//原表
CREATE TABLE order(
	order_id INT;
	product_id INT;
	product_name VARCHAR(100),
	quantity INT,
	PRIMARY KEY (order_id,product_id)
);

//解决方法; 将 product  移动到一个新表。
CREATE TABLE product(
	product_id INT PRIMARY KEY,
	product_name VARCHAR(100)
);

CREATE TABLE orders(
	order_id INT,
	product_id INT,
	quantity INT,
	PRIMARY KEY (order_id,product_id)
);
```



### 3.第三范式 --消除传递依赖

第三范式要求在满足第二范式的基础上，消除非主属性之间的传递依赖。传递依赖是指某个非主属性通过另一个非主属性间接依赖于主键。

**例如：** 如果有一个 `employees` 表，其中包含 `employee_id`、`department_id` 和 `department_name`，并且 `department_name` 依赖于 `department_id`，而 `department_id` 又依赖于 `employee_id`，就存在传递依赖。

**解决方法：** 将 `department_name` 移到一个单独的表中，避免通过 `department_id` 的传递依赖。

示例：

```
// 原表
CREATE TABLE employee(
	employee_id INT PRIMARY KEY,
	name VARCHAR(100),
	department_id INT,
	department_name
);

//解决方法：将department_name  移动到一个新表
CREATE TABLE departments(
	department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
)

CREATE TABLE employees(
	employee_id INT PRIMARY_KEY,
	name VARCHAR(100),
	department_id INT
);
```



### 4.实用建议

在实际的 MySQL 数据库设计中，遵循范式的基本原则是很重要的，但有时也需要根据具体的应用场景作出适当的折衷。以下是一些关于 MySQL 范式的实用建议，帮助你在实际项目中做出更合理的数据库设计决策：

1. **遵循范式，但避免过度规范化**

- 遵循 1NF、2NF、3NF 是好的起点，但过度规范化可能导致复杂的查询和性能问题。
- 根据需求适度反规范化，提高查询效率。

2. **在高频读取场景中反规范化**

- 对于高频读取的系统，适度的反规范化可以减少 JOIN 操作，提升查询性能。
- 例如，存储汇总数据或合并表。

3. **合理使用索引**

- 为常用查询列建立索引，但避免频繁更新的字段加索引，以减少写入开销。

4. **避免过多外键约束**

- 外键保证数据完整性，但可能影响性能。可以在需要时使用外键，但避免过度依赖，特别是高并发场景。

5. **分区表和分库分表**

- 对于大数据量，使用分区表（按时间、ID 等）或分库分表，提高查询性能和系统可扩展性。

6. **适当的数据冗余**

- 在某些场景（如日志、搜索等），适当的数据冗余和缓存可以提高查询效率，避免频繁 JOIN。

7. **选择合适的存储引擎**

- 一般使用 **InnoDB** 引擎，支持事务、行级锁等，适用于大多数应用。
- 如果只读多写少，可选择 **MyISAM** 引擎。

8. **避免存储大量二进制数据**

- 将大文件（图片、视频等）存储在文件系统或对象存储服务中，数据库仅存路径。



### 5.不要对什么都建模

1. **不需要建模的临时数据**

- 如果数据是临时性的、瞬时性的，不需要持久化存储，比如一次性的计算结果或缓存数据，可以避免在数据库中创建表格。
- **例子**：某些计算结果、临时会话数据等

2. **不需要建模的日志数据（除非有高查询需求）**

- 处理日志或审计数据时，如果只是为了简单记录并不需要频繁查询或进行复杂分析，可以将日志数据存储为 **简单的文本文件** 或 **日志管理系统**（如 ELK Stack），而非创建数据库表。
- **例子**：访问日志、系统事件等。

3. **不需要建模的历史数据（如果不涉及频繁查询）**

- 有些历史数据如果不涉及频繁查询，且仅用于偶尔的归档或统计分析，不必在数据库中建立复杂的历史表。
- **例子**：过期数据、某些长时间不用的数据等。



### 6.模型的正向工程

​	模型的正向工程，是指数据库设计模型（ER图, 逻辑模型 ）到实际的数据库实现的过程。

**步骤**

**1.设计数据库模型:**

- 创建 ER 图 或逻辑模型。ER 图通常用来宝ER 图通常用来表示实体（表）、属性（字段）以及实体之间的关系（外键、关联等）。
- ER 图通常用来表示实体（表）、属性（字段）以及实体之间的关系（外键、关联等）。

**2.生成sql脚本**：

- 根据设计的模型生成 SQL 脚本，这些脚本包含创建数据库表的语句、字段定义、主键、外键、约束等。

- 生成的 SQL 脚本通常包括 `CREATE TABLE` 语句、`ALTER TABLE` 语句、以及其他可能的数据库对象（如视图、存储过程等）。

**3.执行sql脚本：**

- 将生成的 SQL 脚本执行到 MySQL 数据库中，从而创建数据库结构。

- 这个步骤通常在 MySQL 客户端或图形化管理工具（如 MySQL Workbench）中完成。



## 创建表

### 1.创建表

在 MySQL 中，创建表（`CREATE TABLE`）是数据库设计中的一个基本操作。通过 `CREATE TABLE` 语句，你可以定义表的名称、字段、数据类型、约束条件等。以下是创建 MySQL 表的基本语法和示例。

语法：

```
CREATE TABLE table_name(
	column1 datatype [constraints],
	column2 datatype [constraints],
	...,
	table_constraints
);
```

- `table_name`：表的名称。

- `column1`, `column2`, ...：表的列（字段）名称。

- `datatype`：字段的数据类型，如 `INT`, `VARCHAR`, `DATE` 等。
- [contraints] :  列的约束条件，如NOT NULL,UNIQUE, AUTO_INCREMENT等
- table_constrains : 表即约束，如 PRIMARY_KEY,FOREIGN_KEY,CHECK  等。



### 2.更改表

在MySQL中，可以使用 ALTER TABLE  语句来更改已经存在的表。这种操作允许你修改表的结构，比如添加，删除或修改字段，约束，索引等。

**常见的的ALTER TABLE操作：**

1.添加字段（ADD）

2.删除字段（DROP）

3.修改字段（MODIFY  或 CHANGE）

4.重命名（RENAME）

5.添加或删除约束（ADD CONSTRAINT 或 DROP CONSTRAINT）

基本语法：

```
ALTER TABLE table_name action
```

- table_name 要修改的表名
- action 表示要执行的操作，可以是ADD,DROP,MODIFY,RENAME等



### 3.创建关系

**创建关系**通常是指在多个表之间定义相互联系的方式，常见的关系包括：**一对多**、**多对多**、**一对一**。这些关系通常通过使用 **外键约束** 来建立。

1. **一对多关系（One-to-Many）**

一对多关系是最常见的关系类型。在这种关系中，一个表中的一行可以与另一个表中的多行相关联。

例子：

假设你有一个 **用户（Users）** 表和一个 **订单（Orders）** 表。每个用户可以有多个订单，而每个订单只属于一个用户。

- `Users` 表：用户信息。
- `Orders` 表：订单信息。

创建一对多关系：

在这种情况下，`Orders` 表会有一个外键字段 `user_id`，该字段引用 `Users` 表的 `user_id`。

```
//创建user表
CREATE TABLE Users(
	user_id INT AUTO_INCREMENT PRIMARY KEY,
	user_name VARCHAR(100) NOT NULL,
	emial VARCHAR(100) NOT NULL
);

//创建order 表，并建立一对多关系。
CREAT TABLE Order(
	order_id INT AUTO_INCREMENT PRIMARY KEY,
	user_id INT, 										//外键
	order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
	total_amount DECIMAL(10,2),
	FOREIGN KEY (user_id) REFERENCE User(user_id)		//外键约束
);
```

- `Orders` 表中的 `user_id` 字段是外键，引用了 `Users` 表的 `user_id` 字段。这就是一种 **一对多** 关系：一个用户可以有多个订单，但每个订单只对应一个用户。

**总结：**

- **一对多关系**：一个表的记录与另一个表的多条记录相关联，通常通过外键实现。
- **多对多关系**：两个表的多条记录相互关联，通常通过第三张关联表来实现。
- **一对一关系**：一个表的记录与另一个表的单条记录相关联，通常通过外键并加上唯一约束来实现。
- 在建立关系时，可以使用 **外键约束** 来确保数据的完整性和一致性，并可以根据需求设置删除和更新操作的规则。



### 4.更改主键和外键约束

​	主键是表中唯一标识每一行的字段或字段组合。在 MySQL 中，更改主键的操作通常包括删除现有主键并重新创建一个新的主键。

**更改主键**

- 删除现有主键

```
ALTER TABLE table_name DROP PRIMARY KEY;
```

- 添加新的主键

```
ALTER TABLE table_name ADD PRIMARY_KEY( column_name);
```

**更改外键约束**

- 删除现有的外键约束

```
ALTER TABLE table_name DROP FROEIGN KEY fk_usr_id;
```

- 添加新的外键约束

```
ALTER TABLE table_name ADD CONSTRAIN column_name FOREIGN KKEY (column_name2) REFERENCE table_name2 (column_name2)
```

**外键约束：**

- **`ON DELETE`**：当引用的行被删除时，如何处理外键关联的行。常见的选项有：
    - `CASCADE`：如果主表中的行被删除，外键表中的关联行也会被删除。

    - `SET NULL`：如果主表中的行被删除，外键表中的关联行的外键值将被设置为 `NULL`。

    - `RESTRICT`：不允许删除主表中的行，除非外键表中没有相关行。

- **`ON UPDATE`**：当主表中的外键值被更新时，如何处理外键表中的行。常见的选项有：

    - `CASCADE`：如果主表中的外键值被更新，外键表中的关联行的外键值也会更新。

    - `SET NULL`：如果主表中的外键值被更新，外键表中的关联行的外键值将被设置为 `NULL`。

示例：

```
-- 创建 Orders 表时添加外键约束，设置删除时级联删除
CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (user_id) REFERENCES Users(user_id) 
    ON DELETE CASCADE  -- 如果用户删除，订单也将被删除
);
```



### 5.字符集和排序规则

在 MySQL 中，**字符集（Character Set）** 和 **排序规则（Collation）** 是非常重要的概念，它们决定了如何存储和比较字符串数据。

1. **字符集（Character Set）**

字符集定义了字符如何以字节的形式存储在数据库中。每种字符集都包含一个或多个字符的集合。例如，UTF-8 是一个字符集，它支持全球几乎所有语言的字符。

常见的字符集：

- **`utf8`**：一个广泛使用的字符集，支持多种语言的字符，但不支持四字节字符（如一些 emoji 表情）。
- **`utf8mb4`**：是 `utf8` 的超集，支持完整的 Unicode 字符集，包括四字节字符（如 emoji）。
- **`latin1`**：西欧语言使用的字符集，包含大部分拉丁字母字符。
- **`gbk`**：用于简体中文的字符集。

2. **排序规则（Collation）**

排序规则定义了在排序、比较字符串时的规则。每个字符集有一个或多个相关的排序规则。排序规则决定了：

- 字符的比较规则（如：字母区分大小写或不区分大小写）。
- 字符的排序顺序（如：`A` 排在 `B` 前，`Z` 排在 `A` 后）。

例如，对于 `utf8` 字符集，常见的排序规则有：

- **`utf8_general_ci`**：不区分大小写的排序规则，比较时忽略字符的大小写。
- **`utf8_bin`**：区分大小写的二进制排序规则，比较时严格按照字节进行比较。

**常见字符集与排序规则示例**

| 字符集    | 排序规则            | 说明                                            |
| --------- | ------------------- | ----------------------------------------------- |
| utf8mb4   | utf8mb4_unicode_ci  | 支持所有 Unicode 字符，包括 emoji，大小写不敏感 |
| `utf8mb4` | `utf8mb4_bin`       | 支持所有 Unicode 字符，严格区分大小写           |
| `utf8`    | `utf8_general_ci`   | 兼容性较好的排序规则，大小写不敏感              |
| `latin1`  | `latin1_swedish_ci` | 适用于西欧语言，大小写不敏感                    |
| `latin1`  | `latin1_bin`        | 适用于西欧语言，严格区分大小写                  |
| gbk       | gbk_chinese_ci      | 适用于简体中文，大小写不敏感                    |



### 6.存储引擎

​	在 MySQL 中，**存储引擎（Storage Engine）** 是用于管理数据库中表数据存储、读取、更新和删除的模块。每种存储引擎有其不同的特性，如事务支持、表锁和行锁、索引类型等。MySQL 支持多种存储引擎，允许开发者根据不同的需求选择最合适的存储引擎。

1. **InnoDB**

    - **特点：**InnoDB 是 MYSQL 默认的存储引擎，支持 ACID(原子性，一致性，隔离性，持久性)事务，行级锁，外键约束和崩溃恢复等。
    - **事务支持：**支持ACID  和 自动提交 （默认）。
    - **数据一致性：**支持外键约束，确保数据的完整性。
    - **锁机制**：支持行级锁和多版本并发控制（MVCC），提高并发性能。
    - **适用场景：**适合需要高并发，事务管理和数据一致性的场景，如银行应用，电商系统等。

   ```
   CREATE TABLE example(
   	id INT PRIMARY KEY,
   	name VARCHAR(100),
   ) ENGINE = InnoDB;
   ```

2. **MyISAM**

    - **特点：**MyISAM是MySQL的经典存储引擎，较早的版本默认使用它。它不支持事务，外键和行级锁，但支持表级锁，全文索引等。
    - **事务支持：**不支持事务。
    - **数据一致性**：不支持外键约束。
    - **锁机制：**仅支持表级锁，操作时锁定整张表，可能影响并发性能。
    - **适用场景**：适合只需要读取性能较高而不需要事务或数据一致性的应用，如日志表、统计数据等。

   ```
   CREATE TABLE example(
   	id INT PRIMARY KEY,
   	name VARCHAR(100)
   )ENGINE = MyISAM;
   ```



## 第十四章：索引

### 1.介绍，高效的索引

​	MySQL 索引（Index）是一种用于加速数据库查询的技术，它通过创建数据表中的一组数据结构，使得在查询数据时可以更高效地查找特定的记录。索引可以让 MySQL 直接定位到数据的存储位置，避免了全表扫描，从而大大提高了查询性能。

​	**索引的基本概念**

- **索引的作用**：索引是数据库中的一种特殊的数据结构，它将表中一个或多个列的值映射到表中记录的物理位置。通过索引，MySQL 可以快速定位到符合条件的数据，而无需扫描整个数据表。
- **类比书籍的目录**：你可以把数据库表中的索引类比为书籍的目录。在一本书里，如果你要查找某个主题的内容，你不需要翻遍整本书，而是查阅目录并直接跳到相关章节。同样，数据库使用索引来帮助查询跳转到需要的数据，而不是扫描所有记录。



### 2.索引  | Indexs

​

### 3.创建索引

​	在MySQL中，索引是通过 CREATE INDEX 或在创建表时定义的。

**创建索引：**

```
CREATE INDEX idx_name ON table_name(column1,column2,...);
```

**创建唯一索引：**

```
CREATE UNIQUE INDEX idx_name ON table_name (column);
```

**创建全文索引**

```
CREATE FULLTEXT INDEX idx_name ON table_name(column);
```

**删除索引**

```
DROP INDEX idx_name ON table_name;
```



### 4.查看索引

语法：

```
SHOW INDEX FROM table_name;
```



### 5.前缀索引

**前缀索引**是指在创建索引时，并不对列中的整个字段进行索引，而是对列中的前一部分内容索引。通过这种方式，可以节省空间并提高索引创建和更新效率，特别是在类的内容较长时。

**前缀索引的创建**

前缀索引可以通过指定索引的长度来实现。具体来说，在创建索引时，可以在列后面指定索引的前缀长度，即只对该列的前 n 个字符进行索引。

语法：

```
CREATE INDEX idx_name ON table_name(column_name(prefix_length));
```

column_name: 要创建索引的列

prefix_length : 是要索引的字符数

例如：

- 假设有一个表 `users`，包含一个 `email` 字段，且该字段为 `VARCHAR(255)` 类型。
- 如果你只关心 `email` 字段的前 10 个字符，可以创建一个前缀索引：

```
CREATE INDEX idx_email_prefix ON users (email(10)); 
```



### 6.全文索引（在MySQL5.6版本后才支持）

​	MySQL 的 **全文索引**（Full-Text Index）是 MySQL 提供的一种特殊类型的索引，用于加速对文本内容的搜索。全文索引主要用于处理大文本字段中的单词搜索，尤其是在大量数据中进行复杂的文本检索时能显著提高查询效率。

**全文索引的特点**

​	**支持的列类型**：通常应用于 `CHAR`, `VARCHAR`, `TEXT` 类型的列。

​	**支持的查询方式**：全文索引使得 MySQL 支持 **自然语言** 或 **布尔模式** 搜索。

​	**不支持前缀匹配**：全文索引查找是基于单词的，而不是前缀或部分匹配。

​	**忽略停用词**：在进行全文索引查询时，MySQL 会自动忽略一些常见的停用词（如 "the"、"is" 等）。

**创建全文索引**

可以在创建表时或者后期通过 `ALTER` 语句来创建全文索引。

- 在创建表时创建全文索引

```
CREATE TABLE table_name(
	id INT PRIMARY KEY,
	title VARCHAR(100),
	body TEXT,
	FULLTEXT(title,body)
);
```

- 在表已创建后，添加全文索引

```
ALTER TABLE table_name ADD FULLTEXT(title,body);
```



**使用全文索引查询数据**

MySQL 提供了两种方式来使用全文索引进行查询：

**自然语言模式（Natural Language Mode）**

这是最常用的全文搜索模式，MySQL 会根据查询字符串中的词频来排名匹配结果。

```
SELECT * FROM table_name WHERE MATCH(title,body) AGAINST('MYSQL全文搜索')；
```

**布尔模式（Boolean Mode）**

布尔模式允许更多的控制，例如你可以使用加号 (`+`)、减号 (`-`)、星号 (`*`) 等符号来控制匹配的方式。比如：

- `+` 确保某个单词必须出现在结果中
- `-` 排除某个单词
- `*` 匹配前缀

例如，使用布尔模式来查找包含“mysql”但不包含“数据库”的记录：

```
SELECT * FROM table_name WHERE MATCH(title,body) ASAINST('+mysql -数据库' IN BOOLEAN MODE)
```



### 7.复合索引

​	在 MySQL 中，**复合索引**（Composite Index）是指在一个索引中包含多个列。它用于提高查询效率，尤其是对于涉及多个列的查询，复合索引可以显著减少数据库的查询时间。

**复合索引的特点**

- **多个列组合**：复合索引是将多个列作为一个索引进行存储和优化查询的。例如，索引可以由 `(column1, column2)` 组成，这样在查询时如果涉及到这两个列的条件，可以提高查询性能。

- **索引顺序**：复合索引中的列是有顺序的，MySQL 会按照索引列的顺序来使用索引进行优化查询。因此，列的顺序非常重要。

- **最左前缀原则**：MySQL 在使用复合索引时遵循“最左前缀原则”，也就是说，索引会首先根据复合索引的最左边的列进行过滤，之后再按顺序使用索引的其他列。因此，在设计复合索引时，列的顺序应考虑到查询条件的顺序。

- **覆盖索引**：如果查询只涉及复合索引的列，则 MySQL 可以使用覆盖索引来直接从索引中获取数据，而不需要回表查询，这样可以进一步提升查询性能。



**复合索引的使用场景**

复合索引适用于以下几种场景：

- **多列作为查询条件**：当查询涉及多个列时，可以创建复合索引来优化查询性能。
- **多列范围查询**：如果查询中的某些列是范围查询条件（例如 `>`、`<`、`BETWEEN` 等），复合索引仍然可以使用，但它的效率会受到影响。通常，在复合索引中，范围查询列应放在索引的最后。
- **排序查询**：如果查询中有 `ORDER BY` 子句，且排序的列与复合索引的列顺序一致，则可以利用复合索引来提高排序性能。

示例

假设有一个 `users` 表：

```
CREATE TABLE users (
    id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    city VARCHAR(100)
);
```

1).创建复合索引

假设你经常查询 `first_name` 和 `last_name`，并且这两个列经常作为查询条件组合出现，创建复合索引可以加速查询：

```
CREATE INDEX idx_name ON users(first_name,last_name);
```

2).查询优化

使用复合索引时，查询可以更高效地执行：

```
SELECT * FORM user WHERE first_name = 'john' AND last_name = 'smith';
```

MySQL 会使用 `idx_name` 索引来加速这条查询。

3). 最左前缀原则

如果复合索引是 `(first_name, last_name)`，那么以下查询都可以利用该索引：

```
-- 使用了最左列 first_name
SELECT * FROM users WHERE first_name = 'John';

-- 使用了最左两列 first_name 和 last_name
SELECT * FROM users WHERE first_name = 'John' AND last_name = 'Doe';
```



### 8.复合索引中的列顺序

​	在 MySQL 中，复合索引的列顺序是至关重要的，因为它直接影响到索引的使用效率。理解复合索引的列顺序规则，有助于我们更好地设计索引，优化查询性能。

**如何设计复合索引的列顺序？**

- **选择性高的列放在前面**：选择性高的列（即有较多不同值的列）应放在复合索引的前面，因为它能更有效地缩小结果集，从而提高查询效率。

  比如：如果 `first_name` 的选择性低（重复值多），而 `age` 的选择性高（不同年龄的用户较多），那么可以考虑将 `age` 放在前面，`first_name` 放在后面。

- **等值查询优先**：等值查询条件应优先放在复合索引前面，因为它们可以更好地利用索引来筛选数据。

- **范围查询列放后面**：范围查询的列应该放在索引的最后，这样可以最大化地使用最左前缀原则。例如，`WHERE col1 = ? AND col2 > ?`，可以将 `col1` 放在前面，`col2` 放在后面。



**复合索引的覆盖效应**

当查询只涉及复合索引中的列时，MySQL 可以通过 **覆盖索引** 来避免回表，从而提高查询效率。覆盖索引的效果与索引列顺序密切相关，通常会选择包含查询字段的前几列来优化性能。

示例：

```
sql


复制代码
SELECT first_name, last_name FROM users WHERE first_name = 'John' AND last_name = 'Doe';
```

如果存在一个复合索引 `(first_name, last_name)`，MySQL 可以直接通过索引返回查询结果，无需回表。

**总结**

- **复合索引的列顺序非常重要**，合理的列顺序可以极大地提高查询效率。
- **最左前缀原则**：查询条件中必须按复合索引列顺序使用，才能有效地利用索引。
- **范围查询列放在后面**，等值查询列应放在前面。
- 设计复合索引时，需要考虑查询条件中列的使用频率和选择性。

通过合理设计复合索引的列顺序，可以显著提高多条件查询的性能。



### 9.索引无效时

​	在 MySQL 中，索引是用于加速查询的工具，在某些情况下，MySQL 可能不会使用它。了解何时索引可能无效，可以帮助你优化查询和提高性能。

1）. **查询中没有使用索引列**

如果查询的条件没有涉及索引的列，MySQL 就不会使用索引。例如，创建了一个包含 `(col1, col2)` 的复合索引，但是查询中没有涉及 `col1` 或 `col2`。

**解决方法**：确保查询条件使用了索引的列，或者为查询列创建适当的索引。

2）. **索引列的数据类型与查询不匹配**

索引列的数据类型与查询条件的数据类型不匹配时，MySQL 可能无法使用索引。例如，如果索引列是 `INT` 类型，而查询条件使用了字符串类型，MySQL 可能无法利用索引。

**解决方法**：确保查询条件的数据类型与索引列的数据类型一致，避免隐式类型转换。

3）. **使用了 `OR` 操作符**

当查询条件中包含 `OR` 操作符时，MySQL 的索引可能无法有效地被利用，尤其是当 `OR` 连接的条件涉及不同的列时。MySQL 会尝试分别为每个条件使用索引，但是由于 `OR` 连接的特性，它可能无法结合多个索引进行优化。

示例：

假设有以下索引：

```
CREATE INDEX idx_col1 ON table(col1);
CREATE INDEX idx_col2 ON table(col2);
```

查询中包含 `OR` 操作符，可能导致索引无法充分利用：

```
-- 使用了 OR 操作符，可能导致索引无效
SELECT * FROM table WHERE col1 = 10 OR col2 = 20;
```

**解决方法**：如果可能，重写查询，避免使用 `OR`，或确保查询条件涉及单个索引。

4）. **范围查询后面的列不能用索引**

在复合索引中，如果查询条件包含范围查询（如 `BETWEEN`、`>`, `<` 等），则范围查询后的列无法使用索引，因为 MySQL 已经无法对范围查询之后的列进行有效筛选。

**解决方法**：尽量避免将范围查询的列放在复合索引中的前面，或者将范围查询的列放在查询条件的最后。

5）.**使用 `LIKE` 查询时，通配符的位置**

`LIKE` 查询可以使用索引，但这取决于通配符的位置。如果通配符出现在字符串的开头，索引就不能被有效利用。

**解决方法**：避免在查询的 `LIKE` 条件中将通配符放在开头。可以使用全文索引（FULLTEXT）或者优化查询条件。

6).**使用了 `DISTINCT`、`GROUP BY` 或 `ORDER BY`**

在某些情况下，使用 `DISTINCT`、`GROUP BY` 或 `ORDER BY` 会导致索引的使用受到限制，尤其是当这些操作涉及多个列或复杂的排序条件时。

**解决方法**：确保 `ORDER BY` 或 `GROUP BY` 中的列顺序与索引的顺序一致。如果涉及多个列的排序，考虑调整索引或查询的排序条件。

...

**总结**

MySQL 索引可能失效的原因包括：

- 查询条件未涉及索引列。
- 索引列的数据类型与查询条件不匹配。
- 使用了 `OR` 操作符。
- 使用了范围查询后的列。
- 使用了 `LIKE` 查询时，通配符在字符串的开头。
- 排序、分组、去重等操作与索引列顺序不一致。
- 表数据量过小，导致不使用索引。
- 索引设计不当，导致索引选择性低或无效。

为了提高查询效率，理解索引失效的原因，并根据实际查询场景设计合适的索引策略。



### 10.使用索引排序

​	在 MySQL 中，使用索引进行排序（`ORDER BY`）是优化查询性能的一种方式。MySQL 会尽量利用索引来排序数据，从而减少排序操作的开销。

**索引排序的基本原理**

索引可以帮助 MySQL 在查询时更高效地进行排序，尤其是当查询结果已经按照索引顺序存储时。例如，使用 `B-tree` 索引时，MySQL 可以直接从索引中获取数据，避免了额外的排序操作。

- 如果查询涉及的列已经在索引中，并且排序顺序与索引的顺序一致，MySQL 可以直接通过索引返回结果，无需进行额外的排序。
- 如果查询中的排序列和索引列顺序匹配，并且不涉及范围查询，MySQL 也可以利用索引排序。

**如何优化使用索引排序**

- 确保查询中的排序与索引顺序一致

- 避免在索引列之后添加范围查询

- 利用覆盖索引



### 11.覆盖索引

​	覆盖索引（Covering Index）是指一个索引包含了查询所需的所有列，允许数据库在检索数据时只通过索引而不需要回表（即不需要访问实际的数据行）。这样可以提高查询性能，特别是在涉及大表时。

**覆盖索引的优点**

1. **减少 I/O 操作**：由于查询只需要访问索引，而无需访问数据行，减少了磁盘 I/O 操作，从而提高了查询效率。
2. **提高查询速度**：因为索引通常比数据表小，所以读取索引的速度更快。
3. **优化性能**：在某些情况下，使用覆盖索引可以显著提高查询性能，尤其是在 SELECT 查询中。

**示例**

假设你有一个名为 `employees` 的表，包含以下列：`id`, `name`, `age`, `salary`。如果你经常执行以下查询：

```sql
SELECT name, age FROM employees WHERE salary > 50000;
```

为了利用覆盖索引，你可以创建一个包含 `salary`, `name`, `age` 的索引：

```sql
CREATE INDEX idx_salary_name_age ON employees(salary, name, age);
```

在这个例子中，`idx_salary_name_age` 是一个覆盖索引，因为它包含了查询中使用的所有列（`name`, `age`）以及用于过滤的列（`salary`）。因此，执行上述查询时，MySQL 可以直接从索引中获取所需的数据，而不需要访问 `employees` 表的实际行。

**注意事项**

1. **索引大小**：创建覆盖索引会增加索引的大小，可能会影响写入性能（INSERT、UPDATE、DELETE）。
2. **选择性**：覆盖索引对高选择性的列更有效，低选择性的列可能不适合创建覆盖索引。
3. **维护成本**：每次对表进行修改时，相关的索引也需要更新，这会增加维护成本。



### 12.维护索引

​	索引是提高查询性能的关键工具，但是随着时间的推移和数据的变化，索引可能会变得不再高效，甚至引发性能问题。为了保持索引的高效性和数据库的整体性能，需要对索引进行定期的维护。

1). **索引维护的基本目标**

- **确保索引有效性**：检查索引是否仍然对查询有帮助，避免过时的索引。
- **优化查询性能**：定期重新组织或重建索引，以优化查询性能。
- **减少磁盘空间的浪费**：避免索引碎片化导致的空间浪费。

2). **索引的常见问题**

1. 索引碎片
    - 随着数据的插入、更新和删除，索引的结构可能会变得碎片化，从而影响查询性能。索引碎片的存在会导致扫描的效率下降。
2. 冗余索引
    - 随着数据库的演变，可能会存在多个冗余的索引，或者一些索引不再被查询使用。这会占用不必要的磁盘空间并降低性能。
3. 无效索引
    - 一些索引可能在查询中未被使用，或者它们的选择性太低，导致不再提供性能提升。

3).**索引的维护策略**

**3.1使用 OPTIMIZE TABLE 重建索引**

MySQL 提供了 `OPTIMIZE TABLE` 语句来重建表和索引，通常在数据删除和更新频繁时使用。它通过对表进行整理，压缩碎片来优化存储空间和提升性能。

```
OPTIZIME TABLE table_name;
```

**功能**：

- 重建表和索引，压缩碎片。
- 对 InnoDB 表进行在线操作，表数据和索引都会被整理并存储在新的表中，原表会被删除。
- 对 MyISAM 表进行优化时，会重建数据文件和索引文件。

**注意事项**：

- `OPTIMIZE TABLE` 会锁定表，因此在大表上执行时，可能会造成较大的性能开销，尤其是在线系统上。
- 适用于数据删除量较大的表，或者索引碎片比较严重的表。

**3.2** **使用 `ANALYZE TABLE` 更新索引统计信息**

`ANALYZE TABLE` 用于更新表的索引统计信息，这有助于优化查询计划。

```
ANALYZE TABLE table_name;
```

- 功能
    - 使 MySQL 更新该表索引的统计信息，帮助查询优化器选择最佳的执行计划。
    - 在索引被大量修改后，建议运行 `ANALYZE TABLE`，以确保查询优化器能基于最新的统计信息做出决策。
- 注意事项
    - 对于大表，运行 `ANALYZE TABLE` 可能需要一些时间，尤其是在更新大量数据后。

**3.3** **删除不再使用的索引**



## 第十五章：数据管理

### 1.保护数据库

​	保护数据库的目标是确保数据的安全性、完整性以及防止未授权访问或操作。为了实现这些目标，MySQL 提供了一系列安全机制和配置选项。数据库的保护不仅包括数据访问控制，还包括数据备份、加密、审计以及防止攻击的措施。



### 2.创建一个用户

使用 `CREATE USER` 命令创建用户

​	示例：

```
CREATE USER 'NEW_USER'@'LOCALHOST' IDENTIFIED BY 'password';
```



### 3.查看用户

​	在 MySQL 中，查看数据库用户的信息主要通过查询系统表（如 `mysql.user`）来实现。系统表 `mysql.user` 存储了关于 MySQL 用户的信息，包括用户权限、主机、认证方法等。你可以使用查询命令来查看用户列表及其相关信息。

1.查看所有用户：

要查看所有的用户，可以查询mysql.user 表

示例

```
SELECT user,host FROM mysql.user;
```

#### 示例输出：

```
sql复制代码+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | localhost |
| myuser           | %         |
| admin            | 192.168.1.1 |
+------------------+-----------+
```

- `root` 用户只允许从 `localhost` 连接。
- `myuser` 用户可以从任意主机 (`%` 表示任何主机) 连接。
- `admin` 用户只允许从特定 IP 地址 `192.168.1.1` 连接。



### 4,删除用户

​	在mysql中，删除用户的的指令通常为 DROP USER 命令。这个命令通常可以删删一个或多个指定的MySQL用户，移除其所有权限，与数据库的关联以及登录访问权限。删除用户时候需要确保用户没有正在使用，否则删除可能会失败。

- 删除单个用户

如果你要删除一个特定的用户，可以使用 `DROP USER` 命令。例如，删除 `user1` 用户：

```
DROP USER 'user'@'localhost';
```

- 删除多个用户

```
DROP USER 'user1'@'localhost','user2'@'localhost' 
```

- 检查是否删除成功

```
SELECT user,localhost FROM mysql.user;
```



### 5.修改密码

​	常见的方法包括使用 `ALTER USER` 命令（推荐方式）和 `SET PASSWORD` 命令。以下是几种修改 MySQL 用户密码的常见方法

- 使用ALTER USER 修改密码 (推荐)

ALTER USER 命令在MySQL5.7 及以上版本中被推荐用于修改用户的密码。

```
ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';
```



- **使用 `SET PASSWORD` 修改密码**（适用于 MySQL 5.6 及更早版本）

```
SET PASSWORD FOR 'username'@'host'= PASSWORD('new_password');
```



### 6.授予权限

​	在 MySQL 中，授予用户权限的操作通常是通过 `GRANT` 语句来实现的。`GRANT` 命令允许你为一个或多个用户指定对特定数据库、表、列或其他对象的访问权限。你可以通过不同的方式和级别来控制用户的权限，例如授予对数据库、表、存储过程、视图等的访问权限。

语法：

```
GRANT privileges ON database_object TO 'username'@'host'
```

- privileges :要授予的权限类型

- database_object: 权限授予的对象，可以是数据库，表，列，或者特定的数据库对象。
- username: 授予权限的mysql用户名
- host : 指定用户可以从那些主机连接数据库/

**常见的权限类型**

**ALL PRIVILEGES**：授予用户所有可用的权限（包括权限管理权限）。

**SELECT**：允许用户查询数据。

**INSERT**：允许用户插入数据。

**UPDATE**：允许用户更新数据。

**DELETE**：允许用户删除数据。

**CREATE**：允许用户创建数据库、表、索引等对象。

**DROP**：允许用户删除数据库、表、视图等对象。

**ALTER**：允许用户修改表结构。

**INDEX**：允许用户创建和删除索引。

**GRANT OPTION**：允许用户授予或撤销权限。

**CREATE TEMPORARY TABLES**：允许用户创建临时表。

**EXECUTE**：允许用户执行存储过程或函数。

**SHOW DATABASES**：允许用户查看数据库列表。



### 7.查看权限

​	在 MySQL 中，查看用户的权限可以通过几种方式实现。最常用的方法是使用 `SHOW GRANTS` 命令来查询某个用户的权限，或者直接查询系统表 `mysql.user` 来获取详细的权限信息。

语法：

```
show GRANT FOR 'user.name'@'host';
```

- `'username'`：要查询权限的用户名。

- `'host'`：用户所在的主机，可以是 `localhost`、`%`（表示任何主机）等。

**查看当前用户的权限**

如果你想查看当前连接的 MySQL 用户的权限，可以使用 `SHOW GRANTS` 不带任何参数：

```
SHOW GRANTS
```

这会显示当前登录用户的所有权限。



### 8.撤销权限

在 MySQL 中，撤销权限是通过 `REVOKE` 命令来实现的。`REVOKE` 命令可以撤销某个用户在特定数据库、表或列上的权限。撤销权限时，你可以选择撤销某个具体的权限（如 `SELECT`、`INSERT`、`UPDATE`）或者撤销某个用户的所有权限。

语法：

```
REVOKE privileges ON database_object FROM 'username'@'host';
```

**`privileges`**：要撤销的权限类型，可以是 `ALL PRIVILEGES` 或具体的权限（如 `SELECT`、`INSERT`、`UPDATE` 等）。

**`database_object`**：权限撤销的对象，可以是数据库（`database_name.*`）、特定表（`database_name.table_name`）或特定列。

**`username`**：要撤销权限的 MySQL 用户名。

**`host`**：用户所在的主机，可以是 `localhost` 或 `%`（表示从任何主机）。

**撤销具体权限**

如果你只想撤销某个用户的特定权限（如 `SELECT`、`INSERT` 等），可以在 `REVOKE` 命令中指定权限类型。例如：

示例 1：撤销 `user1` 用户在 `mydb` 数据库上 `SELECT` 权限

```
REVOKE SELECT ON mydb.* FROM 'user1'@'%';
```

示例 2：撤销 `user1` 用户在 `mydb` 数据库上 `INSERT` 和 `UPDATE` 权限

```
REVOKE INSERT, UPDATE ON mydb.* FROM 'user1'@'%';
```

示例 3：撤销 `user1` 用户在 `mydb` 数据库上对特定表的 `DELETE` 权限

```
REVOKE DELETE ON mydb.customers FROM 'user1'@'%';
```

示例 4：撤销 `user1` 用户对某个特定列的权限

```
REVOKE SELECT(name) ON mydb.customers FROM 'user1'@'%';
```







## 数据库常见面/笔试题

### **1、关系型数据库 和 和 非关系型数据库的区别？**

（1）、关系型数据库通常使用表结构来存储数据，非关系型数据库不使用表结构存储数据，支持多种数据模型如 键值对，文档等。。。

（2）、关系型的数据库支持事务（ACID）,非关系型的数据库不支持事务。

（3）、关系型数据库使用  SQL 进行数据查询。非关系数据库使用特定的接口和API进行数据查询。 关系型数据库 有 MYSQL, Oracle ,SQL server  ，非关系型数据库有MongoDB , Redis。



### **2、MyISAM  和 InnoDB 的区别？**

MyISAM 和 InnoDB 都是 mysql 常用的存储引擎。1、InnoDB 支持事务ACID，（原子性，一致性，隔离性，持久性）。MyISAM 不支持ACID。 2、InnoDB支持行级锁，MyISAM仅支持表级锁。3、InnoDB支持外键约束，通过外键约束在表和表之间建立联系，确保了表的一致性和完整性。MyISM不支持外键约束。



### **3、mvcc**

参考链接：[MVCC多版本并发控制原理总结（最终版） - 郭慕荣 - 博客园](https://www.cnblogs.com/jelly12345/p/14889331.html)

1）、**什么是mvcc ，为什么要有mvcc， mvcc解决什么问题？**

​	mvcc（multi-version- Concurrency -Control）全称多版本并发控制，mvcc在 MySQL 的 InnoDB中实现是为了提高数据库并发性能。用更好的方式取处理  读-写 问题，做到即使有 读-写并问题时，也能不加锁，非阻塞并发读。



2）、**什么是read view 什么是undolog？ readview存储哪些数据代表什么含义？ undolog日志的意义是什么？**

read view  是 InnoDB用于管理事务读取数据的一种机制，它确保了事务在开始时候，只能看到事务开时已提交了的数据。避免未提交事务对当前事务的影响。

readview 存储了 **最小事务id，** **最大事务id**， **活跃事务列表id列表**（正在执行的事务，尚未提交数据的事务id集合），和 **当前事务id**（创建当前事务 readview的事务id ）。

- m_ids,当前有哪些事务正在执行，且还没有提交，这些事物的id就会存在这里；
- min_trx_id,是指m_ids 里的最小值；
- max_trx_id,是指下一个要生成的事务的id。下一个要生成的事务id肯定要比现在所有的id都大。
- creator_trx_id, 每开启一个事务就会生成一个readview，而creator_trx_id就是开启这个事务的id。

参考连接：[MVCC实现原理之ReadView(一步到位)-CSDN博客](https://blog.csdn.net/m0_62436868/article/details/127202062#:~:text=Read View是一个数据库的内部快照，该快照被用于InnoDB存储引擎中的MVCC机制。 简单点说，Read View就是一个快照，保存着数据库某个时刻的数据信息。,Read View会根据事务的隔离级别决定在某个事务开始时，该事务能看到什么信息。 就是说通过Read View，事务可以知道此时此刻能看到哪个版本的数据记录（有可能不是最新版本的，也有可能是最新版本的）。)



undolog 是一种日志文件，它记录了数据库的修改操作，用于实现数据库的回滚操作。undolog 会记录事务修改前的前一个版本，从而可以实现被修改数据的回滚操作。



3）、**undolog日志什么时候生成新数据？**

- 插入（Insert）:    生成撤销日志，记录删除插入记录前的操作。
- 更新（Update）：生成撤销日志，记录更新操作前的旧数据。
- 删除（delete）： 生成撤销日志，记录删除操作前的旧数据。



4）、**readview什么时候产生什么时候消失？他的生命周期和不同事务隔离级别有关系吗？**

readview 在事务创建时并且执行读操作时候动态创建，在事务提交或者事务回滚时，事务消失，生命周期结束。

RU 隔离级别下，readview会读取其他事务尚未提交的数据，允许脏读。

RC隔离级别下，readview 每读取一次数据，就会重新生成一个readview。

RR隔离级别下，事务保持开始的数据库视图，直到事务结束。避免不可重复读问题。

序列化隔离级别下，事务对数据访问的控制更加严格，readview 也更为严格，确保了数据的一致性。



5）、**mysql是如何解决脏读问题和幻读问题的？**

MySQL 通过设置  RC 隔离级别来防止脏读问题的发生。通过设置 串行化（Serizliazable）隔离级别来避免幻读，在 RR隔离级别下，通过设置间隙锁（锁定一个范围，避免其他事务插入新的数据行） 和 Next-key lock（使用行锁 + 间隙锁 锁定了数据行和 前后的 “间隙”） 来避免 幻读。



6）、**mysql是如何实现不同隔离级别的？**

RU:  不使用任何的锁 和 隔离机制来控制，允许事务读取未提交的数据。

RC:  通过 MVCC（多版本并发控制）来确保事务只能读取到已提交的数据。

RR:  解决不可重复读问题 — MySQL 会通过MVCC 来确保在事务的执行期间， 读取的 数据行版本 不会被其他事务修改或删除。  幻读问题 — 通过 Next - key lock  来解决幻读问题。（行锁  + 间隙锁）  间隙锁：防止其他事务插入数据到当前符合查询条件的记录。



7）、什么是**快照读** 什么是**当前读**？分别解释概念和举出对应sql例子？

**快照读** 是事务读取某个时间点的 **一致性快照**,读取的数据是事务开始时候的数据快照，它不会受到其他事务修改数据未提交的影响。readview 记录在事务开始时已提交事务的数据。

```
 --事务A执行快照读
 START TRANSCACTION
 SELECT * FROM employeeS WHERE salary > 5000;
 ---事务B插入数据
 INSERT INOT employees(name,salary) valuse("张三",6000);
 --事务A 继续执行查询，任然会看到刚开始的数据。
 SELECT * FROM employees WHERE salary > 5000;
 
 COMMIT;
```

**当前读**指的是事务读取到的内容是当前数据库的实时内容，可能会受到其他事务修改数据提交的影响，一般通过加行锁来保证当前读的数据不发生变化。

```
--事务A 执行当前读 锁定当前查询的记录
START TRANSCATION
SELECT * FROM employees WHERE id = 101 FOR UPDATE;
--事务B修改数据会被阻塞，直到事务A提交 或者 回滚。
UPDATE employees SET salary = 7000 WHERE id = 101;
COMMIT
```



8）、**mysql如何解决幻读问题的？mysql完全解决幻读问题了吗？如果没有。什么场景解决，什么场景没有解决？**

在序列化（Serializable）隔离级别下，幻读问题得到了完全的解决。在 RR 隔离级别下，通过设置间隙锁来减少了幻读的发生，但有些场景仍然有幻读问题。

例如1、跨页查询或者范围查询。2、没有锁定整个查询范围。



9）、**请详细解释一下mvcc根据read view和undolog匹配数据的流程？**

readview 是事务创建时候的快照，它决定了事务对数据行的那些版本可见。readview 根据事务id 和 已提交的事务id，来判断那些数据是可读取的。

undolog 是事务修改历史数据前的记录，undolog 允许InnoDB 在查询时回溯数据的历史版本。帮助确定当前事务能否看到的某个版本的数据。



### **4、什么是索引下推？**

传统的方式是数据库先通过索引查找数据，然后对结果进行过滤。**索引下推**则是将过滤条件（WHERE子句）直接应用到扫描过程中，只扫描符合条件的数据。

```
SELECT * FROM users WHERE age > 30;
```

使用索引下推时，数据库会直接通过 age 索引过滤除 age > 30 的记录，避免全表扫描。



### **5、你怎么判断联合索引走那一部分？**

通过 EXPLAIN 输出，你可以判断查询走了那些字段。

1. **查看key列：**key 列显示了查询实际使用的索引名。
2. **查看key_len列：** 显示了查询实际使用的索引长度。该长度以字节为单位，反应了联合索引中被使用到的列数。
3. **判断使用了联合索引中的那些列：**一般遵循最左匹配原则 + key_len 的字节数反应了使用了索引的多少列。
4. **检查 Extra列：**如果显示 **Using where**，表示在结果上应用了 where 条件过滤。如果显示 Using index，表示查询完全由索引覆盖，不需要回表。
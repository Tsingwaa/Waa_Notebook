SQL是Structured Query Language（结构化查询语言）的缩写。SQL是一种专门用来与数据库沟通的语言。

SQL语句有如下的优点：

1. SQL不是某个特定数据库供应商专用的语言。几乎所有重要的DBMS（数据库管理系统）都支持SQL，所以学习此语言使你几乎能与所有数据库打交道。
SQL简单易学。它的语句全都是由很强描述性的英语单词组成，而且这些单词的数目不多。
2. SQL虽然看上去很简单，但实际上不是一种强有力的语言，灵活使用其语言元素，可以进行非常复杂和高级的数据库操作。

# 一、基础概念

## 数据库术语

- **数据库（database）**：保存结构性数据的容器（通常为一个文件或一组文件）
- **数据表（table）**：某种特定类型数据的结构化清单。
- **模式（schema）**：关于数据库和表的布局及特性的信息。模式定义了数据在表中如何存储，包含存储什么样的数据，数据如何分解，各部分信息如何命名等信息。数据库和表都有模式。
- **列（column）**：表中的一个字段，所有表都是由一个或多个列组成。
- **行（row)**：表中的一个记录
- **主键（primary key）**：一列（或一组列），其值能唯一标识表中每一行。

## SQL语法

SQL（Structured Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

## SQL语法结构

- **子句**：是语句和查询的组成成分。（在某些情况下，这些都是可选的。）
- **表达式**：可以产生任何标量值，或由列和行的数据库表
- **谓词**：给需要评估的 SQL 三值逻辑（3VL）（true/false/unknown）或布尔真值指定条件，并限制语句和查询的效果，或改变程序流程。
- **查询**：基于特定条件检索数据。这是 SQL 的一个重要组成部分。
- **语句**：可以持久地影响纲要和数据，也可以控制数据库事务、程序流程、连接、会话或诊断。

## 注意事项

- SQL语句不区分大小写，如SELECT、Select和select是等价的；
- 多条SQL语句必须以分号（;）分隔；
- SQL语句中，所有多余空格都被忽略。一行的SQL也可写成多行；
- 数据库表名、列名和值是否区分大小写，依赖于具体的DBMS以及配置；

# 二、SQL语言

## 数据定义语言(Data Definition Language, DDL)

> 负责数据结构定义与数据库对象定义

核心指令为 `CREATE`、 `ALTER`、 `DROP`。

- 数据库 DATABASE

    ```SQL
    CREATE DATABASE test;  -- 创建数据库
    DELETE DATABASE test;  -- 删除数据库
    USE test;  -- 选择数据库
    ```

- 数据表 TABLE

    ```SQL
    -- 普通创建数据表
    CREATE TABLE user(
        id int(10) unsigned NOT NULL COMMENT 'Id',
        username varchar(64) NOT NULL DEFAULT 'default' COMMENT '用户名',
        password varchar(64) NOT NULL DEFAULT 'default' COMMENT '密码',
        email varchar(64) NOT NULL DEFAULT 'default' COMMENT '邮箱',
    ) COMMENT='用户表';  

    -- 根据已有表创建数据表
    CREATE TABLE vip_user AS
    SELECT * FROM user;

    -- 删除数据表
    DROP TABLE user;

    -- 修改数据表
    ALTER TABLE user
    -- 添加列
    ADD age int(3);
    -- 删除列
    DROP COLUMN age;
    -- 修改列
    MODIFY COLUMN age tinyint;
    -- 添加主键
    ADD PRIMARY KEY (id);
    -- 删除主键
    DROP PRIMARY KEY;
    ```

- 视图 VIEW

    基于SQL语句的结果集的可视化的虚拟表，本身不包含数据，也就不能对其进行索引操作。对视图的操作和对普通表的操作一样。

    ```SQL
    -- 创建视图
    CREATE VIEW top_10_user_view AS
    SELECT id, username
    FROM user
    WHERE id < 10;
    -- 删除视图
    DROP VIEW top_10_user_view

- 索引 INDEX
  - 作用：通过索引可以更加快速高效地查询数据；用户无法看到索引，他们只能被用来加速查询。
  - 注意：更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

  ```SQL
  -- 创建索引
  CREATE INDEX user_index
  on user (id);

  -- 创建唯一索引
  CREATE UNIQUE INDEX user_index
  on user (id);

  -- 删除索引
  ALTER TABLE user
  DROP INDEX user_index;
  ```

- 约束：规定表中的数据规则
  - 如果存在违反约束的数据行为，行为会被约束终止。
  - 约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。
  - 约束类型：
    - NOT NULL：指示某列不能存储 NULL 值。
    - UNIQUE：保证某列的每行必须有唯一的值。
    - PRIMARY KEY：NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
    - FOREIGN KEY：保证一个表中的数据匹配另一个表中的值的参照完整性。
    - CHECK：保证列中的值符合指定的条件。
    - DEFAULT：规定没有给列赋值时的默认值。

  ```SQL
  CREATE TABLE Users (
  Id INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '自增Id',
  Username VARCHAR(64) NOT NULL UNIQUE DEFAULT 'default' COMMENT '用户名',
  Password VARCHAR(64) NOT NULL DEFAULT 'default' COMMENT '密码',
  Email VARCHAR(64) NOT NULL DEFAULT 'default' COMMENT '邮箱地址',
  Enabled TINYINT(4) DEFAULT NULL COMMENT '是否有效',
  PRIMARY KEY (Id)
  ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='用户表';

  ```

## 事物控制语言(Transaction Control Language, TCL)

> 负责管理数据库中的事务，管理由DML语句所做的更改，还允许将语句分组为逻辑事务。

核心指令是 `COMMIT`、`ROLLBACK`。

```SQL
-- 开始事务
START TRANSACTION;

-- 插入操作 A
INSERT INTO `user`
VALUES (1, 'root1', 'root1', 'xxxx@163.com');

-- 创建保留点 updateA
SAVEPOINT updateA;

-- 插入操作 B
INSERT INTO `user`
VALUES (2, 'root2', 'root2', 'xxxx@163.com');

-- 回滚到保留点 updateA
ROLLBACK TO updateA;

-- 提交事务，只有操作 A 生效
COMMIT;
```

## 数据控制语言(Data Control Language, DCL)

> 一种可对数据访问权进行控制的指令，可以控制特定用户账户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制权。

核心指令是 `GRANT`、`REVOKE`。

```SQL

```

DCL以控制用户的访问权限为主，因此其指令作法并不复杂。
利用DCL控制权限的指令有：CONNECT、SELECT、INSERT、UPDATE、DELETE、EXECUTE、USAGE、REFERENCES。

## 数据操纵语言(Data Manipulation Language, DML)

> 负责对数据库其中的对象和数据运行访问工作;

核心指令是 `INSERT`、`DELETE`、`UPDATE`、`SELECT`，即增删改查。

### 1. 插入数据 INSERT

- 向表内插入完整的新行

    ```SQL
    INSERT INTO user_tb
    VALUES (10, 'root', 'xxx@163.com');
    ```

- 向表内插入行的一部分

    ```SQL
    INSERT INTO user_tb(username, password, email) 
    VALUES ('admin', 'admin', 'xxx@163.com');
    ```

- 向表内插入查询的数据

    ```SQL
    INSERT INTO user_tb(username)
    SELECT name 
    FROM account_tb;
    ```

### 2. 删除数据 DELETE

- 删除表中的指定数据

    ```SQL
    DELETE FROM user_tb
    WHERE username='root';
    ```

- 清空表中的数据

    ```SQL
    TRUNCATE TABLE user_tb;
    ```

### 3. 更新数据 UPDATE

```SQL
UPDATE user_tb
SET username="robot', password='robot'
WHERE username='root';
```

### 4. 查询数据 SELECT

- 查询单列

    ```SQL
    SELECT prod_name
    FROM products_tb;
    ```

- 查询多列

    ```SQL
    SELECT prod_name, prod_id, prod_price
    FROM products_tb;
    ```

- 查询所有列

    ```SQL
    SELECT *
    FROM products_tb;
    ```

- 查询不同的值
    > DISTINCT 用于返回唯一不同的值。它作用于所有列，也就是说所有列的值都相同才算相同。

    ```SQL
    SELECT DISTINCT prod_id
    FROM products_tb;
    ```

- 根据限制条件查询结果
    > LIMIT限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。

    ```SQL
    -- 返回前5行
    SELECT * FROM product_tb LIMIT 5;
    SELECT * FROM product_tb LIMIT 0, 5;
    -- 返回第3～5行，即返回2，3，4行
    SELECT * FROM product_tb LIMIT 2, 3;
    ```

- 子查询
    > 可嵌套使用，先内层再外层
  - 过滤记录 WHERE

    ```SQL
    -- SELECT
    SELECT * FROM products_tb
    WHERE prod_name = 'soup';
    -- UPDATE
    UPDATE products_tb
    WHERE prod_name = 'soup';
    -- DELETE 
    DELETE FROM products_tb
    WHERE prod_name = 'soup';
    ```

  - 条件运算符: =, <>/!=, >, <, >=, <=, IN, BETWEEN, LIKE

    ```SQL
    -- IN
    SELECT * FROM products_tb
    WHERE prod_name IN ('soup', 'cake');

    -- BETWEEN
    SELECT * FROM products_tb
    WHERE prod_price BETWEEN 3 and 5;

    -- LIKE: 模式匹配运算符
        -- 百分号 % 表示任意字符出现任意次数
        -- 下划线 _ 表示任何字符出现一次
    -- 注意：不要滥用通配符，通配符位于开头，匹配会非常慢
    SELECT prod_id, prod_name, prod_price
    FROM products_tb
    WHERE prod_name LIKE '%bean bag%';
    ```

  - 逻辑运算符: AND, OR, NOT

    ```SQL
    -- AND/OR: AND优先级高于OR
    SELECT *
    FROM products_tb
    WHERE (prod_name = "soup") AND/OR (prod_name = 'cake')

    -- NOT
    SELECT *
    FROM products_tb
    WHERE prod_id NOT BETWEEN 3 and 5
    ```

### 5. 连接 JOIN

基于不同表的共同字段，将来自多个表的行结合起来

- (内)连接 (INNER) JOIN：如果表中有至少一个匹配，则返回行；
- 左连接 LEFT JOIN：返回左表所有行，以及右表中的匹配行；
- 右连接 RIGHT JOIN：返回右表所有行，以及左表中的匹配行；

```SQL
SELECT column_name(s)
FROM table1 
INNER/RIGHT/LEFT JOIN table2
ON table1.column_name = table2.column_name
```

### 6. 组合 UNION

用于合并多个SELECT语句的结果。

值得注意:

- 每个SELECT语句必须拥有相似的数据类型
- 每个SELECT语句中，列的顺序必须相同。

```SQL
-- UNION: 默认选取不同的值（可理解为Python的set.union）
SELECT column_name(s) FROM table1
UNION 
SELECT column_name(s) FROM table2;

-- UNION ALL: 允许重复值（可理解为集合的并集）
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

### 7.排序 ORDER

- ASC(asecending，升序，默认)
- DESC(desecending，降序)

```SQL
-- 按多个列进行排序，并且为每个列指定不同的排序方式
SELECT * FROM products_tb
ORDER BY prod_price DESC, prod_name ASC;
```

### 8. 分组 GROUP

```SQL
SELECT cust_name, COUNT(cust_address) AS addr_num
FROM Customers GROUP BY cust_name;
```

### 9. HAVING

对汇总的GROUP结果进行过滤

```SQL
-- 使用WHERE和HAVING过滤数据
SELECT cust_name, COUNT(*) AS num
FROM Customers
WHERE cust_email IS NOT NULL
GROUP BY cust_name
HAVING COUNT(*) >= 1;
```

# Reference

- [SQL语法速成手册-掘金](https://juejin.cn/post/6844903790571700231)

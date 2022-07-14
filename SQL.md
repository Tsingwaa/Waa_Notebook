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

## SQL语法：
SQL（Structured Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

## SQL语法结构
- **子句**：是语句和查询的组成成分。（在某些情况下，这些都是可选的。）
- **表达式**：可以产生任何标量值，或由列和行的数据库表
- **谓词**：给需要评估的 SQL 三值逻辑（3VL）（true/false/unknown）或布尔真值指定条件，并限制语句和查询的效果，或改变程序流程。
- **查询**：基于特定条件检索数据。这是 SQL 的一个重要组成部分。
- **语句**：可以持久地影响纲要和数据，也可以控制数据库事务、程序流程、连接、会话或诊断。

## SQL语法注意事项
- SQL语句不区分大小写，如SELECT、Select和select是等价的；
- 多条SQL语句必须以分号（;）分隔；
- SQL语句中，所有多余空格都被忽略。一行的SQL也可写成多行；
- 数据库表名、列名和值是否区分大小写，依赖于具体的DBMS以及配置；

# 二、SQL语言
## 数据定义语言(Data Definition Language, DDL)
> 负责数据结构定义与数据库对象定义

核心指令为 `CREATE`、 `ALTER`、 `DROP`。
```SQL
-- 数据库操作
CREATE DATABASE test;  -- 创建数据库
DELETE DATABASE test;  -- 删除数据库
USE test;  -- 选择数据库

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
```

## 事物控制语言(Transaction Control Language, TCL)
> 负责管理数据库中的事务，管理由DML语句所做的更改，还允许将语句分组为逻辑事务。

核心指令是 `COMMIT`、`ROLLBACK`。
```SQL

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

        -- LIKE: 确定字符串是否匹配模式
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

# Reference
- [SQL语法速成手册-掘金](https://juejin.cn/post/6844903790571700231)
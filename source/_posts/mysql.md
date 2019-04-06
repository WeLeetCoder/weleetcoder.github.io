---
title: MySQL 学习【转载】
date: 2019-04-05 16:22:06
tags: [mysql]
categories: [技术学习]
---

# MySQL

> MySQL 的基本操作可以包括两个方面: MySQL 常用语句如高频率使用的增删改查(CRUD)语句和 MySQL 高级功能, 如存储过程, 触发器, 事务处理器等. 而这两个方面又可以细分如下:

![mysql](https://ws1.sinaimg.cn/large/007Ie89nly1g1t17iconcj30td0qojru.jpg)
#### Mysql 常用语句

1.  表（或者数据库）的 CRUD。
2.  表数据的 `CRUD`，其中表数据查询使用最多，也更复杂。查询可以按照单表还是多表可以分为： 单表`SELECT`查询和夺标的连接查询（`INNER JOIN`，`LEFT JOIN`，`RIGHT JOIN`和`FULL JOIN`）以及组合查询`UNION`以及组合查询`UNION`和`UNION ALL`。
3.  SQL 语句中各个关键字的执行顺序。

## 1. 表（或数据库）操作语句

#### 1.1 查询表（或数据库）

1.  **获取所有可用的数据库：** `SHOW DATABASES`。
2.  **选择数据库：**`USE <databaseName>`。
3.  _用于显示数据库服务器状态信息：_`SHOW STATUS`。
4.  _用来显示授权用户的安全权限：_ `SHOW GRANTS`。
5.  _用来显示数据服务器或警告信息：_ `SHOW ERRORS` 或者`SHOW WARNINGS`。
6.  **用于显示创建数据库是的创建语句：** `SHOW CREATE DATABASE <databaseName>`。
7.  **用于显示创建表示的创建语句：** `SHOW CREATE DATABASE <tableName>`。
8.  **获取当前所选的数据库中所有可用的表：** `SHOW TABLES` 。
9.  **获取表中所有列的信息：** `SHOW COLUMNS FROM <tableName>`；`DESCRIBE` 语句也具有相同的效果： `DESCRIBE <tableName>`。

### 1.2 新建表或数据库

1.  _新建数据库：_ `CREATE DATABASE <databaseName>`。
2.  **创建表可以使用 `CREATE TABLE` 语句:**

```sql
CREATE TABLE customers (
    cust_id INT NOT NULL AUTO_INCREMENT,
    cust_name CHAR(50) NOT NULL,
    cust_age INT NULL DEFAULT 18,
    PRIMARY KEY(cust_id)
) ENGINE = INNODB;
```

###### 解释：

1.  允许 **NULL** 值， 则说明再插入行数据时允许不给出该列的值，而 **NOT NULL** 则表示在插入或更新该列数据，必须明确给出该列的值。
2.  **DEFAULT**表示该列的默认值，在插入行数据时，若没有给出该列的值就会使用其指定的默认值；
3.  **PRIMARY KEY** 用于指定主键，主键可以指定一列数据，而可以由多列数据组合构成，如 `PRIMARY KEY(cust_id, cust_name)`;
4.  **ENGINE** 用于指定引擎类型。  
    常见的引擎类型又这些：  
    （1）InnoDB 是一个支持可靠的事务处理的引擎，但是不支持全文本搜索；  
    （2）MyISAM 是一个新能极高的引擎，它支持全文本搜索，但不支持事务处理；  
    （3）MEMORY 在功能上等同与 MyISAM，由于数据存储在内存中，速度很快（特别适何临时表）；

3.在创建表的时候可以使用 **FOREING KEY** 来创建外键，即一个表中的`FOREING KEY`
指向另一个表中的 **PRIMARY KEY**。外键 **FOREING kEY**用于约束破坏表的联结（连接）动作，保证连个表的数据完整性。同时也能防止非法数据插入外键列，因为该列值必须指向另一个表的主键。实例如下：

```sql
CREATE TABLE Orders (
    Id_O int NOT NULL,
    OrderNo int NOT NULL,
    Id_P int,
    PRIMARY KEY (Id_O),
    FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
)
```

### 1.3 删除表或数据库

1.  **删除数据库：** `DROP DATABASE <databaseName>`。
2.  **删除表：** `DROP TABLE <tableName>`。

### 1.4 更新表

1.  更新表结构信息可以使用 **ALTER TABLE** 字句，如为表怎加一列：
    `ALTER TABLE vendors ADD vend_name CHAR(20)`。另外进场用于定义外键，如：

```sql
ALTER TABLE customers
ADD CONSTRAINT fk_custormer_orders  
FOREIGN KEY (cust_id)
REFERENCES orders (order_cust)
```

2.  重命名表， 使用 `RENAME` 子句。 `RENAME TABLE backup_customers TO customers, backup_vendors TO vendors`；更改多个表名，之间用逗号间隔。

## 2 表数据操作语句

### 2.1 查询表数据

> 基本操作语句

1.  根据过滤条件春训表中的单列或者多列或者全部列的信息 **SELECT FROM WHERE：** `SELECT cust_id, cust_name, FROM customers WHERE cust_id=10000`;
    其中，过滤条件操作符有： `=, <>, !=, <, <=, >, >=` 和 `BETWEEN AND, IS NULL`;

2.  为查询出的某一列信息去重 **DISTINCT：**
    `SELECT DISTINCT cust_name FROM customers`;

3.  限制单列查询结果的行数： `SELECT cust_name FROM customers LIMIT 5`; *LIMIT*后跟一个数值，表示从第 0 行开始取，共取 5 行数据；如果 **LIMIT 5,5** 表示从第 6 行（数据库中行数是从 0 开始的，所以是第 6 行）开始取，共取 5 行数据。

4.　**ORDER BY** 子句取一个或者多个列，据此对输出进行排序：
`SELECT cust_id, cust_name FROM customers ORDER BY cust_id DESC, cust_name`；

5.  **IN** 操作符用来指定条件范围，范围中的每个条件都可以进行匹配：
    `SELECT cust_id, cust_name FROM customers WHERE cust_id IN (10000, 20000)`；

6.  **LIKE** 操作符用来表明模糊查询，与之配合使用的通配符有 **`% _`**，`%` 表示任何字符出现任何次数； **\_** 表示匹配一个字符：
    `SELECT cust_id, cust_name FROM customers WhERE cust_name LIKE '%happy%'`;

7.  使用分组查询并可以满足一定的分组过滤条件 **GROUP BY HAVING**。如检索总计订单金额大于 50 的订单号和订单总金额，并按总金额进行排序：
    `SELECT order_num, SUM(quantity * item_price) AS order_total FROM orderitems GROUP BY order_num HAVING SUM(quantity * item_price) >= 50 ORDER BY order_total`;

8.  **WHERE** 和 **HAVING** 的比较。_WHERE_ 是行级过滤，而 _HAVING_ 组级过滤。被 _WHERE_ 过滤掉的数据不会出现在分组中。 _WHERE_ 中通配符以及多个 _WHERE_ 子句的连接同样适用于 _HAVING_ 子句；

9.  **GROUP BY** 的使用注意事项：  
    （1）`GROUP BY` 子句中可以嵌套分组（即通过多个列进行分组 `GROUP BY cust_id, cust_name`），但是尽心数据汇总时，是在最后规定的分组上进行；  
    （2）`GROUP BY` 子句中列出的每个列都必须时检索列或者时有效的表达式。  
    （3）如果有 _NULL_ 值，将 _NULL_ 值作为一个分组尽心返回，若果有多行 _NULL_ 值，他们将分为一组。

10. 嵌套其他查询中的查询，被称之为 **子查询**。执行过程由里向外，里层查询结作为外层查询的条件：
    `SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'happy')`；当然，多表的查询可以是用联结（连接）查询。

> 联结（连接）查询

1.  内联结又称之为内部联结，是基于两个表之间的相等测试。如果不加过滤条件，会造成 [_笛卡尔积_](https://zh.wikipedia.org/wiki/%E7%AC%9B%E5%8D%A1%E5%84%BF%E7%A7%AF)。
    `SELECT vend_name, prod_name, prod_price FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id`;同样可以使用 `WHERE` 子句进行多表联结查询，但是更推荐使用 `INNER JOIN` 等联结方式；
2.  外部链接包括左外联结 **`LEFT JOIN`** 和右外联结 **`RIGHT JOIN`**。例如查询每个客户的订单数：`SELECT customers.cust_id, orders.orders_num FROM customers LEFT JOIN orders ON orders.cust_id = customers.cust_id`；_LEFT JOIN_ 会全部返回坐标数据，_RIGHT JOIN_ 会全部返回右表数据，*FULL JOIN*会将左右两个表的数据全部返回；
3.  联合查询与聚集函数一起使用。如查询每个客户的订单数：
    `SELECT customers.cust_name, customers.cust_id, COUNT(order.order_num) AS num_ord FROM customers INNER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id`;

> 组合查询

1.  多个查询（**SELECT**）可以使用 **UNION** 将多个查询结果尽心合并成一个结果集返回，**UNION** 必须宝行两个及两个以上的 **SELECT** 查询，斌且每个毕珣宝行相同的列、表达式或聚集函数，数据类型不必完全相同，**MySQL** 会进行隐式的类型转换。

2.  **UNION** 返回的是去重后的结果，如果不需要去重则可以使用 **UNION ALL**；

3.  可以多组合查询使用 **ORDER BY** 进行排序,但是是针对最终的结果集进行排序,而不是其中单个 **SELECT** 查询进行排序, 因此对于组合查询来说 **ORDER BY** 子句只有一个。
    `SELECT vend_id， prod_id, prod_price FROM products WHERE prod_price > 5 UNION SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN(1001, 1002) ORDER BY vend_id`；

> 使用函数对数据进行处理

1.  拼接列名：
    `SELECT Concat (vendName, '('vendCountry,')') From vendors ORDER BY vendName`;

2.  执行算数表达式计算：
    `SELECT prodId, quantity, price, quantity * price AS expandedPrice FROM ORDERiTEMS`;

3.  文本处理函数如 Upper(), LTrim()等函数。 比如使用 Upper 函数将文本转换成大写：
    `SELECT vendName, Upper(vendName) FROM vendors ORDER BY vendName`

4.  时间和日期处理函数，如 Date(), Day()等。
    `SELECT COUNT(*) AS numbers, MIN(prod_price) AS price_min,MAX(PROD_PRICE) AS price_max, AVG(prod_price) AS price_avg FROM products`;

5.  数值处理函数，如 ABS(),COS()等；

6.  常用的聚集函数。如 AVG(),COUNT(),MAX(),MIN()以及 SUM()。
    `SELECT COUNT(*) AS numbers, MIN(prod_price) AS price_min, MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg FROM products`;

### 插入表数据

1.  向表中插入行数据可以使用 **INSERT INTO** 子句，更安全的方式是指定列名。 `INSERT INTO customers (cust_name, cust_email) VALUES ('happy', 'happy@gmail.com');` 在 **INSERT INTO** 子句中能够省略列名的前提条件是：该列可以允许定义为 NULL 值或者在定义时给出去了默认值；
2.  如果插入多行数据可以将多组值用逗号进行分隔即可。
    `INSERT INTO customers (cust_name, cust_email) VALUES ('happy', 'happy@gmail.com'),('smart', 'smart@gmail.com');`
3.  将查询出来的数据插入表中，可以使用 **INSERT SELECT** 语句。
    `INSERT INTO customers(cust_id, cust_contact) SELECT cust_id, cust_conntact FROM customer WHERE cust_id > 5;` 其中 SELECT 中可以带 WHERE 过滤条件；**INSERT SELECT**通常被用于复制表数据。

### 更新表数据

1.  如果要更新表数据的话，使用 **UPDATE** 子句：
    `UPDATE customers SET cust_name = 'happy', cust_email = 'happy@gmail.com' WHERE cust_id = 1001;`
2.  注意： 如果不加 **WHERE** 条件指定到某一行的话，会更新表中某一列的全部的数据。

### 删除表数据

1.  如果从表中删除数据的话，可以使用 **DELETE** 子句。
    `DELETE FROM customers WHERE cust_id = 10086;`
    删除的数据必定是表中行数据，而不是某一列。因此，与 **UPDATE** 子句相比，**DELETE** 子句并不需要指定是哪一列，而仅仅只需要指定具体的表明即可；
2.  **注意： 如果不添加 WHERE 指定条件的话，会将整个表中的所有行数据全部删除。另外，DELETE 只是删除表中的数据，而不会删除表结构信息；**
3.  如果想删除表中的全部的数据，可以使用 **TRUNCATE**，比 **DELETE** 删除效率更高；

## SQL 中关键字执行顺序

在 SQL 语句中每个关键字都会按照顺序往下执行，而每一步操作，都会生成一个虚拟表，最后产生的虚拟表会作为执行的最终结果返回。下面的是常用的关键字的执行顺序：

```
(1)FROM <left_table>
(2)ON <join_condition>
(3)<join_type> JOIN <right_table>
(4)WHERE <where_condition>
(5)GROUP BY<group_by_list>
(6)WITH{CUBE|ROLLUP}
(7)HAVING<having_condition>
(8)SELECT
(9)DISTINCT<select_list>
(10)ORDER BY<order_by_list>
(11)LIMIT<limit_number>
```

1.  **FROM**：对 FROM 左边的表和邮编的表计算笛卡尔积，产生虚拟表 VT1；
2.  **ON**：对虚拟表 VT1 进行 ON 筛选，只有那些符合<join_condition> 条件的行才会被记录在虚拟表 VT2 中；
3.  **JOIN**：如果是 OUT JOIN，name 将保留表中（如左表或者右表）未匹配的行作为外部行添加到虚拟表 VT2 中，从而产生虚拟表 VT3；
4.  **WHERE**：对虚拟表 VT3 进行 WHERE 条件过滤，只有符合<where_condition>的记录才会别放入到虚拟表 VT4；
5.  **GROUP BY**：根据 GROUP BY 子句中的列，对虚拟表 VT4 进行分组操作，产生虚拟表 VT5；
6.  **CUBE | ROLLUP**：对虚拟表 VT5 进行 CUBE 或者 ROLLUP 操作，产生虚拟表 VT6；
7.  **HAVING**：对虚拟表 VT6 进行 HAVING 条件过滤，只有符合<having_condition>的记录才会别插入到虚拟表 VT7 中；
8.  **SELECT**：执行 SELECT 操作，选择指定的列，插入到虚拟表 VT8 中；
9.  **DISTINCT**：对虚拟表 VT8 中的记录进行去重，产生虚拟表 VT9；
10. **ORDER BY**：将虚拟表 VT9 中的记录按照<order_by_list>进行排序操作，产生虚拟表 VT10；
11. **LIMIT**：取出指定行的记录，产生虚拟表 VT11，并将结果返回。

## 索引

> MySQL 缩影的简历对于 MySQL 的高效运行是很重要的，索引可以大大提高 MySQL 的检索速度。索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，而组合索引，即一个索引包含多个列。

### 创建索引

创建索引有两种方式，一种是直接利用 **CREATE INDEX**进行创建，另外一种则是通过修改表结构来进行添加，则是利用 **ALTER TABLE**语句。

1.  使用`CREATE INDEX`

    语法为：

    ```sql
    CREATE [UNIQUE|FULLTEXT|SPATIAL]
    INDEX index_name
    [USING index_type]
    ON table_name (index_col_name,...)
    ```

    其中对应的语法变量信息如下：

    > [UNIQUE | FULLTEXT | SPATIAL]

    其中括号中的这三个关键字表示创建索引类型，他们分别表示 **唯一索引，全文索引，空间索引** 三种不同的索引类型。如果我们不指定任何关键字，则默认为普通索引。

    > index_name

    `index_name` 表示索引的名称，又用户自行定义，仪表与以后对该索引进行管理操作。

    > index_type

    `index_type` 表示索引的具体实现凡是，在 MySQL 中，有两种不同形式的索引 —— **BTREE** 索引和 **HASH** 索引。在存储引擎为 **MyISAM** 和 **InnoDB** 的表中只能使用 BTREE，其默认值就是 BTREE；在存储引擎为 MEMORY 或者 HEAP 的表中可以使用 HASH 和 BTREE 两种类型的索引，其默认值为 HASH。

    > index_col_name

    `index_col_name` 表示需要创建索引的字段名称，我们还可以针对多个字段创建复合索引，只需要多个字段名称之间以英文逗号隔开即可。此外，对于 CHAR 或者 VARCHAR 类型的字段，我们还可以值使用字段内容前面的一部分来闯进啊索引，只需要在对应的字段名称后米昂加上形如（_length_）的指令即可，表示只需要使用字段内容前面的 _length_ 个字符来创建索引。在这里我们以 customers 表的 cust_name 字段（类型为 VARCHAR（50））为例，使用 cust_name 字段的 6 个字符前缀来创建索引。

    ```
    CREATE INDEX idx_cust_name ON user (cust_name(6));
    ```

2.  使用 ALTER TABLE

    语法为：

    ```sql
    ALTER TABLE table_name
    ADO [UNIQUE|FULLTEXT|SPATIAL]
    INDEX index_name (index_col_name,...)
    [USING index_type]
    ```

### 删除索引

删除指定表中的特定名称的索引，语法为：

```sql
ALTER TABLE table_name
DROP INDEX index_name;
```

例如删除名称为 idx_cust_name 的索引，其 SQL 语句为：

```sql
ALTER TABLE customers
DROP INDEX idx_cust_name;
```

### 修改索引

在 MySQL 中并没有提供修改索引的直接指令，一般情况下，我们需要先删除掉元索引，再根据需要创建一个同名的索引，从而变相的实现修改索引的操作。

```sql
-- 先删除索引
ALTER TABLE user
DROP INDEX idx_user_username;
--  再以修改后的内容创建同名索引
CREATE INDEX idx_cust_name ON customers (cust_name(8));
```

### 查看索引

再 MySQL 中，要查看某个数据库表中的索引也非常简单，只需要使用一下两个命令中的任意一种即可。

```sql
-- 如果查看索引前，没有使用 use db_name 等命令指定具体的数据库，则必须加上 FROM db_name
SHOW INDEX FROM table_name [FROM db_name]
-- 如果查看索引前，没有使用 use db_name 等命令指定具体的数据库，则必须加上 db_name. 前缀
SHOW INDEX FROM [db_name.]table_name
```

## 存储过程

1.  什么是存储过程？存储过程简单的来说，就是为了复用性或者实现复杂的业务功能，而保存的一条或者多条 MySQL 语句的集合，可将其视为批文件；

2.  为什么使用存储过程？  
    （1）通过把处理封装在容易使用的单元中，简化复杂的操作；  
    （2）由于不要求反复建立一系列的处理步骤，这保证了数据的完整性，如果所有的开发人员和应用程序都使用同一存储过程，则所使用的代码都是相同的；  
    （3）简化对变动的管理。如果表名、列名或业务逻辑有变化，只需要更改存储过程的代码，使用它的开发人员升值不需要知道这些变化，也就是具备了安全性；  
    （4）提高了性能，因为使用存储过程单独使用 SQL 语句要快；  
    （5）存储过程可用来编写功能更灵活的代码。因此，存储过程的具备三个特性： **简单可复用、安全、高性能**；
3.  存储过程的缺点？  
    （1）存储过程编比基本的 SQL 语句更加复杂，需要更高的技能；  
    （2）可能没有创建存储过程的权限，数据库管理员可能会限制存储过程的权限，允许用户使用存储过程，而不允许用户自由创建存储过程；

> #### 创建存储过程

1.  创建存储过程。如需要统计用户订单总金额，如果该用户需要交税的话，订单总金额则需要再加上税费：

```sql
DELIMITER //
 CREATE PROCEDURE ordertotal(
 IN custid INT,
 IN taxable BOOLEAN,
 OUT ototal DECIMAL(8,2)
 )COMMENT 'obtain total order price'

 BEGIN

 /*declare variable for total*/
 DECLARE total DECIMAL(8,2);
 DECLARE taxrate INT DEFAULT 6;


 /*get the order total*/

 SELECT SUM(item_price*item_quantity) INTO total
 FROM customers
 WHERE cust_id = custid;

 /*is this taxable?*/

 IF taxable THEN
 	SELECT total+(total/100*taxrate) INTO total;
 END IF;
 SELECT total INTO ototal;
 END //
```

有这样的一些细节：

1.  使用 `CREATE PROCEDURE` 语句进行创建，()圆括号中为存储过程的参数，其中参数类型有：
    1.  **IN** 类型，表示传递给存储过程；
    2.  **OUT** 类型，表示存储过程返回的结果，在调用存储过程是需要传入@开始的变量；
    3.  **INOUT** 类型，表示在存储过程中可以传入和传出；
2.  _DECLARE_ 用力啊生命一个变量，如这里的 total，taxrate。注意 MySQL 中定义变量时都是变量名在前，数据类型在后。
3.  存储过程具体逻辑写在 _BEGIN END_ 之间；
4.  将值付给变量使用 _INTO_ 关键字；
5.  由于存储过程中每个 SQL 语句中用 `;` 作为分隔符，回合单个 SQL 造成冲突，因此可以使用 _DELIMITER_ 重新定义分类符，如该例子中定义 `//` 作为分隔符，自然存储过程结尾就用 `END //` 结尾，而不再是 END。同时，分隔符 `//` 成对出现后，回复到默认的 `;` 作为分隔符；

> #### 执行存储过程

1.  使用 _CALL_ 子句执行存储过程， _CALL_ 子句接受存储过程的名称以及需要传递的参数。

```sql
CALL ordertotal(1, TRUE, @total);
SELECT @total;
```

如果存储过程中定义了 OUT 类型的输入参数，那么在执行存储过程时需要传入变量，如这里的@total，并且变量都是用@开始的。如果存储过程中么有参数的话，就用空圆括号表示即可：
`CALL ordertotal()`；

> #### 删除存储过程

删除存储过程，可以使用 _DROP PROCEDURE_ 子句。如：
`DROP PROCEDURE ordertotal`；

> #### 查询存储过程

1.  显示创建一个存储过程的语句，可以使用 _SHOW CREATE PROCEDURE_。如：
    `SHOW CREATE PROCEDURE ordertotal`；
2.  查询所有存储过程的状态，如果在定义存储过程中时用 _COMMENT_ 添加注释，可以查看。同时可以 LIKE 进行过滤结果。如：
    `SHOW PROCEDURE STATUS LIKE '%ORDER%'`；

## 事物处理

1.  什么是事务？  
    事务处理是用来维护数据库的完整性，它保证成批的 MySQL 操作要么完全执行，要么完全不执行。事务处理是一种机制，用来管理必须成批执行的 MySQL 操作，它们要么是作为整体执行或者完全不执行。

2.  关键概念：

    1.  事务：是指一组 SQL 语句；
    2.  回退：是指撤销指定的 SQL 语句的过程；
    3.  提交：指将为存储的 SQL 语句的结果写入数据库表中；
    4.  保留点：指事务处理中设置的临时占位符，可以对他发布回退；

3.  如何执行事务？

    ```sql
    START TRANSACTION;
    INSERT INTO customers (cust_name, item_quantity) VALUES('1', 5, 18);
    SELECT * FROM customers;
    SAVEPOINT insertinto;
    INSERT INTO customers (cust_name, item_price, item_quantity) VALUES ('2', 5, 18);
    ROLLBACK TO insertinto;
    ```

    执行结果为：插入数据（'1', 5, 18)有效，因为，只会从保留点 SAFEPOINT 之后开始回退，也就是说保留点 SAFEPOINT 之前的 SQL 语句执行的结果仍然有效。

    有这样一些细节：

    1.  **START TRANSACTION** 用来表示下面的 SQL 语句集为一段事务；
    2.  **SAFEPOINT** 用于指定保留点 insertinto；
    3.  **ROLLBACK TO** 表示草你个指定保留点开始回退，也就是说保留点之前的 SQL 语句执行结果任然有效。如果仅仅使用 ROLLBACK 进行回退的话就表示从 START TRANSACTION 之后所有的 SQL 语句执行效果都会撤销；

4.  MySQL 提交（写或保存）操作室自动进行的，这称之为隐含提交。但是在事务处理块中，提交不会隐含进行，要使用 _COMMIT_ 子句进行提交。如：
    ```sql
    START TRANSACTION; INSERT INTO customers (cust_name, item_price, item_quantity) VALUES ('1', 5, 18); INSERT INTO customers (cust_name, item_price, item_quantity) VALUES ('2', 5, 18); COMMIT;
    ```
    采用 _COMMIT_ 提交事务，如果两条 SQL 语句都执行成功，才会将数据都写入表中。

## 触发器

1.  什么是触发器？

    当某条 SQL 语句发生时，自动执行某些其他的 SQL 语句的时候就需要使用触发器。触发器只能响应： **DELETE，INSERT，UPDATE** 这三个特定操作。

2.  如何创建触发器？

    创建触发器时需要给出最重要的四条信息：

    1.  全局唯一的触发器名；
    2.  触发器关联的表；
    3.  触发器在合适执行（操作执行之前或者之后）
    4.  触发器应该响应的活动（_DELETE，INSERT，UPDATE_）；

    由于触发器只能响应特定的三种类型的操作，因此可闯进啊的触发器也就三种类型：**INSERT**触发器，**DELETE**触发器，以及 **UPDATE**触发器。

    > #### INSERT 触发器

    在执行 INSERT 触发器时，有这样几点需要注意：

    1.  在 INSERT 触发器代码内，可以引用一个名为 NEW 的虚拟表，可以用 NEW 来访问刚插入的行数据；
    2.  在 BEFORE INSERT 触发器中，NEW 中的值可以被更新；
    3.  对于 AUTO_INCREMENT 列，NEW 在 INSERT 执行之前包含 0，在 INSERT 执行之后包含行的自定生成值。

    创建一个 INSERT 触发器，每次插入一行数据，每次会返回单签常茹的行数据的 id：

    ```sql
    /*创建触发器*/
    CREATE TRIGGER insertcustomers AFTER INSERT ON customers
    FOR EACH ROW SELECT NEW.cust_id INTO @newinsertid;

    /*执行触发器*/
    INSERT INTO customers （cust_name, item_price, item_quantity) VALUES ('2', 5, 18);
    SELECT @newinsertid;
    ```

    有这样一些细节：

    1.  使用 _CREATE TRIGGER_ 来创建触发器；
    2.  _AFTER INSERT_ 表明在插入行数据之后，触发器才会执行特定操作；
    3.  _FOR EACH ROW_ 表示对插入的每一行数据，触发器都起作用；
    4.  针对 INSERT 触发器，可以使用虚拟表 NEW，来使用刚插入的行数据。比如例子中：
        `SELECT NEW.cust_id INTO @newinsertid 表示将新插入的行数据的 id 赋值给变量@newinsertid；

    > #### DELETE 触发器

    _DELETE_ 触发器在 DELETE 语句执行之前或者之后，需要知道以下两点：

    1.  在 DELETE 触发器代码内，可以应用一个名为 OLD 的虚拟表，来访问被删除的行；
    2.  OLD 表中的数据只能读，不能被更新，而在 INSERT 触发器中，就可以通过 NEW 来更新被插入的行数据；

    例如,针对 customers 表，当删除一行数据时，返回被删除的 `cust_id` 以及 `cust_name`：

    ```sql
    /*创建DELETE触发器*/
    DELIMITER //
    CREATE TRIGGER insertcustomers AFTER DELETE ON customers
    FOR EACH ROW
    BEGIN
    SELECT OLD.cust_name INTO @deletecustname;
    SELECT OLD.cust_id INTO @deletecustid;
    END //

    /*调用DELETE触发器*/
    DELETE FROM customers WHERE cust_id = 3;
    SELECT @deletecustname;
    SELECT @deletecustid;
    ```

    基本上与创建 INSERT 触发器一样，只不过在 DELETE 触发器中只能使用 OLD 来访问被删除的行数据。

> #### UPDATE 触发器

UPDATE 触发器在 UPDATE 语句执行前或者之后执行，需要知道几点：

1.  在 BEFORE UPDATE 触发器中可以使用 NEW 和 OLD 来访问数据，而在 AFTER UPDATE 触发器中使用 NEW 来访问数据会报错，只能使用 OLD 来访问数据；
2.  在 BEFORE UPDATE 触发器中，NEW 中的值可以改变，即允许更改将用户 UPDATE 的数据；
3.  OLD 中的行数据只能读，不能被更新；

一个 UPDATE 触发器示例如下：

```sql
/*创建触发器*/

DELIMITER //
DELETE TRIGGER insertcustomers BEFORE UPDATE ON customers
FOR EACH ROW
BEGIN

SELECT NEW.cust_name INTO @beforeupdate;
SET NEW.cust_name = 'reset_name';
SELECT OLD.cust_name INTO @afterupdate;
END //

/*调用UPDATE触发器*/
UPDATE customers SET cust_name = 'happy' WHERE cust_id = 5;
SELECT @beforeupdate;
SELECT @afterupdate;
```

    输出为@beforeupdate 为 'happy'，而@afterupdate 为 'reset_name'。有这样一些细节：
    
    1.  NEW 虚拟表中的数据可以更改， 如这里采用
        `SET NEW.cust_name = 'reset_name';`，将待更新的
        `cust_name` 由 `happy` 变成了 `reset_name`；
    2.  在 BEFORE UPDATE 触发器中可以使用 NEW 和 OLD 来访问数据，而在 AFTER UPDATE 触发器冲使用 NEW 来访问数据会报错；

3.如何删除触发器？

删除触发器，可以使用 _DROP TRIGGER_ 语句，比如：
`DROP TRIGGER insertcustomers;`。触发器不能更新或者覆盖，如果要修改触发器，必须删除这个触发器。



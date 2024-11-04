# DataBase
=======
# DataBase
- Data: 描述事物的符号记录（有语义）
- DataBase: 长期储存在计算机内，有组织的，可共享的大量数据的集合
- DataBase Management System: 位于用户与操作系统直接的一层数据管理软件
- DataBase System:由数据库，数据库管理系统（及其应用开发工具），应用程序和数据库管理员组成的存储，管理，处理和维护数据的系统
### 1.2 数据模型（data model）:
- 组成元素：数据结构（对象及其联系），数据操作（增删改查），数据的完整性约束条件
- 现实世界： （信息世界）设计人员设计用户的**概念模型** --> （机器世界）**逻辑模型**：层次/网状/关系 --> **物理模型**： 存储/存取
- 概念模型： E-R图（实体，属性，联系）
- 逻辑模型：1. 非关系模型/格式化模型：层次模型（树），网状模型（有向图）；2. 关系模型：规范化的（二维表）
### 1.3 数据库系统结构-三级模式结构
三级结构指：外模式（每一个用户建立的数据结构及特征），模式（全体用户所有数据逻辑结构和特征），内模式（物理结构，存储方式）
### 1.3 数据库系统结构-二级映像功能
- 模式/外模式：可变的，不唯一
- 模式/内模式：唯一的

## 关系数据库
### 2.1 数据结构及形式化定义：
- 数据结构：关系 - 扁平的二维表 eg 导师，专业，学生
- 形式化定义：
  - **域**-相同数据类型的集合eg 老师域，专业域，学生姓名域；**笛卡尔积**：一种操作，对域进行笛卡尔积，就会形成二维表，每一行是一个**元组**，
  元组内每一个元素，就是**分量**
  - 关系（二维表），候选码，主码，全码
  - 主属性，非主属性
### 2.2 关系操作
- 关系操作：集合操作方式
  - 查询操作：**选择，投影**，连接，除，**并，差**，交，**笛卡尔积**
  - 插入，删除，修改操作
- 关系数据语言：
  - 关系代数语言
  - 关系盐酸语言：元组关系，域关系
  - 结构化查询语言(SQL)
### 2.3 关系完整性（约束条件）：
- 实体完整性：面向基本关系；主码作为唯一性标识；主属性不能为空
- 参照完整性：外码-表与表之间的连接
- 用户定义的完整性：反映某一具体应用所涉及的数据必须满足的语义要求
### 2.4 关系代数（重点：查询语言）：
- 运算对象：关系
- 运算结构：关系
- 分类： 传统的集合运算， 专门的关系运算
- 集合运算（二目运算）：并 U, 差 -， 交∩， 笛卡尔积 X
- 关系运算：
  - 选择：从关系R中选取使逻辑表达式F为真的元组，是从**行**的角度进行的运算
  - 投影：是从**列**的角度进行运算，但投影成功之后不仅取消了原关系中的某些列，还可能取消某些元组（避免重复行）
  - 连接：从两个关系的笛卡尔积中选取属性间满足一定条件的元组
  - 除： R ➗ S 同时从行和列的角度进行运算的
## chapter 2
### the SELECT statement
Capitalize words for keywords, eg USE, SELECTE
```declarative
SELECT name, unit_price, unit_price * 1.1 AS new_price
FROM customers 
```
### AND, OR, NOT operator
```declarative
SELECT *
FROM Customers
WHERE birth_data > '1990-01-01' OR (points > 1000 AND state = 'VA')
-- AND has higher precedence
```
### IN operator
```declarative
SELECT *
FROM Customers
WHERE state NOT IN('VA', 'CA', 'TX')
```
### BETWEEN
```declarative
SELECT *
FROM customers
WHERE points BETWEEN 1000 AND 2000
```
### LIKE
```declarative
SELECT *
FROM customers
WHERE last_name LIKE 'b%' -- last name starts with b; '%b%' b in last name;'%b' b is the last char
WHERE last_name LIKE 'b___y' --last name has 5 chars, starts with b, follow by 3 chars, and end with y
-- % any number of characters
-- _ single char
```
```declarative
--Get the customers whose addresses contain TRAIL or AVENUE
SELECT *
FROM customers
WHERE address LIKE '%trail%' OR
    address LIKE '%AVENUE'
```
### REGEXP
```declarative
SELECT *
FROM customers
WHERE last_name REGEXP '[a-h]e'
-- ^ beginning '^field' means last name starts with field
-- $ end 'field$' means last name ends with field
-- | logical or 'field|rose|nasy' last name contains field or rose or nasy
-- [abcd]
-- [a-f] means range a,b,c,d,e,f
```
```declarative
-- get the customers whose
-- first names are ELKA or AMBUR
-- last names end with EY or ON
--last names start with MY or contains SE
-- last names contain B followed by R or U
SELECT *
FROM customers
WHERE first_name REGEXP 'elka|ambur'
WHERE last_name REGEXP 'ey$|on$'
WHERE last_name REGEXP '^my|se'
WHERE last_name REGEXP 'br|bu' == WHERE last_name REGEXP 'b[ru]'
```
### IS NULL 
```declarative
SELECT *
FROM customers
WHERE phone IS NULL -- return customers without phone
WHERE phone IS NOT NULL --return customers with phone
```
### ORDER BY
```declarative
SELECT *
FROM customers
ORDER BY state DESC , first_name DESC
```
The orders of clauses matter
SELECT - FROM - WHERE - ORDER BY - LIMIT
## Chapter 3 
### Inner Joins
Join colums from multiple tables"
```
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c
  On o.customer_id = c.customer_id
```
```dtd
SELECT order_id, oi.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p 
	ON oi.product_id = p.product_id
```
### Joining Across Databases
```dtd
SELECT *
FROM order_items oi
JOIN sql_inventory.products p
  ON oi.product_id = p.product_id
```
prefix the tables that are not in current database. 
eg sql_inventory.products the table products exists in database sql_inventory

### Self joins
```dtd
USE sql_hr;
SELECT e.employee_id, e.first_name, m.first_name AS manager
FROM employees e
JOIN employees m
	ON e.reports_to = m.employee_id
```
Join the table with itself is the same as join the table with another table.
The only difference is you need usd different alias.
### Joining multiple tables
```dtd
USE sql_store;
SELECT 
	o.order_id, 
    o.order_date,
    c.first_name, 
    c.last_name,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```
```dtd
USE sql_invoicing;
SELECT 
	c.client_id, 
    c.name,
    p.date,
    p.amount,
    pm.name
FROM payments p
JOIN clients c
	ON p.client_id = c.client_id
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
```
### Outer Joins
Using 'LEFT / RIGHT' before 'JOIN'
```dtd
USE sql_store;
SELECT p.product_id, p.name, oi.quantity
FROM products p
LEFT JOIN order_items oi
	ON p.product_id = oi.product_id
```
LEFT JOIN: get all the products in products table, whether the condition(p.product_id = oi.product_id) true or not  
### Outer Join Between Multiple tables
```dtd
USE sql_store;
SELECT 
	o.order_date,
    o.order_id,
    c.first_name AS customer,
    s.name AS shipper,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
LEFT JOIN shippers s
	ON o.shipper_id = s.shipper_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```
### the USING Clause
```dtd
USE sql_invoicing;
SELECT 
	p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM payments p
JOIN clients c
	USING (client_id)
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
```
USING (column name): the column must exit in both tables with same name.
### Natural Joins
It isn't recommend.
### Cross Joins
```dtd
-- implicit syntax
        USE sql_store;
        SELECT
        sh.name AS shipper,
        p.name AS product
        FROM shippers sh, products p
        ORDER BY sh.name
```
```dtd
-- explicit syntax
        USE sql_store;
        SELECT
        sh.name AS shipper,
        p.name AS product
        FROM shippers sh
        CROSS JOIN products p
        ORDER BY sh.name

```
### Unions
the number of column each query returned should equal.
the name of column based on the first query.
```dtd
USE sql_store;
SELECT 
	customer_id,
	first_name,
    points,
    'Bronze' AS type
    
FROM customers c
WHERE points < '2000'
UNION
SELECT 
	c.customer_id,
	c.first_name,
    points,
    'Silver' AS type
    
FROM customers c
WHERE points BETWEEN 2000 AND 3000
UNION
SELECT 
	c.customer_id,
	c.first_name,
    points,
    'Gold' AS type
FROM customers c
WHERE points > '3000'
ORDER BY first_name

```
## Column Attributes
### Inserting a Row
```
USE sql_store;
INSERT INTO customers (
	-- customer_id,
    first_name,
    last_name,
    birth_date,
    address,
    city,
    state)
VALUE (
	-- DEFAULT,
    'Jon',
    'Cogen',
    '1990-02-22',
    'Address',
    'LA',
    'CA')
```
You can omit the rows you want to use default values.
### Inserting multiple rows
```
USE sql_store;
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUE ('Product1', 10, 1.95),
	  ('Product2', 11, 1.95),
      ('Product3', 12, 1.95)
```
### Inserting Hierarchical Rows
```
USE sql_store;
INSERT INTO orders(customer_id, order_date, status)
VALUE (1, '2019-02-02', 1);

INSERT INTO order_items
VALUES
	(LAST_INSERT_ID(), 1, 1, 2.95),
    (LAST_INSERT_ID(), 2, 1, 3.95)
```
### Creating a copy of a table
```
-- show client_name column
-- select clients who had payments
USE sql_invoicing;
CREATE TABLE invoices_archived AS
SELECT 
	i.invoice_id,
    i.number,
    c.name as client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM invoices i
JOIN clients c
	USING (client_id)
WHERE payment_date IS NOT NULL
```
NOTE: In the achived table, PK(primary key) and AI(automatic increment) 
are not selected.
### Updating a Single Row
```
USE sql_invoicing;
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE invoice_id = 3
```
### Updating  multiple rows
```
USE sql_store;
UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01'
```
### Using Subqueries
NOTE: Execute the subqueries before executing the whole queries
```
USE sql_invoicing;
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE client_id IN 
	(SELECT client_id
	 FROM clients
	 WHERE state IN ('CA', 'NY'))
```
```
USE sql_store;
UPDATE orders
SET comments = 'golden customer'
WHERE customer_id IN 
	(SELECT customer_id
	 FROM customers
	 WHERE points > 3000)
```
### Deleting Rows
```
DELETE FROM invoices
WHERE client_id = (
  SELECT *
  FROM clients
  WHERE name = 'Myworks'
)
```
### Restoring the Database
File -> Open SQL script -> restore file name -> execute -> 
back to navigator panel -> refresh
## Aggregate Functions
```
USE sql_invoicing;
SELECT
	MAX(invoice_total) AS hightest,
    MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total * 1.1) AS total,
    COUNT(invoice_total) AS number_of_invoices,
    COUNT(payment_date) AS count_of_payments,
    COUNT(DISTINCT client_id) AS total_records
FROM invoices
WHERE invoice_date > '2019-01-01'
```
- COUNT(payment_date): return all non-null elements
- COUNT(*): return non-null and null elements
- COUNT(**DISTINCT** client_id): return unique elements.
```
  SELECT
	'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date between '2019-01-01' AND '2019-06-30'
UNION
SELECT
    'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date between '2019-07-01' AND '2019-12-31'
UNION
SELECT
    'Total' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date between '2019-01-01' AND '2019-12-31'
```
### GROUP BY
Remember the order: SELECT - FROM - WHERE - GROUP BY - ORDER BY
```
USE sql_invoicing;
SELECT
	date,
    pm.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
GROUP BY date, payment_method
ORDER BY date
```
NOTE:
first, group by date
second, join pm, group by payment_method
### The HAVING Clause
The differences between WHERE and HAVING:
- WHERE Clause fitler data before GROUP BY
- HAVING Clause filter data after GROUP BY
- HAVING clause can include aggregate function, eg> SUM, MAX, COUNT...
- WHERE allow to filter conditions that don't show in SELECT Clause
- HAVING only filter conditions that show in SELECT Clause
-- First we should know the order of execution of Clauses i.e FROM > WHERE > GROUP BY > HAVING > DISTINCT > SELECT > ORDER BY. Since WHERE Clause gets executed before GROUP BY Clause the records cannot be filtered by applying WHERE to a GROUP BY applied records.
"HAVING is same as the WHERE clause but is applied on grouped records".
first the WHERE clause fetches the records based on the condition then the GROUP BY clause groups them accordingly and then the HAVING clause fetches the group records based on the having condition.

```
USE sql_store;
SELECT 
	c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM customers c
JOIN orders o USING (customer_id)
JOIN order_items oi USING (order_id)
WHERE state = 'VA'
GROUP BY 
	c.customer_id,
    c.first_name,
    c.last_name
HAVING total_sales > 100
```
### the ROLLUP Operator
Add the summary row at the end of table
```dtd
SELECT 
	pm.name AS payment_method,
    SUM(amount) AS total
FROM payments p
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
GROUP BY pm.name WITH ROLLUP
```
### GRUOPING Operator
```
SELECT
	IF(GROUPING(terms_id) = 1, 'Grand totals', terms_id),
    IF(GROUPING(vendor_id) = 1, 'Term id totals', vendor_id),
    MAX(payment_date) AS max_payment_date,
    SUM(invoice_total - payment_total - credit_total) AS balance_due
FROM invoices
GROUP BY terms_id, vendor_id WITH ROLLUP
-- The GROUPING function is co1nmo1tly used to replace the nulls that are generated by WITH ROLLUP with literal values. To do that, you can use it with the IF function 
-- The IF function evaluates the expression in the frrst argument and returns the second argument if the expression is true and the third argument if the expression is false. 
```
### the IN operator
```dtd
SELECT *
FROM clients
WHERE client_id NOT IN (
	SELECT DISTINCT client_id
	FROM invoices
	JOIN clients USING (client_id)
)
```
- find customers who have ordered lettuce (id = 30)
```dtd
SELECT
    DISTINCT customer_id,
    first_name,
    last_name
FROM customers c
JOIN orders o USING (customer_id)
JOIN order_items oi USING (order_id)
WHERE oi.product_id = 3
```
### the ALL keyword
-select invoices larger than all invoices of client 3
method 1: MAX function
```dtd
SELECT *
FROM invoices
WHERE invoice_total > (
	SELECT MAX(invoice_total)
    FROM invoices
    WHERE client_id = 3
)
```
Method 2: ALL keyword
```dtd
SELECT *
FROM invoices
WHERE invoice_total > ALL (
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
)
```
### the ANY keyword
-select clients with at least two invoices
Method 1: Using IN
```dtd
SELECT *
FROM clients
WHERE client_id IN (
	SELECT client_id
	FROM invoices
	GROUP BY client_id
	HAVING COUNT(*) >= 2
)
```
Method 2: = ANY
```dtd
SELECT *
FROM clients
WHERE client_id = ANY (
	SELECT client_id
	FROM invoices
	GROUP BY client_id
	HAVING COUNT(*) >= 2
)
``` 
### Correlated Subqueries
```dtd
-- get invoices that are larger than the client's average invoice amount
SELECT *
FROM invoices i
WHERE invoice_total > (
	SELECT AVG(invoice_total)
	FROM invoices
	WHERE client_id = i.client_id
)
ORDER BY client_id
```
### the EXISTS operator
method 1: NOT IN
```dtd
 -- find the products that have never been ordered
        SELECT
        product_id,
        name
        FROM products
        WHERE product_id NOT IN (
          SELECT product_id
          FROM  order_items
        )

```
method 2: NOT EXISTS
```dtd
-- find the products that have never been ordered
SELECT
      product_id,
      name
FROM products p
WHERE NOT EXISTS (
      SELECT product_id
      FROM  order_items
      WHERE p.product_id = product_id
)
```
### Subqueries in SELECT
```dtd
SELECT 
      client_id,
      name,
      (SELECT SUM(invoice_total)
          FROM invoices
          WHERE client_id = c.client_id) AS total_sales,
      (SELECT AVG(invoice_total) FROM invoices) AS average,
      (SELECT total_sales - average)    
FROM clients c
```
NOTE: expression doesn't allow use column alias
### Subqueries in FROM
The method of Subqueries in FROM only used for simple queries.
Because this method make main query complicate.
### Numeric function
```dtd
SELECT 
  ROUND(5.8),
  CEILING(5.2),
  FLOOR(5.2),
  ABS(-5.2)
```
### String functions
```dtd
SELECT LOWER('SKY') 
        -- UPPER(str)
        -- LTRIM(str)
        -- RTRIM(str)
        -- TRIM(str)
        -- LEFT/RIGHT(str, num) return num characters of str
        -- SUBSTRING(str, num1, num2) return substring start site is num1 and length is num2
        -- LOCATE('N', 'Kindergarten') return 3
        -- LOCATE('q', 'Kindergarten') return 0
        -- LOCATE('garten', 'Kindergarten') return 7
        -- LOCATE('garden', 'Kindergarten') return 0
        -- REPLACE('Kindergarten', 'garten', 'garden) return Kindergarden

```
### Date Function
RETURN int type
```dtd
SELECT NOW(), CURDATE(), CURTIME(), 
      YEAR(NOW()),MONTH(NOW()), DAY(NOW()),
      HOUR(NOW()), MIN(NOW()), SECOND(NOW())
```
return string type
```dtd
SELECT MONTHNAME(NOW()) --september
```
```dtd
SELECT  EXTRACT(YEAR FROM NOW()) --2024
```
```dtd
SELECT  DATE_FORMAT(NOW(), '%M %d, %Y') --September 23, 2024
```
```dtd
SELECT  TIME_FORMAT(NOW(), '%H: %i %p') --10: 26 AM
```
### Calculating Dates and time
```dtd
SELECT  DATE_ADD(NOW(), INTERVAL 1 YEAR) --2025-09-23 10:28:26
```
```dtd
SELECT  DATE_SUB(NOW(), INTERVAL 1 YEAR) --2023-09-23 10:30:06
```
```dtd
SELECT  DATEDIFF('2019-01-01', '2020-02-01') -- -396
```
```dtd
SELECT time_to_sec('09:00') --32400
```
```dtd
SELECT time_to_sec('09:00') - time_to_sec('09:01') -- -60
```
### IFNULL and COALESCE
IFNULL: substitute the null value with something else.
COALESCE: return the first non-null value
```dtd
USE sql_store;
SELECT
      order_id,
      -- IFNULL(shipper_id, 'Not assigned') AS shipper
      COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM orders
```
-- created the table including full name and phone number of customers
-- Using CONCAT to combine fist name and last name
-- Using IFNULL to set phone number from NULL to Unknown
```dtd
USE sql_store;
SELECT
      CONCAT(first_name, ' ', last_name) AS customer,
      IFNULL(phone, 'Unknown') AS phone
FROM customers
```
### the IF function
Separate orders into two groups:
group1 named 'Active', order_date is this year
group2 named 'Archived', order_date is before this year
method 1 - IF function:
```dtd
USE sql_store;
SELECT
      order_id,
      order_date,
      IF(
            YEAR(order_date) = YEAR(NOW()), -- condition
            'Active', -- condition is true
            'Archived') AS category --condition is false
FROM orders
```
Method 2: UNION
```dtd
USE sql_store;
SELECT
      order_id,
      order_date,
      'Active' AS type
FROM orders
WHERE YEAR(order_date) = 2019 
UNION
SELECT
      order_id,
      order_date,
      'Archived' AS type
FROM orders
WHERE YEAR(order_date) < 2019 
```
Using COUNT with GROUP BY
```dtd
USE sql_store;
SELECT
      p.product_id,
      p.name,
      COUNT(*) AS orders,
      IF(
          COUNT(*) > 1,
          'Many times',
          'Once') AS frequency
FROM products p
JOIN order_items oi USING(product_id)
GROUP BY p.product_id, p.name
```
### the CASE operator
```dtd
USE sql_store;
SELECT
      CONCAT(first_name, ' ', last_name) AS customer,
      points,
      CASE
          WHEN points > 3000 THEN 'Gold'
          WHEN points >= 2000 THEN 'Silver'
          ELSE 'Bronze'
	END AS category
FROM customers
```
### Creating Views
```dtd
CREATE VIEW clients_balance AS
SELECT
      c.client_id,
      c.name,
      SUM(i.invoice_total - i.payment_total) AS balance
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY c.client_id, c.name
```
### Altering or Dropping VIEW
Method 1: DROP VIEW [view name] --Drop and recreate
Method 2: CREATE OR REPLACE VIEW [view name]
Method 3: save it to do source control
### Updatable Views
Updatable views don't include:
- DISTINCT
- Aggregate Functions(MIN, MAX, SUM...)
- GROUP BY / HAVING
- UNION
-- created the updatable view:
```dtd
CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT
	invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0 -- can't use alias name 'balance'
```
-- update the invoices_with_balance
```dtd
DELETE FROM invoices_with_balance
WHERE invoice_id = 1
```
```dtd
UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2
```
## data models
### conceptual: only represent entities and relationships
entity relationship
UML
modelling tools: Microsoft Visio, draw.io, LucidCharts
### logical: we almost know what structure and what table we need to store data
add more details
### physical
open MySQL workbench -> file -> new model -> EER(enhance entity relationship) -> right click "mydb" -> edit schema (to rename it) -> add new diagram -> add new table -> set primary keys -> set foreign keys 
singular - entity (eg student)
plural - table name (students)
column :
first_name, VARCHAR(50): short length of string, using 50; for middle length, using 255
NN: NON-NULL
AI: automatic increment
## Normalization
### 1NF- first normal form
Each cell should have a single value and we cannot have repeated columns 
### 2NF -second normal form
every table should describe one entity, and every column in that table should describe that entity
### 3NF - third normal form
a column in a table should not be derived from other columns
### forward engineering
database -> forward engineer ->
### synchronizing a model
if you change one table, you need update database
database -> sychronize engineer
### reverse engineering
if you want change the database which doesn't have model
before you do reverse engineering, make sure no model is openning. Otherwise, mySQL will add this database into that model
database -> reverse engineer
### creating and dropping table
```
CREAT DATABASE IF NOT EXISTS table_sql
DROP DATABASE IF EXISTS table_sql
```
### creating tables
```
CREATE DATABASE IF NOT EXISTS sql_store2;
USE sql_store2;
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers
(
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name  VARCHAR(50) NOT NULL,
    points      INT NOT NULL DEFAULT 0,
    email       VARCHAR(255) NOT NULL UNIQUE
);
```
### altering tables
```
ALTER TABLE customers
    ADD last_name VARCHAR(50) NOT NULL AFTER first_name,
    ADD city      VARCHAR(50) NOT NULL,
    MODIFY COLUMN first_name VARCHAR(55) DEFAULT '',
    DROP points;
```
### creating relationships
```
CREATE DATABASE IF NOT EXISTS sql_store2;
USE sql_store2;
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers
(
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name  VARCHAR(50) NOT NULL,
    points      INT NOT NULL DEFAULT 0,
    email       VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE orders
(
    order_id    INT PRIMARY KEY,
    customer_id INT NOT NULL,
    FOREIGN KEY fk_orders_customers (customer_id)
        FEFERENCES customers (customer_id)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
```
### altering primaty key
```
ALTER TABLE order
    ADD PRIMARY KEY (order_id),
    DROP PRIMARY KEY,
    DROP FOREIGN KEY fk_orders_customers,
    ADD FOREIGN KEY fk_orders_customers,
    ADD FOREIGN KEY fk_orders_customers (customer_id)
        REFERENCES customers (customer_id)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
```
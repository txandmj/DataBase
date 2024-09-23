# DataBase
=======
# DataBase
- Data: 描述十五的符号记录（有语义）
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
- WHERE allow to filter conditions that don't show in SELECT Clause
- HAVING only filter conditions that show in SELECT Clause
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
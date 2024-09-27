INSERT for entering data into the Employees Table

```sql

INSERT INTO Employees
(employee_id, employee_name, email, home_address, phone, job_title, date_start, salary, department)
VALUES 
(101, 'Rory Mcgavin', 'rory@test.com', '12 van Gogh Crescent', 0822665487, 'CMO', '12/05/2020', 25000, 'Marketing'),
(102, 'Jade Redford', 'jade@test.com', '28 Juliet Street', 0784451028, 'Sales Team Lead', '02/06/2022', 14000, 'Sales'),
(103, 'Margeret van Tonder', 'margeret@test.com', '155 The Views', 0658842552, 'Accounts Manager', '10/25/2019', 15000, 'Finance'),
(104, 'Sibu Nkosi', 'sibu@test.com', '24 Radial Drive', 0645588849, 'Technician', '05/09/2017', 18000, 'RND'),
(105, 'Ryan Staniforth', 'ryan@test.com', '101 Hazeldon Estate', 0844426459, 'Sales Agent', '06/05/2019', 12000, 'Sales'),
(106, 'Sebastian Smith', 'sebastian@test.com', '2 Blake Street', 0652254889, 'CFO', '08/02/2015', 32000, 'Finance')
;

```

---

UPDATE for adding a column based off another column

```sql

UPDATE Employees
SET projected_salary_2025 = salary * 1.1

```

---

DELETE to remove a single row of data

```sql

DELETE FROM Employees
WHERE employee_id = 150

```

---

DELETE to remove all data in the table while keeping its structure and attributes

```sql

DELETE FROM Employees;

```

---

DROP TABLE to remove the entire table, including its structure and attributes

```sql

DROP TABLE Employees;

```

---

SELECT statement for all records in the Employees table

```sql

SELECT *
FROM Employees

```

SELECT statement including a WHERE clause

```sql

SELECT *
FROM Employees
WHERE department = 'Sales'
;

```

---

#### JOINS

##### Ecommerce Data obtained on Kaggle

INNER JOIN to join matching values in the 'fact' and 'item' tables

```sql

SELECT 
	i.item_key,
	i.item_name,
	i.item_description,
	i.man_country,
	i.supplier,
	i.unit,
	f.quantity,
	f.unit,
	f.unit_price,
	f.total_price
FROM
	dbo.fact_table AS f
INNER JOIN
	dbo.item_dim AS i
	ON f.item_key=i.item_key
;

```

INNER JOIN to join matching values in the 'fact', 'item' and 'transaction' tables

```sql

SELECT 
	i.item_key,
	i.item_name,
	i.item_description,
	i.man_country,
	i.supplier,
	i.unit,
	f.quantity,
	f.unit,
	f.unit_price,
	f.total_price,
	t.trans_type,
	t.bank_name
FROM
	((dbo.fact_table AS f
INNER JOIN
	dbo.item_dim AS i
ON
	f.item_key=i.item_key)
INNER JOIN
	dbo.Trans_dim AS t
ON f.payment_key=t.payment_key)
;

```

LEFT JOIN to print all records from the 'customer_dim' table with matching records from the 'fact' table

```sql

SELECT 
	c.customer_key,
	c.customer_name,
	c.contact_no,
	c.nid,
	f.quantity,
	f.unit,
	f.unit_price,
	f.total_price
FROM
	dbo.customer_dim AS c
LEFT JOIN
	dbo.fact_table AS f
ON
	c.customer_key=f.customer_key
;

```

RIGHT JOIN to print all records from the 'time_dim' table with matching records from the 'fact_table'

```sql

SELECT
	t.time_key,
	t.date_record,
	t.hour_record,
	t.day_record,
	t.week_record,
	t.month_record,
	t.quarter_record,
	t.year_record,
	f.quantity,
	f.unit,
	f.unit_price,
	f.total_price
FROM
	dbo.fact_table AS f
RIGHT JOIN
	dbo.time_dim AS t
ON
	f.time_key=t.time_key
ORDER BY
	date_record

```


SELF JOIN to determine if there are any duplicate values in the table

```sql

SELECT
	tr.payment_key,
	tr.trans_type,
	tr.bank_name,
	t.payment_key,
	t.trans_type,
	t.bank_name
FROM
	dbo.Trans_dim AS tr
JOIN
	dbo.Trans_dim AS t
ON
	tr.payment_key=t.payment_key
AND
	tr.payment_key<>t.payment_key
;

```

SELF JOIN to compare values in the same table

```sql

SELECT
	tr.payment_key,
	tr.trans_type,
	tr.bank_name,
	t.payment_key,
	t.trans_type,
	t.bank_name
FROM
	dbo.Trans_dim AS tr
JOIN
	dbo.Trans_dim AS t
ON
	tr.payment_key=t.payment_key
;

```

CROSS JOIN to create a cartesian table, which is not useful in this scenario but is useful when wanting to populate a table with combinations of data from two tables

```sql

SELECT 
	customer_name,
	contact_no,
	nid,
	division,
	district,
	upazila
FROM
	dbo.customer_dim
CROSS JOIN
	store_dim
;

```







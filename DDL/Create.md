#### Creating a new table for employees including a primary key

```sql

CREATE TABLE Employees (
	emp_id INT PRIMARY KEY,
	employee_name VARCHAR(50) NOT NULL,
	email VARCHAR(50) NOT NULL,
	home_address VARCHAR(50) NOT NULL,
	phone VARCHAR(10) NOT NULL,
	job_title VARCHAR(20) NOT NULL,
	date_start DATE NOT NULL,
	salary NUMERIC(10,2) NOT NULL,
	department VARCHAR(20) NOT NULL
	);

```

---

#### Creating a backup of the employees table

```sql

CREATE TABLE EmployeesBackup 
    AS 
    SELECT * 
    FROM Employees;

```

---

DROP

```sql


```

---

UPDATE 

```sql


```

---

ALTER TABLE

```sql


```

---

TRUNCATE

```sql


```

---





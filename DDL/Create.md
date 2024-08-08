#### Creating a new table for employees including a primary key

```sql

CREATE TABLE Employees (
	emp_id INT PRIMARY KEY,
	employee_name VARCHAR(50) NOT NULL,
	email VARCHAR(50) NOT NULL,
	home_address VARCHAR(50) NOT NULL,
	phone varchar(10) NOT NULL,
	job_title varchar(10) NOT NULL,
	date_start date,
	salary numeric NOT NULL,
	department varchar(20) NOT NULL
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





## Stored Procedures

---

#### Definition of a stored procedure

- A stored procedure in SQL Server is a group of one or more Transact-SQL statements, or a reference to a Microsoft .NET Framework common runtime language (CLR) method.

---

#### Examples of stored procedures

#### Say we wanted to have sales totals available per month and year, we could create the following stored procedure :

```sql

CREATE PROCEDURE sp_yearly_monthly_sales
AS

SELECT YEAR(o.order_purchase_timestamp) AS year,
	   MONTH(o.order_purchase_timestamp) AS month,
	   SUM(p.payment_value) AS total_sales
FROM dbo.df_Payments AS p
JOIN dbo.df_Orders AS o
ON p.order_id = o.order_id
GROUP BY YEAR(order_purchase_timestamp), MONTH(order_purchase_timestamp)
ORDER BY year, month ASC
;

-- To run the procedure

EXEC yearly_monthly_sales
;

```

#### Say we wanted to be able to bring up all associated information for an invoice, we could create a stored procedure to efficiently bring up any invoice based on its invoice_id

```sql

-- Creating the procedure with a parameter

CREATE PROCEDURE sp_invoice_details @invoice_id VARCHAR(50)
AS

SELECT *
FROM dbo.supermarket_sales
WHERE invoice_id = @invoice_id
;

-- Executing the procedure with an input

EXEC dbo.sp_invoice_details @invoice_id = '750-67-8428'
;

```

#### Say we wanted to be able to view total units and sales quantities by date, or even by todays date, we could create a stored procedure 

```sql

-- Creating the procedure with a parameter

CREATE PROCEDURE sp_tblsupermarket_sales_todays_sales @transaction_date DATE
AS

SELECT SUM(quantity) AS units_sold,
       SUM(order_value) AS total_sales
FROM dbo.supermarket_sales
WHERE transaction_date = @transaction_date

-- Executing the procedure with an input

EXEC dbo.sp_tblsupermarket_sales_todays_sales @transaction_date = '2019-01-27'
;

```

#### Say we wanted to view customer contact details, sales value and sales volume efficiently, we could create the following stored procedure:

```sql

-- Creating the stored procedure with a parameter

CREATE PROCEDURE sp_sales_by_customer 
    @name VARCHAR(50)
AS
BEGIN
    SELECT c.name,
           c.contact_no,
           s.quantity,
           s.total_price
    FROM dbo.customer_dim AS c
    JOIN dbo.fact_table AS s
    ON c.customer_key = s.customer_key
    WHERE c.name = @name;
END
GO

-- Executing the procedure with an input

EXEC dbo.sp_sales_by_customer @name = 'sumit'

```



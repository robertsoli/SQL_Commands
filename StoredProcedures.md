## Stored Procedures

---

#### Definition of a stored procedure

- A stored procedure in SQL Server is a group of one or more Transact-SQL statements, or a reference to a Microsoft .NET Framework common runtime language (CLR) method.

---

#### Examples of stored procedures

#### Say we wanted to have sales totals available per month and year, we could create the following stored procedure :

```sql

CREATE PROCEDURE yearly_monthly_sales
AS

SELECT YEAR(o.order_purchase_timestamp) AS year,
	   MONTH(o.order_purchase_timestamp) AS month,
	   SUM(p.payment_value) AS total_sales
FROM dbo.df_Payments AS p
JOIN dbo.df_Orders AS o
ON p.order_id = o.order_id
GROUP BY YEAR(order_purchase_timestamp), MONTH(order_purchase_timestamp)
ORDER BY year, month ASC

-- To run the procedure

EXEC yearly_monthly_sales

```

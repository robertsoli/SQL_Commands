## Subquery

---

#### Definition of a subquery : 

- A subquery is a query that is nested inside a SELECT, INSERT, UPDATE, or DELETE statement, or inside another subquery.

---

#### Types of subqueries : 

**Scalar Subquery** : Returns a single value

**Column Subquery** : Returns a single column of values

**Multiple column subqueries** : Returns one or more columns.

**Single row subquery** : Returns a single row of values.

**Multiple row subquery** : Returns one or more rows.

**Table Subquery** : Returns a result set that can be treated as a table

**Correlated subqueries** : Reference one or more columns in the outer SQL statement. The subquery is known as a correlated subquery because the subquery is related to the outer SQL statement.

**Nested subqueries** : Subqueries that are placed within another subquery.

---

#### Example of a Scalar Subquery

#### Say we wanted to determine the top 10 orders in terms of order value, that are above the average order value

```sql

SELECT TOP 10 
	   order_id,
	   payment_value
FROM dbo.df_Payments
WHERE payment_value > (SELECT AVG(payment_value) FROM dbo.df_Payments)
ORDER BY payment_value DESC

```

#### Say 

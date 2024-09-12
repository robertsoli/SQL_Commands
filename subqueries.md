## Subquery

---

#### Definition of a subquery : 

- A subquery is a query that is nested inside a SELECT, INSERT, UPDATE, or DELETE statement, or inside another subquery.

---

#### Types of subqueries : 

**Scalar Subquery** : Returns a single value -- One done

**Column Subquery** : Returns a single column of values -- One done

**Multiple column subqueries** : Returns one or more columns.

**Single row subquery** : Returns a single row of values.

**Multiple row subquery** : Returns one or more rows.

**Table Subquery** : Returns a result set that can be treated as a table

**Correlated subqueries** : Reference one or more columns in the outer SQL statement. The subquery is known as a correlated subquery because the subquery is related to the outer SQL statement.

**Nested subqueries** : Subqueries that are placed within another subquery.

---

#### Example of a Scalar Subquery to determine the percentage of patients who have cancer in a given medical dataset

```sql

SELECT 
    CAST(
        (SELECT COUNT(Medical_Condition) FROM dbo.healthcare_dataset WHERE Medical_Condition = 'Cancer') AS DECIMAL(10, 2)
    ) / 
    CAST(
        (SELECT COUNT(DISTINCT Name) FROM dbo.healthcare_dataset) AS DECIMAL(10, 2)
    ) * 100 AS cancer_percentage
;

```

#### Example of a Column Subquery to print a list of the top 10 invoices whose total order amount is greater than the average

```sql

SELECT TOP 10 invoice_id
FROM dbo.supermarket_sales
WHERE order_value > 
	(SELECT AVG(order_value) 
         FROM dbo.supermarket_sales)
ORDER BY order_value DESC
;

```



#### Example of a multiple column subquery to 


```sql



```

#### Say 

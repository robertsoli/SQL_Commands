## Subquery

---

#### Definition of a subquery : 

- A subquery is a query that is nested inside a SELECT, INSERT, UPDATE, or DELETE statement, or inside another subquery.

---

#### Types of subqueries : 

**Scalar Subquery** : Returns a single value -- One done

**Column Subquery** : Returns a single column of values -- One done

**Multiple column subqueries** : Returns one or more columns. - One done

**Single row subquery** : Returns a single row of values. - One done

**Multiple row subquery** : Returns one or more rows. - 

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



#### Example of a multiple column subquery to determine the transaction volume and average, grouped by month and gender


```sql

SELECT month_of_transaction,
	   gender,
	   total_transaction_value,
	   customer_count,
	   avg_transaction_value
 FROM 
 (
	SELECT 
		MONTH(date) AS month_of_transaction,
		gender,
		SUM(amount) AS total_transaction_value,
		COUNT(transaction_id) AS customer_count,
		AVG(amount) AS avg_transaction_value
	FROM dbo.ANZ
	GROUP BY 
		MONTH(date), 
		gender
 ) AS           subquery
 ORDER BY 
   		month_of_transaction ASC
;

```

#### Example of a single row subquery to determine the customer who has the highest available balance

```sql

SELECT     customer_id,
	   first_name,
	   gender,
	   age,
	   balance
FROM   
	   dbo.ANZ
WHERE 
	   balance = (
	SELECT MAX(balance)
	FROM dbo.ANZ)
;

```

#### Example of a multiple row subquery to determine the high value customers(customers whose transaction values are in the top 25% in terms of transaction amount)
#### and the amount of high value transactions they have made

```sql

WITH TransactionQuartiles AS (
    SELECT 
           transaction_id,
           customer_id,
	   first_name,
           amount,
           NTILE(4) OVER (ORDER BY amount DESC) AS quartile
    FROM dbo.ANZ
),
HighValueTransactions AS (
    SELECT 
           transaction_id,
           customer_id,
           first_name,
           amount
    FROM   TransactionQuartiles
    WHERE  quartile = 4
),
CustomerHighValueCounts AS (
    SELECT 
           customer_id,
           first_name,
           COUNT(transaction_id) AS high_value_transaction_count
    FROM HighValueTransactions
    GROUP BY customer_id, first_name
)
SELECT 
       customer_id,
       first_name,
       high_value_transaction_count
FROM CustomerHighValueCounts
ORDER BY high_value_transaction_count DESC
;

```



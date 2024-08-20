#### Common Table Expression examples

Below is a CTE for printing orders that are higher than the average :

```sql

WITH avg_orders AS (
SELECT customer_key,
	     AVG(total_price) AS avg_order_total
FROM dbo.fact_table
GROUP BY customer_key)
SELECT f.customer_key,
	     f.total_price
FROM dbo.fact_table AS f
JOIN avg_orders AS a
ON f.customer_key=a.customer_key
WHERE f.total_price > a.avg_order_total
ORDER BY total_price DESC

```

Say we wanted to find the bottom 500 clients in terms of order size, we could use the below CTE:

```sql

WITH low_quantity_orders AS (
SELECT customer_key,
       AVG(quantity) AS avg_quantity
FROM dbo.fact_table
GROUP BY customer_key)
SELECT TOP 500 
	     f.customer_key,
       f.quantity
FROM dbo.fact_table AS f
JOIN low_quantity_orders AS l
ON f.customer_key = l.customer_key
WHERE l.avg_quantity > f.quantity
ORDER BY quantity ASC

```

Perhaps we wanted to find sales totals by supplier and the country in which the supplier resides, we could use the following CTE:

```sql

WITH sales_summary AS (
    SELECT item_key, SUM(total_price) AS total_sales
    FROM dbo.fact_table
    GROUP BY item_key
)
SELECT s.man_country, s.supplier, ss.total_sales
FROM dbo.item_dim s
JOIN sales_summary ss ON s.item_key = ss.item_key;

```

Suppose we wanted to find out what percentage of total sales each supplier contributed, we could use the following CTE :

```sql

WITH total_sales AS (
    SELECT SUM(total_price) AS overall_total
    FROM fact_table
),
supplier_sales AS (
    SELECT i.supplier, SUM(f.total_price) AS total_sales
    FROM item_dim i
    JOIN fact_table f ON i.item_key = f.item_key
    GROUP BY i.supplier
)
SELECT ss.supplier, 
       ss.total_sales, 
       CAST((ss.total_sales / t.overall_total) * 100 AS numeric(10,2)) AS SalesContributionPercentage
FROM supplier_sales ss
JOIN total_sales t ON 1 = 1;

```


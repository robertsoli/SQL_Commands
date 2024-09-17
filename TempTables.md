## Temporary Tables

---

### Definition of a temporary table 

A temporary table is a base table that exists only for the duration of a specific database session and is not stored permanently in the database. It allows for the storage and manipulation of data during the session, providing better performance for repeated data usage compared to views.

### Some reasons to use temporary tables

- **Simplifying Complex Queries:** Breaking down complex queries into more manageable parts.
- **Improving Performance:** Avoiding repeated calculations by storing intermediate results.
- **Isolating Data for Testing:** Debugging and validating parts of your queries.
- **Storing Results for Subsequent Operations:** Facilitating further analysis and operations.
- **Facilitating ETL Processes:** Managing intermediate stages in data transformation.
- **Handling Large Data Sets:** Processing data in manageable chunks.

---

### Examples of Temporary Tables in use when simplifying queries

Say we wanted to do some calculations to determine profit margin, or average shipping cost per order, we could create a temporary table containing the aggregated data to simplify the querying process:

```sql

-- Dropping the table if it exists, always good practice

DROP TABLE IF EXISTS #SupplierTransactions

```

```sql

-- Creating the temporary table

CREATE TABLE #SupplierTransactions
(
	supplier_name VARCHAR(50),
	total_shipping_costs DECIMAL(18,2),
	total_manufacturing_costs DECIMAL(18,2),
	total_revenue DECIMAL(18,2),
	total_order_quantities INT
)
;

```

```sql

-- Inserting values into #SupplierTransactions

INSERT INTO #SupplierTransactions (supplier_name, total_shipping_costs, total_manufacturing_costs, total_revenue, total_order_quantities)
SELECT 
	supplier_name,
	SUM(order_quantities * shipping_costs) AS total_shipping_costs,
	SUM(order_quantities * manufacturing_costs) AS total_manufacturing_costs,
	SUM(total_revenue) AS total_revenue,
	SUM(order_quantities) AS total_order_quantities
FROM dbo.supply_chain_data
GROUP BY supplier_name
;

```

Now that we have the temporary table, lets do some calculations. First, lets calculate the total profit generated and the profit margin by each supplier :

```sql

SELECT 
	supplier_name,
	total_shipping_costs,
	total_manufacturing_costs,
	total_revenue,
(total_revenue - (total_shipping_costs + total_manufacturing_costs)) AS total_profit,
ROUND((total_revenue - (total_shipping_costs + total_manufacturing_costs)) / total_revenue * 100,2) AS profit_margin
FROM #SupplierTransactions
;

```

Then lets look at what the average shipping cost per unit is by supplier, again using the temporary table to reduce the complexity of the query we are writing:

```sql

SELECT 
	supplier_name,
	AVG(total_shipping_costs / total_order_quantities) AS avg_shipping_cost_per_unit
FROM 
	#SupplierTransactions
GROUP BY
	supplier_name

```



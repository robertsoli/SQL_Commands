Some business tasks to show window functions in use: 

1. Determine MOM and YOY sales using LAG Function, OVER and PARTITION BY

```sql

SELECT 
    	ap.FullName,
	MONTH(i.ConfirmedDeliveryTime) AS month,
	YEAR(i.ConfirmedDeliveryTime) AS year,
    	SUM(il.ExtendedPrice) AS total_sales,
    	SUM(il.LineProfit) AS total_profit,
    	SUM(il.ExtendedPrice) - LAG(SUM(il.ExtendedPrice)) OVER (PARTITION BY ap.FullName ORDER BY YEAR(i.ConfirmedDeliveryTime)) AS YoYGrowth,
	SUM(il.ExtendedPrice) - LAG(SUM(il.ExtendedPrice)) OVER (PARTITION BY ap.FullName ORDER BY MONTH(i.ConfirmedDeliveryTime)) AS MoMGrowth
FROM 
    	Sales.InvoiceLines AS il
JOIN 
    	Sales.Invoices AS i ON i.InvoiceID = il.InvoiceID
JOIN 
    	Application.People AS ap ON i.SalespersonPersonID = ap.PersonID
WHERE
    	i.ConfirmedDeliveryTime IS NOT NULL
GROUP BY 
    	ap.FullName, YEAR(i.ConfirmedDeliveryTime), MONTH(i.ConfirmedDeliveryTime)
ORDER BY 
    	ap.FullName, month, year;
;

```

The OVER function is also useful for calculating running totals, so lets do so for the most recent years sales 

In this query, the SUM(total_sales) OVER (ORDER BY year, month, day) computes a running total of sales. This function iteratively adds the total_sales for each row in the specified order (by date), providing a cumulative figure that reflects the total sales from the start of the data set up to that day.

```sql

WITH DailySales AS (
SELECT 
	YEAR(i.ConfirmedDeliveryTime) AS year,
	MONTH(i.ConfirmedDeliveryTime) AS month,
	DAY(i.ConfirmedDeliveryTime) AS day,
	SUM(il.ExtendedPrice) AS total_sales,
	COUNT(DISTINCT i.InvoiceID) AS invoice_count,
	AVG(il.ExtendedPrice) AS avg_sale_per_invoice
FROM
  	sales.Invoices AS i
JOIN 	sales.InvoiceLines AS il
ON 	il.InvoiceID = i.InvoiceID
WHERE
    	YEAR(i.ConfirmedDeliveryTime) = 2016
AND 	i.ConfirmedDeliveryTime IS NOT NULL 
GROUP BY 
    	YEAR(i.ConfirmedDeliveryTime),
    	MONTH(i.ConfirmedDeliveryTime),
    	DAY(i.ConfirmedDeliveryTime)
)

SELECT 
	year,
	month,
	day,
	total_sales,
	SUM(total_sales) OVER (ORDER BY year, month, day) AS running_total,
	invoice_count,
	total_sales / NULLIF(invoice_count, 0) AS avg_sale_per_invoice
FROM
  	DailySales
ORDER BY
	year,
	month,
	day
;

```

Next up lets create a query to rank product performance by monthly sales and units sold, using the RANK function. 
We are also using the OVER() PARTITION BY function to create smaller manageable subsets of the data, in this case by month, which allows for independant calculations within each segment.

```sql

WITH MonthlyProductSales2016 AS (
SELECT
	il.StockItemID,
	il.Description,
	MONTH(i.ConfirmedDeliveryTime) AS month,
	SUM(il.ExtendedPrice) AS total_sales,
	SUM(il.Quantity) AS total_units
FROM 
	sales.Invoices AS i
JOIN
	sales.InvoiceLines AS il
ON
	i.InvoiceID = il.InvoiceID
WHERE 
	YEAR(i.ConfirmedDeliveryTime) = 2016
GROUP BY 
	il.StockItemID,
	MONTH(i.ConfirmedDeliveryTime),
	il.Description
)

SELECT 
	StockItemID,
	Description,
	month,
	total_sales,
	total_units,
	RANK() OVER(PARTITION BY month ORDER BY total_sales DESC) AS sales_ranking
FROM
	MonthlyProductSales2016
ORDER BY
	month, sales_ranking
;

```

#### DENSERANK



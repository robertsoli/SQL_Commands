## Views

---

### Description of a view :

A view is a virtual table whose contents are defined by a query. Like a table, a view consists of a set of named columns and rows of data. Unless indexed, a view does not exist as a stored set of data values in a database. The rows and columns of data come from tables referenced in the query defining the view and are produced dynamically when the view is referenced.

Besides user-defined views, SQL Server also provides the following view types :

- Indexed views
- Partitioned views
- System views

---

### Data Sources

#### We will be using various data sources for the examples below, they are listed here

- pubs dataset : 11 tables relating to authors, titles, stores, employees, publishers and sales

---

### Examples of user-defined views

#### To focus, simplify, and customize the perception each user has of the database.

#### As a security mechanism by allowing users to access data through the view, without granting the users permissions to directly access the underlying base tables.

- Create department specific views, some ideas may include omitting confidential information, also show row level security and column level security

#### Say we wanted to view authors, the titles they have published and their sales to date, we could create the following view:

```sql

CREATE VIEW vw_author_titles_sales
AS

SELECT
     a.au_fname,
     a.au_lname,
     t.title,
     t.price,
     t.ytd_sales,
     ta.title_id
FROM dbo.authors AS a
INNER JOIN dbo.titleauthor AS ta
ON ta.au_id = a.au_id
INNER JOIN dbo.titles AS t
ON ta.title_id = t.title_id
;

```

For the examples below we will be using the AdventureWorks dataset from Microsoft due to its size and schema. The goal is to create some views that would be useful by having clean and ready data on hand for department specific reporting.

The tables we will be using in order: 

Sales - list them here as you go
Name table

Some specific business tasks: 

Include back ordered invoices 

---

For the examples below we will be using the AdventureWorks dataset from Microsoft due to its size and schema. The goal is to create some views that would be useful by having clean and ready data on hand for department specific reporting.

The tables we will be using in order: 

Sales - list them here as you go
Name table

Some specific business tasks: 

Include back ordered invoices 

---

Monthly Sales Summary view for the Sales Department

```sql

CREATE VIEW v_sales_summary_may_2016
AS

SELECT 
	YEAR(i.ConfirmedDeliveryTime) AS year,
	MONTH(i.ConfirmedDeliveryTime) AS month,
	a.FullName AS sales_person,
	COUNT(DISTINCT il.InvoiceID) AS order_count,
	SUM(il.Quantity) AS units_sold,
	SUM(il.ExtendedPrice) AS total_sales,
	SUM(il.TaxAmount) AS total_tax,
	SUM(il.LineProfit) AS total_profit
FROM Sales.Invoices AS i
JOIN Sales.InvoiceLines AS il
ON i.InvoiceID = il.InvoiceID
JOIN Application.People AS a
ON a.PersonID = i.SalespersonPersonID
WHERE a.IsSalesperson = 1
AND YEAR(i.ConfirmedDeliveryTime) = 2016
AND MONTH(i.ConfirmedDeliveryTime) = 5
AND i.ConfirmedDeliveryTime IS NOT NULL
GROUP BY YEAR(i.ConfirmedDeliveryTime),
		 MONTH(i.ConfirmedDeliveryTime),
		 a.FullName
;

```

Creating a View for HR that contains all employee details

```sql

CREATE VIEW v_employee_details
AS 

SELECT 
	p.PersonID,
	p.FullName,
	p.PhoneNumber,
	p.EmailAddress
FROM 
	Application.People AS p
WHERE
     p.IsEmployee = 1
;

```

Creating a View for the Warehouse Department to list product stock levels that are below the ideal reorder level, keeping in mind the target stock level set

```sql

CREATE VIEW v_stock_order
AS

SELECT 
	StockItemID,
	reorder_quantity
FROM(
	SELECT 
		sih.StockItemID,
	SUM(CASE 
        WHEN sih.QuantityOnHand < sih.ReorderLevel THEN (sih.TargetStockLevel - sih.QuantityOnHand) 
        ELSE 0 
     END) AS reorder_quantity
FROM 
         Warehouse.StockItemHoldings AS sih
GROUP BY 
         sih.StockItemID) AS sub
WHERE 
         reorder_quantity != 0
;

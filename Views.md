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

View for Monthly Sales Summary

Table Sales.CustomerTransactions for the following columns:

ConfirmedDeliveryTime to extract and group by week and month and year if needed 
SalespersonPersonId to determine sales volume and performance per sales person
Quantity for unit volume
COUNT DISTINCT InvoiceID for number of orders
TaxAmount for tax total
LineProfit for Profit total
ExtendedPrice for total sales
CustomerCategories.CustomerCategoryName for which type of client, ie wholesale, agent, novelty shop, etc (group data by these groups as well for the report) JOINED ON 
Customers.CustomerCategoryID in both tables



Joined with 

Application.People on PersonID (IsSalesPerson column to filter out these individuals)
Reason is to get their name for use in the sales report view


```sql



```

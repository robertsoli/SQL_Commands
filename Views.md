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

#### 

#### The indexes below are created for improving query performance on all tables in a database named Ecommerce Supply Chain

#### Clustered Indexes will be created for each table to sort and store the data rows in the table based on their key values.
#### Non-Clustered Indexes will be created for each table to improve query performance on frequently queried columns.

---

##### We'll create a Clustered Index for the Customers table on customer_id

```sql


CREATE CLUSTERED INDEX IX_tblCustomers_customer_id
ON dbo.df_Customers (customer_id ASC)

```

##### We'll create a Clustered Index for the OrderItems table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblOrderItems_order_id
ON dbo.df_OrderItems (order_id ASC)

```

##### We'll create a Clustered Index for the Orders table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblOrders_order_id
ON dbo.df_Orders (order_id ASC)

```




---

##### Non Clustered index for use in linking orders to payments for the finance department 

```sql


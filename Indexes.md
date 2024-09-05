#### The indexes below are created for improving query performance on all tables in a database named Ecommerce Supply Chain

#### Clustered Indexes will be created for each table to sort and store the data rows in the table based on their key values.
- Add criteria for what constitutes a good selection of a clustered index

#### Non-Clustered Indexes will be created for each table to improve query performance on frequently queried columns.
- Add criteria for what constitutes a good selection of a non clustered index

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

##### We'll create a Clustered Index for the Payments table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblPayments_order_id
ON dbo.df_Payments (order_id ASC)

```

##### We'll create a Clustered Index for the Products table on product_id

```sql

CREATE CLUSTERED INDEX IX_tblProducts_product_id
ON dbo.df_Products (product_id ASC)

```

---

*Include details as to why non clustered indexes are useful before the below*


##### Non Clustered index for the customer_city column

```sql

CREATE NONCLUSTERED INDEX IX_tblCustomers_customer_city
ON dbo.df_Customers (customer_city ASC)

```




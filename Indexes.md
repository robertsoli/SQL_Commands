## Indexes

#### The indexes below are created for improving query performance on all tables in a database named Ecommerce Supply Chain.
#### Selection criteria for clustered indexes are based on the index being a unique identifier, and ideally ever increasing. 
#### Selection criteria for non-clustered indexes are to improve the performance of frequently used queries not covered by the clustered index.

---

**Clustered Indexes** will be created for each table to sort and store the data rows in the table based on their key values.

Definition of a clustered index:

- A clustered index sorts and stores the data rows of the table or view in order based on the clustered index key. The clustered index is implemented as a B-tree index structure that supports fast retrieval of the rows, based on their clustered index key values.

##### A Clustered Index for the Customers table on customer_id

```sql

CREATE CLUSTERED INDEX IX_tblCustomers_customer_id
ON dbo.df_Customers (customer_id ASC)

```

##### A Clustered Index for the OrderItems table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblOrderItems_order_id
ON dbo.df_OrderItems (order_id ASC)

```

##### A create a Clustered Index for the Orders table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblOrders_order_id
ON dbo.df_Orders (order_id ASC)

```

##### A Clustered Index for the Payments table on order_id

```sql

CREATE CLUSTERED INDEX IX_tblPayments_order_id
ON dbo.df_Payments (order_id ASC)

```

##### A Clustered Index for the Products table on product_id

```sql

CREATE CLUSTERED INDEX IX_tblProducts_product_id
ON dbo.df_Products (product_id ASC)

```

---

**Non-clustered Indexes** will be created for each table to improve query performance on frequently queried columns.

Definition of a non-clustered index 

- A non-clustered index can be defined on a table or view with a clustered index or on a heap. Each index row in the non-clustered index contains the non-clustered key value and a row locator. This locator points to the data row in the clustered index or heap having the key value. The rows in the index are stored in the order of the index key values, but the data rows are not guaranteed to be in any particular order unless a clustered index is created on the table.

##### A non-clustered index for the Customers table on the customer_city column

```sql

CREATE NONCLUSTERED INDEX IX_tblCustomers_customer_city
ON dbo.df_Customers (customer_city ASC)

```

#### A non-clustered index for the OrderItems table on the price column

```sql

CREATE NONCLUSTERED INDEX IX_tblOrderItems_price
ON dbo.df_OrderItems (price ASC)

```

#### A non-clustered index for the OrderItems table on the shipping_charges column

```sql

CREATE NONCLUSTERED INDEX IX_tblOrderItems_shipping_charges
ON dbo.df_OrderItems (shipping_charges ASC)

```

#### A non-clustered index for the Orders table on the order_purchase_timestamp column

```sql

CREATE NONCLUSTERED INDEX IX_tblOrders_order_purchase_timestamp
ON dbo.df_Orders (order_purchase_timestamp ASC)

```

#### A non-clustered index for the Payments table on the payment_value column

```sql

CREATE NONCLUSTERED INDEX IX_tblPayments_payment_value
ON dbo.df_Payments (payment_value ASC)

```


#### A non-clustered index for the Products table on the product_weight_g column

```sql

CREATE NONCLUSTERED INDEX IX_tblProducts_product_weight_g
ON dbo.df_Products (product_weight_g ASC)

```

---

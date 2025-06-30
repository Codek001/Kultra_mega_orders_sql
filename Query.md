
# /* EDA of KMS */
-- Knowing how much data you have is a first step in exploratory data analysis. 
/* The data provided in the kms_table consisted of 20 variables and 8399 observations. 
There are 8399 records. 1 variable was dropped (product_name). */
/* While the data provided in the Order_status_table consisted of 2 variables and 572 observations. */
``` sql
SELECT * FROM dsa_db.kms;
SELECT * FROM dsa_db.order_status;

SELECT COUNT(Row_ID) FROM dsa_db.kms
WHERE Row_ID IS NOT NULL;
-- the total records in kms_table is 8,399

SELECT COUNT(*) FROM dsa_db.order_status;
-- the total records in order_status is 572. There is no missing data.
SELECT order_status.Status, COUNT(*) FROM dsa_db.order_status
GROUP BY order_status.Status;
-- The order-status revealed that there are 572 retured ordered
```


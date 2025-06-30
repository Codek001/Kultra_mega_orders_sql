
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

``` sql
-- DATA CLEANING
SELECT COUNT(Product_Base_Margin) FROM dsa_db.kms
WHERE Product_Base_Margin = 0;
-- 63 blank spaces were replaced with 0.

-- Handling Missing Values
-- Fill missing values in the 'Product_Base_Margin' column with the average margin. The missings were first replaced with 0
UPDATE dsa_db.kms
SET Product_Base_Margin = (SELECT ROUND(AVG(Product_Base_Margin), 2) FROM dsa_db.kms)
WHERE Product_Base_Margin = 0;
SELECT  COUNT(Product_Base_Margin) FROM dsa_db.kms
WHERE Product_Base_Margin IS NULL;

 (SELECT ROUND(AVG(Product_Base_Margin), 2) FROM dsa_db.kms); 
 -- The mean of the Product_Base_Margin is 0.51
 SELECT  Product_Base_Margin FROM dsa_db.kms;
 -- Update Product_Base_Margin with the average value where it is 0
-- Fixing the error by using a subquery correctly
SET SQL_SAFE_UPDATES = 0;
UPDATE kms
SET Product_Base_Margin = (
    SELECT ROUND(AVG(Product_Base_Margin), 2) 
    FROM (SELECT Product_Base_Margin FROM kms WHERE Product_Base_Margin != 0) AS subquery
)
WHERE Product_Base_Margin = 0;
(SELECT ROUND(AVG(Product_Base_Margin), 2) FROM dsa_db.kms); 
 -- The mean of the Product_Base_Margin is 0.51
 
-- Data Type Conversion
-- Convert 'Order_Date' and 'Ship_Date' to DATE type
-- Fix incorrect date format before conversion
UPDATE dsa_db.kms
SET Order_Date = STR_TO_DATE(Order_Date, '%m/%d/%Y')
WHERE STR_TO_DATE(Order_Date, '%m/%d/%Y') IS NOT NULL;
UPDATE dsa_db.kms
SET Ship_Date = STR_TO_DATE(Ship_Date, '%m/%d/%Y')
WHERE STR_TO_DATE(Ship_Date, '%m/%d/%Y') IS NOT NULL;

-- Standardizing Categorical Values
--  Standardize 'Order_Priority' values
UPDATE dsa_db.kms
SET Order_Priority = 'Not Specified'
WHERE Order_Priority IS NULL OR Order_Priority = '';
-- There is no missing variable
```
``` sql
-- To perform an exploratory data analysis (EDA) using MySQL, we will follow these steps:
SELECT * FROM kms LIMIT 100;
-- Describe the structure of each table
DESCRIBE dsa_db.kms;
-- Calculate summary statistics for numerical columns
-- Order_Quantity
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Order_Quantity) AS average_value,
    MIN(kms.Order_Quantity) AS min_value,
    MAX(kms.Order_Quantity) AS max_value
FROM kms;
-- The mean value of Order_Quantity is 25.57, the minimum value is 1 and maximum is 50 
-- Sales
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Sales) AS average_value,
    MIN(kms.Sales) AS min_value,
    MAX(kms.Sales) AS max_value
FROM kms;
-- The mean value of Sales is 1775.89 , the minimum value is 2.24 and maximum is 89061.05 
-- Discount
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Discount) AS average_value,
    MIN(kms.Discount) AS min_value,
    MAX(kms.Discount) AS max_value
FROM kms;
-- The mean value of Discount is 0.05 , the minimum value is 0 and maximum is 0.25
-- Profit 
 SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Profit) AS average_value,
    MIN(kms.Profit) AS min_value,
    MAX(kms.Profit) AS max_value
FROM kms;
-- The mean value of Profit is 181.18 , the minimum value is -14140.7 (loss) and maximum is 27220.29
-- Unit_price
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Unit_Price) AS average_value,
    MIN(kms.Unit_Price) AS min_value,
    MAX(kms.Unit_Price) AS max_value
FROM kms;
-- The mean value of Unit_Price is 89.35 , the minimum value is 0.99 and maximum is 6783.02
-- Unit_price
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Unit_Price) AS average_value,
    MIN(kms.Unit_Price) AS min_value,
    MAX(kms.Unit_Price) AS max_value
FROM kms;
-- The mean value of Unit_Price is 89.35 , the minimum value is 0.99 and maximum is 6783.02
-- Shipping_Cost
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Shipping_Cost) AS average_value,
    MIN(kms.Shipping_Cost) AS min_value,
    MAX(kms.Shipping_Cost) AS max_value
FROM kms;
-- The mean value of Shipping_Cost is 12.84 , the minimum value is 0.49 and maximum is 164.73
-- Product_Base_Margin
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Product_Base_Margin) AS average_value,
    MIN(kms.Product_Base_Margin) AS min_value,
    MAX(kms.Product_Base_Margin) AS max_value
FROM kms;
-- The mean value of Product_Base_Margin is 0.51 , the minimum value is 0.35 and maximum is 0.85
-- Distribution of order priorities
-- Check how many orders fall into each priority category.
SELECT 
    Order_Priority, 
    COUNT(*) AS frequency
FROM kms
GROUP BY Order_Priority
ORDER BY frequency DESC;
-- the highest Order_Priority is High with with order of 1768, then Low	with (1720).

--  Most common shipping modes
-- Identify the most frequently used shipping modes.
SELECT 
    Ship_Mode, 
    COUNT(*) AS frequency
FROM kms
GROUP BY Ship_Mode
ORDER BY frequency DESC;
-- the most frequently used shipping mode is  Regular Air	with (6270), then Delivery Truck(1146) and Express Air(983). 
-- Profitability by product category
-- Analyze which product categories are the most profitable
SELECT 
    Product_Category, 
    SUM(Profit) AS total_profit
FROM kms
GROUP BY Product_Category
ORDER BY total_profit DESC;
-- It reveals that Technology and Office Supplies are the most profitable product categories
-- Customer segmentation analysis
-- Explore the distribution of customer segments.
SELECT 
    Customer_Segment, 
    COUNT(*) AS frequency
FROM kms
GROUP BY Customer_Segment
ORDER BY frequency DESC;
-- The most common customer segment is corporate, followed by Home Office, then Consumer and Small business.
```

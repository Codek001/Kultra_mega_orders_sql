
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
``` sql
-- Questions
-- 1. Which product category had the highest sales?
SELECT 
    Product_Category, 
    MAX(Sales) AS highest_sales
FROM kms
GROUP BY Product_Category
ORDER BY highest_sales DESC;
-- It reveals that Technology product category had the highest sales (89061.05)

-- 2. What are the Top 3 and Bottom 3 regions in terms of sales?
SELECT Region, SUM(Sales) AS total_sales 
FROM kms 
GROUP BY Region
ORDER BY total_sales DESC
LIMIT 3;
-- the Top 3 regions in terms of sales are West, Ontario, and Prarie
SELECT Region, SUM(Sales) AS total_sales 
FROM kms 
GROUP BY Region
ORDER BY total_sales ASC
LIMIT 3;
-- the bottom 3 regions in terms of sales are Nunavut, Northwest Territories, and Yukon
-- 3. Altering column names
SELECT COUNT(*) FROM kms WHERE `Product_Sub-Category` = 'appliances';
-- we have total number of `Product_Sub-Category` = 'appliances' to be 434
-- Rename column 'Product_Sub-Category' to 'Product_Sub_Category'
ALTER TABLE kms 
CHANGE COLUMN `Product_Sub-Category` Product_Sub_Category VARCHAR(255);
-- 3. What were the total sales of appliances in Ontario?
SELECT Product_Sub_Category, SUM(Sales) AS total_sales
FROM kms
WHERE Product_Sub_Category = 'appliances' AND Region = 'Ontario';
-- the total sales of appliances in Ontario is 202346.84
-- 4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers
SELECT Customer_Name, SUM(Sales) AS total_sales 
FROM kms 
GROUP BY Customer_Name
ORDER BY total_sales ASC
LIMIT 10;
/*Customer Engagement: Increase engagement with these customers through personalized marketing campaigns, loyalty programs, or special discounts.
Product Recommendations: Analyze their purchase history to recommend products that align with their interests or needs.
Feedback and Improvement: Collect feedback from these customers to understand their needs and improve the product or service offerings.
Enhanced Customer Service: Provide exceptional customer service to build stronger relationships and encourage repeat purchases.
Cross-Selling and Upselling: Identify opportunities to cross-sell or upsell products that complement their previous purchases.*/

-- 5. KMS incurred the most shipping cost using which shipping method?
SELECT Ship_Mode, SUM(Shipping_Cost) AS total_shipping_cost
FROM kms
GROUP BY Ship_Mode
ORDER BY total_shipping_cost DESC
LIMIT 1;
-- Delivery Truck shipping method incurred the most shipping cost
-- 6. Who are the most valuable customers, and what products or services do they typically purchase?
SELECT Customer_Name,  Product_Category, Round(SUM(Sales),3) AS sales
FROM kms 
GROUP BY Customer_Name,  Product_Category
ORDER BY sales DESC
LIMIT 10;
-- it revealed that the most valuable customers purchased typically technological products
-- 7. Which small business customer had the highest sales?
SELECT Customer_Name, MAX(Sales) AS highest_sales
FROM kms
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY highest_sales DESC
LIMIT 1;
-- Dannies Kane is the small business customer who had the highest sales
-- 8. Which Corporate Customer placed the most number of orders in 2009 – 2012?
SELECT Customer_Name, COUNT(Order_ID) AS most_order
FROM kms
WHERE Customer_Segment = 'Corporate' AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY most_order DESC
LIMIT 1;
-- Adam Hart is the Corporate Customer who placed the most number of orders in 2009 – 2012
-- 9. Which consumer customer was the most profitable one?
SELECT Customer_Name, MAX(Profit) AS most_profitable
FROM kms
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY most_profitable DESC
LIMIT 1;
-- Emily Phan is the consumer customer that was the most profitable
-- 10. Which customer returned items, and what segment do they belong to?
SELECT k.Customer_Name, k.Customer_Segment
FROM kms AS k
JOIN order_status AS o
	ON k.Order_ID = o.Order_ID
WHERE o.Status = 'Returned';

SELECT Order_ID, Customer_Name, Customer_Segment, Order_Priority, DATEDIFF(Ship_Date, Order_Date) AS date_diff
FROM kms 
WHERE Order_ID IN (
		SELECT Order_ID FROM order_status WHERE Status = 'Returned')
ORDER BY date_diff DESC;
-- customers from the 3 segments returned goods, 872 order_id goods were returned while the difference between the Order_Date and Shipping_Date in days ranges between 14 to 0 day, with just 1 good for 14 days, others from 9 days to 0 day. Majority of the goods were delivered less than 4 days. 
Select COUNT(*) from order_status;

SELECT DISTINCT k.Customer_Name, k.Customer_Segment, COUNT(k.Customer_Segment) AS count_gr
FROM kms AS k
JOIN order_status AS o
	ON k.Order_ID = o.Order_ID
WHERE o.Status = 'Returned'
GROUP BY k.Customer_Name, k.Customer_Segment
ORDER BY count_gr DESC;
--  customers from the 3 segments returned goods, 872 order_id goods were returned

-- 11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer
SELECT Ship_Mode, Order_Priority, SUM(Shipping_Cost) AS shipping
FROM kms
GROUP BY Ship_Mode, Order_Priority
ORDER BY shipping DESC; 
-- The result showed that the delivery truck mode has the highest mean of shipping_cost value among all the shipping methods and the order_priority, it can be deduced that it is most expensive. While the most used is Regular Air
-- Even though, The goal is to ensure that high-priority orders are shipped using faster methods like Express Air, while lower-priority orders use more economical methods like Delivery Truck.
-- I would advice the company appropriately spent shipping costs based on the Order Priority and consider more goods shipped using faster methods except for critical orders using delivery Truck.
SELECT Ship_Mode, Order_Priority, COUNT(Shipping_Cost) AS shipping
FROM kms
GROUP BY Ship_Mode, Order_Priority
ORDER BY shipping DESC; 

SELECT Ship_Mode, Order_Priority, AVG(Shipping_Cost) AS shipping
FROM kms
GROUP BY Ship_Mode, Order_Priority
ORDER BY shipping DESC; 

-- Shipping_Cost
SELECT 
    COUNT(*) AS total_rows,
    AVG(kms.Shipping_Cost) AS average_value,
    MIN(kms.Shipping_Cost) AS min_value,
    MAX(kms.Shipping_Cost) AS max_value
FROM kms;
-- The mean value of Shipping_Cost is 12.84 , the minimum value is 0.49 and maximum is 164.73

SELECT Ship_Mode, Order_Priority, Ship_Date, Order_Date, DATEDIFF(Ship_Date, Order_Date) AS date_diff
FROM kms
ORDER BY date_diff DESC
LIMIT 1000;

SELECT Ship_Mode, Order_Priority, SUM(Shipping_Cost) AS total_shipping_cost
FROM kms
GROUP BY Ship_Mode, Order_Priority
ORDER BY Order_Priority, total_shipping_cost DESC;
```

# Kultra_mega_stores_orders_sql
Documentation of my data analysis project using SQL (DSA_Incubator_hub project).

# Exploratory Data Analysis (EDA) of KMS

## Introduction

This report provides a comprehensive exploratory data analysis (EDA) of the KMS dataset. The analysis aims to uncover insights into the data, identify patterns, and provide actionable recommendations for the management of KMS. The dataset consists of two main tables: `kms_table` with 20 variables and 8399 observations, and `order_status_table` with 2 variables and 572 observations.

## Data Overview

### KMS Table

The `kms_table` contains 20 variables and 8399 observations. Initially, one variable, `product_name`, was dropped from the analysis. The table provides detailed information about orders, including dates, quantities, sales, discounts, profits, and more.

Table: kms
Columns:
1.	Row ID (int): A unique identifier for each row in the dataset.
2.	Order ID (int): A unique identifier for each order.
3.	Order Date (text): The date on which the order was placed.
4.	Order Priority (text): The priority level of the order (e.g., High, Medium, Low).
5.	Order Quantity (int): The number of units ordered.
6.	Sales (double): The total sales amount for the order.
7.	Discount (double): The discount applied to the order.
8.	Ship Mode (text): The mode of shipping used for the order (e.g., Standard, Express).
9.	Profit (double): The profit made from the order.
10.	Unit Price (double): The price per unit of the product.
11.	Shipping Cost (double): The cost incurred for shipping the order.
12.	Customer Name (text): The name of the customer who placed the order.
13.	Province (text): The province where the customer is located.
14.	Region (text): The region where the customer is located.
15.	Customer Segment (text): The segment to which the customer belongs (e.g., Consumer, Corporate).
16.	Product Category (text): The category of the product ordered (e.g., Technology, Furniture).
17.	Product Sub-Category (text): The sub-category of the product ordered.
18.	Product Name (text): The name of the product ordered.
19.	Product Container (text): The type of container used for the product (e.g., Box, Wrap).
20.	Product Base Margin (double): The base margin for the product.
21.	Ship Date (text): The date on which the order was shipped.


### Order Status Table

The `order_status_table` consists of 2 variables and 572 observations. This table records the status of orders, indicating whether they were returned.

## Data Cleaning

### Handling Missing Values

- **Product Base Margin**: Initially, 63 blank spaces in the `Product_Base_Margin` column were replaced with 0. These were then updated to the average margin value of 0.51.

### Data Type Conversion

- **Date Conversion**: The `Order_Date` and `Ship_Date` columns were converted to the DATE type to facilitate date calculations.

### Standardizing Categorical Values

- **Order Priority**: Missing or empty values in the `Order_Priority` column were standardized to 'Not Specified'.

## Summary Statistics

### Numerical Columns

- **Order Quantity**: Mean = 25.57, Min = 1, Max = 50
- **Sales**: Mean = 1775.89, Min = 2.24, Max = 89061.05
- **Discount**: Mean = 0.05, Min = 0, Max = 0.25
- **Profit**: Mean = 181.18, Min = -14140.7, Max = 27220.29
- **Unit Price**: Mean = 89.35, Min = 0.99, Max = 6783.02
- **Shipping Cost**: Mean = 12.84, Min = 0.49, Max = 164.73
- **Product Base Margin**: Mean = 0.51, Min = 0.35, Max = 0.85

## Exploratory Analysis

### Distribution of Order Priorities

The highest order priority is 'High' with 1768 orders, followed by 'Low' with 1720 orders.

### Shipping Modes

The most frequently used shipping mode is 'Regular Air' with 6270 shipments, followed by 'Delivery Truck' (1146) and 'Express Air' (983).

### Profitability by Product Category

The analysis reveals that 'Technology' and 'Office Supplies' are the most profitable product categories.

### Customer Segmentation

The most common customer segment is 'Corporate', followed by 'Home Office', 'Consumer', and 'Small Business'.

## Key Insights and Recommendations

1. **Product Category Sales**: The 'Technology' product category had the highest sales, indicating a strong market demand.

2. **Regional Sales**: The top 3 regions in terms of sales are 'West', 'Ontario', and 'Prarie', while the bottom 3 are 'Nunavut', 'Northwest Territories', and 'Yukon'.

3. **Customer Engagement**: To increase revenue from the bottom 10 customers, consider personalized marketing campaigns, product recommendations, and enhanced customer service.

4. **Shipping Costs**: The 'Delivery Truck' method incurred the most shipping cost. It's recommended to align shipping methods with order priorities to optimize costs.

5. **Valuable Customers**: The most valuable customers typically purchase technological products. Focus on maintaining strong relationships with these customers.

6. **Returned Items**: Customers from all segments returned goods, with 872 orders returned. Most goods were delivered in less than 5 days.

7. **Order Priority and Shipping**: Ensure high-priority orders are shipped using faster methods like 'Express Air', while lower-priority orders use more economical methods like 'Delivery Truck'.

## Conclusion

The EDA of the KMS dataset provides valuable insights into sales, customer behavior, and operational efficiency. By addressing the identified areas, KMS can enhance its market position and profitability.

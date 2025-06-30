
---

# 📊 Exploratory Data Analysis Report for KMS Dataset

## 📁 Data Overview

| Table          | Observations | Variables                             | Missing Data                                              | Notes                       |
| -------------- | ------------ | ------------------------------------- | --------------------------------------------------------- | --------------------------- |
| `kms`          | 8,399        | 21 (20 after dropping `product_name`) | Few missing/zero entries in `Product_Base_Margin` handled | Main transactional dataset  |
| `order_status` | 572          | 2                                     | None                                                      | Used to track return status |
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

---

## 🔍 Data Cleaning & Preprocessing

### 1. **Dropped Unnecessary Column**

* `product_name` was removed due to redundancy or lack of analytical relevance.

### 2. **Handling Missing Values**

* `Product_Base_Margin`: 63 blanks replaced with `0`.
* Then replaced all `0` entries with the **mean value** of non-zero margins (0.51).

### 3. **Date Type Conversion**

* `Order_Date` and `Ship_Date` were cleaned and converted to `DATE` format for accurate temporal analysis.

### 4. **Standardizing Categorical Variables**

* Null or blank entries in `Order_Priority` were replaced with `'Not Specified'`.

---

## 📈 Summary Statistics

| Metric                | Mean    | Min      | Max      |
| --------------------- | ------- | -------- | -------- |
| `Order_Quantity`      | 25.57   | 1        | 50       |
| `Sales`               | 1775.89 | 2.24     | 89061.05 |
| `Discount`            | 0.05    | 0        | 0.25     |
| `Profit`              | 181.18  | -14140.7 | 27220.29 |
| `Unit_Price`          | 89.35   | 0.99     | 6783.02  |
| `Shipping_Cost`       | 12.84   | 0.49     | 164.73   |
| `Product_Base_Margin` | 0.51    | 0.35     | 0.85     |

---

## 📊 Distribution Insights

### ✅ Order Priorities

* Most orders were categorized as **High (1768)** and **Low (1720)** priority.

### 🚚 Shipping Modes

* **Regular Air** is the dominant shipping mode (6270), followed by **Delivery Truck (1146)** and **Express Air (983)**.

### 💸 Profitability by Category

* **Technology** and **Office Supplies** yielded the highest total profits.

### 👥 Customer Segmentation

* **Corporate** customers were the largest group, followed by **Home Office**, **Consumer**, and **Small Business**.

---

## ❓ Business Questions & Answers

### 1. **Which Product Category Had the Highest Sales?**

* 📌 **Technology** with the highest individual sales figure (89,061.05).

### 2. **Top & Bottom 3 Regions by Sales**

* **Top:** West, Ontario, Prarie
* **Bottom:** Nunavut, Northwest Territories, Yukon

### 3. **Total Sales of Appliances in Ontario**

* 💰 **202,346.84** in sales.

### 4. **Recommendations for Bottom 10 Customers**

* 📢 Focus on:

  * Personalized marketing
  * Product recommendations
  * Gathering feedback
  * Upselling/cross-selling
  * Improved service

### 5. **Highest Shipping Cost by Mode**

* **Delivery Truck** incurred the highest total shipping cost.

### 6. **Most Valuable Customers & Purchases**

* Top customers primarily bought **Technology** products.

### 7. **Top Small Business Buyer**

* 🏆 **Dannies Kane**

### 8. **Corporate Orders (2009–2012)**

* 🥇 **Adam Hart** placed the most.

### 9. **Most Profitable Consumer**

* 💎 **Emily Phan**

### 10. **Returns by Segment**

* 872 orders returned across all customer segments.

### 11. **Shipping Cost Efficiency by Priority**

* Express Air = faster but cost less
* Regular Air = Most used
* Delivery Truck = costlier and slower
* Recommendation: align **Order Priority** with appropriate **Shipping Mode**.


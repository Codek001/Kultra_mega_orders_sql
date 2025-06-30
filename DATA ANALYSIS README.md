

---

# 📊 Exploratory Data Analysis Report for KMS Dataset

## 📁 Data Overview

| Table          | Observations | Variables                             | Missing Data                                              | Notes                       |
| -------------- | ------------ | ------------------------------------- | --------------------------------------------------------- | --------------------------- |
| `kms`          | 8,399        | 20 (19 after dropping `product_name`) | Few missing/zero entries in `Product_Base_Margin` handled | Main transactional dataset  |
| `order_status` | 572          | 2                                     | None                                                      | Used to track return status |

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

* Express Air = faster but costlier
* Delivery Truck = cheaper but slower
* Recommendation: align **Order Priority** with appropriate **Shipping Mode**.

-

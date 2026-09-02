#  Zepto Inventory Analysis using SQL

<p align="center">
  <b>SQL • MySQL • Data Cleaning • Inventory Analysis • Business Insights</b><br>
  A portfolio project analyzing Zepto inventory data to understand pricing, discounts, stock availability, revenue potential, and inventory distribution.
</p>

---

##  Project Overview

Zepto is an Indian quick-commerce company that delivers groceries and everyday essentials through a mobile application. Its inventory contains products across multiple categories with different prices, discounts, quantities, weights, and stock statuses.

This project uses **MySQL/SQL** to clean, transform, and analyze Zepto inventory data. The objective is to convert raw inventory records into meaningful business insights that can support **inventory management, pricing decisions, product planning, and data-driven decision-making**.

The project covers **3,732 raw product records across 9 columns and 14 categories**. After the relevant cleaning steps, the analysis works with **3,731 products**.

---

##  Project Objectives

- Clean and prepare the raw inventory dataset.
- Identify missing values, duplicates, and invalid records.
- Analyze **in-stock vs out-of-stock** products.
- Understand product pricing and discount patterns.
- Identify best-value and high-MRP products.
- Estimate category-wise revenue potential.
- Analyze price per gram for products above 100g.
- Segment products into **Low, Medium, and Bulk** weight categories.
- Analyze total inventory weight by category.
- Generate business insights using SQL.

---

## ️ Dataset

| Metric | Value |
|---|---:|
| Raw records | **3,732** |
| Columns | **9** |
| Categories | **14** |
| Duplicate rows found | **2** |
| Records with MRP = ₹0 | **1** |
| In-stock products | **3,279** |
| Out-of-stock products | **453** |
| Maximum discount | **51%** |

### Main Columns

| Column | Description |
|---|---|
| `Category` | Product category |
| `name` | Product name |
| `mrp` | Maximum Retail Price, initially stored in paise |
| `discountPercent` | Discount percentage |
| `availableQuantity` | Available quantity |
| `discountedSellingPrice` | Selling price after discount, initially stored in paise |
| `weightInGms` | Product weight in grams |
| `outOfStock` | Stock availability indicator |
| `quantity` | Quantity/weight-related value used in the analysis |

> **Currency conversion:** MRP and discounted selling price were converted from paise to Indian Rupees by dividing by 100.

---

#  Data Cleaning & Preparation

The first stage of the project focused on preparing the raw data for analysis.

### 1. Identify NULL Values

The PPT contains a query checking NULL values in the available product fields.

```sql
SELECT * FROM zeptoo
WHERE name IS NULL
   OR mrp IS NULL
   OR discountpercent IS NULL
   OR availablequantity IS NULL
   OR discountedsellingprice IS NULL
   OR weightingms IS NULL
   OR outofstock IS NULL
   OR ...;
```

> **Output:** The PPT output shows the column headers with no returned data rows, indicating that no matching NULL records were returned by this check. The screenshot in the PPT is cropped at the end of the query, so the final condition is not reproduced here as an invented statement.

**PPT evidence:**

![Q1 Query](assets/q01_query.png)

![Q1 Output](assets/q01_output.png)

---

### 2. Select 10 Rows Using `LIMIT`

```sql
SELECT * FROM zeptoo
LIMIT 10;
```

**Output:** 10 sample records were displayed from the dataset.

![Q2 Query](assets/q02_query.png)

![Q2 Output](assets/q02_output.png)

---

### 3. Identify Unique Categories Using `DISTINCT`

```sql
SELECT DISTINCT category
FROM zeptoo
GROUP BY category;
```

**Output:** The analysis identified **14 product categories**.

![Q3 Query](assets/q03_query.png)

![Q3 Output](assets/q03_output.png)

---

### 4. Analyze In-Stock vs Out-of-Stock Products

```sql
SELECT outofstock, COUNT(*)
FROM zeptoo
GROUP BY outofstock;
```

**Output:**

| Stock Status | Products |
|---|---:|
| In Stock | **3,279** |
| Out of Stock | **453** |

**Key insight:** 453 products were out of stock, approximately **12%** of the raw dataset.

![Q4 Query](assets/q04_query.png)

![Q4 Output](assets/q04_output.png)

---

### 5. Identify Repeated Product Names

```sql
SELECT name, COUNT(*) AS "number of SKUs"
FROM zeptoo
GROUP BY name
HAVING COUNT(*) > 1
ORDER BY COUNT(*) DESC;
```

**Output:** The query returned product names appearing multiple times, along with their number of SKUs.

![Q5 Query](assets/q05_query.png)

![Q5 Output](assets/q05_output.png)

---

### 6. Convert Paise to Indian Rupees

```sql
UPDATE zeptoo
SET mrp = mrp / 100.0,
    discountedsellingprice = discountedsellingprice / 100.0;

SELECT mrp, discountedsellingprice
FROM zeptoo;
```

**Output:** Product MRP and discounted selling prices were displayed in **Indian Rupees (₹)**.

![Q6 Query](assets/q06_query.png)

![Q6 Output](assets/q06_output.png)

---

#  Business Analysis & SQL Queries

## 7.  Top 10 Best-Value Products by Discount

**Business Question:** Find the top 10 best-value products based on discount percentage.

```sql
SELECT DISTINCT name, mrp, discountpercent
FROM zeptoo
ORDER BY discountpercent DESC
LIMIT 10;
```

**Output:** The query returned the **10 products with the highest discount percentages**.

**Insight:** The analysis identified the Top 10 best-value products based on discount percentage.

![Q7 Query](assets/q07_query.png)

![Q7 Output](assets/q07_output.png)

---

## 8.  High-MRP Products That Are Out of Stock

**Business Question:** What are the products with high MRP but out of stock?

```sql
SELECT DISTINCT name, mrp
FROM zeptoo
WHERE outofstock = 'true'
ORDER BY mrp DESC;
```

**Output / Key Result:**
- **453 products** were out of stock.
- **2 products with MRP > ₹500** were found among the out-of-stock products.

This highlights products that may require additional inventory attention.

![Q8 Query](assets/q08_query.png)

![Q8 Output](assets/q08_output.png)

---

## 9.  Estimated Revenue by Category

**Business Question:** Calculate estimated revenue for each category.

```sql
SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zeptoo
GROUP BY category
ORDER BY total_revenue;
```

### Output

| Category | Estimated Revenue (₹) |
|---|---:|
| Fruits & Vegetables | 10,846 |
| Meats, Fish & Eggs | 20,693 |
| Biscuits | 25,019 |
| Dairy, Bread & Batter | 55,051 |
| Beverages | 55,051 |
| Health & Hygiene | 64,180 |
| Home & Cleaning | 122,661 |
| Packaged Food | 224,385 |
| Ice Cream & Desserts | 224,385 |
| Chocolates & Candies | 224,385 |
| Personal Care | 270,849 |
| Paan Corner | 270,849 |
| Cooking Essentials | **337,131** |
| Munchies | **337,131** |

**Key Insight:** **Cooking Essentials** and **Munchies** were the highest revenue-generating categories in the displayed output, at approximately **₹3.37 lakh each**.

> Revenue logic used in the project: `Discounted Selling Price × Available Quantity`.

![Q9 Query](assets/q09_query.png)

![Q9 Output](assets/q09_output.png)

---

## 10.  Products with MRP > ₹500 and Discount < 10%

**Business Question:** Find products where MRP is greater than ₹500 and discount is less than 10%.

```sql
SELECT DISTINCT name, mrp, discountpercent
FROM zeptoo
WHERE mrp > 500
  AND discountPercent < 10
ORDER BY mrp DESC, discountpercent DESC;
```

**Output / Key Result:** **82 products** met the criteria.

This helps identify relatively expensive products with limited discounts.

![Q10 Query](assets/q10_query.png)

![Q10 Output](assets/q10_output.png)

---

## 11.  Top 5 Categories by Average Discount

```sql
SELECT category,
       AVG(discountPercent) AS avg_discount
FROM zeptoo
GROUP BY category
ORDER BY AVG(discountPercent) DESC
LIMIT 5;
```

### Output

| Rank | Category | Average Discount % |
|---:|---|---:|
| 1 | **Fruits & Vegetables** | **15.4624%** |
| 2 | Meats, Fish & Eggs | 11.0317% |
| 3 | Packaged Food | 8.3247% |
| 4 | Ice Cream & Desserts | 8.3247% |
| 5 | Chocolates & Candies | 8.3247% |

**Key Insight:** **Fruits & Vegetables** had the highest average discount at approximately **15.5%**.

![Q11 Query](assets/q11_query.png)

![Q11 Output](assets/q11_output.png)

---

## 12. ️ Price per Gram for Products Above 100g

```sql
SELECT DISTINCT name,
       discountedSellingPrice,
       weightingms,
       ROUND(discountedSellingPrice / weightingms, 2) AS price_per_gram
FROM zeptoo
WHERE weightingms > 100
ORDER BY price_per_gram;
```

**Output / Key Result:**
- **2,660 products** weighing more than 100g were compared.
- Products were sorted by price per gram to identify better value.

**Formula:**

```text
Price per Gram = Selling Price / Weight in Grams
```

![Q12 Query](assets/q12_query.png)

![Q12 Output](assets/q12_output.png)

---

## 13. ️ Product Weight Segmentation

**Business Question:** Group products into Low, Medium, and Bulk categories.

```sql
SELECT DISTINCT category,
       weightingms,
       CASE
           WHEN weightingms < 1000 THEN 'low'
           WHEN weightingms < 5000 THEN 'medium'
           ELSE 'bulk'
       END AS weight_category
FROM zeptoo
ORDER BY weightingms DESC;
```

### Output Logic

| Weight | Segment |
|---|---|
| `< 1000g` | Low |
| `1000g – < 5000g` | Medium |
| `>= 5000g` | Bulk |

### Dataset-Level Result

| Weight Segment | Products |
|---|---:|
| Low | **2,114** |
| Medium | **1,491** |
| Bulk | **126** |
| **Total** | **3,731** |

![Q13 Query](assets/q13_query.png)

![Q13 Output](assets/q13_output.png)

---

## 14.  Total Inventory Weight per Category

```sql
SELECT category,
       SUM(weightingms * availablequantity) AS total_weight
FROM zeptoo
GROUP BY category
ORDER BY total_weight DESC;
```

### Output

| Category | Total Weight (g) |
|---|---:|
| Cooking Essentials | **1,404,326** |
| Munchies | **1,404,326** |
| Packaged Food | 490,797 |
| Ice Cream & Desserts | 490,797 |
| Chocolates & Candies | 490,797 |
| Home & Cleaning | 373,161 |
| Personal Care | 348,187 |
| Paan Corner | 348,187 |
| Dairy, Bread & Batter | 143,735 |
| Beverages | 143,735 |
| Health & Hygiene | 142,904 |
| Fruits & Vegetables | 91,794 |
| Biscuits | 84,431 |
| Meats, Fish & Eggs | 48,016 |

**Key Insight:** Cooking Essentials had the highest displayed inventory weight at **1,404,326g (≈1,404kg)**. The PPT summarizes this as approximately **1,405kg**.

![Q14 Query](assets/q14_query.png)

![Q14 Output](assets/q14_output.png)

---

#  Key Business Insights

### Inventory
- **453 products** were out of stock, representing approximately **12%** of the raw dataset.
- **2 high-MRP products (>₹500)** were out of stock.

### Pricing & Discounts
- The **Top 10 best-value products** were identified based on discount percentage.
- **82 products** had MRP above ₹500 with discounts below 10%.
- **Fruits & Vegetables** offered the highest average discount at approximately **15.5%**.

### Revenue
- **14 product categories** were analyzed.
- **Cooking Essentials** and **Munchies** showed the highest estimated revenue in the displayed output at approximately **₹3.37 lakh each**.

### Product Value
- **2,660 products** above 100g were compared using price-per-gram analysis.

### Inventory Weight
- **3,731 cleaned products** were classified into Low, Medium, and Bulk weight segments.
- Cooking Essentials and Munchies showed the highest displayed total inventory weight.

---

#  Business Value

The analysis can support business decisions in several areas:

| Area | Potential Use |
|---|---|
| **Inventory Management** | Identify out-of-stock and high-value unavailable products |
| **Pricing Strategy** | Compare MRP, selling price, and discount levels |
| **Promotional Strategy** | Identify categories with higher average discounts |
| **Revenue Planning** | Compare estimated revenue across categories |
| **Product Value** | Use price-per-gram to compare products |
| **Inventory Planning** | Understand weight and quantity distribution |
| **Decision Making** | Convert raw inventory data into actionable insights |

---

#  SQL Concepts Demonstrated

This project demonstrates practical use of:

- `SELECT`
- `WHERE`
- `DISTINCT`
- `LIMIT`
- `GROUP BY`
- `ORDER BY`
- `HAVING`
- `CASE`
- `COUNT()`
- `SUM()`
- `AVG()`
- `ROUND()`
- Aggregate functions
- Conditional filtering
- Sorting and ranking
- Data cleaning
- Data transformation
- Calculated fields
- Revenue calculations
- Inventory segmentation

---

#  Project Workflow

```text
Raw Zepto Inventory Dataset
            ↓
      Data Inspection
            ↓
      NULL & Duplicate Checks
            ↓
      Data Cleaning
            ↓
      Paise → INR Conversion
            ↓
     Stock Availability Analysis
            ↓
 Pricing & Discount Analysis
            ↓
     Revenue Estimation
            ↓
 Price-per-Gram Analysis
            ↓
 Weight Segmentation
            ↓
Inventory Weight by Category
            ↓
      Business Insights
```

---

#  Recommended GitHub Repository Structure

```text
Zepto-Inventory-Analysis/
│
├──  README.md
├── ️ zepto_inventory_analysis.sql
├──  zepto_inventory_dataset.xlsx
│
└──  assets/
    ├── q01_query.png
    ├── q01_output.png
    ├── q02_query.png
    ├── q02_output.png
    ├── ...
    ├── q14_query.png
    └── q14_output.png
```

> The `assets` folder contains the query and output screenshots extracted from the project PPT. Keep this folder in the same repository as `README.md` so the images render correctly on GitHub.

---

#  How to Use the Project

### 1. Clone the repository

```bash
git clone <your-github-repository-link>
```

### 2. Create a MySQL database

```sql
CREATE DATABASE zepto_inventory;
USE zepto_inventory;
```

### 3. Import the dataset

Create a table with the required fields:

```sql
CREATE TABLE zepto_inventory (
    Category VARCHAR(255),
    name VARCHAR(255),
    mrp INT,
    discountPercent INT,
    availableQuantity INT,
    discountedSellingPrice INT,
    weightInGms INT,
    outOfStock BOOLEAN,
    quantity INT
);
```

### 4. Run the SQL analysis

Execute the SQL queries from:

```text
zepto_inventory_analysis.sql
```

The queries cover data cleaning, transformation, inventory analysis, pricing, discounts, revenue estimation, and product segmentation.

---

#  Conclusion

This project demonstrates how **SQL can be used to solve practical business problems**, not just perform basic database queries.

By analyzing **3,731 cleaned Zepto inventory products**, the project uncovered patterns in **stock availability, pricing, discounts, estimated revenue, product value, and inventory weight**.

The analysis highlights opportunities to improve **inventory availability, pricing strategy, promotional planning, category management, and overall data-driven decision-making**.

---

## ‍ Author

**Kanduri Manikanta**  
MBA – Finance & Business Analytics

### Skills Demonstrated

`SQL` `MySQL` `Data Cleaning` `Data Transformation` `Exploratory Data Analysis` `Inventory Analysis` `Business Analysis` `Data-Driven Decision Making`

---

### ️ Disclaimer

This is a **portfolio/learning project** based on the provided Zepto inventory dataset. The estimated revenue and other analytical figures are derived from the supplied dataset and should **not be interpreted as official Zepto business data**.

---

 **If you found this project useful, consider giving the repository a star!**

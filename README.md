# Zepto Inventory Analysis using SQL

<p align="center">
 <b>SQL • MySQL • Data Cleaning • Inventory Analysis • Business Insights</b><br>
 A portfolio project analyzing Zepto inventory data to understand pricing, discounts, stock availability, revenue potential, and inventory distribution.
</p>

---

## Project Overview

Zepto is an Indian quick-commerce company that delivers groceries and everyday essentials through a mobile application. Its inventory contains products across multiple categories with different prices, discounts, quantities, weights, and stock statuses.

This project uses **MySQL/SQL** to clean, transform, and analyze Zepto inventory data. The objective is to convert raw inventory records into meaningful business insights that can support **inventory management, pricing decisions, product planning, and data-driven decision-making**.

The project covers **3,732 raw product records across 9 columns and 14 categories**. After the relevant cleaning steps, the analysis works with **3,731 products**.

---

## Project Objectives

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

## Dataset

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

# Data Cleaning & Preparation

The first stage of the project focused on preparing the raw data for analysis.

### 1. Identify NULL Values


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

> **Output:** No matching NULL records were returned by this check.


#### SQL Query
```sql
SELECT * FROM zeptoo
WHERE name IS NULL
 OR mrp IS NULL
 OR discountpercent IS NULL
 OR availablequantity IS NULL
 OR discountedsellingprice IS NULL
 OR weightingms IS NULL
 OR outofstock IS NULL
 OR ...
```

#### Output
```text
Category | name | mrp | discountPercent | availableQuantity | discountedSellingPrice | weightInGms | outOfStock | quantity
(No rows returned)
```


---

### 2. Select 10 Rows Using `LIMIT`

```sql
SELECT * FROM zeptoo
LIMIT 10;
```

**Output:** 10 sample records were displayed from the dataset.

#### SQL Query
```sql
SELECT * FROM zeptoo
LIMIT 10;
```

#### Output
```text
Category | name | mrp | discountPercent | availableQuantity | discountedSellingPrice | weightInGms | outOfStock | quantity
Fruits & Vegetables | Onion | 25 | 16 | 3 | 21 | 1000 | FALSE | 1
Fruits & Vegetables | Tomato Hybrid | 42 | 16 | 3 | 35 | 1000 | FALSE | 1
Fruits & Vegetables | Tender Coconut | 51 | 15 | 3 | 43 | 58 | FALSE | 1
Fruits & Vegetables | CorianderLeaves | 20 | 15 | 3 | 17 | 100 | FALSE | 100
Fruits & Vegetables | Ladies Finger | 14 | 14 | 3 | 12 | 250 | FALSE | 250
Fruits & Vegetables | Potato | 35 | 17 | 3 | 29 | 1000 | FALSE | 1
Fruits & Vegetables | Lemon | 75 | 16 | 3 | 63 | 200 | FALSE | 200
Fruits & Vegetables | Watermelon | 58 | 15 | 3 | 9 | 58 | FALSE | 1
Fruits & Vegetables | Capsicum Green | 23 | 17 | 3 | 19 | 250 | FALSE | 250
Fruits & Vegetables | Chilli Green | 19 | 15 | 5 | 16 | 100 | FALSE | 100
```

---

### 3. Identify Unique Categories Using `DISTINCT`

```sql
SELECT DISTINCT category
FROM zeptoo
GROUP BY category;
```

**Output:** The analysis identified **14 product categories**.

#### SQL Query
```sql
SELECT DISTINCT category
FROM zeptoo
GROUP BY category;
```

#### Output
```text
category
-------------------------
Fruits & Vegetables
Cooking Essentials
Munchies
Dairy, Bread & Batter
Beverages
Packaged Food
Ice Cream & Desserts
Chocolates & Candies
Meats, Fish & Eggs
Personal Care
Paan Corner
Home & Cleaning
Health & Hygiene
Biscuits
```


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

#### SQL Query
```sql
SELECT outofstock, COUNT(*)
FROM zeptoo
GROUP BY outofstock;
```

#### Output
```text
outofstock | count(*)
-----------+--------
FALSE | 3274
TRUE | 453
```


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

#### SQL Query
```sql
SELECT name, COUNT(*) AS "number of SKUs"
FROM zeptoo
GROUP BY name
HAVING COUNT(*) > 1
ORDER BY COUNT(*) DESC;
```

#### Output
```text
name | number of SKUs
-------------------------------------------------+---------------
Arden Eggs White | 10
Saffola Veggie Twist Masala Oats | 10
Quaker Oats | 10
Sunfeast YiPPee! Pasta Treat - Sour Cream Onion | 10
Sunfeast YiPPee! Magic Masala Noodles | 10
Mother's Recipe Tamarind Paste | 10
Amul Delicious Fat Spread - Cholesterol Free | 10
Kellogg's Real Almond & Honey Corn Flakes | 9
Amul Fresh Cream | 8
iD Idli & Dosa Batter | 7
Everest Garam Masala | 6
Everest Chicken Masala | 6
Everest Kitchen King Masala | 6
Godrej Yummiez Chicken Nuggets | 6
Zorabian Chicken Smoked Ham | 6
Godrej Yummiez Chilli Chicken Sausages | 6
Everest Tandoori Chicken Masala | 6
Godrej Yummiez Chicken Punjabi Tikka | 6
Maggi Magic Cubes Extra Chicken | 6
Yummiez Chicken Garlic Fingers Pouch | 6
Zorabian Chicken Kheema Parathas | 6
Godrej Yummiez Chicken Breakfast Salami | 6
Prasuma Vegetable Momos | 6
```


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

#### SQL Query
```sql
UPDATE zeptoo
SET mrp = mrp / 100.0,
 discountedsellingprice = discountedsellingprice / 100.0;

SELECT mrp, discountedsellingprice
FROM zeptoo;
```

#### Output
```text
Category | name | mrp | discountPercent | availableQuantity | discountedSellingPrice | weightInGms | outOfStock | quantity
Fruits & Vegetables | Onion | 25 | 16 | 3 | 21 | 1000 | FALSE | 1
Fruits & Vegetables | Tomato Hybrid | 42 | 16 | 3 | 35 | 1000 | FALSE | 1
Fruits & Vegetables | Tender Coconut | 51 | 15 | 3 | 43 | 58 | FALSE | 1
Fruits & Vegetables | CorianderLeaves | 20 | 15 | 3 | 17 | 100 | FALSE | 100
Fruits & Vegetables | Ladies Finger | 14 | 14 | 3 | 12 | 250 | FALSE | 250
Fruits & Vegetables | Potato | 35 | 17 | 3 | 29 | 1000 | FALSE | 1
Fruits & Vegetables | Lemon | 75 | 16 | 3 | 63 | 200 | FALSE | 200
Fruits & Vegetables | Watermelon | 58 | 15 | 3 | 9 | 58 | FALSE | 1
Fruits & Vegetables | Capsicum Green | 23 | 17 | 3 | 19 | 250 | FALSE | 250
Fruits & Vegetables | Chilli Green | 19 | 15 | 3 | 16 | 100 | FALSE | 100
Fruits & Vegetables | Banana Robusta | 29 | 17 | 3 | 24 | 348 | FALSE | 6
Fruits & Vegetables | Garlic Indian | 11 | 18 | 3 | 9 | 100 | FALSE | 100
Fruits & Vegetables | Cauliflower | 26 | 15 | 3 | 22 | 58 | FALSE | 1
Fruits & Vegetables | Ginger | 14 | 14 | 3 | 12 | 200 | FALSE | 200
Fruits & Vegetables | Spinach | 19 | 15 | 3 | 16 | 250 | FALSE | 250
Fruits & Vegetables | Muskmelon | 42 | 16 | 3 | 35 | 58 | FALSE | 1
Fruits & Vegetables | Cabbage | 15 | 13 | 3 | 13 | 58 | FALSE | 1
Fruits & Vegetables | Methi | 30 | 16 | 3 | 25 | 250 | FALSE | 250
Fruits & Vegetables | Broccoli | 9 | 16 | 3 | 8 | 500 | FALSE | 500
Fruits & Vegetables | Sapota | 30 | 16 | 3 | 25 | 348 | FALSE | 6
```


---

# Business Analysis & SQL Queries

## 7. Top 10 Best-Value Products by Discount

**Business Question:** Find the top 10 best-value products based on discount percentage.

```sql
SELECT DISTINCT name, mrp, discountpercent
FROM zeptoo
ORDER BY discountpercent DESC
LIMIT 10;
```

**Output:** The query returned the **10 products with the highest discount percentages**.

**Insight:** The analysis identified the Top 10 best-value products based on discount percentage.

#### SQL Query
```sql
SELECT DISTINCT name, mrp, discountpercent
FROM zeptoo
ORDER BY discountpercent DESC
LIMIT 10;
```

#### Output
```text
name | mrp | discountpercent
------------------------------------------------------------+-----+----------------
Dukes Waffy Chocolate Wafers | 45 | 51
Dukes Waffy Orange Wafers | 45 | 51
Dukes Waffy Strawberry Wafers | 45 | 51
Ceres Foods Fish Mustard Instant Liquid Masala | 220 | 50
Ceres Foods Laal Maas Instant Liquid Masala | 220 | 50
Ceres Foods Nalli Nihari Instant Liquid Masala | 220 | 50
Chef's Basket Durum Wheat Elbow Pasta | 160 | 50
Chef's Basket Durum Wheat Fusilli Pasta | 160 | 50
Chef's Basket Durum Wheat Penne Pasta | 160 | 50
Dukes Waffy Chocolate Wafer Rolls | 150 | 50
```

---

## 8. High-MRP Products That Are Out of Stock

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

#### SQL Query
```sql
SELECT DISTINCT name, mrp
FROM zeptoo
WHERE outofstock = 'true'
ORDER BY mrp DESC;
```

#### Output
```text
name | mrp
-------------------------------------------------------+-----
Patanjali Cow's Ghee | 565
MamyPoko Pants Standard Diapers, Extra Large... | 399
Aashirvaad Atta With Multigrains | 315
Everest Kashmiri Lal Chilli Powder | 310
Hershey's Cocoa + Almond Spread | 295
Madhur Pure And Hygienic Sugar | 295
MOZZARELLA Block Cheese | 295
Godrej Real Good Chicken Boneless Cubes | 275
Zorabian Chicken Cubes | 270
Kelloggs Corn Flakes With Real Strawberry Pure | 265
Del Monte Pitted Green Olives | 250
```


---

## 9. Estimated Revenue by Category

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

**Key Insight:** **Cooking Essentials** and **Munchies** were the highest revenue-generating categories in the output, at approximately **₹3.37 lakh each**.

> Revenue logic used in the project: `Discounted Selling Price × Available Quantity`.

#### SQL Query
```sql
SELECT category,
 SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zeptoo
GROUP BY category
ORDER BY total_revenue;
```

#### Output
```text
category | total_revenue (₹)
----------------------------+------------------
Fruits & Vegetables | 10846
Meats, Fish & Eggs | 20693
Biscuits | 25019
Dairy, Bread & Batter | 55051
Beverages | 55051
Health & Hygiene | 64180
Home & Cleaning | 122661
Packaged Food | 224385
Ice Cream & Desserts | 224385
Chocolates & Candies | 224385
Personal Care | 270849
Paan Corner | 270849
Cooking Essentials | 337131
Munchies | 337131
```

---

## 10. Products with MRP > ₹500 and Discount < 10%

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

#### SQL Query
```sql
SELECT DISTINCT name, mrp, discountpercent
FROM zeptoo
WHERE mrp > 500
 AND discountPercent < 10
ORDER BY mrp DESC, discountpercent DESC;
```

#### Output
```text
name | mrp | discountpercent
------------------------------------------------------------+-----+----------------
Dhara Kachi Ghani Mustard Oil Jar | 1250 | 8
Saffola Gold (Jar) | 1240 | 0
Dhara Filtered Groundnut Oil (Jar) | 1050 | 1
Fortune Rice Bran Health Oil | 1050 | 1
Dhara Filtered Groundnut Oil (Jar) | 1050 | 0
Fortune Soyabean Oil | 1005 | 0
Fortune Sunlite Refined Sunflower (Jar) | 925 | 0
Surf Excel Matic Powder Front Load | 810 | 7
Surf Excel Matic Top Load | 720 | 9
Pedigree Puppy Dry Dog Food Food Chicken & Milk | 690 | 6
Pedigree Dog Food Adult Meat & Rice | 660 | 7
Lizol Double Concentrate Disinfectant Floor Cleaner | 650 | 8
Nestle Nestle Nan Pro Follow-Up Formula | 650 | 0
Dove Daily Shine Shampoo | 630 | 0
L'Oreal Paris Excellence Creme Hair Color | 620 | 0
L'Oreal Paris Excellence Creme Hair Color, 3Dar... | 620 | 0
L'Oreal Paris Excellence Creme Hair Color, 4Nat... | 620 | 0
L'Oreal Paris Excellence Creme Hair Color, 4.25... | 620 | 0
Nestle Nan Pro 4 Follow Up Formula Powder For... | 620 | 0
Pediasure Premium Chocolate Powdered Health... | 610 | 0
```

> **Result:** 82 products met the criteria in the project analysis.

---

## 11. Top 5 Categories by Average Discount

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

#### SQL Query
```sql
SELECT category,
 AVG(discountPercent) AS avg_discount
FROM zeptoo
GROUP BY category
ORDER BY AVG(discountPercent) DESC
LIMIT 5;
```

#### Output
```text
category | avg_discount
----------------------------+-------------
Fruits & Vegetables | 15.4624
Meats, Fish & Eggs | 11.0317
Packaged Food | 8.3247
Ice Cream & Desserts | 8.3247
Chocolates & Candies | 8.3247
```

---

## 12. Price per Gram for Products Above 100g

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

#### SQL Query
```sql
SELECT DISTINCT name,
 discountedSellingPrice,
 weightingms,
 ROUND(discountedSellingPrice / weightingms, 2) AS price_per_gram
FROM zeptoo
WHERE weightingms > 100
ORDER BY price_per_gram;
```

#### Output
```text
name | discountedSellingPrice | weightingms | price_per_gram
------------------------------------------+------------------------+-------------+---------------
Aashirvaad Iodised Salt | 19 | 1000 | 0.02
Onion | 21 | 1000 | 0.02
Onion | 57 | 3000 | 0.02
Shubh kart - Nirmal sugandhi mogra ... | 28 | 1160 | 0.02
Tata Salt | 24 | 1000 | 0.02
Vicks Cough Drops Menthol | 20 | 1160 | 0.02
Baby Potato | 16 | 500 | 0.03
Beetroot | 13 | 500 | 0.03
Carrot | 15 | 500 | 0.03
Potato | 29 | 1000 | 0.03
Potato | 84 | 3000 | 0.03
Raw Banana | 17 | 500 | 0.03
Shubh kart - Tejas Twisted Cotton Wicks | 28 | 1000 | 0.03
Aashirvaad Atta | 10000 | 10000 | 0.04
Beetroot | 5 | 250 | 0.04
Capsicum | 21 | 500 | 0.04
Carrot | 11 | 250 | 0.04
Pillsbury Chakki Fresh Atta | 375 | 10000 | 0.04
```

> **Result:** 2,660 products weighing more than 100g were compared in the project.

---

## 13. Product Weight Segmentation

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

#### SQL Query
```sql
SELECT DISTINCT category, weightingms,
 CASE
 WHEN weightingms < 1000 THEN 'low'
 WHEN weightingms < 5000 THEN 'medium'
 ELSE 'bulk'
 END AS weight_category
FROM zeptoo
ORDER BY weightingms DESC;
```

#### Output
```text
category | weightingms | weight_category
----------------------------+-------------+----------------
Cooking Essentials | 10000 | bulk
Munchies | 10000 | bulk
Cooking Essentials | 5000 | bulk
Munchies | 5000 | bulk
Home & Cleaning | 4000 | medium
Chocolates & Candies | 3000 | medium
Fruits & Vegetables | 3000 | medium
Home & Cleaning | 3000 | medium
Ice Cream & Desserts | 3000 | medium
Packaged Food | 3000 | medium
Cooking Essentials | 2000 | medium
Home & Cleaning | 2000 | medium
Munchies | 2000 | medium
Home & Cleaning | 1900 | medium
Chocolates & Candies | 1500 | medium
Home & Cleaning | 1500 | medium
Ice Cream & Desserts | 1500 | medium
Munchies | 1500 | medium
Paan Corner | 1500 | medium
Packaged Food | 1500 | medium
Personal Care | 1500 | medium
Chocolates & Candies | 1200 | medium
```

#### Dataset-level summary
```text
Weight Segment | Products
---------------+---------
Low | 2114
Medium | 1491
Bulk | 126
Total | 3731
```

---

## 14. Total Inventory Weight per Category

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

**Key Insight:** Cooking Essentials had the highest displayed inventory weight at **1,404,326g (≈1,404kg)**. This is approximately **1,405kg**.

#### SQL Query
```sql
SELECT category,
 SUM(weightingms * availablequantity) AS total_weight
FROM zeptoo
GROUP BY category
ORDER BY total_weight DESC;
```

#### Output
```text
category | total_weight (g)
----------------------------+----------------
Cooking Essentials | 1404326
Munchies | 1404326
Packaged Food | 490797
Ice Cream & Desserts | 490797
Chocolates & Candies | 490797
Home & Cleaning | 373161
Personal Care | 348187
Paan Corner | 348187
Dairy, Bread & Batter | 143735
Beverages | 143735
Health & Hygiene | 142904
Fruits & Vegetables | 91794
Biscuits | 84431
Meats, Fish & Eggs | 48016
```

---

# Key Business Insights

### Inventory
- **453 products** were out of stock, representing approximately **12%** of the raw dataset.
- **2 high-MRP products (>₹500)** were out of stock.

### Pricing & Discounts
- The **Top 10 best-value products** were identified based on discount percentage.
- **82 products** had MRP above ₹500 with discounts below 10%.
- **Fruits & Vegetables** offered the highest average discount at approximately **15.5%**.

### Revenue
- **14 product categories** were analyzed.
- **Cooking Essentials** and **Munchies** showed the highest estimated revenue in the output at approximately **₹3.37 lakh each**.

### Product Value
- **2,660 products** above 100g were compared using price-per-gram analysis.

### Inventory Weight
- **3,731 cleaned products** were classified into Low, Medium, and Bulk weight segments.
- Cooking Essentials and Munchies showed the highest displayed total inventory weight.

---

# Business Value

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

# SQL Concepts Demonstrated

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

# Project Workflow

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

# Recommended GitHub Repository Structure

```text
Zepto-Inventory-Analysis/
│
├── README.md
├── zepto_inventory_analysis.sql
└── zepto_inventory_dataset.xlsx
``` 


---

# How to Use the Project

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

# Conclusion

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

### Disclaimer

This is a **portfolio/learning project** based on the provided Zepto inventory dataset. The estimated revenue and other analytical figures are derived from the supplied dataset and should **not be interpreted as official Zepto business data**.

---

 **If you found this project useful, consider giving the repository a star!**

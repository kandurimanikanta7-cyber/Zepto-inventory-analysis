# 🛒 Zepto Inventory Analysis using SQL

> **A SQL-based inventory analysis project focused on data cleaning,
> product analysis, pricing, discounts, revenue estimation, and business
> insights.**

------------------------------------------------------------------------

## 📌 Project Overview

This project analyzes a **Zepto inventory dataset** using **MySQL/SQL**
to transform raw product data into meaningful business insights.

The main objective is to understand product availability, pricing,
discounts, revenue potential, product value, and inventory distribution.
SQL queries were used throughout the project for **data cleaning,
transformation, analysis, and insight generation**.

The analysis can help a business make better decisions related to
**inventory management, pricing, discounts, product availability, and
category performance**.

------------------------------------------------------------------------

## 🎯 Project Objectives

-   Clean and prepare raw inventory data for analysis.
-   Understand product availability and stock status.
-   Analyze product pricing and discounts.
-   Identify high-value and best-discounted products.
-   Estimate revenue at category level.
-   Analyze inventory by product weight and quantity.
-   Generate actionable insights using SQL.

------------------------------------------------------------------------

## 🗂️ Dataset

The dataset contains **3,732 product records** across **9 columns** and
covers **14 product categories**.

### Main Columns

  -----------------------------------------------------------------------
  Column                              Description
  ----------------------------------- -----------------------------------
  `Category`                          Product category

  `name`                              Product name

  `mrp`                               Maximum Retail Price, stored in
                                      paise

  `discountPercent`                   Discount percentage offered

  `availableQuantity`                 Available quantity

  `discountedSellingPrice`            Selling price after discount,
                                      stored in paise

  `weightInGms`                       Product weight in grams

  `outOfStock`                        Indicates whether the product is
                                      out of stock

  `quantity`                          Product quantity/weight-related
                                      value used for analysis
  -----------------------------------------------------------------------

> **Note:** MRP and selling price values were converted from paise to
> Indian Rupees by dividing by 100.

------------------------------------------------------------------------

## 🧹 Data Cleaning & Preparation

The raw dataset was cleaned using SQL queries before performing business
analysis.

### Cleaning steps performed:

1.  **Imported the dataset into MySQL**
    -   Loaded the inventory dataset into a SQL database.
    -   Structured the data for analysis.
2.  **Checked for missing values**
    -   Identified NULL values using SQL.
    -   Ensured the dataset was suitable for further analysis.
3.  **Removed duplicate records**
    -   Identified duplicate product records.
    -   Removed duplicate entries to improve data accuracy.
4.  **Converted paise to INR**
    -   MRP and selling prices were originally stored in paise.
    -   Converted them into Indian Rupees using:

    ``` sql
    price_in_rupees = price_in_paise / 100
    ```
5.  **Handled products with MRP = ₹0**
    -   Identified invalid products with zero MRP.
    -   Removed these records from the analysis.
6.  **Analyzed stock availability**
    -   Compared products that were **in stock** and **out of stock**.

------------------------------------------------------------------------

## 📊 Business Insights & Analysis

The following business questions were answered using SQL queries.

### Q1. Top 10 Best-Value Products

Identified the **Top 10 products with the highest discount percentages**
to understand which products provide the biggest price reductions to
customers.

### Q2. High-MRP Products That Are Out of Stock

Identified products with a **high MRP but currently unavailable**,
helping highlight potentially important products that may require
inventory attention.

### Q3. Estimated Revenue by Category

Calculated estimated revenue for each category using the discounted
selling price and quantity.

**Revenue logic:**

``` text
Estimated Revenue = Discounted Selling Price × Quantity
```

### Q4. High-MRP Products with Low Discounts

Found products where:

``` text
MRP > ₹500
AND
Discount < 10%
```

This helps identify relatively expensive products that offer limited
discounts.

### Q5. Top 5 Categories by Average Discount

Calculated the average discount percentage for each category and
identified the **Top 5 categories offering the highest average
discounts**.

### Q6. Price per Gram Analysis

Calculated **price per gram** for products weighing more than 100g to
identify products offering better value for money.

**Formula:**

``` text
Price per Gram = Selling Price / Weight in Grams
```

### Q7. Product Quantity Segmentation

Grouped products into:

-   🟢 **Low**
-   🟡 **Medium**
-   🔵 **Bulk**

based on product quantity/weight to understand different inventory
segments.

### Q8. Total Inventory Weight by Category

Calculated the **total inventory weight for each category** to
understand how inventory is distributed across product categories.

------------------------------------------------------------------------

## 🔍 Dataset Snapshot

Based on the raw dataset:

  Metric                    Value
  ----------------------- -------
  Total Records             3,732
  Total Columns                 9
  Product Categories           14
  Duplicate Rows Found          2
  MRP = ₹0 Records              1
  In-Stock Products         3,279
  Out-of-Stock Products       453
  Maximum Discount            51%

> These figures represent the supplied dataset before/after the relevant
> cleaning steps and are intended for portfolio analysis.

------------------------------------------------------------------------

## 🧠 Key SQL Concepts Used

This project helped apply practical SQL concepts including:

-   `SELECT`
-   `WHERE`
-   `GROUP BY`
-   `ORDER BY`
-   `HAVING`
-   `CASE`
-   `COUNT()`
-   `SUM()`
-   `AVG()`
-   `MAX()`
-   `MIN()`
-   Aggregate functions
-   Filtering and sorting
-   Data cleaning
-   Data transformation
-   Conditional categorization
-   Revenue calculations
-   Inventory analysis

------------------------------------------------------------------------

## 💡 Key Learnings

Through this project, I improved my practical **SQL and MySQL skills**
by working with a real-world inventory dataset. I learned how to clean
and transform raw data, handle missing and duplicate records, perform
calculations, and analyze inventory, pricing, discounts, and revenue.
Most importantly, I learned how to convert raw data into meaningful
business insights that can support **better, data-driven decisions**.

------------------------------------------------------------------------

## 🛠️ Tools & Technologies

  Tool         Purpose
  ------------ ---------------------------------------------
  **MySQL**    Database and SQL analysis
  **SQL**      Data cleaning, transformation, and analysis
  **Excel**    Initial dataset format
  **GitHub**   Project documentation and version control

------------------------------------------------------------------------

## 📁 Suggested Repository Structure

``` text
Zepto-Inventory-SQL-Analysis/
│
├── 📄 README.md
├── 📊 zepto_inventory_dataset.xlsx
├── 🗄️ zepto_inventory_analysis.sql
└── 📸 project_screenshots/
```

------------------------------------------------------------------------

## 🚀 How to Use This Project

### 1. Download or clone the repository

``` bash
git clone <your-github-repository-link>
```

### 2. Create a database in MySQL

``` sql
CREATE DATABASE zepto_inventory;
USE zepto_inventory;
```

### 3. Import the dataset

Load the dataset into a table such as:

``` sql
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

Execute the queries in:

``` text
zepto_inventory_analysis.sql
```

The queries perform data cleaning, transformation, and business
analysis.

------------------------------------------------------------------------

## 📈 Business Value

The analysis provides insights that can support:

-   **Inventory management** -- identify products that are unavailable.
-   **Pricing decisions** -- compare MRP, selling price, and discounts.
-   **Promotional strategy** -- identify categories with higher average
    discounts.
-   **Revenue analysis** -- estimate category-wise revenue potential.
-   **Product value analysis** -- compare products based on price per
    gram.
-   **Inventory planning** -- understand inventory weight and quantity
    distribution.

------------------------------------------------------------------------

## 🏁 Conclusion

This project demonstrates how **SQL can be used beyond basic querying**
to solve practical business problems.

By cleaning raw inventory data and analyzing **stock availability,
pricing, discounts, revenue, product value, and inventory
distribution**, the project converts raw data into useful insights that
can support **smarter inventory and pricing decisions**.

------------------------------------------------------------------------

## 👨‍💻 Skills Demonstrated

**SQL • MySQL • Data Cleaning • Data Transformation • Exploratory Data
Analysis • Inventory Analysis • Business Analysis • Data-Driven Decision
Making**

------------------------------------------------------------------------

### ⚠️ Disclaimer

This is a portfolio/learning project based on the provided Zepto
inventory dataset. The analysis and estimated revenue figures are
derived from the dataset and should not be interpreted as official Zepto
business data.

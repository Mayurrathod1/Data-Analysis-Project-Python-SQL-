
# Retail Orders Data Analysis

This project involves analyzing a retail orders dataset, performing data cleaning, transformation, and then analyzing sales performance using SQL queries and Python.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Dataset Overview](#dataset-overview)
- [Data Preprocessing](#data-preprocessing)
- [SQL Analysis](#sql-analysis)
- [Results and Analysis](#results-and-analysis)
- [Conclusion](#conclusion)

## Introduction

This project focuses on analyzing the `Retail Orders` dataset, which contains transactional data for a retail company. The goal of the analysis is to perform data wrangling tasks, including data cleaning, transformation, and subsequent analysis to gain insights into sales performance, shipping modes, product categories, and more.

## Installation

Follow these steps to set up the project on your local environment:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/retail-orders-analysis.git
   cd retail-orders-analysis
   ```

2. **Install necessary dependencies:**
   You need `pandas`, `kaggle`, and `sqlite3` to run this project. You can install them using pip:
   ```bash
   pip install kaggle pandas sqlite3
   ```

3. **Authenticate Kaggle API:**
   - Obtain your Kaggle API key from your [Kaggle Account](https://www.kaggle.com/docs/api).
   - Save the `kaggle.json` file and place it in the `~/.kaggle/` directory on your machine.

4. **Download the dataset:**
   The dataset can be downloaded using the Kaggle API:
   ```bash
   kaggle datasets download ankitbansal06/retail-orders -f orders.csv
   ```

## Dataset Overview

The dataset contains information about various retail orders, with columns including order details, product information, pricing, and sales data. It includes the following columns:

- **Order ID**: Unique identifier for each order
- **Order Date**: Date when the order was placed
- **Ship Mode**: Mode of shipment (e.g., Standard, Same Day)
- **Segment**: Customer segment (e.g., Consumer, Corporate)
- **Country**: Country where the order was placed
- **City**: City where the order was placed
- **State**: State where the order was placed
- **Region**: Geographic region of the order
- **Category**: Product category
- **Sub Category**: Sub-category of the product
- **Product ID**: Unique identifier for the product
- **Cost Price**: Cost of the product
- **List Price**: List price of the product
- **Quantity**: Number of items purchased
- **Discount Percent**: Discount offered on the product

The dataset contains over 9,900 rows of transaction data.

## Data Preprocessing

Data preprocessing is a crucial step in preparing the dataset for analysis. The following transformations were applied:

### 1. **Handling Missing Values:**
   - Null values in `Ship Mode` were replaced with meaningful data, such as filtering out values like "Not Available" and "unknown".

### 2. **Renaming Columns:**
   - Column names were converted to lowercase and spaces were replaced with underscores for consistency and ease of access.

### 3. **Feature Engineering:**
   - **Discount Calculation**: A new column `discount` was created, which is calculated as `list_price * discount_percent * 0.01`.
   - **Sale Price**: A new column `sale_price` was derived as `list_price - discount`.
   - **Profit**: A new column `profit` was added, calculated as `sale_price - cost_price`.

### 4. **Datetime Conversion:**
   - The `order_date` column was converted from an object data type to a datetime format.

### 5. **Removing Unnecessary Columns:**
   - The `list_price`, `cost_price`, and `discount_percent` columns were dropped as they were no longer needed for further analysis.

## SQL Analysis

The analysis was performed using SQL queries executed on a SQLite database, which was populated with the preprocessed dataset.

### 1. **Top Products by Sales:**
   The query retrieves the top 10 products by total sales:
   ```sql
   SELECT product_id, SUM(sale_price) AS sales
   FROM df_orders
   GROUP BY product_id
   ORDER BY sales DESC
   LIMIT 10;
   ```

### 2. **Top Products by Region:**
   The query retrieves the top 5 products per region based on sales:
   ```sql
   WITH cte AS (
       SELECT region, product_id, SUM(sale_price) AS sales
       FROM df_orders
       GROUP BY region, product_id
   )
   SELECT *
   FROM (
       SELECT *,
              ROW_NUMBER() OVER (PARTITION BY region ORDER BY sales DESC) AS rn
       FROM cte
   ) A
   WHERE rn <= 5;
   ```

### 3. **Monthly Sales by Year:**
   The SQL query analyzes monthly sales trends over the years:
   ```sql
   WITH cte AS (
       SELECT STRFTIME('%Y', order_date) AS order_year,
              STRFTIME('%m', order_date) AS order_month,
              SUM(sale_price) AS sales
       FROM df_orders
       GROUP BY STRFTIME('%Y', order_date), STRFTIME('%m', order_date)
   )
   SELECT *
   FROM cte;
   ```

## Results and Analysis

After running the above SQL queries, the results were extracted, showing key trends in sales performance. Key findings include:

- The **top products by total sales** were identified, with some technology products leading the list.
- **Regional preferences** were revealed by showing the top 5 products per region.
- **Monthly sales trends** showed fluctuations in demand throughout the year, providing insights for inventory and marketing strategies.

## Conclusion

This project demonstrates how data cleaning, transformation, and analysis using Python and SQL can help derive valuable insights from retail transaction data. The analysis can be used to drive business decisions such as optimizing inventory, targeting specific customer segments, and improving sales strategies.

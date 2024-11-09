# Retail Orders Analysis & Data Transformation

## 📑 Project Overview
This project focuses on analyzing a retail orders dataset obtained from Kaggle. The analysis involves data cleaning, preprocessing, and transforming the data into a SQLite database. Additionally, the project derives insights like the top-selling products and sales performance across different regions.

## 📂 Table of Contents
- [Project Overview](#project-overview)
- [Dataset Description](#dataset-description)
- [Installation & Setup](#installation--setup)
- [Data Preprocessing](#data-preprocessing)
- [Data Analysis](#data-analysis)
- [Database Operations](#database-operations)
- [Insights & Results](#insights--results)

---

## 📊 Dataset Description
- **Source**: [Kaggle - Retail Orders](https://www.kaggle.com/datasets/ankitbansal06/retail-orders)
- **File**: `orders.csv`
- The dataset contains information on customer orders, including fields like `order_date`, `ship_mode`, `category`, `cost_price`, `list_price`, `discount_percent`, etc.

### Sample Rows
| Order ID | Order Date | Ship Mode    | Segment  | Region | Category     | List Price | Quantity | Discount Percent |
|----------|------------|--------------|----------|--------|--------------|------------|----------|------------------|
| 1        | 2023-03-01 | Second Class | Consumer | South  | Furniture    | 260        | 2        | 2                |
| 2        | 2023-08-15 | Second Class | Consumer | South  | Furniture    | 730        | 3        | 3                |

---

## ⚙️ Installation & Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/retail-orders-analysis.git
   cd retail-orders-analysis
   ```

2. **Install Required Libraries**
   ```bash
   pip install pandas numpy sqlite3 kaggle
   ```

3. **Download Dataset**
   Ensure you have your Kaggle API token set up:
   ```python
   !kaggle datasets download ankitbansal06/retail-orders -f orders.csv
   ```

4. **Unzip Dataset**
   ```python
   import zipfile
   zip_ref = zipfile.ZipFile('orders.csv.zip', 'r')
   zip_ref.extractall()
   zip_ref.close()
   ```

---

## 🧹 Data Preprocessing

1. **Loading & Cleaning Data**
   - Load the data using `pandas`.
   - Handle missing values by replacing specific entries (e.g., 'Not Available' and 'unknown') with NaN.
   - Normalize column names by converting to lowercase and replacing spaces with underscores.
   
   ```python
   df = pd.read_csv('orders.csv', na_values=['Not Available', 'unknown'])
   df.columns = df.columns.str.lower().str.replace(' ', '_')
   ```

2. **Feature Engineering**
   - Created new columns:
     - `discount` = `list_price` * `discount_percent` / 100
     - `sale_price` = `list_price` - `discount`
     - `profit` = `sale_price` - `cost_price`

3. **Data Type Conversion**
   - Converted `order_date` to a `datetime` object for easier analysis:
     ```python
     df['order_date'] = pd.to_datetime(df['order_date'], format="%Y-%m-%d")
     ```

4. **Dropping Unnecessary Columns**
   - Removed columns like `list_price`, `cost_price`, and `discount_percent` to reduce data size:
     ```python
     df.drop(columns=['list_price', 'cost_price', 'discount_percent'], inplace=True, errors='ignore')
     ```

---

## 🗃️ Database Operations

1. **Load Data into SQLite**
   - Exported the cleaned DataFrame to a SQLite database:
     ```python
     import sqlite3
     conn = sqlite3.connect('df_orders')
     df.to_sql('df_orders', con=conn, index=False, if_exists='replace')
     ```

2. **Running SQL Queries**
   - Retrieved top-selling products:
     ```sql
     SELECT product_id, SUM(sale_price) AS sales
     FROM df_orders
     GROUP BY product_id
     ORDER BY sales DESC
     LIMIT 10;
     ```

   - Analyzed regional sales performance using CTEs:
     ```sql
     WITH cte AS (
         SELECT region, product_id, SUM(sale_price) AS sales
         FROM df_orders
         GROUP BY region, product_id
     )
     SELECT * FROM (
         SELECT *, ROW_NUMBER() OVER (PARTITION BY region ORDER BY sales DESC) AS rn
         FROM cte
     ) A WHERE rn <= 5;
     ```

---

## 📈 Insights & Results
- **Top Products by Sales**:
  - The top-selling product was **'TEC-CO-10004722'** with total sales of **$119,028**.
- **Regional Analysis**:
  - The Central region’s top product was `'TEC-CO-10004722'` with sales of **$33,950**.
  - In the South region, `'TEC-MA-10002412'` dominated with **$43,468.8** in sales.

Dataset Download
The dataset is available on Kaggle and can be downloaded using the Kaggle API.

Authenticate with the Kaggle API:

python
Copy code
!kaggle.api.authenticate()
Download the dataset:

bash
Copy code
!kaggle datasets download ankitbansal06/retail-orders -f orders.csv
Extract the downloaded zip file:

python
Copy code
import zipfile
zip_ref = zipfile.ZipFile('/content/orders.csv.zip')
zip_ref.extractall()  # Extract to the current directory
zip_ref.close()
Data Preprocessing
Loading Data
Load the dataset into a Pandas DataFrame:

python
Copy code
import pandas as pd
df = pd.read_csv('orders.csv', na_values=['Not Available', 'unknown'])
Handling Missing Values
Missing values in Ship Mode were handled by replacing specific values (Not Available, unknown) with NaN.

Renaming Columns
Column names were standardized to lowercase and spaces replaced with underscores:

python
Copy code
df.columns = df.columns.str.lower()
df.columns = df.columns.str.replace(' ', '_')
Data Transformation
The order_date was converted to a datetime format:

python
Copy code
df['order_date'] = pd.to_datetime(df['order_date'], format="%Y-%m-%d")
Feature Engineering
New Features
Discount: The discount was calculated using list_price and discount_percent.
Sale Price: The sale price was calculated by subtracting the discount from the list_price.
Profit: Profit was calculated by subtracting the cost_price from the sale_price.
python
Copy code
df['discount'] = df['list_price'] * df['discount_percent'] * 0.01
df['sale_price'] = df['list_price'] - df['discount']
df['profit'] = df['sale_price'] - df['cost_price']
Dropping Unnecessary Columns
After feature engineering, the columns list_price, cost_price, and discount_percent were removed:

python
Copy code
df.drop(columns=['list_price', 'cost_price', 'discount_percent'], inplace=True, errors='ignore')
Data Storage
Saving Data to SQLite Database
The cleaned data was stored in an SQLite database for easy querying:

python
Copy code
import sqlite3
conn = sqlite3.connect('df_orders')
df.to_sql('df_orders', con=conn, index=False, if_exists='replace')
Querying the Data
A sample SQL query to retrieve the first 10 rows from the df_orders table:

python
Copy code
cursor = conn.cursor()
cursor.execute('''SELECT * FROM df_orders LIMIT 10;''')
rows = cursor.fetchall()
for row in rows:
    print(row)
conn.close()
Code Explanation
Data Loading and Preprocessing: The data was loaded and cleaned by handling missing values and renaming columns to make the data easier to work with.
Feature Engineering: Additional features like discount, sale_price, and profit were created based on existing columns.
SQLite Storage: The cleaned dataset was stored in an SQLite database, enabling efficient querying and analysis.
Conclusion
This project demonstrates basic data analysis tasks such as loading, cleaning, and transforming a retail dataset. It also showcases how to store the processed data in an SQLite database for later use or further analysis. Future work could include building a predictive model or performing detailed analysis on the sales performance across various regions and product categories.

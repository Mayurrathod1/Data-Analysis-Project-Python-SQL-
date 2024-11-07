Retail Orders Data Processing

This project processes and analyzes a retail orders dataset downloaded from Kaggle. The script performs various data cleaning, transformation, and analysis tasks, including handling missing values, renaming columns, and calculating derived metrics.
Dataset

The dataset used in this project is available on Kaggle: Retail Orders Dataset. Make sure to download the orders.csv file or use the Kaggle API for an automated download.
Installation

    Install the necessary libraries:

!pip install kaggle pandas

Authenticate Kaggle API (Ensure your Kaggle API token is set up properly):

!kaggle.api.authenticate()

Download the dataset:

    !kaggle datasets download ankitbansal06/retail-orders -f orders.csv

Project Workflow

    Data Loading: The script loads the dataset and inspects the first few rows.
    Missing Values: Missing values are handled by replacing specific keywords ('Not Available', 'unknown') with NaN.
    Column Renaming: Column names are standardized to lowercase and spaces are replaced with underscores.
    Derived Columns:
        discount: Calculated based on list price and discount percentage.
        sale_price: List price minus discount.
        profit: Sale price minus cost price.
    Data Type Conversion: The order_date column is converted to datetime format for easier analysis.
    Data Export to SQL: The cleaned DataFrame is exported to an SQLite database for further analysis.

Example Usage

# Load data and preview
df = pd.read_csv('orders.csv')

# Process data, handle missing values, rename columns, calculate derived columns
# ... (other data processing code here)

# Export cleaned data to SQLite
import sqlite3
conn = sqlite3.connect('df_orders')
df.to_sql('df_orders', con=conn, index=False, if_exists='replace')

Output

    Data Analysis: Calculations for discount, sale price, and profit allow for basic financial analysis of each order.
    SQL Storage: The processed data is stored in an SQLite database, enabling easy retrieval and further SQL-based analysis.

Dependencies

    pandas: Data manipulation and analysis
    sqlite3: SQL database for structured data storage
    kaggle: Kaggle API for dataset download

License

This project uses data under the CC0-1.0 License.

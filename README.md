
# Retail Orders Data Analysis Project

This project is a data analysis and SQL exploration of retail orders using a dataset from Kaggle. The project includes loading, cleaning, and manipulating data to derive insights about retail orders.

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Setup and Installation](#setup-and-installation)
- [Data Processing Steps](#data-processing-steps)
  - [Step 1: Downloading the Dataset](#step-1-downloading-the-dataset)
  - [Step 2: Data Preprocessing](#step-2-data-preprocessing)
  - [Step 3: Database Integration](#step-3-database-integration)
  - [Step 4: SQL Queries for Insights](#step-4-sql-queries-for-insights)
- [Usage](#usage)
- [Dependencies](#dependencies)
- [Results](#results)
- [License](#license)

## Project Overview
The goal of this project is to perform analysis on a retail orders dataset, focusing on data cleaning, transformation, and querying using SQL. The project demonstrates steps to:
- Clean and preprocess raw data
- Extract, transform, and load (ETL) data into an SQLite database
- Perform data queries for business insights

## Dataset
The dataset used in this project is hosted on Kaggle and contains information about retail orders, including:
- Order dates
- Shipping modes
- Product categories and costs
- Customer segments

**Dataset URL:** [Retail Orders on Kaggle](https://www.kaggle.com/datasets/ankitbansal06/retail-orders)

## Setup and Installation
Follow these steps to set up the project environment and download the dataset.

### Prerequisites
1. Install Python (>=3.8) and necessary libraries, especially `kaggle` for data downloading and `pandas` for data processing.
2. Set up a Kaggle API key to authenticate and download the dataset.



# Retail Orders Analysis

A Python-based data analysis project that processes and analyzes retail order data using pandas and SQLite.

## Project Overview
This project analyzes retail order data to extract insights about sales performance, regional trends, and product performance. The analysis includes:

Data cleaning and preprocessing
Derived metrics calculation (discount, sale price, profit)
Database integration with SQLite
Time-based sales analysis
Regional performance analysis

Data Description
The project uses the 'Retail Orders' dataset from Kaggle, containing:

Order details (ID, date, shipping)
Customer segmentation
Geographic information
Product categories
Pricing and profit metrics

Setup and Dependencies
pythonCopy# Required libraries
import pandas as pd
import sqlite3
import zipfile
from kaggle.api import KaggleApi
Data Processing Steps

Data Loading and Extraction

Download dataset from Kaggle
Extract from zip file
Initial data exploration


Data Cleaning

Handle missing values in 'Ship Mode'
Standardize column names (lowercase, underscore format)
Convert date strings to datetime objects


Feature Engineering

Calculate discount amounts
Determine sale prices
Calculate profit margins


Database Integration

Create SQLite database
Load processed data into database
Create SQL queries for analysis



Key Analysis

Top Product Analysis
sqlCopySELECT product_id, SUM(sale_price) AS sales
FROM df_orders
GROUP BY product_id
ORDER BY sales DESC
LIMIT 10;

Regional Performance

Top 5 products by sales in each region
Regional sales distribution
Geographic performance analysis


Time Series Analysis

Monthly sales comparison (2022 vs 2023)
Category performance by month
Growth trends analysis



Key Findings

Product Performance

Top performing product: TEC-CO-10004722 (Sales: $119,028)
Strong performance in technology category


Regional Insights

East region shows highest sales for product TEC-CO-10004722
Distinct product preferences across regions


Growth Analysis

Machines category showed highest growth: $70,910 increase
February 2023 was the strongest month with $256,248 in sales



Database Schema
Table: df_orders

order_id (INT)
order_date (DATETIME)
ship_mode (TEXT)
segment (TEXT)
country (TEXT)
city (TEXT)
state (TEXT)
postal_code (INT)
region (TEXT)
category (TEXT)
sub_category (TEXT)
product_id (TEXT)
quantity (INT)
discount (FLOAT)
sale_price (FLOAT)
profit (FLOAT)

# E-Commerce Data Analysis

## Dataset
Online Retail II from UCI: https://archive.ics.uci.edu/dataset/502/online+retail+ii

## Imports and DataFrames
```import pandas as pd
file_path = 'online_retail_II.xlsx'
df_2009 = pd.read_excel(file_path, sheet_name='Year 2009-2010')
df_2010 = pd.read_excel(file_path, sheet_name='Year 2010-2011')
```
## Data Sanity Checks
- df_2009.info(): used to get a structural look at the dataset, look for missing or wrong data types.
- df_2009.describe(): used to get a feel of the data. Are the numbers reasonable? Are there outliers and strange values?
- df_2009.isna().sum(): used to get the total number of null items in each column of the dataset.

## Data Cleaning
- Remove rows with missing Customer ID or Description to prevent issues in customer-level analysis.
- Remove rows with non-positive Quantity or Price to get rid of returns and invalid transactions.
- Remove canceled orders (Invoice starting with 'C').
- Create Total Sales column that captures the total sale done by a customer.
- Convert InvoiceDate to datetime to enable time-based analysis (recency).
- Remove duplicate rows to prevent double counting.
- Convert Customer ID to integer to make IDs consistent and clean.
- Reset index, which gives a clean, sequential index.
- Remove rows that are most likely outliers (“wholesale” purchases), to better reflect typical consumer behavior.
- Group rows by Transaction because we are interested in analyzing customers rather than individual items.

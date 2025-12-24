# Task1_pandas

# Data Cleaning and Merging Project

A Python-based data cleaning pipeline that processes customer, order, and payment datasets, performing comprehensive data quality improvements before merging them into a single, analysis-ready dataset.

## Overview

This project demonstrates data cleaning best practices using Pandas to handle three related CSV files containing customer information, order details, and payment records. The pipeline addresses common data quality issues including duplicates, missing values, inconsistent formatting, and data type conversions.

## Features

- **Automated Data Cleaning**: Handles multiple data quality issues across three datasets
- **Robust Error Handling**: Validates relationships between datasets
- **Standardization**: Ensures consistent formatting across all fields
- **Data Integration**: Merges cleaned datasets while preserving data integrity

## Dataset Requirements

The project expects three CSV files:

1. **customers.csv** - Customer information
   - customer_id
   - name
   - email
   - city
   - signup_date

2. **orders.csv** - Order transactions
   - order_id
   - customer_id
   - order_date
   - total_amount

3. **payments.csv** - Payment records
   - payment_id
   - order_id
   - payment_status
   - payment_date

## Data Cleaning Steps

### Customers Data
- Remove duplicate customer records
- Fill missing email addresses with default value `no-email@example.com`
- Standardize city names (trim whitespace, capitalize properly)
- Convert signup_date to datetime format

### Orders Data
- Convert order_date to datetime format
- Extract and convert total_amount to float (handles currency symbols and formatting)
- Filter out orders with invalid customer_id references

### Payments Data
- Standardize payment_status values (lowercase, trim whitespace)
- Remove duplicate payment records
- Keep only payments with valid order_id references

### Data Integration
- Merge customers with orders (left join on customer_id)
- Merge result with payments (left join on order_id)
- Export final dataset to `cleaned_data.csv`

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/data-cleaning-project.git
cd data-cleaning-project

# Install required packages
pip install pandas numpy
```

## Usage

```python
import pandas as pd

# Run the cleaning pipeline
python clean_data.py
```

The script will:
1. Read the three CSV files from the current directory
2. Apply all cleaning transformations
3. Merge the datasets
4. Export the result to `cleaned_data.csv`

## Output

The cleaned dataset includes all fields from the three original datasets with:
- No duplicate records
- Standardized formats
- Valid relationships between tables
- Proper data types for all columns

## Dependencies

- Python 3.7+
- pandas >= 1.3.0
- numpy >= 1.21.0

## Project Structure

```
data-cleaning-project/
│
├── clean_data.py          # Main cleaning script
├── customers.csv          # Customer data (input)
├── orders.csv             # Order data (input)
├── payments.csv           # Payment data (input)
├── full_dataset.csv       # Final output
└── README.md             # This file
```

## Example Code Snippet

```python
import pandas as pd

# Load data
customers = pd.read_csv('customers.csv')
orders = pd.read_csv('orders.csv')
payments = pd.read_csv('payments.csv')

# Clean customers
customers = customers.drop_duplicates()
customers['email'] = customers['email'].fillna('no-email@example.com')
customers['city'] = customers['city'].str.strip().str.title()
customers['signup_date'] = pd.to_datetime(customers['signup_date'])

# Merge datasets
final_df = customers.merge(orders, on='customer_id', how='left')
final_df = final_df.merge(payments, on='order_id', how='left')

# Export
final_df.to_csv('cleaned_data.csv', index=False)
```

## Data Quality Improvements

| Issue | Solution |
|-------|----------|
| Duplicate records | `drop_duplicates()` |
| Missing emails | Filled with default value |
| Inconsistent city names | Trimmed and title-cased |
| String dates | Converted to datetime |
| Currency formatting | Extracted numeric values |
| Invalid references | Filtered using valid IDs |
| Inconsistent status values | Lowercased and trimmed |




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
- Convert signup_date to datetime format using `pd.to_datetime()`
- Fill missing email addresses with default value `ddd@gmail.com`
- Standardize city names to lowercase for consistency

### Orders Data
- Convert order_date to datetime format
- Extract numeric values from total_amount by removing 'EGP' prefix and converting to float
- Filter orders using inner join to keep only records with valid customer_id

### Payments Data
- Standardize payment_status values to uppercase
- Remove duplicate payment records based on payment_id (keeping last occurrence)
- Filter payments using inner join to keep only records with valid order_id

### Data Integration
- First merge: Join customers with cleaned orders (inner join on customer_id)
- Second merge: Join result with cleaned payments (inner join on order_id)
- Export final dataset to `full_dataset.csv`



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

The cleaned dataset (`full_dataset.csv`) includes all fields from the three original datasets with:
- No duplicate payment records
- Standardized formats (lowercase cities, uppercase payment status)
- Only valid relationships between tables (inner joins ensure data integrity)
- Proper data types for date and numeric columns
- Currency values cleaned and converted to float

## Dependencies

- Python 3.7+
- pandas >= 1.3.0
- numpy >= 1.21.0

## Project Structure

```
data-cleaning-project/
│
├── task.py          # Main cleaning script
├── customers.csv          # Customer data (input)
├── orders.csv             # Order data (input)
├── payments.csv           # Payment data (input)
├── full_dataset.csv       # Final cleaned output
└── README.md              # This file
```

## Example Code Snippet

```python
import pandas as pd

# Load data
data_customer = pd.read_csv('customers.csv')
data_orders = pd.read_csv('orders.csv')
data_payments = pd.read_csv('payments.csv')

# Clean customers data
data_customer['signup_date'] = pd.to_datetime(data_customer['signup_date'])
data_customer.fillna({'email': "ddd@gmail.com"}, inplace=True)
data_customer['city'] = data_customer['city'].str.lower()

# Clean orders data
data_orders['order_date'] = pd.to_datetime(data_orders['order_date'])
data_orders['total_amount'] = (data_orders['total_amount']
    .str.replace('EGP', '', regex=False)
    .str.strip()
    .astype(float))

# Filter orders with valid customer_id
new_orders = pd.merge(data_orders, data_customer[['customer_id']], 
                      on='customer_id', how='inner')

# Clean payments data
data_payments['payment_status'] = data_payments['payment_status'].str.upper()
new_payments = data_payments.drop_duplicates(subset='payment_id', keep='last')

# Filter payments with valid order_id
new_payments_2 = pd.merge(new_payments, new_orders[['order_id']], 
                          on='order_id', how='inner')

# Merge all datasets
temp_df = pd.merge(data_customer, new_orders, on='customer_id', how='inner')
full_dataset = pd.merge(temp_df, new_payments_2, on='order_id', how='inner')

# Export final dataset
full_dataset.to_csv('full_dataset.csv', index=False)
```

## Data Quality Improvements

| Issue | Solution |
|-------|----------|
| Missing emails | Filled with `ddd@gmail.com` |
| Inconsistent city names | Converted to lowercase |
| String dates | Converted to datetime using `pd.to_datetime()` |
| Currency formatting (EGP) | Removed prefix and converted to float |
| Invalid customer references | Inner join with customer_id |
| Duplicate payments | Removed using `drop_duplicates()` on payment_id (kept last) |
| Inconsistent payment status | Converted to uppercase |
| Invalid order references | Inner join with order_id |


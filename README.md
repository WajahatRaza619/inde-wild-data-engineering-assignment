# Data Engineering Assignment – Inde Wild

## Problem Statement

The objective of this assignment is to build a robust data pipeline that ingests, cleans, standardizes, and aggregates sales data from multiple B2B vendors i.e Zepto, Myntra, Nykaa, Blinkit, each providing data in different formats and schemas.

The final goal is to generate a unified daily sales dataset with a consistent schema.

## Approach

### 1. Data Ingestion

Data was ingested from multiple CSV files corresponding to different vendors.
Each dataset had its own schema and format.
Python (Pandas) was used to read and process all files.

### 2. Data Cleaning & Standardization

Each dataset was processed individually to standardize the schema:

#### Common Transformations:

Converted date columns into a standard datetime format.
Renamed columns to match a unified schema:

  * product_identifier
  * total_units
  * total_revenue
* Handled missing values using:

  * Row filtering for critical fields (date, product_identifier)
  * Filling non-critical nulls with default values (0)

### 3. Dataset-Specific Logic

#### Zepto

Used 'SKU Number' as product identifier.
Revenue was directly available as 'Gross Merchandise Value'.
Units taken from 'Sales (Qty) - Units'.

#### Myntra

Converted 'order_created_date' from 'YYYYMMDD' format.
Used 'style_id' as product identifier.
Revenue from 'mrp_revenue', units from sales.

#### Nykaa

Revenue was not directly available.
Computed as:
  total_revenue = Selling Price × Total Qty
  
Used 'SKU Code' as product identifier.

#### Blinkit

Used 'item_id' as product identifier.
Revenue already aggregated in 'mrp' hence no logic required.
Units taken from 'qty_sold'.


### 4. Data Transformation & Aggregation

All cleaned datasets were concatenated into a single DataFrame.

Aggregation was performed using:
  * 'date'
  * 'product_identifier'
  * 'data_source'

Metrics aggregated:
  * total_units = sum
  * total_revenue = sum

### 5. Output Generation

Final dataset exported as final_sales.csv

Schema as specified in the problem statement:
   * date | product_identifier | total_units | total_revenue | data_source


## Assumptions

* Each row in the dataset represents a valid transaction.
* Revenue columns in Zepto, Myntra, and Blinkit are already aggregated per row.
* Nykaa revenue must be derived using price × quantity.
* Product identifiers are consistent within each vendor.
* Missing values in critical fields (date, product_identifier) are dropped.


## Challenges Encountered

* Handling different date formats across datasets (DD-MM-YYYY vs YYYYMMDD).
* Identifying correct revenue logic for each vendor.
* Ensuring consistency across datasets with different schemas.
* Avoiding manual preprocessing and ensuring full automation in code.


## How to Run

* Upload all CSV files to the working directory (or Google Colab environment which I used)
* Run the pipeline script:
     python pipeline.py OR in Google Colab:

* Execute all cells sequentially
* Output file will be generated: final_sales.csv

## Tech Stack

* Python
* Pandas

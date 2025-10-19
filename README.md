# Modular ETL Pipeline in Azure Databricks using Lakeflow Jobs
This project demonstrates how to build a multi-stage ETL pipeline on Databricks using PySpark and Lakeflow Jobs, following a Bronze–Silver–Gold architecture.

# Stage 1 – Ingestion (Bronze Layer)

Dynamically read both CSV and JSON files from the raw folder in Azure Data Lake.

Used dbutils to extract file names and formats.

Logged ingestion status (status, file_name, timestamp) in a Delta table.

Moved processed files after successful runs.

# Stage 2 – Validation (Silver Layer)

Applied schema validation: OrderID, CustomerID, OrderDate, Amount must not be null.

Performed lookup validation for State and ProductCode against reference tables stored in Azure SQL.

Partitioned data into category=good and category=bad.

Generated a validation error summary and stored it in category=bad folder.

# Stage 3 – Transformation

Converted dates to YYYY-MM-DD format.

Split FullName using element_at(split(...), 1/2) to get FirstName and LastName.

Removed duplicates and logged bad data separately.
Normalized product names (lowercasing and joining with product master table in SQL).
Stored final clean data into the Processed folder in the silver layer.

# Stage 4 – Aggregation (Gold Layer)

Total sales per day.

Total sales per product and per state.

Top 5 customers by purchase amount.

7-day moving average of daily sales.

Results stored in the Gold layer for reporting & analytics.

# Lakeflow Jobs

Configured Databricks Lakeflow Job with tasks:

ingestion.py

validation.py

transformation.py

aggregation.py

Enabled notifications for success & failure to monitor pipeline health.

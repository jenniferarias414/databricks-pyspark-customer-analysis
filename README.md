# Databricks PySpark Customer Analysis

## Overview
This is a beginner Databricks + PySpark project built using sample data provided by Databricks (samples.bakehouse).

The goal of this project was to practice core data engineering concepts using PySpark DataFrames, including reading from the catalog, transforming data, joining tables, and performing aggregations.

---

## Tools & Technologies
- Databricks
- PySpark
- Delta Sharing (sample datasets)
- Python

---

## Key Concepts Practiced

### 1. Reading Data from Catalog

```python
customer = spark.table('samples.bakehouse.sales_customers')
transaction = spark.table('samples.bakehouse.sales_transactions')
```

- samples = catalog  
- bakehouse = schema (database)  
- sales_customers = table  

---

### 2. Data Exploration

```python
customer.printSchema()
customer.display()
customer.show()
```

- printSchema() → structure of data  
- display() → Databricks-only visualization  
- show() → raw output  

---

### 3. Transformations

```python
customer = customer.withColumn(
    'full_name',
    F.concat_ws(' ', F.col('first_name'), F.col('last_name'))
)
```

- Creating new columns  
- Using PySpark functions  

---

### 4. Distinct Count

```python
transaction.select('customerID').distinct().count()
```

Counts unique customers in transactions.

---

### 5. Joining DataFrames

```python
customer_transaction = customer.join(
    transaction,
    F.substring(customer.customerID.cast("string"), -3, 3) ==
    F.substring(transaction.customerID.cast("string"), -3, 3),
    'inner'
)
```

NOTE: This join is for learning purposes only.  
It joins on the last 3 digits of customerID, which is NOT a real-world best practice.

---

### 6. Aggregations

```python
revenue_per_customer = customer_transaction_clean.groupBy('customerID', 'full_name') \
    .agg(F.sum('totalPrice').alias('total_revenue'))
```

---

### 7. Window Functions (Ranking)

```python
w = Window.orderBy(F.desc('total_revenue'))

top_customers = revenue_per_customer \
    .withColumn('rank', F.rank().over(w)) \
    .filter(F.col('rank') <= 10)
```

---

## Final Output
Top 10 customers by total revenue.

---

## What I Learned
- Difference between display() and show()
- How Spark DataFrames differ from Pandas
- How joins and aggregations work in PySpark
- Basic Databricks workflow (notebooks + clusters)
- How to document a project in GitHub

---

## Future Improvements
- Use proper join keys instead of substring logic  
- Add partitioning to window functions  
- Optimize performance for large datasets  

---

## Project Structure

```
.
├── notebooks/
│   └── customer_analysis.py
├── images/
│   └── output.png
└── README.md
```

---

## Notes
This project is part of my Data Engineering learning journey.  
Focused on building consistency and practical experience with PySpark and Databricks.

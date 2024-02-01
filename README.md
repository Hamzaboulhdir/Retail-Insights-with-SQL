# Retail Insights with SQL (MySQL)

## Overview
This project focuses on extracting meaningful insights from sales and customer data using SQL in MySQL. The aim is to demonstrate how SQL can be utilized to answer key business questions and derive actionable insights from complex datasets.

## Datasets
The datasets used in this project are available on Kaggle:
- [Sales and Customer Data](https://www.kaggle.com/datasets/dataceo/sales-and-customer-data?select=customer_data.csv)

## Business Questions
1. What is the total revenue generated in 2022?
2. What is the most popular product category in terms of quantity sold?
3. What are the top three shopping malls in terms of sales revenue?
4. What is the gender distribution across different product categories?
5. What is the age distribution for different payment methods?
6. What is the average spending per age group?
7. How do monthly sales trends look throughout the year?

## SQL Queries and Results

### Merging the Datasets
**Query:**
```sql
SELECT 
  sales.*,
  sales.quantity * sales.price AS total_price
FROM sales_customer_data.sales_raw AS sales
JOIN sales_customer_data.customer_raw AS customer
ON sales.customer_id = customer.customer_id;


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
```

## 1. Total Revenue Generated in 2022
**Query:**
```sql
SELECT 
  SUM(quantity * price) AS total_revenue
FROM sales_customer_data.sales_raw
WHERE EXTRACT(year FROM invoice_date) = 2022;
```
Result:

| Total Revenue   |
| --------------- |
| 115,436,814.08  |

## 2. Most Popular Product Category
**Query:**
```sql
SELECT 
  category, 
  SUM(quantity) AS total_quantity
FROM sales_customer_data.sales_raw
GROUP BY category
ORDER BY total_quantity DESC;
```
Result:

| Category          | Total Quantity |
| ----------------- | -------------- |
| Clothing          | 103,558        |
| Cosmetics         | 45,465         |
| Food & Beverage   | 44,277         |
| Toys              | 30,321         |
| Shoes             | 30,217         |
| Technology        | 15,021         |
| Books             | 14,982         |
| Souvenir          | 14,871         |

`Interpretation:`
Clothing emerged as the most popular product category with over 100,000 units sold, significantly higher than other categories, reflecting its strong market demand.

## 3. Top 3 Shopping Malls by Revenue
**Query:**
```sql
SELECT  
  shopping_mall,
  SUM(quantity * price) AS total_revenue
FROM sales_customer_data.sales_raw
GROUP BY shopping_mall
ORDER BY total_revenue DESC
LIMIT 3;
```
Result:

| Shopping Mall       | Total Revenue   |
| ------------------- | --------------- |
| Mall of Istanbul    | 50,872,481.68   |
| Kanyon              | 50,554,231.10   |
| Metrocity           | 37,302,787.33   |

## 4. Gender Distribution Across Product Categories
**Query:**
```sql
SELECT 
  gender, 
  category, 
  COUNT(*) AS count
FROM sales_customer_data.sales_raw AS sales
JOIN sales_customer_data.customer_raw AS customer
ON sales.customer_id = customer.customer_id
GROUP BY gender, category
ORDER BY count DESC;
```
Result:

| Category         | Female | Male  |
| ---------------- | ------ | ----- |
| Clothing         | 20,652 | 13,835|
| Cosmetics        | 9,070  | 6,027 |
| Food & Beverage  | 8,804  | 5,972 |
| Toys             | 6,085  | 4,002 |
| Technology       | 2,981  | 2,015 |

`Interpretation:`
Female customers have a higher purchase count in every category, especially in clothing and cosmetics, indicating a gender-specific market trend in these categories.

## 5. Age Distribution for Payment Methods
**Query:**
```sql
SELECT 
  CASE
    WHEN age BETWEEN 0 AND 25 THEN '0-25'
    WHEN age BETWEEN 26 AND 40 THEN '26-40'
    WHEN age BETWEEN 41 AND 50 THEN '41-50'
    WHEN age BETWEEN 51 AND 70 THEN '51-70'
    ELSE 'other'
    END AS age_range,
  payment_method,
  COUNT(*) AS count
FROM sales_customer_data.customer_raw AS customer
GROUP BY age_range, payment_method
ORDER BY count DESC;
```
Result:

| Age Range | Payment Method | Count   |
| --------- | -------------- | ------- |
| 51-70     | Cash           | 16,169  |
| 26-40     | Cash           | 12,944  |
| 51-70     | Credit Card    | 12,660  |
| 26-40     | Credit Card    | 10,123  |
| 41-50     | Cash           | 8,451   |
| 51-70     | Debit Card     | 7,225   |
| 0-25      | Cash           | 6,833   |
| 41-50     | Credit Card    | 6,696   |
| 26-40     | Debit Card     | 5,830   |
| 0-25      | Credit Card    | 5,419   |
| 41-50     | Debit Card     | 3,897   |
| 0-25      | Debit Card     | 3,091   |
| Other     | Cash           | 50      |
| Other     | Debit Card     | 36      |
| Other     | Credit Card    | 33      |

## 6. Average Spending Per Age Group
**Query:**
```sql
SELECT 
  CASE
    WHEN age BETWEEN 0 AND 25 THEN '0-25'
    WHEN age BETWEEN 26 AND 40 THEN '26-40'
    WHEN age BETWEEN 41 AND 50 THEN '41-50'
    WHEN age BETWEEN 51 AND 70 THEN '51-70'
    ELSE 'other'
    END AS age_group,
  AVG(quantity * price) AS avg_spending
FROM sales_customer_data.sales_raw AS sales
JOIN sales_customer_data.customer_raw AS customer
ON sales.customer_id = customer.customer_id
GROUP BY age_group;
```

Result:

| Age Group | Average Spending |
| --------- | ---------------- |
| 26-40     | $2,533.84        |
| 51-70     | $2,529.35        |
| 41-50     | $2,559.31        |
| 0-25      | $2,481.61        |
| Other     | $2,329.93        |

## 7. Sales Trends Over Months
**Query:**
```sql
SELECT 
  EXTRACT(month FROM invoice_date) AS month,
  SUM(quantity * price) AS monthly_sales
FROM sales_customer_data.sales_raw
GROUP BY month
ORDER BY month;
```

Result:

| Month | Monthly Sales    |
| ----- | ---------------- |
| 1     | $28,891,525.59   |
| 2     | $26,625,090.10   |
| 3     | $21,956,191.33   |
| 4     | $18,715,685.98   |
| 5     | $19,719,331.10   |
| 6     | $18,933,775.30   |
| 7     | $20,378,722.63   |
| 8     | $19,282,361.29   |
| 9     | $18,795,794.91   |
| 10    | $20,545,090.43   |
| 11    | $18,207,139.95   |
| 12    | $19,455,085.64   |

`Interpretation:`
Sales peaked in January, followed by a decline with some fluctuations throughout the year. The trend suggests a strong start to the year with gradual normalization of sales over the following months.


### Conclusion

This project demonstrates the capability of SQL in analyzing real-world data to uncover valuable business insights. The insights derived can aid in strategic decision-making and uncover opportunities for growth and optimization.

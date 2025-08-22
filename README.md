# Retail-Sales-Analysis-SQL-Project
# 🛒 Retail Sales Analysis  

![SQL](https://img.shields.io/badge/SQL-Queries-blue?logo=postgresql&logoColor=white)  
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-green?logo=postgresql)  
![Beginner Friendly](https://img.shields.io/badge/Level-Beginner-brightgreen)  
![EDA](https://img.shields.io/badge/EDA-Exploratory%20Data%20Analysis-orange)  
![Project](https://img.shields.io/badge/Project-Type%3A%20Practice-lightgrey)  

This project demonstrates **SQL skills and techniques** typically used by data analysts to explore, clean, and analyze retail sales data. It involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.  
 

---

## 📑 Table of Contents  
1. [Objectives](#-objectives)  
2. [Project Structure](#-project-structure)  
   - [Database Setup](#1-database-setup)  
   - [Data Exploration & Cleaning](#2-data-exploration--cleaning)  
   - [Data Analysis & Business Queries](#3-data-analysis--business-queries)  
3. [Findings](#-findings)  
4. [Reports](#-reports)  
5. [Conclusion](#-conclusion)  

---

## 📌 Objectives
- **Database Setup:** Create and populate a retail sales database with provided data.  
- **Data Cleaning:** Identify and remove records with missing/null values.  
- **Exploratory Data Analysis (EDA):** Perform basic analysis to understand the dataset.  
- **Business Analysis:** Answer specific business questions and derive insights using SQL.  

---

## 📂 Project Structure

### 1. Database Setup
```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
2. Data Exploration & Cleaning

Record Count

SELECT COUNT(*) FROM retail_sales;


Unique Customers

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;


Unique Categories

SELECT DISTINCT category FROM retail_sales;


Check for NULL Values

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;


Delete NULL Records

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

3. Data Analysis & Business Queries
🔹 Sales on a specific date (2022-11-05)
SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';

🔹 Clothing sales with quantity > 4 in Nov-2022
SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;

🔹 Total sales by category
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

🔹 Average age of customers (Beauty category)
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

🔹 Transactions with total sale > 1000
SELECT *
FROM retail_sales
WHERE total_sale > 1000;

🔹 Transactions by gender & category
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

🔹 Best selling month (per year)
SELECT 
       year,
       month,
       avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) 
                    ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1
WHERE rank = 1;

🔹 Top 5 customers by total sales
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

🔹 Unique customers per category
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

🔹 Sales by time of day (Shift analysis)
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;

📊 Findings

Customer Demographics: Wide age range with purchases across categories like Clothing and Beauty.

High-Value Transactions: Several sales > 1000 indicate premium purchases.

Sales Trends: Monthly analysis highlights peak sales seasons.

Customer Insights: Identified top-spending customers and most popular categories.

📑 Reports

Sales Summary: Total sales, demographics, and category performance.

Trend Analysis: Seasonal & monthly trends across shifts.

Customer Insights: Top customers & unique buyers by category.

✅ Conclusion

This project is a comprehensive SQL case study covering:

Database setup

Data cleaning

Exploratory data analysis

Business-driven SQL queries

The findings provide actionable insights into sales patterns, customer behavior, and product performance, which can drive business decisions in retail.


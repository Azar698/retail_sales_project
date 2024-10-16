****Retail Sales Analysis SQL Project**
****Project Overview**
**Project Title:** Retail Sales Analysis
**Level:** Beginner
**Database:** p1_retail_db

This project showcases SQL skills used in data analysis, including data cleaning, exploration, and answering business-related questions. It involves setting up a retail sales database, analyzing data through SQL queries, and deriving insights. It's ideal for beginners looking to build a strong foundation in SQL.

**Objectives**
**Database Setup:** Create and populate a retail sales database (p1_retail_db).
**Data Cleaning:** Identify and remove records with missing values.
**Exploratory Data Analysis (EDA):** Perform EDA to understand the dataset.
**Business Analysis:** Use SQL queries to answer specific business questions.

**1.Project Structure**
**Database Setup**
CREATE DATABASE p1_retail_db;

**Create table**
CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
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

**2. Data Exploration & Cleaning**

**Record Count:**
SELECT COUNT(*) FROM retail_sales;

**Customer Count:**
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

**Category Count:**
SELECT DISTINCT category FROM retail_sales;

**Null Value Check & Deletion:**
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;


**Data Analysis & Business Queries
Sales on a Specific Date:**

**Sales on a Specific Date:**
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
Transactions for 'Clothing' in Nov 2022:

**Transactions for 'Clothing' in Nov 2022:**
SELECT * FROM retail_sales
WHERE category = 'Clothing' AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11' AND quantity >= 4;
Total Sales by Category:

**Total Sales by Category:**
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
Average Age of 'Beauty' Category Customers:

**Average Age of 'Beauty' Category Customers:**
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
High-Value Transactions (Total Sale > 1000):

**High-Value Transactions (Total Sale > 1000):**
SELECT * FROM retail_sales WHERE total_sale > 1000;
Transactions by Gender and Category:

**Transactions by Gender and Category:**
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;
Best Selling Month Each Year:

**Best Selling Month Each Year:**
SELECT year, month, avg_sale
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year,
           EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS avg_sale,
           RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS t1
WHERE rank = 1;
Top 5 Customers by Total Sales:

**Top 5 Customers by Total Sales:**
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
Unique Customers by Category:


**Unique Customers by Category:**

SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
Orders by Shift (Morning, Afternoon, Evening):

**Orders by Shift (Morning, Afternoon, Evening):**

WITH hourly_sale AS (
    SELECT *, 
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;


**Findings**
**Customer Demographics:** Includes various age groups purchasing across categories.
**High-Value Transactions:** Several transactions had sales above 1000, indicating premium sales.
**Sales Trends:** Monthly sales analysis helps identify peak periods.
**Top Customers:** The top customers contribute significantly to sales.
**Conclusion**
This project provides a hands-on introduction to SQL for data analysts, covering database creation, data cleaning, and analysis. The insights can drive business decisions by understanding sales patterns, customer behavior, and product performance.

# Retail_sales_project
This project involves creating and managing a Retail Sales database for analyzing and querying sales data. It demonstrates essential SQL concepts, including table creation, data querying, filtering, and using aggregate functions to derive meaningful insights from sales records. The dataset includes details about sales transactions such as customer demographics, product categories, and financial metrics like cost of goods sold (COGS) and total sales.

Features
Database Design: The project includes a well-structured table (retail_sales) that holds detailed sales transaction data.
Data Management: Efficient SQL queries to retrieve sales information, filter data based on customer attributes (e.g., gender, age), and perform calculations on sales and financial data.
SQL Queries:
Filter sales by customer demographics (e.g., gender, age, category).
Aggregate sales data by time (e.g., day, month).
Compute key metrics like total sales, average cost of goods sold (COGS), and profit.
Identify missing or incomplete records for data validation.
Table Structure
The retail_sales table includes the following fields:

transaction_id: Unique identifier for each transaction.
sale_date: Date of the sale.
sale_time: Time of the sale.
customer_id: Unique identifier for the customer.
gender: Gender of the customer.
age: Age of the customer.
category: Product category of the sale.
quantity: Quantity of items sold.
price_per_unit: Price per unit of the product.
cogs: Cost of goods sold (COGS).
total_sale: Total sale amount for the transaction.


1. Create the retail_sales Table:
This part defines the table structure for storing retail sales data in sales_db.

sql
Copy code
CREATE TABLE retail_sales
(
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
2. Cleaning Data with NULL Values:
You're first identifying rows with NULL values and then deleting those rows.

sql
Copy code
-- Selecting rows with NULL values
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Deleting rows with NULL values
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3. Data Analysis Queries:
These queries cover different aspects of analysis for sales data:

a) Retrieve all transactions on a specific date:
sql
Copy code
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
b) Transactions for a specific category and minimum quantity in November 2022:
sql
Copy code
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;
c) Calculate total sales for each category:
sql
Copy code
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY category;
d) Find average age of customers purchasing from the "Beauty" category:
sql
Copy code
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
e) Transactions with total sales greater than 1000:
sql
Copy code
SELECT * FROM retail_sales
WHERE total_sale > 1000;
f) Total transactions by gender in each category:
sql
Copy code
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
g) Top 5 customers based on total sales:
sql
Copy code
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
h) Orders per shift (Morning, Afternoon, Evening):
sql
Copy code
WITH hourly_sale AS
(
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
Notes:
You're using the CASE statement effectively to categorize shifts based on sale times.
The use of SUM, AVG, COUNT, and GROUP BY in your queries helps summarize and analyze sales data efficiently.
To retrieve rows where any of the columns contain NULL values, you used the correct OR condition.
This script is ready for implementation on your sales_db database. You can push this to GitHub with a detailed README for context if needed!

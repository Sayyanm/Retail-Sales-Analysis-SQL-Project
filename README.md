# Retail-Sales-Analysis-SQL-Project
# Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.
# Objectives
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
Data Cleaning: Identify and remove any records with missing or null values.
Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

# Project Structure
Database Creation: The project starts by creating a database named *** SQL_PROJECT_1; ***
Table Creation: A table named RETAIL_SALES is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

***
  	CREATE TABLE RETAIL_SALES(

	transactions_id	INT PRIMARY KEY,
	sale_date	DATE,
	sale_time	TIME,
	customer_id	INT,
	gender	VARCHAR(10),
	age	INT,
	category VARCHAR(15),
	quantiy	FLOAT,
	price_per_unit	FLOAT,
	cogs FLOAT,
	total_sale FLOAT

  	);
***

# Data Exploration & Cleaning

Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.

***
  	SELECT 
	COUNT(*)
  	FROM RETAIL_SALES;

 	SELECT * FROM RETAIL_SALES
 	 WHERE 
	transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR
	age IS NULL
	OR
	category IS NULL
	OR
	quantiy IS NULL
	OR
	price_per_unit IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;
 
	DELETE FROM RETAIL_SALES
	WHERE 
	transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR
	age IS NULL
	OR
	category IS NULL
	OR
	quantiy IS NULL
	OR
	price_per_unit IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;
 ***
# Data Analysis

The following SQL queries were developed to answer specific business questions:

1. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
***
	SELECT * FROM RETAIL_SALES
	WHERE sale_date = '2022-11-05';
***

2. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
***
	SELECT * FROM RETAIL_SALES
	WHERE category = 'Clothing'
  	AND quantiy >= 4
  	AND sale_date >= '2022-11-01'
  	AND sale_date < '2022-12-01';
  ***
3. Write a SQL query to calculate the total sales (total_sale) for each category.:

***
	SELECT category, SUM(total_sale) as Net_Sale FROM RETAIL_SALES
	GROUP BY category;
***
4.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
***
	SELECT
	AVG(age) 
	from RETAIL_SALES
	where category = 'Beauty';
***
5. Write a SQL query to find all transactions where the total_sale is greater than 1000.:

***
	SELECT * FROM RETAIL_SALES
	WHERE total_sale > '1000';
***
6. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
***
	SELECT
	category,
	gender,
	count(*) as Total_Tracsaction
	FROM RETAIL_SALES
	GROUP BY category,gender
	ORDER BY 1;
***
7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
***
	-- Step 1: Calculate average total_sale for each year and month
	WITH monthly_avg AS (
    SELECT 
        YEAR(sale_date) AS year,                          -- Extract the year from the sale_date
        MONTH(sale_date) AS month,                        -- Extract the month from the sale_date
        AVG(total_sale) AS avg_monthly_sale               -- Calculate average sale for that month
    FROM RETAIL_SALES
    GROUP BY 
        YEAR(sale_date),                                  -- Group by year
        MONTH(sale_date)                                  -- Group by month
	),

	-- Step 2: Rank the months within each year by avg_monthly_sale (highest first)
	ranked_months AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY year                          -- For each year...
               ORDER BY avg_monthly_sale DESC             -- Rank by highest average monthly sale
           ) AS rn
    FROM monthly_avg
	)

	-- Step 3: Select only the top-ranked month (rn = 1) per year
	SELECT 
    year, 
    month,
    avg_monthly_sale
	FROM ranked_months

***
8. Write a SQL query to find the top 5 customers based on the highest total sales 
***
	SELECT TOP 5
	   customer_id,
	   SUM(total_sale) AS Total_Sales
	FROM RETAIL_SALES
	GROUP BY customer_id
	ORDER BY Total_Sales DESC;

***
9. Write a SQL query to find the number of unique customers who purchased items from each category.:
***
	SELECT
	category,
	COUNT(DISTINCT customer_id) AS Number_of_customer
	FROM RETAIL_SALES
	GROUP BY category;
***

10. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):

***
	SELECT 
    -- Define shift using the hour part of sale_time
    CASE 
        WHEN DATEPART(HOUR, sale_time) < 12 THEN 'Morning'          -- 00:00 to 11:59
        WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon' -- 12:00 to 17:59
        ELSE 'Evening'                                              -- 18:00 to 23:59
    END AS shift,

    -- Count how many orders fall in each shift
    COUNT(*) AS number_of_orders

	FROM RETAIL_SALES

	-- Group by the same shift classification
	GROUP BY 
    CASE 
        WHEN DATEPART(HOUR, sale_time) < 12 THEN 'Morning'
        WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END

	-- Sort the results alphabetically by shift
	ORDER BY shift;
***

# Findings
Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.
# Reports
Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
Trend Analysis: Insights into sales trends across different months and shifts.
Customer Insights: Reports on top customers and unique customer counts per category.
Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.




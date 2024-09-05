# Sales Analysis SQL Project

## Project Overview

**Project Title**: sales_project1.sql 
**Level**: Beginner  
**Database**: Sales_dataset

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.


```sql
-- creat table retal sales

create table retail_sales
	(
	transactions_id int primary key ,
	sale_date date,
	sale_time time,
	customer_id	int,
	gender varchar(15),
	age	int,
	category varchar(15),
	quantiy	int,
	price_per_unit int,
	cogs float,
	total_sale float
	);

select * from retail_sales
	limit 10;

--data exploration 

select count(*) from retail_sales;

--data cleaning 

select * from retail_sales 
where 
	sale_date is null
    or 
    sale_time is null 
    or 
    customer_id is null 
    or 
    gender is null
    or 
    age is null
	or
    category is null
	or
    quantiy is null
	or
    price_per_unit is null
	or
    cogs is null
	or
    total_sale is null;

-- Removing null values 

delete from retail_sales
where 
    sale_date is null
    or 
    sale_time is null 
    or 
    customer_id is null 
    or 
    gender is null
    or 
    age is null
	or
    category is null
	or
    quantiy is null
	or
    price_per_unit is null
	or
    cogs is null
	or
    total_sale is null;

--- data exploration 
--how many unique customer we have 

select count(distinct customer_id)
from retail_sales;

-- total sales 

select sum(total_sale) from retail_sales;

-- unique category
select distinct category from retail_sales;

--  Data Analysis & Business Key Problems with there selutions 


--  Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

select round (avg(age),2)as avg_age_customer
	from retail_sales
    where category = 'Beauty';
   
-- Write a SQL query to find the top 5 customers based on the highest total sales 

select
	customer_id,sum(total_sale) as total_sale
from retail_sales
group by customer_id
order by sum(total_sale) desc
limit 5;

-- Write a SQL query to find all transactions where the total_sale is greater than 1000.

select 
	total_sale
from
where total_sale >'1000';

-- Write a SQL query to find the number of unique customers who purchased items from each category.


select
	category, count(distinct customer_id) as cnt_unique_cust
from retail_sales
group by category;

-- Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1  ; 

-- Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale
AS
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

-- Write a query to find the total total_sale amount for each month 

SELECT 
	EXTRACT(YEAR FROM sale_date) AS Year,
	EXTRACT(MONTH FROM sale_date)AS month ,
	SUM(total_sale) AS total_sales    
FROM retail_sales
GROUP BY 
	EXTRACT(YEAR FROM sale_date),
	EXTRACT(MONTH FROM sale_date)
ORDER BY 1,2;

--Write a query to calculate the number of customers in different age groups (e.g., 18-25, 26-35, 36-45, etc.).

SELECT 
  CASE 
	WHEN age BETWEEN 18 AND 25 THEN 'Young' 
    WHEN age BETWEEN 26 AND 35 THEN 'Adult'
    WHEN age BETWEEN 36 AND 45 THEN 'Senior'
    ELSE 'senior citizen'
    END AS Age_group ,
	COUNT(*) AS no_customer  
FROM retail_sales
GROUP BY age_group;


-- Write a query to calculate the average profit margin for each category, where profit margin is defined as (total_sale - cogs) / total_sale

SELECT 
	category,
	AVG((total_sale - cogs )/total_sale) AS avg_profit_margin  
FROM 
	retail_sales
GROUP BY 
	CATEGORY;

-- Write a query to find the number of transactions and total revenue for each month.

SELECT 
	EXTRACT (year from sale_date) as year ,
	extract (month from sale_date) as month,
	COUNT(*) as Transaction_count,
    SUM(total_sale) AS total_revenu
	FROM retail_sales 
GROUP BY
    EXTRACT (year from sale_date),
	EXTRACT (month from sale_date) 
ORDER BY
    EXTRACT (year from sale_date);



## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.



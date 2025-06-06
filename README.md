# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Platform**: MS SQL Server 

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_sale_db`.
- **Table Creation**: A table named `Retail_sale` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
DROP TABLE IF EXISTS Retail_Sale;
CREATE TABLE Retail_Sale 
(
     transactions_id  INT PRIMARY KEY,
     sale_date	DATE,
     sale_time	TIME,
     customer_id	INT,
     gender	VARCHAR(15),
     age	INT,
     category	VARCHAR(15),
     quantiy	INT,
     price_per_unit	FLOAT,
     cogs	FLOAT,
     total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
-- Observe Top 10 rows from Retail_Sale
SELECT TOP 10 * FROM Retail_Sale;

-- Get the total data entries
SELECT COUNT(*) as total_entries
FROM Retail_Sale;

--Observe the null/empty entries

SELECT * FROM Retail_Sale
WHERE transactions_id IS NULL
OR
sale_date IS NULL
OR
sale_time IS NULL
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
total_sale IS NULL
;

--Delete the null/empty entries

DELETE FROM Retail_Sale
WHERE transactions_id IS NULL
OR
sale_date IS NULL
OR
sale_time IS NULL
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
total_sale IS NULL
;

-- Get the total non-null data entries
SELECT COUNT(*) as total_entries
FROM Retail_Sale;

-- *** Data Exploration ***

-- Number of sales
SELECT COUNT(*) AS total_sales FROM Retail_Sale

-- Number of distinct customers
SELECT COUNT(DISTINCT customer_id) AS total_customers FROM Retail_Sale

-- Number of distinct categories
SELECT DISTINCT category AS categories FROM Retail_Sale
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * FROM Retail_Sale
WHERE sale_date='2022-11-05';
```

2. **Retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT * FROM Retail_Sale
WHERE sale_date>='2022-11-01' AND sale_date<'2022-12-01' AND category='Clothing' AND quantiy>=4;

```

3. **Calculate the total sales (total_sale) for each category.**:
```sql
SELECT category, SUM(total_sale) as net_sale, COUNT(*) as total_orders
From Retail_Sale
GROUP BY category;
```

4. **Find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT AVG(age) as avg_age
From Retail_Sale
WHERE category='Beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT category, gender, COUNT(*) AS total_trans FROM Retail_Sale
GROUP BY category,gender 
ORDER BY category,gender;

```

7. **Calculate the average sale for each month. Find out best selling month in each year**:
```sql
WITH RankedSales AS (
    SELECT 
        YEAR(sale_date) AS sale_year,
        MONTH(sale_date) AS sale_month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY YEAR(sale_date) 
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM Retail_Sale
    GROUP BY YEAR(sale_date), MONTH(sale_date)
)
SELECT 
    sale_year AS year,
    sale_month AS month,
    avg_sale
FROM RankedSales
WHERE rnk = 1;
```

8. **Find the top 5 customers based on the highest total sales.**:
```sql
SELECT TOP 5 customer_id, SUM(total_sale) as total_sales FROM Retail_Sale
GROUP BY customer_id
ORDER BY total_sales DESC;
```

9. **Find the number of unique customers who purchased items from each category.**:
```sql
SELECT COUNT(DISTINCT(customer_id)) as customer_count, category FROM Retail_Sale
GROUP BY category;
```

10. **Create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale AS
(
SELECT *,
CASE 
 WHEN DATEPART(HOUR,sale_time)<12 THEN 'Morning'
 WHEN DATEPART(HOUR,sale_time)>=12 AND DATEPART(HOUR,sale_time)<17 THEN 'Afternoon'
ELSE 'Evening'
END AS shift
FROM Retail_Sale)

SELECT shift,COUNT(*) as total_orders FROM hourly_sale
GROUP BY shift;

```

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

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Sayura Senaratne

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. 

- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/sayura-senaratne-0a7639198/)


Thank you!

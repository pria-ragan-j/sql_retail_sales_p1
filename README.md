# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `retail_sales_p1`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.
## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail_sales_p1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE retail_sales_p1;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE transactions_id	is null
or
sale_date is null	
or
sale_time is null
or
customer_id	is null	
or
gender	is null
or
age	is null
or
category is null
or
quantiy	is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null ;
    

DELETE FROM retail_sales
WHERE transactions_id	is null
or
sale_date is null	
or
sale_time is null
or
customer_id	is null	
or
gender	is null
or
age	is null
or
category is null
or
quantiy	is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null; 
    
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select * from retail_sales
where sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
select * from retail_sales
where 
category = 'Clothing'
and
sale_date >= '2022-11-01' 
and
sale_date <= '2022-11-30'
and 
quantiy >= 4;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select category, sum(total_sale) as Total_sales
from retail_sales
group by category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select category,round(avg(age),2) as average
from retail_sales
where category = 'Beauty'
group by category;
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
select * from retail_sales
where total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select category,gender,count(transactions_id) as number_of_transactions
from retail_sales
group by category,gender;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
select * from 
(select extract (month from sale_date) as month,
extract (year from sale_date) as year,
avg(total_sale) as Average,
rank() over(partition by extract (year from sale_date) order by avg(total_sale) desc ) as rank
from retail_sales
group by month,year
order by average desc)
where rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
select customer_id, sum(total_sale) as total_sales 
from retail_sales
group by customer_id
order by total_sales desc
limit 5 ;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
select category ,count(distinct(customer_id)) as Unique_customers
from retail_sales
group by category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
select shifts,count(*) as number_of_sales from 
(select *,
case when extract (hour from sale_time) <= 12 then 'Morning'
when extract (hour from sale_time) > 12  and extract (hour from sale_time) <= 17
then 'Afternoon'
else 'Evening'
end as shifts from retail_sales)t1
group by shifts
order by number_of_sales desc;
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


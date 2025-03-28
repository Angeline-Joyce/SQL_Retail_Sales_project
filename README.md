# SQL_Retail_Sales_project
 Analysing retail sales using SQL

**Retail Sales Analysis - SQL Project**

**📌 Objective**

The goal of this project is to analyze retail sales data using SQL Server to extract meaningful insights, such as customer trends, best-selling products, peak sales periods, and other key business metrics. The project involves cleaning raw data, transforming it into structured formats, and performing advanced SQL queries to derive valuable insights.

**📂 Dataset Overview**

The dataset contains sales transactions with attributes such as transaction ID, sale date, sale time, customer details, product category, quantity sold, unit price, cost of goods sold (COGS), and total sale value.

Initially, all columns were stored as NVARCHAR due to import issues, and later converted to their appropriate data types (e.g., INT, FLOAT, DATE, TIME).

**🔧 Data Cleaning & Transformation**

Converted Data Types: Changed NVARCHAR columns to appropriate types (DATE, TIME, INT, FLOAT, etc.).

Handled Null & Blank Values: Checked for missing data and removed incomplete records.

**📊 Business Key Problems & SQL Queries**

--CREATE DATABASE
```sql
create database SQL_Sales_project;
```

--CREATE TABLE 
--Since I faced issues while importing the data, I created table with all column datatype as nvarchar 
--and converted into their original datatype after importing the data

```sql
create table retail_sales
            (
		transactions_id	INT Primary key,
		sale_date NVARCHAR(50),
		sale_time NVARCHAR(50),
		customer_id	NVARCHAR(50),
		gender NVARCHAR(50),
		age	NVARCHAR(50),
		category NVARCHAR(50),
		quantiy	NVARCHAR(50),
		price_per_unit	NVARCHAR(50),
		cogs NVARCHAR(50),
		total_sale NVARCHAR(50),
             );
```

-- CONVERTING THE TABLE VALUES TO ITS ORIGINAL DATATYPES
```sql
-- Convert sale_time to TIME
ALTER TABLE retail_sales ALTER COLUMN sale_time TIME;

-- Convert sale_date to DATE
ALTER TABLE retail_sales ALTER COLUMN sale_date DATE;

-- Convert customer_id to INT
ALTER TABLE retail_sales ALTER COLUMN customer_id INT;

-- Convert age to INT
ALTER TABLE retail_sales ALTER COLUMN age INT;

-- Convert gender to VARCHAR(10)
ALTER TABLE retail_sales ALTER COLUMN gender VARCHAR(10);

-- Convert category to VARCHAR(50)
ALTER TABLE retail_sales ALTER COLUMN category VARCHAR(50);

-- Convert quantity to int
ALTER TABLE retail_sales ALTER COLUMN quantiy INT;

-- Convert price_per_unit to FLOAT
ALTER TABLE retail_sales ALTER COLUMN price_per_unit FLOAT;

-- Convert cogs to FLOAT
ALTER TABLE retail_sales ALTER COLUMN cogs FLOAT;

-- Convert total_sale to FLOAT
ALTER TABLE retail_sales ALTER COLUMN total_sale FLOAT;
```

-- DATABASE CLEANING
-- Checked for null and blanks in every column and removed it

```sql
select * from retail_sales
where transactions_id is null
or sale_date is null
or sale_time is null
or customer_id is null
or gender is null
or age is null
or category is null
or quantiy is null
or price_per_unit is null
or cogs is null
or total_sale is null;
```

-- **Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'**

```sql
select * from retail_sales
where sale_date = '2022-11-05';
```
-- **Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing'**
-- **and the quantity sold is more than 3 in the month of Nov-2022**

```sql
select transactions_id, 
       category, 
	   total_sale, 
	   quantiy, 
	   FORMAT(sale_date,'MMM-yyyy') as formatted_date 
from retail_sales
where category = 'Clothing' 
      and quantiy >=3 
	  and FORMAT(sale_date,'MMM-yyyy') = 'Nov-2022';
```

-- **Q.3 Write a SQL query to calculate the total sales (total_sale) for each category**

```sql
select category,
       sum(total_sale) as Total_sales
from retail_sales
group by category;
```

-- **Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**

```sql
select category,
       avg(age) as avg_age
from retail_sales
where category = 'Beauty'
group by category;
```

-- **Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000**

```sql
select *
     from retail_sales
where total_sale > 1000;
```

-- **Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**

```sql
select gender, 
       category, 
       count(transactions_id) as cnt_id
from retail_sales
group by gender, category
order by gender, category;
```

-- **Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**

```sql
with cte as
(
    select YEAR(sale_date) as year_sale,
       MONTH(sale_date) as month_sale, 
       avg(total_sale) as avg_sale,
	   rank() over(partition by YEAR(sale_date) order by avg(total_sale) desc) as rrank 
from retail_sales
group by YEAR(sale_date), MONTH(sale_date)
)
select * from cte
where rrank = 1
```

-- **Q.8 Write a SQL query to find the top 5 customers based on the highest total sales**

```sql
select top 5
	customer_id, 
	sum(total_sale) as total_sales
from retail_sales
group by customer_id
order by total_sales desc;
```

-- **Q.9 Write a SQL query to find the number of unique customers who purchased items from each category**

```sql
select category,
	   count(distinct customer_id) as unique_customer
from retail_sales
group by category;
```

-- **Q.10 Write a SQL query to create each shift and number of orders**
-- **(Example Morning <=12, Afternoon Between 12 & 17, Evening >17)**

```sql
with cte1 as 
(select *, 
	case 
	when DATEPART(hour, sale_time) < 12 then 'Morning'
	when DATEPART(hour, sale_time) between 12 and 17 then 'Afternoon'
	else 'Evening'
	end as shifts
from retail_sales)
select shifts,
	   count(*) as no_of_orders
from cte1 
group by shifts;
```



**📈 Key Insights & Findings**

Best-Selling Month: Identified the top-selling month each year based on total sales volume.

Customer Preferences: Customers in the 'Beauty' category had a higher average age than other categories.

High-Value Transactions: Transactions above 1000 were identified to analyze premium sales.

Gender-Based Sales Trends: Found how sales varied across genders and product categories.

Peak Shopping Hours: Sales were categorized into Morning, Afternoon, and Evening to understand customer shopping behavior.

**📌 Conclusion**

This project demonstrates how SQL can be used for powerful data cleaning, transformation, and analysis in a retail business. The insights gained can help businesses optimize inventory, target marketing strategies, and improve customer experience. Future improvements include integrating Power BI dashboards for real-time visualization and deeper data insights.

**Author**
Angeline Joyce J

--END OF THE PROJECT--

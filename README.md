# RETAIL SALES ANALYSIS

## PROJECT OVERVIEW:
**PROJECT TITLE**: RETAIL SALS ANALYSIS
**LEVEL**:BASIC
**DATABASE**:retail_db

This project is to use SQL by working with retail sales data by exploring and clean the data, and write SQL queries to answer real business questions. 

## OBJECTIVES:
**Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
**Data Cleaning**: Identify and remove any records with missing or null values.
**Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
**Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## PROJECT STRUCTURE
#### 1.Data Exploration & Cleaning
**Modifying and Renaming Data**: Chaning the datas to its appropriate data types and name.
```sql
ALTER TABLE SALES
MODIFY COLUMN ï»¿transactions_id INT PRIMARY KEY,
MODIFY COLUMN sale_time time,
MODIFY COLUMN customer_id int not null,
MODIFY COLUMN gender varchar(10) not null,
MODIFY COLUMN category varchar(20) not null,
MODIFY COLUMN quantiy int not null,
MODIFY COLUMN price_per_unit decimal(10,2) not null,
MODIFY COLUMN cogs decimal(10,2) not null,
MODIFY COLUMN total_sale decimal(10,2) not null,
MODIFY COLUMN age int not null

set sql_safe_updates=1
UPDATE SALES
SET sale_date=str_to_date(sale_date,'%d-%m-%Y')
 
alter table SALES
MODIFY sale_date date not null

ALTER TABLE SALES
RENAME COLUMN ï»¿transactions_id to transaction_id

set sql_safe_updates =0
```

**Removing Nulls**: Using delete function to remove all nulls in all columns.
```sql
DELETE FROM SALES 
WHERE quantiy=0
or
price_per_unit=0
or
cogs=0
or
total_sale=0
```
**Count Of Records**: Determine the total number of records in the dataset.
```sql
SELECT COUNT(*) FROM SALES
```

## DATA ANALYSIS AND FINDINGS:
1.**HOW MANY SALES DONE?**
```sql
SELECT COUNT(transaction_id) as Total_count_of_sales
from SALES;
```
2.**WHAT IS THE CUSTOMER STRENGTH?**
```sql
SELECT COUNT(DISTINCT(customer_id)) as Total_customers 
from SALES;
```
3.**Calculate the total sales (total_sale) for each category**.:
```sql
SELECT COUNT(DISTINCT(category)) as Total_count_of_categories
from SALES
```
## Business Key Problems:
1.**Write a SQL query to retrieve all columns for sales made on '2022-11-05':**
```sql
SELECT * FROM SALES 
WHERE sale_date = '2022-11-05'
```
2.**Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022.**
```sql
SELECT * FROM SALES
WHERE 
   category ="Clothing"
and
   quantiy>=4
and 
	month(sale_date) = 11
and 
    year(sale_date)=2022
```
3.**Write a SQL query to calculate the total sales (total_sale) for each category.:**
 ```sql
 SELECT distinct(category) as categories ,
  sum(total_sale) as total_sales,
  COUNT(*) AS number_of_products
  FROM SALES
  GROUP BY categories;
 ```
4.**Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:**
```sql
SELECT category, ROUND(AVG(age),0) as average_age
FROM SALES
WHERE category="Beauty"
```

5.**Write a SQL query to find all transactions where the total_sale is greater than 1000.:**
```sql
SELECT * FROM SALES
WHERE total_sale > 1000
```
6.**Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:**
 ```sql
 SELECT category,gender,COUNT(transaction_id) as transactions
 from SALES
 GROUP BY category,gender
 order by transactions desc
``` 
 7.**Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:**
 ```sql
 with Best_Months as (
 SELECT MONTHNAME(sale_date) as Month_name,YEAR(sale_date) as year_of_sale ,round(avg(total_sale),0) AS average_sales,
 RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY round(avg(total_sale),0) desc)as average_sales_rank
 from SALES 
 GROUP BY month_name,year_of_sale
 )
 
 select * from Best_Months
 where average_sales_rank =1;
 
 -------------OR-----------------
 
 SELECT 
       yearS,
       monthS,
    avg_sale
FROM 
(    
SELECT 
    YEAR(sale_date) as yearS,
    MONTHNAME(sale_date) as monthS,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) as rankS
FROM SALES
GROUP BY yearS,monthS
) as t1
WHERE rankS  = 1
``` 
 8.**Write a SQL query to find the top 5 customers based on the highest total sales:**
 ```sql
SELECT customer_id, SUM(total_sale) AS TOTAL_SALES 
FROM SALES
GROUP BY customer_id
ORDER BY total_sale desc
LIMIT 5;
```
9.**Write a SQL query to find the number of unique customers who purchased items from each category.:**
```sql
SELECT category,COUNT(DISTINCT(customer_id)) as total_customers
FROM SALES
GROUP BY category 
ORDER BY total_customers desc;
```
10.**Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):**
```sql
WITH HOURLEY_SALES AS (
SELECT 
CASE 
    WHEN HOUR(sale_time)<12 THEN "Morining"
	WHEN HOUR(sale_time) between 12 AND 17 THEN "Afternoon"
	WHEN HOUR(sale_time) between 17 AND 20 THEN "Evening"
    ELSE "Night"
    END AS Shifts,
    COUNT(transaction_id) as Total_sales
    FROM SALES
    GROUP BY Shifts
	ORDER BY HOUR(sale_time)

)
    SELECT * FROM HOURLEY_SALES
  ```
## Findings:
**Customer Demographics:**
The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
**Sales Trends:** Monthly analysis shows variations in sales, helping identify peak seasons.
**Customer Insights:** The analysis identifies the top-spending customers and the most popular product categories.

## Reports:
**Sales Summary:** A detailed report summarizing total sales, customer demographics, and category performance.
**Trend Analysis:** Insights into sales trends across different months and shifts.
**Customer Insights:** Reports on top customers and unique customer counts per category.

  


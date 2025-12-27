```sql
-- ==================================================
-- E-COMMERCE SALES DATA ANALYSIS
-- Author: Adeyinka Adeyelu
-- Description: SQL queries to analyze sales, revenue, profit,
-- customer demographics, product performance, and trends.
-- ==================================================

-- =========================
-- 1. VIEW THE DATA
-- =========================
SELECT *
FROM sales_data;

-- =========================
-- 2. TOTAL SALES & QUANTITY
-- =========================

-- Total number of products sold
SELECT SUM(quantity) AS total_products_sold
FROM sales_data;

-- Total number of products sold per product category
SELECT category, SUM(quantity) AS number_of_products_sold
FROM sales_data
GROUP BY category
ORDER BY number_of_products_sold DESC;

-- Total revenue
SELECT ROUND(SUM(total_amount),0) AS total_revenue
FROM sales_data;

-- Total revenue and number of products sold per category
SELECT category,
ROUND(SUM(total_amount),0) AS sales_amount_per_category,
SUM(quantity) AS number_of_products_sold
FROM sales_data
GROUP BY category
ORDER BY sales_amount_per_category DESC;

-- Total revenue and profit per category
SELECT category,
ROUND(SUM(total_amount),0) AS revenue_per_category,
ROUND(SUM(profit_margin),0) AS profit
FROM sales_data
GROUP BY category
ORDER BY profit DESC;

-- =========================
-- 3. TIME-BASED ANALYSIS
-- =========================

-- Years available in data
SELECT DISTINCT(YEAR(order_date)) AS sales_year
FROM sales_data;

-- Total revenue per year
SELECT YEAR(order_date) AS year,
ROUND(SUM(total_amount),0) AS sales_per_year
FROM sales_data
GROUP BY YEAR(order_date)
ORDER BY year DESC;

-- Monthly sales per category
SELECT FORMAT(TRY_CONVERT(date, order_date), 'yyyy-MM') AS sales_month,
category,
ROUND(SUM(total_amount),0) AS sales
FROM sales_data
GROUP BY FORMAT(TRY_CONVERT(date, order_date), 'yyyy-MM'), category
ORDER BY sales_month, sales DESC;

-- =========================
-- 4. CUSTOMER ANALYSIS
-- =========================

-- Preferred payment method by customer
SELECT payment_method, COUNT(payment_method) AS preferred_payment_method
FROM sales_data
GROUP BY payment_method
ORDER BY preferred_payment_method DESC;

-- Total number of customers per age group
WITH age_data AS (
SELECT CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END AS age_group
FROM sales_data
)
SELECT age_group,
COUNT(*) AS count_of_age_group
FROM age_data
GROUP BY age_group
ORDER BY count_of_age_group DESC;

-- Total revenue per age group
SELECT CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END AS age_group,
ROUND(SUM(total_amount),0) AS total_sales
FROM sales_data
GROUP BY CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END
ORDER BY age_group ASC;

-- Total number of products sold per age group
SELECT CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END AS age_group,
SUM(quantity) AS number_products_sold_per_age_group
FROM sales_data
GROUP BY CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END
ORDER BY age_group ASC;

-- Total number of products sold per age group by category
WITH age_groups AS (
SELECT CASE
WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
WHEN customer_age BETWEEN 25 AND 35 THEN '25-35'
WHEN customer_age BETWEEN 35 AND 49 THEN '35-49'
ELSE '49-69'
END AS age_group,
category,
quantity
FROM sales_data
)
SELECT age_group,
SUM(CASE WHEN category = 'Electronics' THEN quantity ELSE 0 END) AS Electronics,
SUM(CASE WHEN category = 'Fashion' THEN quantity ELSE 0 END) AS Fashion,
SUM(CASE WHEN category = 'Toys' THEN quantity ELSE 0 END) AS Toys,
SUM(CASE WHEN category = 'Grocery' THEN quantity ELSE 0 END) AS Grocery,
SUM(CASE WHEN category = 'Home' THEN quantity ELSE 0 END) AS Home,
SUM(CASE WHEN category = 'Sports' THEN quantity ELSE 0 END) AS Sports,
SUM(CASE WHEN category = 'Beauty' THEN quantity ELSE 0 END) AS Beauty
FROM age_groups
GROUP BY age_group
ORDER BY age_group DESC;

-- Total number of customers and products sold per gender
SELECT customer_gender,
COUNT(customer_gender) AS count_of_gender,
SUM(quantity) AS number_of_products_sold_per_gender
FROM sales_data
GROUP BY customer_gender;

-- Total number of products sold per gender by category
SELECT customer_gender,
SUM(CASE WHEN category = 'Electronics' THEN quantity ELSE 0 END) AS Electronics,
SUM(CASE WHEN category = 'Fashion' THEN quantity ELSE 0 END) AS Fashion,
SUM(CASE WHEN category = 'Toys' THEN quantity ELSE 0 END) AS Toys,
SUM(CASE WHEN category = 'Grocery' THEN quantity ELSE 0 END) AS Grocery,
SUM(CASE WHEN category = 'Home' THEN quantity ELSE 0 END) AS Home,
SUM(CASE WHEN category = 'Sports' THEN quantity ELSE 0 END) AS Sports,
SUM(CASE WHEN category = 'Beauty' THEN quantity ELSE 0 END) AS Beauty
FROM sales_data
GROUP BY customer_gender
ORDER BY customer_gender DESC;

-- =========================
-- 5. REGIONAL ANALYSIS
-- =========================

-- Total revenue per region
SELECT region, ROUND(SUM(total_amount), 0) AS total_sales_by_region
FROM sales_data
GROUP BY region
ORDER BY total_sales_by_region DESC;
```

This format will:

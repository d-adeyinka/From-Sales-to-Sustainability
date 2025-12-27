/********************************************************************
-- E-COMMERCE SALES DATA ANALYSIS
-- Author: Adeyinka Adeyelu
-- Description: SQL queries to analyze sales, revenue, profit, 
-- customer demographics, and product performance from the sales_data table.
-- This file is ready for GitHub and includes syntax highlighting comments.
********************************************************************/

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

-- Distinct years in the dataset
SELECT DISTINCT YEAR(order_date) AS year
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
           WHEN customer_ag_

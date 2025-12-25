# From-Sales-to-Sustainability: An E-commerce Profitability Analysis
This project uses Microsoft SQL Server to analyze a synthetic e-commerce dataset, exploring sales trends, customer behavior, product performance, returns, and profit margins. All SQL queries used for insights are included for reproducibility.

/* =====================================================
   E-COMMERCE SALES ANALYSIS
   From Sales to Sustainability: An E-commerce Profitability Analysis
   Database: Microsoft SQL Server
===================================================== */

-- ===============================
-- 1. DATA OVERVIEW
-- ===============================
SELECT *
FROM sales_data;

-- ===============================
-- 2. SALES PERFORMANCE ANALYSIS
-- ===============================

-- Total products sold
SELECT SUM(quantity) AS total_products_sold
FROM sales_data;

-- Revenue by product category
SELECT 
    category,
    ROUND(SUM(total_amount), 0) AS revenue_per_category
FROM sales_data
GROUP BY category
ORDER BY revenue_per_category DESC;

-- ===============================
-- 3. PROFITABILITY ANALYSIS
-- ===============================

-- Revenue and profit by category
SELECT 
    category,
    ROUND(SUM(total_amount), 0) AS revenue,
    ROUND(SUM(total_amount * profit_margin), 0) AS profit
FROM sales_data
GROUP BY category
ORDER BY profit DESC;

-- ===============================
-- 4. CUSTOMER DEMOGRAPHICS
-- ===============================

-- Sales by age group
WITH age_groups AS (
    SELECT 
        CASE 
            WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
            WHEN customer_age BETWEEN 26 AND 35 THEN '26-35'
            WHEN customer_age BETWEEN 36 AND 49 THEN '36-49'
            ELSE '50+'
        END AS age_group,
        total_amount
    FROM sales_data
)
SELECT 
    age_group,
    ROUND(SUM(total_amount), 0) AS total_sales
FROM age_groups
GROUP BY age_group
ORDER BY total_sales DESC;

-- ===============================
-- 5. RETURNS & OPERATIONS
-- ===============================

-- Return rate
SELECT 
    CAST(SUM(CASE WHEN returned = 1 THEN 1 ELSE 0 END) AS FLOAT) 
    / COUNT(*) * 100 AS return_rate
FROM sales_data;

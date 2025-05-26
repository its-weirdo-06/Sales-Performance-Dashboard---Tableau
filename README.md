Sales Performance Dashboard – Tableau
Description:
Designed an interactive Sales Performance Dashboard in Tableau to analyze and visualize key sales metrics across various customer segments, product categories, and regions. The dashboard enables data-driven decision-making by providing actionable insights into sales trends, profit distribution, and order behavior over time.

Key Features:
Sales KPIs including Total Sales, Total Profit, Avg. Discount, Avg. Order Quantity, and Avg. Delivery Time

Sales breakdown by Customer Segment and Product Category

Regional order distribution by product category

Profitability analysis using bin histograms and scatter plots

Customer Profit vs Sales visual correlation

Interactive filters for drilling down into time periods, segments, or categories

Key Tables:
orders

customers

products

regions

sales

Script (Table Definitions in SQL):
sql
Copy
Edit
-- Create table for customer details
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_segment VARCHAR(50),
    province VARCHAR(50),
    region VARCHAR(50)
);

-- Create table for product information
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    product_category VARCHAR(50),
    product_sub_category VARCHAR(50),
    product_container VARCHAR(50),
    product_base_margin DECIMAL(5,2)
);

-- Create table for sales orders
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    row_id INT,
    order_date DATE,
    ship_date DATE,
    order_priority VARCHAR(20),
    ship_mode VARCHAR(50),
    customer_id INT,
    product_id INT,
    order_quantity INT,
    unit_price DECIMAL(10,2),
    discount DECIMAL(5,2),
    shipping_cost DECIMAL(10,2),
    sales DECIMAL(12,2),
    profit DECIMAL(12,2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
Sample Queries:
sql
Copy
Edit
-- Total Sales and Profit
SELECT 
    SUM(sales) AS total_sales, 
    SUM(profit) AS total_profit 
FROM orders;

-- Average Discount and Order Quantity
SELECT 
    AVG(discount) AS avg_discount,
    AVG(order_quantity) AS avg_order_quantity
FROM orders;

-- Sales by Customer Segment and Product Category
SELECT 
    c.customer_segment, 
    p.product_category, 
    SUM(o.sales) AS total_sales 
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
GROUP BY c.customer_segment, p.product_category;

-- Orders by Region and Category
SELECT 
    c.region, 
    p.product_category, 
    COUNT(*) AS order_count 
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
GROUP BY c.region, p.product_category;

-- Profit Binned in Ranges (1k bins)
SELECT 
    FLOOR(profit / 1000) * 1000 AS profit_bin, 
    COUNT(*) AS order_count 
FROM orders 
GROUP BY FLOOR(profit / 1000) * 1000 
ORDER BY profit_bin;

![image](https://github.com/its-weirdo-06/Sales-Performance-Dashboard---Tableau/assets/83410561/163a5f4e-a016-4878-a71b-963bc325de2c)

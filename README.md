# Northwind Database SQL Analysis

![Northwind ER Diagram](https://github.com/yourusername/yourrepo/raw/main/northwind_er_diagram.png) *(Optional: Add ER diagram image if available)*

## Project Overview
This repository contains SQL queries analyzing the Northwind Traders database - a sample database containing sales data for a fictitious specialty foods export/import company. The workbook covers customer analysis, product performance, order patterns, and business operations metrics.

## Key Analysis Areas

### 1. Customer Insights
```sql
-- Find customers by name patterns
SELECT * FROM customers WHERE CustomerName LIKE 'a%';
SELECT * FROM customers WHERE CustomerName LIKE '%a';
SELECT * FROM customers WHERE CustomerName LIKE "Q_e%";

-- Customers without orders
SELECT a.*, b.*
FROM customers AS a
LEFT JOIN orders AS b ON a.customerId = b.customerId
WHERE b.orderid IS NULL;

-- Customer distribution by country
SELECT COUNT(*) 'Customers#', country
FROM customers
GROUP BY country
ORDER BY 'Customers#' DESC;

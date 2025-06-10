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

-- Products never ordered
SELECT *
FROM products AS p
LEFT JOIN order_details AS od ON p.productid = od.productid
WHERE od.orderid IS NULL;

-- Product category mapping
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID;

-- Meat/Poultry products
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID
WHERE c.CategoryName='Meat/Poultry';

-- Top selling products by revenue
SELECT 
    p.ProductName, 
    SUM(od.quantity * p.price) AS sales
FROM products AS p
INNER JOIN order_details AS od ON p.productid = od.productid
GROUP BY p.ProductName
ORDER BY sales DESC;

-- Category-wise sales volume
SELECT 
    c.CategoryName, 
    COUNT(CategoryName) AS 'Quantity sold'
FROM products AS p
INNER JOIN categories AS c ON p.CategoryID = c.CategoryID
GROUP BY CategoryName;

-- Employee order handling stats
SELECT 
    e.EmployeeID, 
    e.FirstName,
    COUNT(*) AS 'TotalOrders'
FROM employees e
JOIN orders o ON e.EmployeeID = o.EmployeeID
GROUP BY e.EmployeeID
ORDER BY e.EmployeeID;

-- Shipper performance
SELECT 
    Shippers.ShipperName, 
    COUNT(Orders.OrderID) AS NumberOfOrders 
FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;

-- Tokyo Traders products
SELECT *
FROM products
INNER JOIN suppliers ON products.supplierid = suppliers.supplierid
WHERE suppliername ='Tokyo Traders'
ORDER BY products.productname;

-- Supplier-product mapping
SELECT DISTINCT s.SupplierName, p.ProductName
FROM suppliers s
JOIN products p ON s.SupplierID = p.SupplierID;


-- 1996 order analysis
SELECT 
    o.OrderID,
    o.OrderDate,
    c.CustomerID,
    c.CustomerName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
WHERE YEAR(o.OrderDate) = 1996;

-- Complete order details view
SELECT 
    o.OrderID, 
    o.OrderDate,
    c.CustomerName,
    CONCAT(e.FirstName,' ',e.LastName) AS EmployeeName,
    p.ProductName,
    od.Quantity,
    (od.Quantity * p.Price) AS LineTotal
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
JOIN employees e ON o.EmployeeID = e.EmployeeID
JOIN order_details od ON o.OrderID = od.OrderID
JOIN products p ON od.ProductID = p.ProductID;

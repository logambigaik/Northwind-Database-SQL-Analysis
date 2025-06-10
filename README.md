# Northwind Database SQL Script Collection

## 🔍 Basic Queries
USE northwind;
DESC categories;
DESC customers;

text

## 🔎 Filtering & Pattern Matching
-- Customers starting with 'A'
SELECT * FROM customers WHERE CustomerName LIKE 'a%';

-- Customers ending with 'a'
SELECT * FROM customers WHERE CustomerName LIKE '%a';

-- Pattern: Q followed by any char then 'e'
SELECT * FROM customers WHERE CustomerName LIKE "Q_e%";

text

## 🤝 Joins & Relationships
-- Tokyo Traders products
SELECT p.*
FROM products p
INNER JOIN suppliers s ON p.supplierid = s.supplierid
WHERE s.suppliername = 'Tokyo Traders'
ORDER BY p.productname;

-- Customers without orders
SELECT c., o.
FROM customers c
LEFT JOIN orders o ON c.customerId = o.customerId
WHERE o.orderid IS NULL;

-- Full customer-order relationship (UNION implementation)
SELECT * FROM customers
LEFT JOIN orders USING(CustomerID)
WHERE orders.CustomerID IS NOT NULL
UNION
SELECT * FROM customers
RIGHT JOIN orders USING(CustomerID)
WHERE customers.CustomerID IS NOT NULL;

text

## 📊 Aggregation & Grouping
-- Country-city distribution
SELECT
COUNT(DISTINCT city) AS 'UniqueCities',
country
FROM customers
GROUP BY Country;

-- Shipper performance
SELECT
s.ShipperName,
COUNT(o.OrderID) AS OrderCount
FROM Orders o
LEFT JOIN Shippers s USING(ShipperID)
GROUP BY ShipperName;

-- Product sales analysis
SELECT
p.ProductName,
SUM(od.quantity) AS TotalUnits,
SUM(od.quantity * p.price) AS TotalRevenue
FROM products p
JOIN order_details od USING(ProductID)
GROUP BY p.ProductName
ORDER BY TotalRevenue DESC;

text

## 📈 Business Reports
-- Customer geographic distribution
SELECT
COUNT(*) AS CustomerCount,
country
FROM customers
GROUP BY country
ORDER BY CustomerCount DESC;

-- Category performance
SELECT
c.CategoryName,
COUNT(p.ProductID) AS ProductCount
FROM categories c
LEFT JOIN products p USING(CategoryID)
GROUP BY c.CategoryName;

-- Employee order handling
SELECT
e.EmployeeID,
CONCAT(e.FirstName, ' ', e.LastName) AS Name,
COUNT(o.OrderID) AS HandledOrders
FROM employees e
JOIN orders o USING(EmployeeID)
GROUP BY e.EmployeeID;

text

## 🔗 Relationship Exploration
-- Product-supplier catalog
SELECT DISTINCT
s.SupplierName,
p.ProductName
FROM suppliers s
JOIN products p USING(SupplierID);

-- Complete product info
SELECT
p.ProductName,
c.CategoryName,
s.SupplierName
FROM products p
JOIN categories c USING(CategoryID)
JOIN suppliers s USING(SupplierID);

-- Order fulfillment details
SELECT
o.OrderID,
o.OrderDate,
c.CustomerName,
CONCAT(e.FirstName, ' ', e.LastName) AS Handler
FROM orders o
JOIN customers c USING(CustomerID)
JOIN employees e USING(EmployeeID);

text

## 💡 Key Analysis Highlights
1. **Sales Performance** - Identify top-selling products by revenue
2. **Customer Distribution** - Geographic spread analysis by country
3. **Inventory Management** - Products never ordered (left joins with NULL check)
4. **Employee Productivity** - Order handling metrics per staff member
5. **Supplier Impact** - Product distribution across suppliers

✨ Tip: Use `EXPLAIN` before queries to analyze execution plans for optimization!

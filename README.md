# Northwind Database Analysis

## Database Exploration
First, I examined the structure of two key tables:
- **categories** - Contains product category information
- **customers** - Stores customer data including names and contact details
```sql
USE northwind;
DESC categories;
DESC customers;
```

## 🔎 Filtering & Pattern Matching
These queries demonstrate how to find specific customer patterns:

- **Customers whose names start with 'A'**
```sql
SELECT * 
FROM customers
WHERE CustomerName LIKE 'a%';
```

- **Customers whose names end with 'a'**
```sql
SELECT * 
FROM customers
WHERE CustomerName LIKE '%a';
```

- **Customers whose names match a specific pattern: 'Q_e%'**
```sql
SELECT *
FROM customers 
WHERE CustomerName LIKE 'Q_e%';
```

## 🔗 Join Operations

- **Find products supplied by "Tokyo Traders"**
```sql
SELECT *
FROM products
INNER JOIN suppliers ON products.SupplierID = suppliers.SupplierID
WHERE SupplierName = 'Tokyo Traders'
ORDER BY products.ProductName;
```

- **List of all Product IDs**
```sql
SELECT ProductID FROM products;
```

- **All product details**
```sql
SELECT * FROM products;
```

- **Customers who have never placed an order**
```sql
SELECT a.*, b.*
FROM customers AS a
LEFT JOIN orders AS b ON a.CustomerID = b.CustomerID
WHERE b.OrderID IS NULL;
```

- **Products that have never been ordered**
```sql
SELECT *
FROM products AS p
LEFT JOIN order_details AS od ON p.ProductID = od.ProductID
WHERE od.OrderID IS NULL;
```

- **Cross join: Customer and Employee combinations for a specific customer**
```sql
SELECT * 
FROM customers
CROSS JOIN employees
WHERE customers.CustomerName = 'Alfreds Futterkiste';
```

- **All customers and their orders using LEFT and RIGHT JOIN with UNION**
```sql
SELECT *
FROM customers
LEFT JOIN orders ON customers.CustomerID = orders.CustomerID
WHERE orders.CustomerID IS NOT NULL

UNION

SELECT *
FROM customers
RIGHT JOIN orders ON customers.CustomerID = orders.CustomerID
WHERE customers.CustomerID IS NOT NULL;
```

## 📊 Aggregations & Grouping

- **Count number of cities in each country (filtering for 'uk')**
```sql
SELECT COUNT(DISTINCT City) AS 'Number of customer', Country, City 
FROM customers
GROUP BY Country, City
HAVING City = 'uk';
```

- **Orders count by Shipper**
```sql
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders 
FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;
```

- **Total sales amount by each product (quantity × price)**
```sql
SELECT 
    a.ProductName, 
    a.Price, 
    SUM(b.Quantity) AS total_quantity, 
    SUM(b.Quantity * a.Price) AS TotalSales
FROM products a
JOIN order_details b ON a.ProductID = b.ProductID
GROUP BY a.ProductName, a.Price
ORDER BY TotalSales DESC;
```

- **Alternate method for total product sales**
```sql
SELECT p.ProductName, SUM(od.Quantity * p.Price) AS Sales
FROM products AS p
INNER JOIN order_details AS od ON p.ProductID = od.ProductID
GROUP BY p.ProductName;
```

- **Number of customers in each country**
```sql
SELECT COUNT(*) AS 'Customers#', Country
FROM customers
GROUP BY Country
ORDER BY 'Customers#' DESC;
```

- **Total quantity sold per product**
```sql
SELECT SUM(o.Quantity) AS 'TotalQuantity', p.ProductName
FROM products p
JOIN order_details o ON p.ProductID = o.ProductID
GROUP BY p.ProductName
ORDER BY TotalQuantity DESC;
```

- **Total quantity sold by category**
```sql
SELECT c.CategoryName, COUNT(CategoryName) AS 'Quantity sold'
FROM products AS p
INNER JOIN categories AS c ON p.CategoryID = c.CategoryID
GROUP BY CategoryName;
```

- **Number of orders handled by each employee**
```sql
SELECT e.EmployeeID, e.FirstName, COUNT(*) AS 'TotalOrders'
FROM employees e
JOIN orders o ON e.EmployeeID = o.EmployeeID
GROUP BY e.EmployeeID
ORDER BY e.EmployeeID;
```

## 🧾 Entity Details & Relations

- **List supplier and product combinations**
```sql
SELECT DISTINCT s.SupplierName, p.ProductName
FROM suppliers s
JOIN products p ON s.SupplierID = p.SupplierID;
```

- **Find category of each product**
```sql
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID;
```

- **All products in the "Meat/Poultry" category**
```sql
SELECT p.ProductName, c.CategoryName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID
WHERE c.CategoryName = 'Meat/Poultry';
```

- **Order info: Order ID, date, customer name, and employee name**
```sql
SELECT o.OrderID, o.OrderDate, c.CustomerName, CONCAT(e.FirstName, ' ', e.LastName) AS EmployeeName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
JOIN employees e ON o.EmployeeID = e.EmployeeID;
```

- **Product name, category, and supplier for all products**
```sql
SELECT p.ProductName, c.CategoryName, s.SupplierName
FROM products p
JOIN categories c ON p.CategoryID = c.CategoryID
JOIN suppliers s ON p.SupplierID = s.SupplierID;
```

- **Orders from the year 1996**
```sql
SELECT o.OrderID, o.OrderDate, c.CustomerID, c.CustomerName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
WHERE YEAR(o.OrderDate) = 1996;
```

- **Category-wise product counts**
```sql
SELECT c.CategoryName, COUNT(p.ProductName) AS NoOfProducts
FROM categories c
LEFT JOIN products p ON p.CategoryID = c.CategoryID
GROUP BY c.CategoryName
ORDER BY NoOfProducts;
```

- **Product prices and total quantity ordered**
```sql
SELECT 
    p.ProductName, 
    SUM(o.Quantity) AS QtyOrdered,          
    SUM(o.Quantity * p.Price) AS TotalPrice
FROM products p 
JOIN order_details o ON o.ProductID = p.ProductID
GROUP BY p.ProductName
ORDER BY TotalPrice DESC;
```

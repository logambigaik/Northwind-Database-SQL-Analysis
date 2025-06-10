# Northwind Database Analysis

## Database Exploration
First, I examined the structure of two key tables:
- **categories** - Contains product category information
- **customers** - Stores customer data including names and contact details

## 🔎 Filtering & Pattern Matching
These queries demonstrate how to find specific customer patterns:
- Customers starting with 'A' (`LIKE 'a%'`)
- Customers ending with 'a' (`LIKE '%a'`)
- Customers with names matching "Q followed by any character then 'e'" (`LIKE "Q_e%"`)

## 🤝 Joins & Relationships
Key relationship queries:
- **Supplier-Product Connection**: Finds all products supplied by 'Tokyo Traders'
- **Customer-Order Analysis**: Identifies customers who haven't placed any orders (using `LEFT JOIN` with `NULL` check)
- **Complete Customer-Order Relationship**: Uses `UNION` to combine `LEFT` and `RIGHT` joins for full relationship mapping

## 📊 Aggregation & Grouping
Important analytical queries:
- **Geographic Distribution**: Counts unique cities per country for customers
- **Shipping Performance**: Shows order counts handled by each shipper
- **Product Sales**: Calculates total units sold and revenue per product (sorted by revenue)

## 📈 Business Reports
Valuable business insights:
- **Customer Geography**: Shows customer counts by country (sorted high to low)
- **Category Performance**: Counts products in each category
- **Employee Productivity**: Tracks how many orders each employee has handled

## 🔗 Relationship Exploration
Relationship mapping queries:
- **Supplier-Product Catalog**: Shows which suppliers provide which products
- **Complete Product Info**: Combines product names with their categories and suppliers
- **Order Fulfillment**: Details orders with customer and employee information

## 💡 Key Analysis Highlights
The analysis reveals several critical business insights:
- **Sales Performance**: Identify top revenue-generating products
- **Customer Distribution**: Understand geographic customer concentrations
- **Inventory Management**: Find products never ordered
- **Employee Productivity**: Measure order handling efficiency
- **Supplier Impact**: Analyze product distribution across suppliers

> **Pro Tip**: Use `EXPLAIN` before queries to analyze execution plans for performance optimization! This helps identify potential bottlenecks in complex queries.

These queries provide a comprehensive view of the Northwind database, covering filtering, joins, aggregations, and business reporting - essential for making data-driven decisions.

# 🛍️ Customer & Sales Insights for a Retail Company Using SQL

## 📘 Project Overview

This project presents a SQL-based data analysis of a retail company's customer and sales data. The goal is to extract actionable insights about customer behavior, sales performance, and profitability using structured queries. The dataset includes 500 records across customer and order tables.

---

## 🛠 Technologies Used

- PostgreSQL
- SQL
- pgAdmin (optional)
- Git / GitHub

---

## 🧾 Database Setup

Two tables are used in this project:

### 1. **Customers Table**
| Column        | Description     |
|---------------|-----------------|
| customer_id   | Unique identifier |
| customer_name | Customer's full name |
| region        | Customer region (East, West, North, South) |

### 2. **Orders Table**
| Column        | Data Type    |
|---------------|--------------|
| order_id      | VARCHAR      |
| customer_id   | VARCHAR      |
| product_id    | VARCHAR      |
| sales         | NUMERIC      |
| profit        | NUMERIC      |
| discount      | NUMERIC      |
| region        | VARCHAR      |
| order_date    | DATE         |

---

## 🎯 Objective

To help the retail company:
- Understand customer behavior
- Analyze sales and profit trends
- Identify growth opportunities

---

## 🔎 Key SQL Queries & Insights

### 🔍 Basic Exploration

- **Total Orders**
  ```sql
  SELECT COUNT(order_id) FROM orders;
✅ 500 total orders

Distinct Customers

sql
Copy
Edit
SELECT COUNT(DISTINCT customer_id) FROM customers;
✅ 500 unique customers

Data Types
All 8 columns in orders: 3 numeric, 4 varchar, 1 date

🧮 SELECT, Filtering & Sorting
Top 5 Highest Sales

sql
Copy
Edit
SELECT * FROM orders ORDER BY sales DESC LIMIT 5;
🔝 Max sale: ₹998.14

Discount > 0.2

sql
Copy
Edit
SELECT * FROM orders WHERE discount > 0.2;
🎯 167 records found

Sorted by Profit

sql
Copy
Edit
SELECT * FROM orders ORDER BY profit DESC;
💰 Highest: ₹299.79 | Lowest: ₹-48.27

📊 Aggregations & GROUP BY
Sales and Profit by Region

sql
Copy
Edit
SELECT region, SUM(sales), SUM(profit) FROM orders GROUP BY region;
🌍 East leads with ₹78,495.46 in sales and ₹18,567.66 in profit

Orders per Customer

sql
Copy
Edit
SELECT customer_id, COUNT(*) FROM orders GROUP BY customer_id;
🧑 Max orders by a single customer: 6

🔗 JOINS Exploration
Customer Name, Region & Total Orders

sql
Copy
Edit
SELECT c.customer_name, c.region, COUNT(o.order_id)
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name, c.region;
Profit per Customer

sql
Copy
Edit
SELECT c.customer_name, SUM(o.profit)
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
Customers with No Orders

sql
Copy
Edit
SELECT c.customer_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
🚫 189 customers had no orders

🔁 Subqueries
Customers with Sales > Avg

sql
Copy
Edit
SELECT customer_id
FROM orders
GROUP BY customer_id
HAVING SUM(sales) > (SELECT AVG(sales) FROM orders);
📈 209 customers exceed average sales

Orders with Profit > Region Avg

sql
Copy
Edit
SELECT * FROM orders o
WHERE profit > (
    SELECT AVG(profit)
    FROM orders
    WHERE region = o.region
);
🏆 254 high-profit orders found

Top 3 Profitable Customers

sql
Copy
Edit
SELECT customer_id, SUM(profit) AS total_profit
FROM orders
GROUP BY customer_id
ORDER BY total_profit DESC
LIMIT 3;
🥇 Highest profit: ₹901.88

📐 Set Operations
Common Customers in East & West

sql
Copy
Edit
SELECT customer_id FROM customers WHERE region = 'East'
INTERSECT
SELECT customer_id FROM customers WHERE region = 'West';
❌ 0 common customers

East Customers Not in West

sql
Copy
Edit
SELECT customer_id FROM customers WHERE region = 'East'
EXCEPT
SELECT customer_id FROM customers WHERE region = 'West';
✅ Also returned 0 (exclusive sets)

🧠 Final Insights & Recommendations
🧍 Customer Behavior
311 of 500 customers placed orders → high engagement

Many customers are inactive (useful for targeted marketing)

💸 Sales Strategy
Moderate discounts (10–20%) are most profitable

Focus on repeat buyers (top customers)

📈 Growth Opportunities
Engage 189 inactive users (from LEFT JOIN)

Analyze by sub-category in future datasets


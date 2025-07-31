# Pizza_Sales_Analysis_By_SQL

# 🍕 Pizza Sales SQL Analysis

This project contains a collection of SQL queries used to analyze pizza sales data. It includes basic, intermediate, and advanced queries that demonstrate various data manipulation and reporting techniques useful in real-world business intelligence scenarios.

---

## 📋 Basic Queries

1. **Total number of orders placed**
```sql
SELECT COUNT(*) AS total_orders FROM orders;

Total revenue generated from pizza sales
sql

SELECT SUM(order_details.quantity * pizzas.price) AS total_revenue 
FROM order_details 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id;
Highest-priced pizza

sql

SELECT name, price 
FROM pizzas 
ORDER BY price DESC 
LIMIT 1;
Most common pizza size ordered

sql

SELECT size, COUNT(*) AS count 
FROM pizzas 
JOIN order_details ON pizzas.pizza_id = order_details.pizza_id 
GROUP BY size 
ORDER BY count DESC 
LIMIT 1;
Top 5 most ordered pizza types

sql

SELECT pizzas.name, SUM(order_details.quantity) AS total_quantity 
FROM order_details 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id 
GROUP BY pizzas.name 
ORDER BY total_quantity DESC 
LIMIT 5;


🧩 Intermediate Queries

Total quantity of each pizza category ordered
sql
SELECT pizza_types.category, SUM(order_details.quantity) AS total_quantity 
FROM order_details 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id 
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id 
GROUP BY pizza_types.category;

Order distribution by hour of the day
sql
SELECT EXTRACT(HOUR FROM order_time) AS hour, COUNT(*) AS order_count 
FROM orders 
GROUP BY hour 
ORDER BY hour;
Category-wise distribution of pizzas

sql

SELECT pizza_types.category, COUNT(DISTINCT pizzas.pizza_id) AS pizza_count 
FROM pizzas 
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id 
GROUP BY pizza_types.category;
Average number of pizzas ordered per day

sql

SELECT order_date, AVG(order_details.quantity) AS avg_pizzas_per_order 
FROM orders 
JOIN order_details ON orders.order_id = order_details.order_id 
GROUP BY order_date;
Top 3 pizza types by revenue

sql

SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue 
FROM order_details 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id 
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id 
GROUP BY pizza_types.name 
ORDER BY revenue DESC 
LIMIT 3;


🔍 Advanced Queries
Percentage contribution of each pizza type to total revenue
sql

SELECT pizza_types.name, 
ROUND(SUM(order_details.quantity * pizzas.price) * 100.0 / 
(SELECT SUM(order_details.quantity * pizzas.price) 
 FROM order_details 
 JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id), 2) AS revenue_percentage 
FROM order_details 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id 
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id 
GROUP BY pizza_types.name 
ORDER BY revenue_percentage DESC;
Cumulative revenue over time

sql

SELECT order_date, 
SUM(order_details.quantity * pizzas.price) OVER (ORDER BY order_date) AS cumulative_revenue 
FROM orders 
JOIN order_details ON orders.order_id = order_details.order_id 
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id;
Top 3 pizza types by revenue for each category

sql

SELECT category, name, revenue 
FROM (
    SELECT pizza_types.category, pizza_types.name, 
           SUM(order_details.quantity * pizzas.price) AS revenue, 
           ROW_NUMBER() OVER (PARTITION BY pizza_types.category 
                              ORDER BY SUM(order_details.quantity * pizzas.price) DESC) AS rank 
    FROM order_details 
    JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id 
    JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id 
    GROUP BY pizza_types.category, pizza_types.name
) AS ranked 
WHERE rank <= 3;


📁 Project Structure
README.md: This file.

SQL scripts or query files (optional): Can be separated and organized by complexity.

📌 Notes
This analysis assumes a relational schema with at least the following tables:

orders

order_details

pizzas

pizza_types

🧠 Learnings
This project demonstrates:

Data aggregation using SUM, COUNT, AVG

Joins between related tables

Use of window functions (ROW_NUMBER, OVER)

Grouping and filtering data insights

✅ License
This project is open for educational use. Feel free to fork and modify!

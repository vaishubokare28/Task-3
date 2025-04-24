# 📊 Task 3: SQL for Data Analysis

## 🎯 Objective

Use **SQL queries** to extract, manipulate, and analyze structured data from an **Ecommerce database** using **MySQL**.

---

## 🛠️ Tools Used

- **Database**: MySQL 
- **Dataset**: `Ecommerce_SQL_Database` (self-created schema)

---

## 🗃️ Dataset Overview

### Tables
- `customers` — Customer details
- `categories` — Product categories
- `products` — Product info
- `orders` — Customer orders
- `order_items` — Items in each order
- `payments` — Payments for orders

---

## 🔍 Queries Overview

### a. **SELECT, WHERE, ORDER BY, GROUP BY**

```sql
-- Customers from USA, sorted by name
SELECT customer_name, country
FROM customers
WHERE country = 'USA'
ORDER BY customer_name;

-- Group: Total orders by country
SELECT c.country, COUNT(o.order_id) AS total_orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.country
ORDER BY total_orders DESC;
```

---

### b. **JOINS (INNER, LEFT, RIGHT)**

```sql
-- INNER JOIN: Orders with customer names
SELECT o.order_id, c.customer_name, o.order_date
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;

-- LEFT JOIN: Products and categories
SELECT p.product_name, c.category_name
FROM products p
LEFT JOIN categories c ON p.category_id = c.category_id;

-- RIGHT JOIN Simulation (MySQL workaround)
SELECT c.category_name, p.product_name
FROM categories c
LEFT JOIN products p ON p.category_id = c.category_id;
```

---

### c. **Subqueries**

```sql
-- Customers who spent above average
SELECT customer_id, customer_name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    GROUP BY customer_id
    HAVING SUM(oi.quantity * oi.unit_price) > (
        SELECT AVG(total_spent)
        FROM (
            SELECT SUM(oi.quantity * oi.unit_price) AS total_spent
            FROM orders o
            JOIN order_items oi ON o.order_id = oi.order_id
            GROUP BY o.customer_id
        ) AS spending
    )
);
```

---

### d. **Aggregate Functions (SUM, AVG)**

```sql
-- Total revenue per product
SELECT p.product_name, SUM(oi.quantity * oi.unit_price) AS revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_name;

-- Average customer spending
SELECT c.customer_name, AVG(oi.quantity * oi.unit_price) AS avg_spending
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_name;
```

---

### e. **Create Views for Analysis**

```sql
-- View: Total spend per customer
CREATE VIEW customer_total_spend AS
SELECT c.customer_id, c.customer_name, SUM(oi.quantity * oi.unit_price) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_id;
```

---

### f. **Optimize Queries with Indexes**

```sql
-- Indexes for performance
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_products_category_id ON products(category_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

---

## 📘 Outcome

By completing this task, I have:
- Learned how to extract, filter, and join relational data
- Practiced query performance optimization via indexing
- Created analytical views and subqueries
- Strengthened SQL skills essential for data analysis roles

---


# 9. Join Tables

## Apa itu JOIN?

JOIN digunakan untuk menggabungkan data dari dua atau lebih tabel berdasarkan kolom yang berelasi.

## Setup Contoh Tabel

```sql
-- Tabel customers
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

-- Tabel orders
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    order_date DATE,
    total_amount DECIMAL(10,2)
);

-- Insert sample data
INSERT INTO customers (name, email) VALUES
('John Doe', 'john@example.com'),
('Jane Smith', 'jane@example.com'),
('Bob Johnson', 'bob@example.com');

INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, '2025-01-10', 150.00),
(1, '2025-01-15', 200.00),
(2, '2025-01-12', 175.00);
```

---

## INNER JOIN

Mengembalikan rows yang memiliki matching value di kedua tabel.

### Syntax

```sql
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

### Contoh

```sql
SELECT 
    c.name,
    c.email,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

**Hasil:**

```
    name     |       email       | order_id | order_date | total_amount
-------------+-------------------+----------+------------+--------------
 John Doe    | john@example.com  |        1 | 2025-01-10 |       150.00
 John Doe    | john@example.com  |        2 | 2025-01-15 |       200.00
 Jane Smith  | jane@example.com  |        3 | 2025-01-12 |       175.00
```

**Catatan:** Bob Johnson tidak muncul karena tidak punya order.

---

## LEFT JOIN (LEFT OUTER JOIN)

Mengembalikan semua rows dari tabel kiri dan matching rows dari tabel kanan. Jika tidak ada match, NULL untuk kolom kanan.

### Syntax

```sql
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

### Contoh

```sql
SELECT 
    c.name,
    c.email,
    o.order_id,
    o.total_amount
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

**Hasil:**

```
    name     |       email       | order_id | total_amount
-------------+-------------------+----------+--------------
 John Doe    | john@example.com  |        1 |       150.00
 John Doe    | john@example.com  |        2 |       200.00
 Jane Smith  | jane@example.com  |        3 |       175.00
 Bob Johnson | bob@example.com   |     NULL |         NULL
```

### Find Customers Without Orders

```sql
SELECT 
    c.customer_id,
    c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

---

## RIGHT JOIN (RIGHT OUTER JOIN)

Mengembalikan semua rows dari tabel kanan dan matching rows dari tabel kiri.

### Syntax

```sql
SELECT columns
FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```

### Contoh

```sql
SELECT 
    c.name,
    o.order_id,
    o.total_amount
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

**Catatan:** Jarang digunakan karena bisa diubah jadi LEFT JOIN dengan swap table.

---

## FULL OUTER JOIN

Mengembalikan semua rows dari kedua tabel. NULL untuk yang tidak match.

### Syntax

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2 ON table1.column = table2.column;
```

### Contoh

```sql
SELECT 
    c.name,
    o.order_id
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

---

## CROSS JOIN

Cartesian product: setiap row di table1 dengan setiap row di table2.

### Syntax

```sql
SELECT columns
FROM table1
CROSS JOIN table2;
```

### Contoh

```sql
SELECT 
    c.name,
    p.product_name
FROM customers c
CROSS JOIN products p;
```

**Catatan:** Hati-hati! CROSS JOIN menghasilkan rows = table1_rows × table2_rows.

---

## SELF JOIN

Join tabel dengan dirinya sendiri.

### Contoh: Employee dan Manager

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    manager_id INTEGER
);

SELECT 
    e.name AS employee_name,
    m.name AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;
```

---

## Multiple JOINs

Join lebih dari 2 tabel.

### Contoh: Customers, Orders, Order Items, Products

```sql
SELECT 
    c.name AS customer_name,
    o.order_id,
    o.order_date,
    p.product_name,
    oi.quantity,
    oi.price
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id;
```

### Complex Example

```sql
SELECT 
    c.name AS customer_name,
    c.email,
    o.order_id,
    o.order_date,
    p.product_name,
    cat.category_name,
    oi.quantity,
    oi.price,
    oi.quantity * oi.price AS subtotal
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories cat ON p.category_id = cat.category_id
WHERE o.order_date >= '2025-01-01'
ORDER BY o.order_date DESC;
```

---

## JOIN dengan WHERE

Kombinasi JOIN dan filtering.

```sql
SELECT 
    c.name,
    o.order_id,
    o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 100
  AND o.order_date >= '2025-01-01';
```

---

## JOIN dengan Aggregation

```sql
-- Total orders per customer
SELECT 
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS total_orders,
    COALESCE(SUM(o.total_amount), 0) AS total_spent
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spent DESC;
```

---

## USING Clause

Shortcut untuk join jika kolom sama nama.

```sql
-- Dengan ON
SELECT * 
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;

-- Dengan USING
SELECT * 
FROM customers
JOIN orders USING (customer_id);
```

---

## NATURAL JOIN

Automatic join berdasarkan kolom dengan nama sama.

```sql
SELECT *
FROM customers
NATURAL JOIN orders;
```

**Catatan:** Tidak recommended karena implicit dan bisa error jika ada kolom dengan nama sama tapi berbeda meaning.

---

## JOIN Performance Tips

### 1. Index Foreign Keys

```sql
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

### 2. Select Only Needed Columns

```sql
-- Good
SELECT c.name, o.order_id
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;

-- Avoid
SELECT *
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### 3. Filter Early

```sql
-- Good: Filter before join if possible
SELECT c.name, o.order_id
FROM customers c
JOIN (
    SELECT * FROM orders WHERE order_date >= '2025-01-01'
) o ON c.customer_id = o.customer_id;
```

---

## Common Patterns

### Get Latest Order per Customer

```sql
SELECT DISTINCT ON (c.customer_id)
    c.customer_id,
    c.name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id, o.order_date DESC;
```

### Get Top N Items per Category

```sql
SELECT *
FROM (
    SELECT 
        p.*,
        ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY price DESC) as rank
    FROM products p
) ranked
WHERE rank <= 5;
```

### Find Orphan Records

```sql
-- Products without any orders
SELECT p.*
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.order_item_id IS NULL;
```

---

## Subquery vs JOIN

Kapan menggunakan subquery vs JOIN?

### Use JOIN:
- Ketika butuh columns dari multiple tables
- Better performance untuk large datasets
- More readable untuk complex queries

```sql
-- JOIN
SELECT c.name, o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### Use Subquery:
- Ketika butuh aggregation dulu
- One-time filtering
- Clearer logic untuk some cases

```sql
-- Subquery
SELECT name
FROM customers
WHERE customer_id IN (
    SELECT customer_id FROM orders WHERE total_amount > 1000
);
```

---

## Contoh Praktis E-Commerce

### Customer Order Summary

```sql
SELECT 
    c.customer_id,
    c.name,
    c.email,
    COUNT(DISTINCT o.order_id) AS total_orders,
    COUNT(oi.order_item_id) AS total_items,
    COALESCE(SUM(o.total_amount), 0) AS total_spent,
    MAX(o.order_date) AS last_order_date,
    CASE 
        WHEN MAX(o.order_date) >= CURRENT_DATE - INTERVAL '30 days' THEN 'Active'
        WHEN MAX(o.order_date) >= CURRENT_DATE - INTERVAL '90 days' THEN 'Inactive'
        ELSE 'Dormant'
    END AS customer_status
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
LEFT JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_id, c.name, c.email
ORDER BY total_spent DESC;
```

### Product Sales Report

```sql
SELECT 
    p.product_id,
    p.product_name,
    cat.category_name,
    COUNT(DISTINCT oi.order_id) AS times_ordered,
    SUM(oi.quantity) AS total_quantity_sold,
    ROUND(AVG(oi.price), 2) AS avg_sell_price,
    SUM(oi.quantity * oi.price) AS total_revenue
FROM products p
LEFT JOIN categories cat ON p.category_id = cat.category_id
LEFT JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id, p.product_name, cat.category_name
ORDER BY total_revenue DESC NULLS LAST;
```

### Monthly Sales by Category

```sql
SELECT 
    DATE_TRUNC('month', o.order_date) AS month,
    cat.category_name,
    COUNT(DISTINCT o.order_id) AS order_count,
    SUM(oi.quantity) AS items_sold,
    SUM(oi.quantity * oi.price) AS revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories cat ON p.category_id = cat.category_id
WHERE o.order_date >= '2024-01-01'
GROUP BY DATE_TRUNC('month', o.order_date), cat.category_name
ORDER BY month DESC, revenue DESC;
```

---

## Best Practices

### DO (Lakukan)

1. **Gunakan table aliases**
   ```sql
   SELECT c.name, o.order_id
   FROM customers c
   JOIN orders o ON c.customer_id = o.customer_id;
   ```

2. **Index foreign keys**
   ```sql
   CREATE INDEX idx_orders_customer_id ON orders(customer_id);
   ```

3. **Use INNER JOIN ketika jelas**
   ```sql
   -- Explicit
   INNER JOIN
   
   -- Avoid implicit
   FROM customers c, orders o WHERE c.customer_id = o.customer_id
   ```

4. **Select specific columns**
5. **Use LEFT JOIN untuk optional relationships**

### DON'T (Jangan)

1. Jangan gunakan NATURAL JOIN
2. Jangan JOIN terlalu banyak tables sekaligus
3. Jangan lupa WHERE saat filter
4. Jangan SELECT * di production
5. Jangan lupa index

---

## Troubleshooting

### Problem: Too many rows returned

**Solusi:** Cek untuk many-to-many relationships, gunakan DISTINCT atau GROUP BY.

```sql
SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### Problem: NULL values unexpected

**Solusi:** Use COALESCE atau check JOIN type.

```sql
SELECT 
    c.name,
    COALESCE(SUM(o.total_amount), 0) AS total
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name;
```

### Problem: Slow JOIN query

**Solusi:** 
- Add indexes
- Filter early
- Reduce columns selected
- Check EXPLAIN ANALYZE

---

## Rangkuman

- INNER JOIN: rows yang match di kedua tabel
- LEFT JOIN: semua dari kiri + matching dari kanan
- RIGHT JOIN: semua dari kanan + matching dari kiri
- FULL OUTER JOIN: semua rows dari kedua tabel
- CROSS JOIN: cartesian product
- SELF JOIN: join table dengan dirinya sendiri
- Multiple JOINs: join > 2 tables
- USING: shortcut untuk join dengan kolom sama nama
- Index foreign keys untuk performa
- Use table aliases untuk readability
- Combine dengan WHERE, GROUP BY, ORDER BY
- LEFT JOIN berguna untuk find missing relationships

---

**Sebelumnya:** [08. Insert, Update, Delete](08-insert-update-delete.md)  
**Selanjutnya:** [10. Aggregate Functions](10-aggregate-functions.md)


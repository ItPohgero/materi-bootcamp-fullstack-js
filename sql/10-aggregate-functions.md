# 10. Aggregate Functions dan GROUP BY

## Aggregate Functions

Aggregate functions melakukan kalkulasi pada set of values dan mengembalikan single value.

---

## COUNT

Menghitung jumlah rows.

```sql
-- Count semua rows
SELECT COUNT(*) FROM products;

-- Count non-NULL values
SELECT COUNT(email) FROM users;

-- Count distinct values
SELECT COUNT(DISTINCT category) FROM products;
```

**Contoh:**

```sql
-- Total customers
SELECT COUNT(*) AS total_customers FROM customers;

-- Customers dengan email
SELECT COUNT(email) AS customers_with_email FROM customers;

-- Jumlah kategori unik
SELECT COUNT(DISTINCT category) AS total_categories FROM products;
```

---

## SUM

Menjumlahkan nilai numeric.

```sql
SELECT SUM(price) FROM products;
SELECT SUM(quantity) FROM order_items;
```

**Contoh:**

```sql
-- Total revenue
SELECT SUM(total_amount) AS total_revenue FROM orders;

-- Total items terjual
SELECT SUM(quantity) AS total_items_sold FROM order_items;
```

---

## AVG

Menghitung rata-rata (average).

```sql
SELECT AVG(price) FROM products;
SELECT AVG(age) FROM users;
```

**Contoh:**

```sql
-- Rata-rata harga produk
SELECT ROUND(AVG(price), 2) AS avg_price FROM products;

-- Rata-rata order amount
SELECT ROUND(AVG(total_amount), 2) AS avg_order_value FROM orders;
```

---

## MIN dan MAX

Nilai minimum dan maximum.

```sql
-- Minimum
SELECT MIN(price) FROM products;
SELECT MIN(order_date) FROM orders;

-- Maximum
SELECT MAX(price) FROM products;
SELECT MAX(order_date) FROM orders;
```

**Contoh:**

```sql
-- Price range
SELECT 
    MIN(price) AS cheapest,
    MAX(price) AS most_expensive
FROM products;

-- Date range
SELECT 
    MIN(order_date) AS first_order,
    MAX(order_date) AS latest_order
FROM orders;
```

---

## GROUP BY

Mengelompokkan rows berdasarkan nilai kolom.

### Syntax

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1;
```

### Basic Example

```sql
-- Count products per category
SELECT 
    category,
    COUNT(*) AS product_count
FROM products
GROUP BY category;
```

**Hasil:**

```
  category   | product_count
-------------+---------------
 Electronics |            45
 Clothing    |            30
 Books       |            25
```

---

## GROUP BY dengan Multiple Columns

```sql
SELECT 
    category,
    status,
    COUNT(*) AS product_count
FROM products
GROUP BY category, status
ORDER BY category, status;
```

---

## GROUP BY dengan Multiple Aggregates

```sql
SELECT 
    category,
    COUNT(*) AS total_products,
    AVG(price) AS avg_price,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    SUM(stock_quantity) AS total_stock
FROM products
GROUP BY category
ORDER BY avg_price DESC;
```

---

## HAVING

Filter hasil GROUP BY (seperti WHERE tapi untuk aggregates).

### Syntax

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1
HAVING aggregate_function(column2) condition;
```

### Contoh

```sql
-- Categories dengan lebih dari 10 products
SELECT 
    category,
    COUNT(*) AS product_count
FROM products
GROUP BY category
HAVING COUNT(*) > 10;
```

### WHERE vs HAVING

```sql
-- WHERE: filter sebelum GROUP BY
-- HAVING: filter setelah GROUP BY

SELECT 
    category,
    COUNT(*) AS product_count,
    AVG(price) AS avg_price
FROM products
WHERE is_active = true  -- Filter rows dulu
GROUP BY category
HAVING AVG(price) > 100  -- Filter hasil group
ORDER BY avg_price DESC;
```

---

## Contoh Praktis

### Sales by Category

```sql
SELECT 
    c.category_name,
    COUNT(DISTINCT o.order_id) AS total_orders,
    SUM(oi.quantity) AS total_items_sold,
    ROUND(AVG(oi.price), 2) AS avg_price,
    SUM(oi.quantity * oi.price) AS total_revenue
FROM categories c
JOIN products p ON c.category_id = p.category_id
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o ON oi.order_id = o.order_id
GROUP BY c.category_id, c.category_name
HAVING SUM(oi.quantity * oi.price) > 1000
ORDER BY total_revenue DESC;
```

### Customer Segmentation

```sql
SELECT 
    CASE 
        WHEN total_spent < 100 THEN 'Bronze'
        WHEN total_spent < 500 THEN 'Silver'
        WHEN total_spent < 1000 THEN 'Gold'
        ELSE 'Platinum'
    END AS customer_tier,
    COUNT(*) AS customer_count,
    ROUND(AVG(total_spent), 2) AS avg_spent,
    SUM(total_spent) AS tier_revenue
FROM (
    SELECT 
        c.customer_id,
        c.name,
        SUM(o.total_amount) AS total_spent
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.name
) customer_totals
GROUP BY customer_tier
ORDER BY 
    CASE customer_tier
        WHEN 'Platinum' THEN 1
        WHEN 'Gold' THEN 2
        WHEN 'Silver' THEN 3
        WHEN 'Bronze' THEN 4
    END;
```

### Monthly Sales Report

```sql
SELECT 
    DATE_TRUNC('month', order_date) AS month,
    COUNT(DISTINCT customer_id) AS unique_customers,
    COUNT(order_id) AS total_orders,
    SUM(total_amount) AS revenue,
    ROUND(AVG(total_amount), 2) AS avg_order_value
FROM orders
WHERE order_date >= '2024-01-01'
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month DESC;
```

---

## Window Functions (Advanced)

Window functions seperti aggregate tapi tidak collapse rows.

### ROW_NUMBER

```sql
SELECT 
    product_name,
    category,
    price,
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY price DESC) AS rank
FROM products;
```

### RANK dan DENSE_RANK

```sql
SELECT 
    product_name,
    category,
    price,
    RANK() OVER (PARTITION BY category ORDER BY price DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY category ORDER BY price DESC) AS dense_rank
FROM products;
```

### Running Total

```sql
SELECT 
    order_date,
    total_amount,
    SUM(total_amount) OVER (ORDER BY order_date) AS running_total
FROM orders
ORDER BY order_date;
```

### Moving Average

```sql
SELECT 
    order_date,
    total_amount,
    AVG(total_amount) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7_days
FROM orders
ORDER BY order_date;
```

---

## STRING_AGG

Aggregate string values.

```sql
-- Gabungkan product names per category
SELECT 
    category,
    STRING_AGG(product_name, ', ' ORDER BY product_name) AS products
FROM products
GROUP BY category;
```

---

## ARRAY_AGG

Aggregate values into array.

```sql
SELECT 
    category,
    ARRAY_AGG(product_name ORDER BY price DESC) AS products,
    ARRAY_AGG(price ORDER BY price DESC) AS prices
FROM products
GROUP BY category;
```

---

## FILTER Clause

Apply WHERE condition to specific aggregates.

```sql
SELECT 
    category,
    COUNT(*) AS total_products,
    COUNT(*) FILTER (WHERE price > 100) AS expensive_products,
    COUNT(*) FILTER (WHERE stock_quantity = 0) AS out_of_stock,
    ROUND(AVG(price) FILTER (WHERE is_active = true), 2) AS avg_active_price
FROM products
GROUP BY category;
```

---

## ROLLUP dan CUBE

### ROLLUP

Subtotals dan grand totals.

```sql
SELECT 
    category,
    status,
    COUNT(*) AS product_count
FROM products
GROUP BY ROLLUP (category, status)
ORDER BY category NULLS FIRST, status NULLS FIRST;
```

### CUBE

All possible combinations of subtotals.

```sql
SELECT 
    category,
    status,
    COUNT(*) AS product_count
FROM products
GROUP BY CUBE (category, status);
```

---

## GROUPING SETS

Specify exact grouping combinations.

```sql
SELECT 
    category,
    status,
    COUNT(*) AS product_count
FROM products
GROUP BY GROUPING SETS (
    (category, status),
    (category),
    (status),
    ()
);
```

---

## Performance Tips

### Index untuk GROUP BY

```sql
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_orders_date ON orders(order_date);
```

### Avoid Subqueries jika Bisa

```sql
-- Slow
SELECT 
    category,
    COUNT(*),
    (SELECT AVG(price) FROM products) AS overall_avg
FROM products
GROUP BY category;

-- Fast
SELECT 
    category,
    COUNT(*),
    AVG(price) OVER() AS overall_avg
FROM products
GROUP BY category;
```

---

## Best Practices

### DO (Lakukan)

1. **Include GROUP BY columns in SELECT**
   ```sql
   -- Good
   SELECT category, COUNT(*)
   FROM products
   GROUP BY category;
   ```

2. **Use HAVING untuk filter aggregates**
   ```sql
   SELECT category, COUNT(*)
   FROM products
   GROUP BY category
   HAVING COUNT(*) > 10;
   ```

3. **Order aggregation results**
   ```sql
   SELECT category, SUM(price)
   FROM products
   GROUP BY category
   ORDER BY SUM(price) DESC;
   ```

4. **Use aliases untuk aggregates**
   ```sql
   SELECT 
       category,
       COUNT(*) AS total_products,
       AVG(price) AS avg_price
   FROM products
   GROUP BY category;
   ```

### DON'T (Jangan)

1. Jangan SELECT non-grouped columns tanpa aggregate
2. Jangan gunakan WHERE untuk filter aggregates (use HAVING)
3. Jangan lupa ORDER BY untuk presentasi

---

## Common Patterns

### Top N per Group

```sql
SELECT *
FROM (
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY price DESC) AS rn
    FROM products
) ranked
WHERE rn <= 3;
```

### Percentage of Total

```sql
SELECT 
    category,
    COUNT(*) AS product_count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) AS percentage
FROM products
GROUP BY category
ORDER BY product_count DESC;
```

### Year-over-Year Growth

```sql
SELECT 
    EXTRACT(YEAR FROM order_date) AS year,
    SUM(total_amount) AS revenue,
    LAG(SUM(total_amount)) OVER (ORDER BY EXTRACT(YEAR FROM order_date)) AS prev_year_revenue,
    ROUND(
        (SUM(total_amount) - LAG(SUM(total_amount)) OVER (ORDER BY EXTRACT(YEAR FROM order_date))) 
        / LAG(SUM(total_amount)) OVER (ORDER BY EXTRACT(YEAR FROM order_date)) * 100,
        2
    ) AS growth_percent
FROM orders
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year;
```

---

## Troubleshooting

### Problem: "column must appear in GROUP BY clause"

**Solusi:** Tambahkan kolom ke GROUP BY atau gunakan aggregate.

```sql
-- Error
SELECT category, product_name, COUNT(*)
FROM products
GROUP BY category;

-- Fix
SELECT category, product_name, COUNT(*)
FROM products
GROUP BY category, product_name;
```

### Problem: HAVING dengan non-aggregate column

**Solusi:** Gunakan WHERE atau tambahkan ke GROUP BY.

```sql
-- Error
SELECT category, COUNT(*)
FROM products
GROUP BY category
HAVING price > 100;

-- Fix
SELECT category, COUNT(*)
FROM products
WHERE price > 100
GROUP BY category;
```

---

## Rangkuman

- Aggregate functions: COUNT, SUM, AVG, MIN, MAX
- GROUP BY mengelompokkan rows
- HAVING filter hasil GROUP BY
- WHERE filter sebelum GROUP BY, HAVING filter setelah
- Window functions untuk analysis tanpa collapse rows
- STRING_AGG dan ARRAY_AGG untuk aggregate non-numeric
- FILTER clause untuk conditional aggregates
- ROLLUP dan CUBE untuk subtotals
- Index kolom yang sering di-group
- Combine dengan JOIN untuk analysis complex data

---

**Sebelumnya:** [09. Join Tables](09-join-tables.md)  
**Selanjutnya:** [README - Daftar Lengkap Materi](README.md)


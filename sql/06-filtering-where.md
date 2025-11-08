# 6. Filtering Data - WHERE

## Klausa WHERE

WHERE digunakan untuk memfilter data berdasarkan kondisi tertentu.

### Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

---

## Comparison Operators

### Equal (=)

```sql
SELECT * FROM users WHERE age = 25;
SELECT * FROM products WHERE category = 'Electronics';
SELECT * FROM orders WHERE status = 'completed';
```

### Not Equal (!= atau <>)

```sql
SELECT * FROM users WHERE status != 'banned';
SELECT * FROM products WHERE stock_quantity <> 0;
```

### Greater Than (>)

```sql
SELECT * FROM products WHERE price > 100;
SELECT * FROM users WHERE age > 18;
```

### Less Than (<)

```sql
SELECT * FROM products WHERE stock_quantity < 10;
SELECT * FROM orders WHERE total_amount < 50;
```

### Greater Than or Equal (>=)

```sql
SELECT * FROM users WHERE age >= 18;
SELECT * FROM products WHERE price >= 50;
```

### Less Than or Equal (<=)

```sql
SELECT * FROM products WHERE price <= 1000;
SELECT * FROM users WHERE age <= 65;
```

---

## Logical Operators

### AND

Semua kondisi harus TRUE.

```sql
SELECT * FROM products 
WHERE price > 100 AND stock_quantity > 0;

SELECT * FROM users 
WHERE age >= 18 AND country = 'Indonesia';

SELECT * FROM orders 
WHERE status = 'completed' 
  AND total_amount > 500 
  AND order_date >= '2025-01-01';
```

### OR

Salah satu kondisi TRUE.

```sql
SELECT * FROM products 
WHERE category = 'Electronics' OR category = 'Computers';

SELECT * FROM users 
WHERE city = 'Jakarta' OR city = 'Surabaya';

SELECT * FROM orders 
WHERE status = 'pending' OR status = 'processing';
```

### NOT

Negasi kondisi.

```sql
SELECT * FROM users WHERE NOT status = 'banned';

SELECT * FROM products WHERE NOT category = 'Discontinued';

-- Sama dengan
SELECT * FROM users WHERE status != 'banned';
```

### Kombinasi AND, OR, NOT

Gunakan tanda kurung untuk prioritas.

```sql
SELECT * FROM products 
WHERE (category = 'Electronics' OR category = 'Computers') 
  AND price < 1000;

SELECT * FROM users 
WHERE age >= 18 
  AND (city = 'Jakarta' OR city = 'Bandung') 
  AND NOT status = 'inactive';
```

---

## BETWEEN

Memfilter dalam range nilai.

```sql
SELECT * FROM products 
WHERE price BETWEEN 100 AND 500;

-- Sama dengan
SELECT * FROM products 
WHERE price >= 100 AND price <= 500;
```

**Contoh:**

```sql
-- Range angka
SELECT * FROM users 
WHERE age BETWEEN 18 AND 30;

-- Range tanggal
SELECT * FROM orders 
WHERE order_date BETWEEN '2025-01-01' AND '2025-01-31';

-- NOT BETWEEN
SELECT * FROM products 
WHERE price NOT BETWEEN 100 AND 500;
```

---

## IN

Memfilter dengan list nilai.

```sql
SELECT * FROM products 
WHERE category IN ('Electronics', 'Computers', 'Phones');

-- Sama dengan
SELECT * FROM products 
WHERE category = 'Electronics' 
   OR category = 'Computers' 
   OR category = 'Phones';
```

**Contoh:**

```sql
-- IN dengan angka
SELECT * FROM users 
WHERE id IN (1, 5, 10, 15);

-- IN dengan string
SELECT * FROM orders 
WHERE status IN ('pending', 'processing', 'shipped');

-- NOT IN
SELECT * FROM products 
WHERE category NOT IN ('Discontinued', 'Out of Stock');

-- IN dengan subquery
SELECT * FROM products 
WHERE category_id IN (
    SELECT category_id FROM categories WHERE is_active = true
);
```

---

## LIKE

Pattern matching untuk string.

### Wildcard Characters

- `%` : Cocok dengan nol atau lebih karakter
- `_` : Cocok dengan tepat satu karakter

```sql
-- Dimulai dengan 'John'
SELECT * FROM users 
WHERE name LIKE 'John%';

-- Diakhiri dengan 'son'
SELECT * FROM users 
WHERE name LIKE '%son';

-- Mengandung 'and'
SELECT * FROM users 
WHERE name LIKE '%and%';

-- Tepat 5 karakter
SELECT * FROM users 
WHERE name LIKE '_____';

-- Dimulai J, 4 karakter
SELECT * FROM users 
WHERE name LIKE 'J____';
```

**Contoh Praktis:**

```sql
-- Email dengan domain gmail
SELECT * FROM users 
WHERE email LIKE '%@gmail.com';

-- Nomor telepon dimulai 081
SELECT * FROM customers 
WHERE phone LIKE '081%';

-- Produk dengan 'phone' di nama
SELECT * FROM products 
WHERE product_name LIKE '%phone%';

-- NOT LIKE
SELECT * FROM users 
WHERE email NOT LIKE '%@spam.com';
```

### ILIKE (Case-Insensitive)

PostgreSQL specific: LIKE tapi case-insensitive.

```sql
-- Case-sensitive (hanya 'John')
SELECT * FROM users WHERE name LIKE 'john%';

-- Case-insensitive ('John', 'john', 'JOHN')
SELECT * FROM users WHERE name ILIKE 'john%';
```

### SIMILAR TO

Regex-like pattern matching.

```sql
SELECT * FROM users 
WHERE email SIMILAR TO '%@(gmail|yahoo|hotmail).com';
```

---

## IS NULL / IS NOT NULL

Cek nilai NULL.

```sql
-- Cari rows dengan NULL
SELECT * FROM users WHERE phone IS NULL;

-- Cari rows tanpa NULL
SELECT * FROM users WHERE phone IS NOT NULL;

-- Kombinasi dengan kondisi lain
SELECT * FROM products 
WHERE discount_price IS NOT NULL 
  AND stock_quantity > 0;
```

**Contoh:**

```sql
-- Users tanpa email
SELECT name, phone FROM users WHERE email IS NULL;

-- Products dengan discount
SELECT product_name, price, discount_price 
FROM products 
WHERE discount_price IS NOT NULL;
```

**Catatan:** Jangan gunakan `= NULL` atau `!= NULL`, harus `IS NULL` atau `IS NOT NULL`.

---

## EXISTS

Cek apakah subquery mengembalikan data.

```sql
-- Customers yang punya order
SELECT * FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
);

-- Customers yang tidak punya order
SELECT * FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
);
```

---

## ANY / ALL

Membandingkan dengan hasil subquery.

### ANY

TRUE jika kondisi TRUE untuk salah satu nilai.

```sql
-- Products lebih mahal dari minimal satu product di category 'Electronics'
SELECT * FROM products
WHERE price > ANY (
    SELECT price FROM products WHERE category = 'Electronics'
);
```

### ALL

TRUE jika kondisi TRUE untuk semua nilai.

```sql
-- Products lebih mahal dari semua products di category 'Electronics'
SELECT * FROM products
WHERE price > ALL (
    SELECT price FROM products WHERE category = 'Electronics'
);
```

---

## Filtering dengan Date/Time

```sql
-- Exact date
SELECT * FROM orders 
WHERE order_date = '2025-01-15';

-- Date range
SELECT * FROM orders 
WHERE order_date BETWEEN '2025-01-01' AND '2025-01-31';

-- Year
SELECT * FROM orders 
WHERE EXTRACT(YEAR FROM order_date) = 2025;

-- Month
SELECT * FROM orders 
WHERE EXTRACT(MONTH FROM order_date) = 1;

-- Today
SELECT * FROM orders 
WHERE order_date = CURRENT_DATE;

-- Last 7 days
SELECT * FROM orders 
WHERE order_date >= CURRENT_DATE - INTERVAL '7 days';

-- This month
SELECT * FROM orders 
WHERE order_date >= DATE_TRUNC('month', CURRENT_DATE);
```

---

## Filtering dengan CASE

```sql
SELECT 
    product_name,
    price,
    stock_quantity
FROM products
WHERE 
    CASE 
        WHEN category = 'Electronics' THEN price < 1000
        WHEN category = 'Clothing' THEN price < 200
        ELSE price < 500
    END;
```

---

## Contoh Query Kompleks

### E-Commerce: Find Active Products

```sql
SELECT 
    product_id,
    product_name,
    category,
    price,
    stock_quantity
FROM products
WHERE 
    is_active = true
    AND stock_quantity > 0
    AND (
        category IN ('Electronics', 'Computers', 'Phones')
        OR price BETWEEN 100 AND 1000
    )
    AND product_name NOT LIKE '%Discontinued%'
    AND created_at >= '2024-01-01'
ORDER BY price DESC;
```

### Find Customers with Recent Orders

```sql
SELECT DISTINCT
    c.customer_id,
    c.name,
    c.email
FROM customers c
WHERE 
    c.status = 'active'
    AND c.email IS NOT NULL
    AND EXISTS (
        SELECT 1 
        FROM orders o 
        WHERE o.customer_id = c.customer_id
          AND o.order_date >= CURRENT_DATE - INTERVAL '30 days'
    )
    AND c.country IN ('Indonesia', 'Singapore', 'Malaysia');
```

### Products Needing Restock

```sql
SELECT 
    product_name,
    category,
    stock_quantity,
    CASE 
        WHEN stock_quantity = 0 THEN 'Out of Stock'
        WHEN stock_quantity < 10 THEN 'Critical'
        WHEN stock_quantity < 50 THEN 'Low'
        ELSE 'Normal'
    END AS stock_status
FROM products
WHERE 
    is_active = true
    AND (
        stock_quantity = 0
        OR stock_quantity < 10
    )
    AND category NOT IN ('Discontinued', 'Pre-order')
ORDER BY stock_quantity ASC;
```

---

## Performance Tips

### Index untuk Kolom yang Sering di-Filter

```sql
-- Buat index
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_orders_date ON orders(order_date);

-- Composite index
CREATE INDEX idx_products_category_price ON products(category, price);
```

### Hindari Function pada WHERE (Menghilangkan Index)

```sql
-- Bad (tidak pakai index)
SELECT * FROM users WHERE LOWER(email) = 'john@example.com';

-- Good (pakai index)
SELECT * FROM users WHERE email = 'john@example.com';
```

### Gunakan EXISTS daripada IN untuk Subquery

```sql
-- Lebih lambat
SELECT * FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);

-- Lebih cepat
SELECT * FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

---

## Best Practices

### DO (Lakukan)

1. **Gunakan tanda kurung untuk klaritas**
   ```sql
   WHERE (condition1 OR condition2) AND condition3
   ```

2. **Gunakan operator yang tepat**
   ```sql
   -- Good
   WHERE status IN ('active', 'pending')
   
   -- Avoid
   WHERE status = 'active' OR status = 'pending'
   ```

3. **Index kolom yang sering di-filter**

4. **Gunakan BETWEEN untuk range**
   ```sql
   WHERE price BETWEEN 100 AND 500
   ```

5. **Specific conditions dulu (short-circuit)**
   ```sql
   WHERE is_active = true AND expensive_calculation()
   ```

### DON'T (Jangan)

1. Jangan gunakan `= NULL`, gunakan `IS NULL`
2. Jangan lupa tanda kurung untuk kombinasi AND/OR
3. Jangan gunakan function pada indexed column di WHERE
4. Jangan filter data yang bisa di-constraint di table

---

## Troubleshooting

### Problem: Query tidak return data yang expected

**Solusi:** Cek NULL values dan case-sensitivity.

```sql
-- Cek NULL
WHERE column IS NOT NULL

-- Case-insensitive
WHERE LOWER(column) = LOWER('value')
-- atau
WHERE column ILIKE 'value'
```

### Problem: Query terlalu lambat

**Solusi:** 
- Tambah index
- Simplify conditions
- Gunakan EXPLAIN ANALYZE

```sql
EXPLAIN ANALYZE
SELECT * FROM products WHERE category = 'Electronics';
```

---

## Rangkuman

- WHERE digunakan untuk filter data
- Comparison operators: `=`, `!=`, `>`, `<`, `>=`, `<=`
- Logical operators: AND, OR, NOT
- BETWEEN untuk range nilai
- IN untuk list nilai
- LIKE untuk pattern matching (% dan _)
- ILIKE untuk case-insensitive LIKE
- IS NULL / IS NOT NULL untuk cek NULL
- EXISTS untuk cek keberadaan data di subquery
- Gunakan tanda kurung untuk kombinasi kondisi
- Index kolom yang sering di-filter untuk performa
- Gunakan EXISTS daripada IN untuk subquery

---

**Sebelumnya:** [05. Basic Query - SELECT](05-basic-query-select.md)  
**Selanjutnya:** [07. Sorting dan Limiting](07-sorting-limiting.md)


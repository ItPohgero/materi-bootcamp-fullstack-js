# 7. Sorting dan Limiting

## ORDER BY - Mengurutkan Data

ORDER BY digunakan untuk mengurutkan hasil query.

### Syntax

```sql
SELECT column1, column2
FROM table_name
ORDER BY column1 [ASC|DESC];
```

---

## Ascending (ASC)

Urutan naik (default).

```sql
-- ASC adalah default
SELECT * FROM products ORDER BY price;

-- Explicit ASC
SELECT * FROM products ORDER BY price ASC;
```

**Hasil:**

```
 product_name | price
--------------+-------
 Keyboard     |   50
 Mouse        |   75
 Monitor      |  250
 Laptop       | 1000
```

---

## Descending (DESC)

Urutan turun.

```sql
SELECT * FROM products ORDER BY price DESC;
```

**Hasil:**

```
 product_name | price
--------------+-------
 Laptop       | 1000
 Monitor      |  250
 Mouse        |   75
 Keyboard     |   50
```

---

## Multiple Column Sorting

Urut berdasarkan beberapa kolom.

```sql
SELECT * FROM products 
ORDER BY category ASC, price DESC;
```

**Contoh:**

```sql
-- Urut by category (A-Z), lalu price (tinggi-rendah)
SELECT 
    category,
    product_name,
    price
FROM products
ORDER BY category ASC, price DESC;
```

**Hasil:**

```
 category    | product_name | price
-------------+--------------+-------
 Electronics | Laptop       |  1000
 Electronics | Phone        |   800
 Electronics | Tablet       |   500
 Furniture   | Desk         |   300
 Furniture   | Chair        |   150
```

---

## ORDER BY dengan Expressions

```sql
-- Sort by calculated value
SELECT 
    product_name,
    price,
    stock_quantity,
    price * stock_quantity AS total_value
FROM products
ORDER BY price * stock_quantity DESC;
```

**Contoh:**

```sql
-- Sort by discount percentage
SELECT 
    product_name,
    price,
    discount_price,
    (price - COALESCE(discount_price, price)) / price * 100 AS discount_percent
FROM products
ORDER BY discount_percent DESC;
```

---

## ORDER BY dengan CASE

```sql
SELECT 
    product_name,
    status
FROM products
ORDER BY 
    CASE status
        WHEN 'active' THEN 1
        WHEN 'pending' THEN 2
        WHEN 'discontinued' THEN 3
    END;
```

**Contoh Custom Sort:**

```sql
-- Sort by priority
SELECT 
    order_id,
    status,
    total_amount
FROM orders
ORDER BY 
    CASE status
        WHEN 'urgent' THEN 1
        WHEN 'processing' THEN 2
        WHEN 'pending' THEN 3
        WHEN 'completed' THEN 4
    END,
    order_date DESC;
```

---

## ORDER BY dengan NULL

NULL values biasanya ditempatkan terakhir saat ASC, pertama saat DESC.

### NULLS FIRST / NULLS LAST

```sql
-- NULL di akhir saat ASC
SELECT * FROM products 
ORDER BY discount_price ASC NULLS LAST;

-- NULL di awal saat DESC
SELECT * FROM products 
ORDER BY discount_price DESC NULLS FIRST;
```

**Contoh:**

```sql
SELECT 
    product_name,
    discount_price
FROM products
ORDER BY discount_price ASC NULLS LAST;
```

---

## ORDER BY Position

Menggunakan nomor posisi kolom.

```sql
-- Sort by kolom ke-2, lalu kolom ke-3
SELECT 
    product_name,
    category,
    price
FROM products
ORDER BY 2, 3 DESC;
```

**Catatan:** Tidak recommended, lebih baik gunakan nama kolom untuk readability.

---

## LIMIT - Membatasi Jumlah Rows

LIMIT membatasi jumlah rows yang dikembalikan.

```sql
-- Ambil 10 rows pertama
SELECT * FROM products LIMIT 10;

-- Ambil 5 rows termurah
SELECT * FROM products 
ORDER BY price ASC 
LIMIT 5;
```

**Contoh:**

```sql
-- Top 10 produk termahal
SELECT 
    product_name,
    category,
    price
FROM products
ORDER BY price DESC
LIMIT 10;
```

---

## OFFSET - Skip Rows

OFFSET skip sejumlah rows dari awal.

```sql
-- Skip 5 rows pertama, ambil 10 rows berikutnya
SELECT * FROM products 
ORDER BY price DESC
LIMIT 10 OFFSET 5;
```

**Contoh:**

```sql
-- Rows 11-20
SELECT * FROM products 
ORDER BY product_id
LIMIT 10 OFFSET 10;
```

---

## Pagination

Kombinasi LIMIT dan OFFSET untuk pagination.

### Formula Pagination

```
OFFSET = (page_number - 1) * page_size
LIMIT = page_size
```

**Contoh Implementation:**

```sql
-- Page 1 (items 1-20)
SELECT * FROM products 
ORDER BY product_id
LIMIT 20 OFFSET 0;

-- Page 2 (items 21-40)
SELECT * FROM products 
ORDER BY product_id
LIMIT 20 OFFSET 20;

-- Page 3 (items 41-60)
SELECT * FROM products 
ORDER BY product_id
LIMIT 20 OFFSET 40;
```

### Pagination Function (Concept)

```sql
-- Page number: 3, Page size: 20
SELECT * FROM products
ORDER BY product_id
LIMIT 20 OFFSET (3 - 1) * 20;  -- OFFSET 40
```

---

## FETCH (SQL Standard Alternative)

FETCH adalah SQL standard alternative untuk LIMIT.

```sql
-- Same as LIMIT 10
SELECT * FROM products
ORDER BY price
FETCH FIRST 10 ROWS ONLY;

-- Same as LIMIT 10 OFFSET 5
SELECT * FROM products
ORDER BY price
OFFSET 5 ROWS
FETCH NEXT 10 ROWS ONLY;
```

---

## Contoh Praktis

### Top Customers by Total Spend

```sql
SELECT 
    c.customer_id,
    c.name,
    COUNT(o.order_id) AS total_orders,
    SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spent DESC
LIMIT 10;
```

### Latest Products

```sql
SELECT 
    product_id,
    product_name,
    category,
    price,
    created_at
FROM products
WHERE is_active = true
ORDER BY created_at DESC
LIMIT 20;
```

### Products Needing Restock (Lowest Stock First)

```sql
SELECT 
    product_name,
    category,
    stock_quantity,
    price
FROM products
WHERE 
    is_active = true
    AND stock_quantity < 20
ORDER BY 
    stock_quantity ASC,
    category ASC
LIMIT 50;
```

### Pagination dengan Total Count

```sql
-- Get page data dan total count
WITH paginated AS (
    SELECT 
        product_id,
        product_name,
        price,
        COUNT(*) OVER() AS total_count
    FROM products
    WHERE is_active = true
    ORDER BY product_name
    LIMIT 20 OFFSET 0
)
SELECT 
    product_id,
    product_name,
    price,
    total_count,
    CEIL(total_count::NUMERIC / 20) AS total_pages
FROM paginated;
```

---

## DISTINCT dengan ORDER BY

DISTINCT dapat dikombinasi dengan ORDER BY.

```sql
SELECT DISTINCT category
FROM products
ORDER BY category;
```

**Catatan:** ORDER BY harus menggunakan kolom yang ada di SELECT atau aggregation dari kolom tersebut.

```sql
-- Error: ORDER BY price tidak ada di SELECT
SELECT DISTINCT category
FROM products
ORDER BY price;

-- OK: ORDER BY menggunakan aggregation
SELECT DISTINCT category
FROM products
ORDER BY MAX(price) DESC;
```

---

## Random Order

Mengurutkan secara random.

```sql
-- PostgreSQL specific
SELECT * FROM products
ORDER BY RANDOM()
LIMIT 10;

-- Get random sample
SELECT * FROM users
WHERE is_active = true
ORDER BY RANDOM()
LIMIT 100;
```

**Catatan:** RANDOM() lambat untuk tabel besar. Gunakan alternatif untuk production.

---

## Performance Optimization

### Index untuk ORDER BY

```sql
-- Buat index untuk kolom yang sering di-sort
CREATE INDEX idx_products_price ON products(price);
CREATE INDEX idx_orders_date ON orders(order_date DESC);

-- Composite index untuk multiple column sorting
CREATE INDEX idx_products_category_price 
ON products(category, price DESC);
```

### LIMIT dengan Index

LIMIT + ORDER BY dengan index sangat cepat.

```sql
-- Sangat cepat jika ada index di price
SELECT * FROM products
ORDER BY price
LIMIT 10;
```

### Pagination Alternatives untuk Large Datasets

Untuk dataset besar, OFFSET lambat. Gunakan keyset pagination:

```sql
-- Traditional pagination (slow for large offset)
SELECT * FROM products
ORDER BY product_id
LIMIT 20 OFFSET 10000;

-- Keyset pagination (fast)
SELECT * FROM products
WHERE product_id > 10020  -- Last ID from previous page
ORDER BY product_id
LIMIT 20;
```

---

## Best Practices

### DO (Lakukan)

1. **Selalu gunakan ORDER BY dengan LIMIT**
   ```sql
   -- Good
   SELECT * FROM products ORDER BY created_at DESC LIMIT 10;
   
   -- Bad (unpredictable order)
   SELECT * FROM products LIMIT 10;
   ```

2. **Index kolom yang sering di-sort**
   ```sql
   CREATE INDEX idx_products_price ON products(price);
   ```

3. **Gunakan LIMIT untuk testing**
   ```sql
   SELECT * FROM large_table ORDER BY id LIMIT 100;
   ```

4. **Specific sort columns**
   ```sql
   -- Good
   ORDER BY created_at DESC, product_id DESC
   
   -- Avoid
   ORDER BY 1, 2
   ```

5. **Consider pagination alternatives untuk large datasets**

### DON'T (Jangan)

1. Jangan lupa ORDER BY saat menggunakan LIMIT
2. Jangan gunakan OFFSET besar (> 1000) tanpa index
3. Jangan sort tanpa perlu
4. Jangan gunakan RANDOM() untuk tabel besar
5. Jangan gunakan column position (ORDER BY 1)

---

## Troubleshooting

### Problem: Inconsistent results dengan LIMIT tanpa ORDER BY

**Solusi:** Selalu gunakan ORDER BY.

```sql
-- Add ORDER BY
SELECT * FROM products
ORDER BY product_id
LIMIT 10;
```

### Problem: Slow pagination dengan OFFSET besar

**Solusi:** Gunakan keyset pagination.

```sql
-- Keyset pagination
SELECT * FROM products
WHERE product_id > :last_id
ORDER BY product_id
LIMIT 20;
```

### Problem: NULL values muncul di posisi unexpected

**Solusi:** Gunakan NULLS FIRST/LAST.

```sql
SELECT * FROM products
ORDER BY discount_price ASC NULLS LAST;
```

---

## Query Examples

### E-Commerce: Best Selling Products

```sql
SELECT 
    p.product_name,
    p.category,
    COUNT(oi.order_item_id) AS times_sold,
    SUM(oi.quantity) AS total_quantity_sold
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id, p.product_name, p.category
ORDER BY total_quantity_sold DESC
LIMIT 20;
```

### Recent Active Users

```sql
SELECT 
    user_id,
    username,
    last_login,
    total_orders
FROM users
WHERE 
    is_active = true
    AND last_login >= CURRENT_DATE - INTERVAL '30 days'
ORDER BY last_login DESC
LIMIT 50;
```

### Products by Price Range with Pagination

```sql
SELECT 
    product_name,
    category,
    price,
    stock_quantity
FROM products
WHERE 
    price BETWEEN 100 AND 500
    AND is_active = true
ORDER BY 
    price ASC,
    product_name ASC
LIMIT 20 OFFSET 0;  -- Page 1
```

---

## Rangkuman

- ORDER BY untuk mengurutkan hasil query
- ASC = ascending (naik), DESC = descending (turun)
- Bisa sort multiple columns
- NULLS FIRST / NULLS LAST untuk handle NULL
- LIMIT membatasi jumlah rows
- OFFSET skip rows dari awal
- LIMIT + OFFSET untuk pagination
- FETCH adalah SQL standard alternative
- Selalu gunakan ORDER BY dengan LIMIT
- Index kolom yang sering di-sort
- Gunakan keyset pagination untuk large datasets
- Avoid OFFSET besar untuk performa

---

**Sebelumnya:** [06. Filtering Data - WHERE](06-filtering-where.md)  
**Selanjutnya:** [08. Insert, Update, Delete](08-insert-update-delete.md)


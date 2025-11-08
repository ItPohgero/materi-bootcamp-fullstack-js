# 8. Insert, Update, Delete

## INSERT - Menambah Data

INSERT digunakan untuk menambahkan data baru ke tabel.

### Syntax Dasar

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

---

## INSERT Single Row

```sql
-- Insert dengan specify columns
INSERT INTO users (username, email, age)
VALUES ('john_doe', 'john@example.com', 25);

-- Insert semua columns (harus urut sesuai tabel)
INSERT INTO users
VALUES (DEFAULT, 'jane_doe', 'jane@example.com', 30, CURRENT_TIMESTAMP);
```

**Contoh:**

```sql
INSERT INTO products (product_name, category, price, stock_quantity)
VALUES ('Laptop Dell', 'Electronics', 999.99, 50);

INSERT INTO customers (first_name, last_name, email, phone)
VALUES ('John', 'Doe', 'john@example.com', '081234567890');
```

---

## INSERT Multiple Rows

```sql
INSERT INTO table_name (column1, column2)
VALUES 
    (value1a, value2a),
    (value1b, value2b),
    (value1c, value2c);
```

**Contoh:**

```sql
INSERT INTO products (product_name, category, price, stock_quantity)
VALUES 
    ('Mouse Logitech', 'Electronics', 29.99, 100),
    ('Keyboard Mechanical', 'Electronics', 79.99, 75),
    ('Monitor 24"', 'Electronics', 199.99, 30),
    ('Webcam HD', 'Electronics', 49.99, 50);
```

---

## INSERT dengan DEFAULT

```sql
-- Gunakan DEFAULT untuk kolom dengan default value
INSERT INTO products (product_name, price)
VALUES ('New Product', 99.99);
-- stock_quantity akan use default value (0)
-- created_at akan use CURRENT_TIMESTAMP
```

---

## INSERT dengan RETURNING

Mengembalikan data yang baru di-insert.

```sql
INSERT INTO products (product_name, price)
VALUES ('Gaming Mouse', 59.99)
RETURNING product_id, product_name, created_at;
```

**Hasil:**

```
 product_id | product_name | created_at
------------+--------------+-------------------------
        101 | Gaming Mouse | 2025-01-15 10:30:00
```

**Multiple rows:**

```sql
INSERT INTO users (username, email)
VALUES 
    ('user1', 'user1@example.com'),
    ('user2', 'user2@example.com')
RETURNING user_id, username;
```

---

## INSERT dari SELECT

Copy data dari tabel lain.

```sql
INSERT INTO table_name (column1, column2)
SELECT column1, column2
FROM other_table
WHERE condition;
```

**Contoh:**

```sql
-- Copy produk discontinued ke archive table
INSERT INTO archived_products (product_name, category, price)
SELECT product_name, category, price
FROM products
WHERE status = 'discontinued';

-- Copy top customers ke VIP table
INSERT INTO vip_customers (customer_id, name, total_spent)
SELECT 
    c.customer_id,
    c.name,
    SUM(o.total_amount)
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
HAVING SUM(o.total_amount) > 10000;
```

---

## INSERT dengan ON CONFLICT (UPSERT)

Handle duplicate key conflicts.

### Syntax

```sql
INSERT INTO table_name (column1, column2)
VALUES (value1, value2)
ON CONFLICT (conflict_column) DO NOTHING;

-- atau

INSERT INTO table_name (column1, column2)
VALUES (value1, value2)
ON CONFLICT (conflict_column) 
DO UPDATE SET column2 = EXCLUDED.column2;
```

**Contoh:**

```sql
-- Do nothing jika email sudah ada
INSERT INTO users (username, email)
VALUES ('john_doe', 'john@example.com')
ON CONFLICT (email) DO NOTHING;

-- Update jika username sudah ada
INSERT INTO users (username, email, age)
VALUES ('john_doe', 'john@example.com', 25)
ON CONFLICT (username) 
DO UPDATE SET 
    email = EXCLUDED.email,
    age = EXCLUDED.age,
    updated_at = CURRENT_TIMESTAMP;
```

---

## UPDATE - Mengubah Data

UPDATE digunakan untuk mengubah data yang sudah ada.

### Syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

**Penting:** Selalu gunakan WHERE! Tanpa WHERE, semua rows akan ter-update.

---

## UPDATE Single Column

```sql
-- Update satu kolom
UPDATE products
SET price = 899.99
WHERE product_id = 1;

-- Update dengan calculation
UPDATE products
SET price = price * 1.1  -- Naikkan 10%
WHERE category = 'Electronics';
```

---

## UPDATE Multiple Columns

```sql
UPDATE products
SET 
    price = 799.99,
    stock_quantity = 100,
    updated_at = CURRENT_TIMESTAMP
WHERE product_id = 1;
```

**Contoh:**

```sql
UPDATE customers
SET 
    phone = '081234567890',
    address = 'Jl. Sudirman No. 123',
    city = 'Jakarta',
    updated_at = NOW()
WHERE customer_id = 5;
```

---

## UPDATE dengan CASE

```sql
UPDATE products
SET price = CASE 
    WHEN category = 'Electronics' THEN price * 0.9  -- 10% discount
    WHEN category = 'Clothing' THEN price * 0.8     -- 20% discount
    WHEN category = 'Books' THEN price * 0.85       -- 15% discount
    ELSE price
END
WHERE is_active = true;
```

---

## UPDATE dengan Subquery

```sql
-- Update dari data di tabel lain
UPDATE products
SET category_name = (
    SELECT category_name 
    FROM categories 
    WHERE categories.category_id = products.category_id
);

-- Update berdasarkan aggregation
UPDATE customers
SET total_orders = (
    SELECT COUNT(*) 
    FROM orders 
    WHERE orders.customer_id = customers.customer_id
);
```

---

## UPDATE dengan JOIN (FROM)

```sql
UPDATE products
SET stock_quantity = stock_quantity - oi.quantity
FROM order_items oi
WHERE products.product_id = oi.product_id
  AND oi.order_id = 12345;
```

**Contoh:**

```sql
-- Update product price from supplier table
UPDATE products p
SET price = s.wholesale_price * 1.5
FROM suppliers s
WHERE p.supplier_id = s.supplier_id
  AND s.is_active = true;
```

---

## UPDATE dengan RETURNING

```sql
UPDATE products
SET stock_quantity = stock_quantity - 1
WHERE product_id = 10
RETURNING product_id, product_name, stock_quantity;
```

**Contoh:**

```sql
UPDATE orders
SET status = 'shipped', shipped_at = CURRENT_TIMESTAMP
WHERE order_id = 100
RETURNING order_id, status, shipped_at;
```

---

## DELETE - Menghapus Data

DELETE digunakan untuk menghapus data.

### Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

**Penting:** Selalu gunakan WHERE! Tanpa WHERE, semua rows akan terhapus.

---

## DELETE dengan WHERE

```sql
-- Delete single row
DELETE FROM products
WHERE product_id = 1;

-- Delete multiple rows
DELETE FROM products
WHERE category = 'Discontinued';

-- Delete dengan multiple conditions
DELETE FROM users
WHERE status = 'inactive' 
  AND last_login < CURRENT_DATE - INTERVAL '1 year';
```

---

## DELETE dengan Subquery

```sql
-- Delete products yang tidak pernah dipesan
DELETE FROM products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id FROM order_items
);

-- Delete inactive users
DELETE FROM users
WHERE user_id IN (
    SELECT user_id FROM user_activity
    WHERE last_active < CURRENT_DATE - INTERVAL '2 years'
);
```

---

## DELETE dengan JOIN (USING)

```sql
DELETE FROM products p
USING categories c
WHERE p.category_id = c.category_id
  AND c.is_active = false;
```

**Contoh:**

```sql
-- Delete orders dari inactive customers
DELETE FROM orders o
USING customers c
WHERE o.customer_id = c.customer_id
  AND c.status = 'deleted';
```

---

## DELETE dengan RETURNING

```sql
DELETE FROM products
WHERE product_id = 10
RETURNING product_id, product_name;
```

**Contoh:**

```sql
-- Log deleted items
DELETE FROM temporary_data
WHERE created_at < CURRENT_DATE - INTERVAL '7 days'
RETURNING *;
```

---

## TRUNCATE vs DELETE

### DELETE
- Menghapus rows satu per satu
- Bisa menggunakan WHERE
- Trigger akan fired
- Lebih lambat untuk table besar

```sql
DELETE FROM table_name WHERE condition;
```

### TRUNCATE
- Menghapus semua data sekaligus
- Tidak bisa menggunakan WHERE
- Trigger tidak fired
- Sangat cepat

```sql
TRUNCATE TABLE table_name;

-- Dengan restart identity
TRUNCATE TABLE table_name RESTART IDENTITY;

-- Dengan cascade
TRUNCATE TABLE table_name CASCADE;
```

**Contoh:**

```sql
-- Delete semua temporary data
TRUNCATE TABLE temp_logs;

-- Reset auto-increment
TRUNCATE TABLE test_data RESTART IDENTITY;
```

---

## Transactions

Gunakan transactions untuk operasi yang harus atomic.

```sql
BEGIN;

INSERT INTO orders (customer_id, total_amount)
VALUES (1, 100.00)
RETURNING order_id;

INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES (101, 5, 2, 50.00);

UPDATE products
SET stock_quantity = stock_quantity - 2
WHERE product_id = 5;

COMMIT;
-- Jika ada error, gunakan ROLLBACK;
```

**Contoh dengan Error Handling:**

```sql
BEGIN;

-- Try to insert order
INSERT INTO orders (customer_id, total_amount)
VALUES (1, 100.00);

-- Check stock
DO $$
BEGIN
    IF (SELECT stock_quantity FROM products WHERE product_id = 5) < 2 THEN
        RAISE EXCEPTION 'Insufficient stock';
    END IF;
END $$;

-- Update stock
UPDATE products
SET stock_quantity = stock_quantity - 2
WHERE product_id = 5;

COMMIT;
```

---

## Best Practices

### DO (Lakukan)

1. **Selalu gunakan WHERE untuk UPDATE/DELETE**
   ```sql
   -- Good
   UPDATE users SET status = 'active' WHERE user_id = 1;
   
   -- Dangerous
   UPDATE users SET status = 'active';  -- Updates ALL rows!
   ```

2. **Gunakan transactions untuk multiple operations**
   ```sql
   BEGIN;
   INSERT INTO ...;
   UPDATE ...;
   DELETE FROM ...;
   COMMIT;
   ```

3. **Test dengan SELECT dulu**
   ```sql
   -- Test WHERE clause
   SELECT * FROM users WHERE status = 'inactive';
   
   -- Lalu delete
   DELETE FROM users WHERE status = 'inactive';
   ```

4. **Backup data penting sebelum DELETE**
   ```sql
   -- Backup ke table lain
   CREATE TABLE users_backup AS SELECT * FROM users;
   
   -- Lalu delete
   DELETE FROM users WHERE ...;
   ```

5. **Gunakan RETURNING untuk audit**
   ```sql
   DELETE FROM products WHERE status = 'discontinued'
   RETURNING *;
   ```

### DON'T (Jangan)

1. Jangan UPDATE/DELETE tanpa WHERE
2. Jangan lupa COMMIT setelah BEGIN
3. Jangan insert duplicate data (use UPSERT)
4. Jangan delete data tanpa backup
5. Jangan lupa test di development dulu

---

## Common Patterns

### Upsert Pattern

```sql
INSERT INTO product_inventory (product_id, quantity)
VALUES (1, 100)
ON CONFLICT (product_id)
DO UPDATE SET 
    quantity = product_inventory.quantity + EXCLUDED.quantity,
    updated_at = CURRENT_TIMESTAMP;
```

### Soft Delete Pattern

```sql
-- Jangan hard delete, update status
UPDATE users
SET 
    is_deleted = true,
    deleted_at = CURRENT_TIMESTAMP
WHERE user_id = 1;

-- Query exclude deleted
SELECT * FROM users WHERE is_deleted = false;
```

### Batch Update

```sql
UPDATE products
SET is_featured = true
WHERE product_id IN (
    SELECT product_id
    FROM order_items
    GROUP BY product_id
    HAVING COUNT(*) > 100
);
```

---

## Troubleshooting

### Problem: UPDATE/DELETE affects wrong rows

**Solusi:** Test dengan SELECT dulu.

```sql
-- Test
SELECT * FROM users WHERE email LIKE '%spam%';

-- Jika OK, baru delete
DELETE FROM users WHERE email LIKE '%spam%';
```

### Problem: Foreign key violation saat DELETE

**Solusi:** Delete child records dulu atau gunakan CASCADE.

```sql
-- Opsi 1: Delete child dulu
DELETE FROM order_items WHERE order_id = 1;
DELETE FROM orders WHERE order_id = 1;

-- Opsi 2: Use CASCADE (if FK has ON DELETE CASCADE)
DELETE FROM orders WHERE order_id = 1;
```

### Problem: Lupa COMMIT

**Solusi:** Always COMMIT atau ROLLBACK setelah BEGIN.

```sql
BEGIN;
-- ... operations ...
COMMIT;  -- Jangan lupa!
```

---

## Rangkuman

- INSERT untuk menambah data baru
- Multiple inserts lebih efisien dari single insert berulang
- RETURNING mengembalikan data yang di-insert/update/delete
- ON CONFLICT untuk handle duplicate (UPSERT)
- UPDATE untuk mengubah data existing
- DELETE untuk menghapus data
- Selalu gunakan WHERE untuk UPDATE/DELETE
- TRUNCATE lebih cepat dari DELETE untuk hapus semua data
- Gunakan transactions untuk operasi yang harus atomic
- Test dengan SELECT sebelum UPDATE/DELETE
- Consider soft delete daripada hard delete

---

**Sebelumnya:** [07. Sorting dan Limiting](07-sorting-limiting.md)  
**Selanjutnya:** [09. Join Tables](09-join-tables.md)


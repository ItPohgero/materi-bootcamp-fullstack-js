# 5. Basic Query - SELECT

## Perintah SELECT

SELECT adalah perintah paling fundamental dalam SQL untuk mengambil data dari database.

### Syntax Dasar

```sql
SELECT column1, column2, ...
FROM table_name;
```

---

## SELECT Semua Kolom

Menggunakan `*` untuk memilih semua kolom.

```sql
SELECT * FROM users;
```

**Contoh Output:**

```
 id |  name   |     email      | age
----+---------+----------------+-----
  1 | John    | john@mail.com  | 25
  2 | Jane    | jane@mail.com  | 30
  3 | Bob     | bob@mail.com   | 28
```

**Catatan:** Hindari `SELECT *` di production code. Pilih kolom yang dibutuhkan saja untuk performa.

---

## SELECT Kolom Tertentu

```sql
SELECT column1, column2 FROM table_name;
```

**Contoh:**

```sql
SELECT name, email FROM users;
```

**Output:**

```
  name   |     email
---------+----------------
 John    | john@mail.com
 Jane    | jane@mail.com
 Bob     | bob@mail.com
```

---

## Alias (AS)

Memberikan nama alternatif untuk kolom atau tabel.

### Column Alias

```sql
SELECT 
    column_name AS alias_name
FROM table_name;
```

**Contoh:**

```sql
SELECT 
    name AS full_name,
    email AS email_address,
    age AS user_age
FROM users;
```

**Output:**

```
 full_name | email_address  | user_age
-----------+----------------+----------
 John      | john@mail.com  | 25
 Jane      | jane@mail.com  | 30
```

**Alias dengan spasi (gunakan quotes):**

```sql
SELECT 
    name AS "Full Name",
    email AS "Email Address"
FROM users;
```

### Table Alias

```sql
SELECT u.name, u.email
FROM users AS u;

-- AS optional
SELECT u.name, u.email
FROM users u;
```

---

## DISTINCT

Menghilangkan duplikat rows.

```sql
SELECT DISTINCT column_name
FROM table_name;
```

**Contoh:**

```sql
-- Tanpa DISTINCT
SELECT city FROM customers;
```

**Output:**

```
   city
----------
 Jakarta
 Bandung
 Jakarta
 Surabaya
 Bandung
```

```sql
-- Dengan DISTINCT
SELECT DISTINCT city FROM customers;
```

**Output:**

```
   city
----------
 Jakarta
 Bandung
 Surabaya
```

**DISTINCT Multiple Columns:**

```sql
SELECT DISTINCT city, country FROM customers;
```

---

## COUNT

Menghitung jumlah rows.

```sql
-- Count semua rows
SELECT COUNT(*) FROM users;

-- Count kolom tertentu (tidak hitung NULL)
SELECT COUNT(email) FROM users;

-- Count distinct values
SELECT COUNT(DISTINCT city) FROM customers;
```

**Contoh:**

```sql
SELECT COUNT(*) AS total_users FROM users;
```

**Output:**

```
 total_users
-------------
          10
```

**Count dengan alias:**

```sql
SELECT 
    COUNT(*) AS total_users,
    COUNT(DISTINCT city) AS total_cities
FROM customers;
```

---

## Expressions dan Calculations

Melakukan kalkulasi dalam SELECT.

### Arithmetic Operators

```sql
SELECT 
    price,
    quantity,
    price * quantity AS total,
    price * quantity * 0.1 AS tax
FROM order_items;
```

**Operators:**
- `+` : Addition
- `-` : Subtraction
- `*` : Multiplication
- `/` : Division
- `%` : Modulo
- `^` : Exponentiation

**Contoh:**

```sql
SELECT 
    product_name,
    price,
    price * 0.9 AS discounted_price,
    price - (price * 0.9) AS discount_amount
FROM products;
```

### String Concatenation

```sql
-- Using ||
SELECT 
    first_name || ' ' || last_name AS full_name
FROM users;

-- Using CONCAT()
SELECT 
    CONCAT(first_name, ' ', last_name) AS full_name
FROM users;
```

**Contoh:**

```sql
SELECT 
    'Customer: ' || name AS label,
    CONCAT(city, ', ', country) AS location
FROM customers;
```

---

## NULL Values

NULL adalah nilai yang tidak diketahui atau tidak ada.

### Cek NULL

```sql
-- IS NULL
SELECT * FROM users WHERE email IS NULL;

-- IS NOT NULL
SELECT * FROM users WHERE email IS NOT NULL;
```

### COALESCE

Mengembalikan nilai pertama yang tidak NULL.

```sql
SELECT 
    name,
    COALESCE(phone, 'No phone') AS phone_number
FROM users;
```

**Contoh:**

```sql
SELECT 
    product_name,
    COALESCE(discount_price, price) AS final_price
FROM products;
```

### NULLIF

Mengembalikan NULL jika dua nilai sama.

```sql
SELECT NULLIF(value1, value2);

-- Contoh
SELECT NULLIF(10, 10);  -- Returns NULL
SELECT NULLIF(10, 20);  -- Returns 10
```

---

## CASE Statement

Conditional logic dalam SELECT.

### Simple CASE

```sql
SELECT 
    name,
    CASE status
        WHEN 'active' THEN 'User is active'
        WHEN 'inactive' THEN 'User is inactive'
        WHEN 'banned' THEN 'User is banned'
        ELSE 'Unknown status'
    END AS status_description
FROM users;
```

### Searched CASE

```sql
SELECT 
    name,
    age,
    CASE 
        WHEN age < 18 THEN 'Minor'
        WHEN age BETWEEN 18 AND 65 THEN 'Adult'
        ELSE 'Senior'
    END AS age_group
FROM users;
```

**Contoh Praktis:**

```sql
SELECT 
    product_name,
    price,
    stock_quantity,
    CASE 
        WHEN stock_quantity = 0 THEN 'Out of Stock'
        WHEN stock_quantity < 10 THEN 'Low Stock'
        WHEN stock_quantity < 50 THEN 'Medium Stock'
        ELSE 'In Stock'
    END AS stock_status,
    CASE 
        WHEN price < 100 THEN 'Budget'
        WHEN price < 500 THEN 'Mid-Range'
        ELSE 'Premium'
    END AS price_category
FROM products;
```

---

## LIMIT dan OFFSET

Membatasi jumlah rows yang dikembalikan.

### LIMIT

```sql
-- Ambil 5 rows pertama
SELECT * FROM users LIMIT 5;
```

### OFFSET

```sql
-- Skip 5 rows pertama, ambil 10 rows berikutnya
SELECT * FROM users LIMIT 10 OFFSET 5;
```

**Untuk Pagination:**

```sql
-- Page 1 (rows 1-10)
SELECT * FROM products LIMIT 10 OFFSET 0;

-- Page 2 (rows 11-20)
SELECT * FROM products LIMIT 10 OFFSET 10;

-- Page 3 (rows 21-30)
SELECT * FROM products LIMIT 10 OFFSET 20;

-- Formula: OFFSET = (page_number - 1) * items_per_page
```

---

## Subqueries dalam SELECT

Subquery adalah query di dalam query.

```sql
SELECT 
    product_name,
    price,
    (SELECT AVG(price) FROM products) AS avg_price,
    price - (SELECT AVG(price) FROM products) AS difference_from_avg
FROM products;
```

**Contoh:**

```sql
SELECT 
    name,
    (SELECT COUNT(*) FROM orders WHERE orders.customer_id = customers.id) AS order_count
FROM customers;
```

---

## Casting dan Type Conversion

Mengubah tipe data.

```sql
-- Cast ke INTEGER
SELECT '123'::INTEGER;
SELECT CAST('123' AS INTEGER);

-- Cast ke VARCHAR/TEXT
SELECT 123::TEXT;
SELECT CAST(123 AS VARCHAR);

-- Cast date/time
SELECT '2025-01-15'::DATE;
SELECT CAST('2025-01-15' AS DATE);
```

**Dalam Query:**

```sql
SELECT 
    order_id,
    order_date::DATE AS date_only,
    total_amount::INTEGER AS rounded_amount
FROM orders;
```

---

## Functions dalam SELECT

### String Functions

```sql
SELECT 
    UPPER(name) AS uppercase_name,
    LOWER(email) AS lowercase_email,
    LENGTH(name) AS name_length,
    SUBSTRING(phone FROM 1 FOR 3) AS area_code,
    TRIM(name) AS trimmed_name
FROM users;
```

### Numeric Functions

```sql
SELECT 
    ROUND(price, 2) AS rounded_price,
    CEIL(price) AS ceiling_price,
    FLOOR(price) AS floor_price,
    ABS(discount) AS absolute_discount
FROM products;
```

### Date/Time Functions

```sql
SELECT 
    CURRENT_DATE AS today,
    CURRENT_TIME AS now_time,
    CURRENT_TIMESTAMP AS now_timestamp,
    NOW() AS now,
    AGE(birth_date) AS age,
    EXTRACT(YEAR FROM created_at) AS year
FROM users;
```

---

## Contoh Praktis: E-Commerce Query

```sql
-- Query kompleks dengan berbagai fitur
SELECT 
    p.product_id,
    p.product_name AS "Product Name",
    c.category_name AS "Category",
    p.price AS "Original Price",
    COALESCE(p.discount_price, p.price) AS "Final Price",
    p.price - COALESCE(p.discount_price, p.price) AS "Discount",
    ROUND((p.price - COALESCE(p.discount_price, p.price)) / p.price * 100, 2) AS "Discount %",
    p.stock_quantity AS "Stock",
    CASE 
        WHEN p.stock_quantity = 0 THEN 'Out of Stock'
        WHEN p.stock_quantity < 10 THEN 'Low Stock'
        ELSE 'In Stock'
    END AS "Status",
    CASE 
        WHEN p.price < 100 THEN 'Budget'
        WHEN p.price < 500 THEN 'Mid-Range'
        ELSE 'Premium'
    END AS "Price Category",
    (SELECT COUNT(*) FROM order_items oi WHERE oi.product_id = p.product_id) AS "Times Sold"
FROM products p
JOIN categories c ON p.category_id = c.category_id
WHERE p.is_active = true
LIMIT 20;
```

---

## Best Practices

### DO (Lakukan)

1. **Pilih kolom yang diperlukan saja**
   ```sql
   -- Good
   SELECT id, name, email FROM users;
   
   -- Avoid
   SELECT * FROM users;
   ```

2. **Gunakan alias yang jelas**
   ```sql
   SELECT 
       u.name AS customer_name,
       COUNT(o.order_id) AS total_orders
   FROM users u
   LEFT JOIN orders o ON u.id = o.customer_id;
   ```

3. **Format query dengan rapi**
   ```sql
   SELECT 
       column1,
       column2,
       column3
   FROM table_name
   WHERE condition
   ORDER BY column1;
   ```

4. **Gunakan LIMIT untuk testing**
   ```sql
   SELECT * FROM large_table LIMIT 10;
   ```

### DON'T (Jangan)

1. Jangan gunakan `SELECT *` di production
2. Jangan lupa LIMIT saat query tabel besar
3. Jangan buat query yang terlalu kompleks tanpa perlu

---

## Troubleshooting

### Problem: "column does not exist"

**Solusi:** Cek nama kolom dengan `\d table_name` atau gunakan quotes untuk case-sensitive.

### Problem: Query terlalu lambat

**Solusi:** 
- Tambahkan LIMIT
- Buat index
- Pilih kolom spesifik, bukan `*`

### Problem: NULL values unexpected

**Solusi:** Gunakan COALESCE atau IS NULL checks.

---

## Rangkuman

- SELECT digunakan untuk mengambil data dari tabel
- Gunakan kolom spesifik, hindari `SELECT *`
- AS untuk membuat alias
- DISTINCT untuk menghilangkan duplikat
- COUNT untuk menghitung rows
- Expressions untuk kalkulasi
- CASE untuk conditional logic
- LIMIT dan OFFSET untuk pagination
- COALESCE dan NULLIF untuk handle NULL
- Functions untuk manipulasi data

---

**Sebelumnya:** [04. Tipe Data PostgreSQL](04-tipe-data.md)  
**Selanjutnya:** [06. Filtering Data - WHERE](06-filtering-where.md)


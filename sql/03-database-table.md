# 3. Database dan Table

## Database

Database adalah container yang menyimpan semua objek database seperti tabel, view, function, dll.

### Membuat Database

```sql
-- Syntax dasar
CREATE DATABASE nama_database;

-- Dengan options
CREATE DATABASE nama_database
    WITH 
    OWNER = postgres
    ENCODING = 'UTF8'
    LC_COLLATE = 'en_US.UTF-8'
    LC_CTYPE = 'en_US.UTF-8'
    TEMPLATE = template0;
```

**Contoh:**

```sql
-- Buat database sederhana
CREATE DATABASE myshop;

-- Buat database dengan owner tertentu
CREATE DATABASE company_db OWNER admin_user;

-- Buat database dengan encoding khusus
CREATE DATABASE international_db
    WITH ENCODING = 'UTF8'
    LC_COLLATE = 'id_ID.UTF-8';
```

### Melihat Daftar Database

```sql
-- Di psql
\l
\list

-- Dengan SQL query
SELECT datname FROM pg_database;

-- Dengan detail
SELECT 
    datname as "Database",
    pg_size_pretty(pg_database_size(datname)) as "Size"
FROM pg_database
ORDER BY pg_database_size(datname) DESC;
```

### Menggunakan/Switch Database

```sql
-- Di psql
\c nama_database
\connect nama_database

-- Contoh
\c myshop
```

### Menghapus Database

```sql
DROP DATABASE nama_database;

-- Dengan IF EXISTS
DROP DATABASE IF EXISTS nama_database;
```

**Contoh:**

```sql
DROP DATABASE old_database;
DROP DATABASE IF EXISTS test_db;
```

**Catatan:** Tidak bisa menghapus database yang sedang digunakan.

### Rename Database

```sql
ALTER DATABASE nama_lama RENAME TO nama_baru;
```

**Contoh:**

```sql
ALTER DATABASE myshop RENAME TO online_shop;
```

---

## Table (Tabel)

Table adalah struktur yang menyimpan data dalam baris dan kolom.

### Membuat Table

**Syntax:**

```sql
CREATE TABLE nama_table (
    kolom1 tipe_data constraints,
    kolom2 tipe_data constraints,
    ...
);
```

**Contoh Sederhana:**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Contoh Lengkap:**

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    stock_quantity INTEGER DEFAULT 0,
    category VARCHAR(50),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Constraints (Batasan)

Constraints adalah aturan yang diterapkan pada kolom untuk menjaga integritas data.

#### PRIMARY KEY

Mengidentifikasi secara unik setiap baris.

```sql
-- Single column
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- Composite primary key
CREATE TABLE order_details (
    order_id INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    PRIMARY KEY (order_id, product_id)
);
```

#### FOREIGN KEY

Membuat relasi ke tabel lain.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Dengan ON DELETE dan ON UPDATE
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

#### NOT NULL

Kolom harus memiliki nilai (tidak boleh kosong).

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

#### UNIQUE

Nilai dalam kolom harus unik.

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(100) UNIQUE
);
```

#### CHECK

Validasi nilai kolom.

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10, 2) CHECK (price > 0),
    stock_quantity INTEGER CHECK (stock_quantity >= 0),
    discount_percent INTEGER CHECK (discount_percent BETWEEN 0 AND 100)
);
```

#### DEFAULT

Nilai default jika tidak diisi.

```sql
CREATE TABLE posts (
    post_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    status VARCHAR(20) DEFAULT 'draft',
    views INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Melihat Struktur Table

### Di psql

```sql
-- List semua tabel
\dt

-- Describe table
\d nama_table

-- Describe dengan detail
\d+ nama_table
```

### Dengan SQL Query

```sql
-- List semua tabel
SELECT table_name 
FROM information_schema.tables 
WHERE table_schema = 'public';

-- Lihat kolom dalam tabel
SELECT column_name, data_type, character_maximum_length
FROM information_schema.columns
WHERE table_name = 'users';

-- Lihat detail lengkap
SELECT 
    column_name,
    data_type,
    is_nullable,
    column_default
FROM information_schema.columns
WHERE table_name = 'products'
ORDER BY ordinal_position;
```

---

## Mengubah Table

### ALTER TABLE - Menambah Kolom

```sql
ALTER TABLE nama_table 
ADD COLUMN nama_kolom tipe_data constraints;
```

**Contoh:**

```sql
-- Tambah kolom tunggal
ALTER TABLE users 
ADD COLUMN phone VARCHAR(20);

-- Tambah kolom dengan default
ALTER TABLE products 
ADD COLUMN brand VARCHAR(50) DEFAULT 'Generic';

-- Tambah multiple kolom
ALTER TABLE customers
ADD COLUMN address TEXT,
ADD COLUMN city VARCHAR(50),
ADD COLUMN postal_code VARCHAR(10);
```

### ALTER TABLE - Menghapus Kolom

```sql
ALTER TABLE nama_table 
DROP COLUMN nama_kolom;
```

**Contoh:**

```sql
ALTER TABLE users DROP COLUMN phone;

-- Drop dengan cascade (hapus dependencies)
ALTER TABLE users DROP COLUMN email CASCADE;
```

### ALTER TABLE - Mengubah Tipe Data

```sql
ALTER TABLE nama_table 
ALTER COLUMN nama_kolom TYPE tipe_data_baru;
```

**Contoh:**

```sql
-- Ubah tipe data
ALTER TABLE products 
ALTER COLUMN price TYPE NUMERIC(12, 2);

-- Dengan USING untuk konversi
ALTER TABLE users 
ALTER COLUMN age TYPE INTEGER USING age::INTEGER;
```

### ALTER TABLE - Rename Kolom

```sql
ALTER TABLE nama_table 
RENAME COLUMN nama_lama TO nama_baru;
```

**Contoh:**

```sql
ALTER TABLE users 
RENAME COLUMN username TO user_name;
```

### ALTER TABLE - Menambah Constraint

```sql
-- Add NOT NULL
ALTER TABLE users 
ALTER COLUMN email SET NOT NULL;

-- Add UNIQUE
ALTER TABLE users 
ADD CONSTRAINT users_email_unique UNIQUE (email);

-- Add CHECK
ALTER TABLE products 
ADD CONSTRAINT price_positive CHECK (price > 0);

-- Add FOREIGN KEY
ALTER TABLE orders 
ADD CONSTRAINT fk_customer 
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);
```

### ALTER TABLE - Menghapus Constraint

```sql
ALTER TABLE nama_table 
DROP CONSTRAINT nama_constraint;
```

**Contoh:**

```sql
ALTER TABLE products 
DROP CONSTRAINT price_positive;

ALTER TABLE users 
DROP CONSTRAINT users_email_unique;
```

---

## Menghapus Table

```sql
-- Drop table
DROP TABLE nama_table;

-- Drop dengan IF EXISTS
DROP TABLE IF EXISTS nama_table;

-- Drop dengan CASCADE (hapus dependencies)
DROP TABLE nama_table CASCADE;

-- Drop multiple tables
DROP TABLE table1, table2, table3;
```

**Contoh:**

```sql
DROP TABLE IF EXISTS old_users;
DROP TABLE temp_data CASCADE;
```

---

## TRUNCATE Table

Menghapus semua data dalam tabel tanpa menghapus struktur tabel.

```sql
TRUNCATE TABLE nama_table;

-- Restart identity (reset SERIAL)
TRUNCATE TABLE nama_table RESTART IDENTITY;

-- Dengan CASCADE
TRUNCATE TABLE nama_table CASCADE;
```

**Perbedaan TRUNCATE vs DELETE:**

| Aspek | TRUNCATE | DELETE |
|-------|----------|--------|
| **Speed** | Sangat cepat | Lebih lambat |
| **WHERE** | Tidak bisa | Bisa |
| **Rollback** | Bisa (dalam transaction) | Bisa |
| **Trigger** | Tidak trigger | Trigger |
| **Reset Identity** | Bisa | Tidak |

---

## Contoh Praktis: Membuat Database E-Commerce

```sql
-- 1. Buat database
CREATE DATABASE ecommerce;

-- 2. Connect ke database
\c ecommerce

-- 3. Buat tabel customers
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    city VARCHAR(50),
    country VARCHAR(50),
    postal_code VARCHAR(10),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 4. Buat tabel categories
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL UNIQUE,
    description TEXT
);

-- 5. Buat tabel products
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    stock_quantity INTEGER DEFAULT 0 CHECK (stock_quantity >= 0),
    category_id INTEGER,
    image_url VARCHAR(255),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

-- 6. Buat tabel orders
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) DEFAULT 'pending',
    shipping_address TEXT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- 7. Buat tabel order_details
CREATE TABLE order_details (
    order_detail_id SERIAL PRIMARY KEY,
    order_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    price DECIMAL(10, 2) NOT NULL,
    subtotal DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- 8. Lihat semua tabel
\dt
```

---

## Best Practices

### Naming Conventions

- Gunakan lowercase dengan underscore: `user_profiles`
- Nama tabel plural: `users`, `products`
- Nama kolom singular: `user_id`, `product_name`
- Primary key: `table_name_id` atau `id`
- Foreign key: `referenced_table_id`

### Design Tips

- Selalu gunakan PRIMARY KEY
- Gunakan SERIAL atau UUID untuk ID
- Definisikan NOT NULL untuk kolom penting
- Gunakan CHECK constraint untuk validasi
- Buat INDEX untuk kolom yang sering di-query
- Gunakan FOREIGN KEY untuk integritas referensial
- Tambahkan `created_at` dan `updated_at` untuk audit

### Security

- Jangan simpan password plaintext
- Gunakan proper data types
- Validasi data dengan constraints
- Limit character length
- Backup database secara regular

---

## Troubleshooting

### Problem: "relation already exists"

**Solusi:**

```sql
DROP TABLE IF EXISTS nama_table;
CREATE TABLE nama_table (...);
```

### Problem: "cannot drop table because other objects depend on it"

**Solusi:**

```sql
DROP TABLE nama_table CASCADE;
```

### Problem: "permission denied for table"

**Solusi:**

```sql
GRANT ALL PRIVILEGES ON TABLE nama_table TO username;
```

---

## Rangkuman

- Database adalah container untuk menyimpan tabel dan objek lain
- CREATE DATABASE untuk membuat database baru
- CREATE TABLE untuk membuat tabel dengan kolom dan constraints
- Constraints: PRIMARY KEY, FOREIGN KEY, NOT NULL, UNIQUE, CHECK, DEFAULT
- ALTER TABLE untuk mengubah struktur tabel
- DROP TABLE untuk menghapus tabel
- TRUNCATE untuk menghapus semua data tanpa menghapus struktur
- Gunakan naming conventions yang konsisten
- Selalu definisikan constraints untuk integritas data

---

**Sebelumnya:** [02. Instalasi dan Konfigurasi](02-instalasi-konfigurasi.md)  
**Selanjutnya:** [04. Tipe Data PostgreSQL](04-tipe-data.md)


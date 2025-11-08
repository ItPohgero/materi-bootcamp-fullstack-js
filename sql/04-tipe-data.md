# 4. Tipe Data PostgreSQL

## Overview Tipe Data

PostgreSQL mendukung berbagai macam tipe data untuk menyimpan berbagai jenis informasi. Memilih tipe data yang tepat penting untuk:

- Efisiensi penyimpanan
- Performa query
- Integritas data
- Validasi data

---

## Tipe Data Numerik

### Integer Types

**SMALLINT**
- Range: -32,768 to 32,767
- Storage: 2 bytes
- Use case: ID untuk tabel kecil, flags

```sql
CREATE TABLE products (
    stock_quantity SMALLINT
);
```

**INTEGER (INT)**
- Range: -2,147,483,648 to 2,147,483,647
- Storage: 4 bytes
- Use case: ID, counters, quantities

```sql
CREATE TABLE users (
    user_id INTEGER,
    age INTEGER
);
```

**BIGINT**
- Range: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
- Storage: 8 bytes
- Use case: Large IDs, timestamps

```sql
CREATE TABLE transactions (
    transaction_id BIGINT,
    amount_cents BIGINT
);
```

**SERIAL Types (Auto-increment)**

```sql
-- SMALLSERIAL (auto-increment SMALLINT)
CREATE TABLE small_table (
    id SMALLSERIAL PRIMARY KEY
);

-- SERIAL (auto-increment INTEGER)
CREATE TABLE medium_table (
    id SERIAL PRIMARY KEY
);

-- BIGSERIAL (auto-increment BIGINT)
CREATE TABLE large_table (
    id BIGSERIAL PRIMARY KEY
);
```

### Decimal Types

**DECIMAL / NUMERIC**
- Precision: up to 131072 digits before decimal, 16383 after
- Storage: variable
- Use case: Money, exact calculations

```sql
CREATE TABLE products (
    price DECIMAL(10, 2),  -- 10 digits total, 2 after decimal
    weight NUMERIC(8, 3)   -- 8 digits total, 3 after decimal
);

-- Contoh nilai
INSERT INTO products (price, weight) VALUES (199.99, 1.500);
```

**REAL**
- Precision: 6 decimal digits
- Storage: 4 bytes
- Use case: Scientific calculations (approximate)

```sql
CREATE TABLE measurements (
    temperature REAL
);
```

**DOUBLE PRECISION**
- Precision: 15 decimal digits
- Storage: 8 bytes
- Use case: Scientific calculations (approximate)

```sql
CREATE TABLE calculations (
    result DOUBLE PRECISION
);
```

**Catatan:** Gunakan DECIMAL/NUMERIC untuk money, bukan REAL atau DOUBLE PRECISION.

---

## Tipe Data String/Text

**VARCHAR(n)**
- Variable length dengan limit
- Storage: actual length + overhead
- Use case: String dengan panjang terbatas

```sql
CREATE TABLE users (
    username VARCHAR(50),
    email VARCHAR(100)
);
```

**CHAR(n)**
- Fixed length (padding dengan space)
- Storage: n bytes
- Use case: Fixed-length codes

```sql
CREATE TABLE countries (
    country_code CHAR(2),  -- 'US', 'ID', etc
    iso_code CHAR(3)       -- 'USA', 'IDN', etc
);
```

**TEXT**
- Unlimited length
- Storage: actual length + overhead
- Use case: Long text, descriptions

```sql
CREATE TABLE posts (
    title VARCHAR(200),
    content TEXT
);
```

**Perbandingan:**

| Type | Length | Storage | Use Case |
|------|--------|---------|----------|
| CHAR(n) | Fixed | n bytes | Fixed codes |
| VARCHAR(n) | Variable, limited | actual + overhead | Usernames, emails |
| TEXT | Unlimited | actual + overhead | Articles, descriptions |

---

## Tipe Data Date/Time

**DATE**
- Format: YYYY-MM-DD
- Range: 4713 BC to 5874897 AD
- Storage: 4 bytes

```sql
CREATE TABLE users (
    birth_date DATE,
    registration_date DATE DEFAULT CURRENT_DATE
);

-- Insert
INSERT INTO users (birth_date) VALUES ('1990-05-15');
```

**TIME**
- Format: HH:MI:SS
- Range: 00:00:00 to 24:00:00
- Storage: 8 bytes

```sql
CREATE TABLE schedules (
    start_time TIME,
    end_time TIME
);

-- Insert
INSERT INTO schedules VALUES ('09:00:00', '17:00:00');
```

**TIMESTAMP**
- Format: YYYY-MM-DD HH:MI:SS
- Range: 4713 BC to 294276 AD
- Storage: 8 bytes

```sql
CREATE TABLE logs (
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert
INSERT INTO logs (created_at) VALUES ('2025-01-15 10:30:45');
```

**TIMESTAMP WITH TIME ZONE (TIMESTAMPTZ)**
- Sama seperti TIMESTAMP tapi menyimpan timezone
- Recommended untuk aplikasi global

```sql
CREATE TABLE events (
    event_time TIMESTAMPTZ DEFAULT NOW()
);

-- Insert
INSERT INTO events (event_time) VALUES ('2025-01-15 10:30:45+07');
```

**INTERVAL**
- Menyimpan periode waktu

```sql
CREATE TABLE subscriptions (
    duration INTERVAL
);

-- Insert
INSERT INTO subscriptions VALUES ('1 month');
INSERT INTO subscriptions VALUES ('30 days');
INSERT INTO subscriptions VALUES ('2 years 3 months');
```

---

## Tipe Data Boolean

**BOOLEAN (BOOL)**
- Values: TRUE, FALSE, NULL
- Storage: 1 byte

```sql
CREATE TABLE products (
    is_active BOOLEAN DEFAULT true,
    is_featured BOOLEAN DEFAULT false
);

-- Insert
INSERT INTO products VALUES (true, false);
INSERT INTO products VALUES ('yes', 'no');  -- Also valid
INSERT INTO products VALUES (1, 0);         -- Also valid
```

---

## Tipe Data JSON

**JSON**
- Text-based JSON
- Storage: as text
- Slower for queries

**JSONB**
- Binary JSON (Recommended)
- Storage: binary format
- Faster for queries, supports indexing

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    attributes JSONB
);

-- Insert
INSERT INTO products (product_name, attributes) VALUES
('Laptop', '{"brand": "Dell", "ram": "16GB", "storage": "512GB SSD"}'),
('Phone', '{"brand": "Samsung", "color": "Black", "storage": "128GB"}');

-- Query JSON
SELECT product_name, attributes->>'brand' as brand
FROM products;

-- Query nested JSON
SELECT product_name, attributes->'specs'->>'cpu' as cpu
FROM products;
```

---

## Tipe Data Array

PostgreSQL mendukung array untuk semua tipe data.

```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    scores INTEGER[],
    tags TEXT[]
);

-- Insert
INSERT INTO students (name, scores, tags) VALUES
('John', ARRAY[85, 90, 78], ARRAY['math', 'science']),
('Jane', '{92, 88, 95}', '{"english", "history"}');  -- Alternative syntax

-- Query array
SELECT name, scores[1] as first_score FROM students;

-- Search in array
SELECT * FROM students WHERE 'math' = ANY(tags);
```

---

## Tipe Data UUID

UUID (Universally Unique Identifier) untuk ID yang globally unique.

```sql
-- Enable extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE users (
    user_id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    username VARCHAR(50)
);

-- Insert
INSERT INTO users (username) VALUES ('john_doe');
-- user_id otomatis generate: '550e8400-e29b-41d4-a716-446655440000'
```

---

## Tipe Data Enum

Custom type dengan nilai terbatas.

```sql
-- Buat enum type
CREATE TYPE order_status AS ENUM ('pending', 'processing', 'shipped', 'delivered', 'cancelled');

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    status order_status DEFAULT 'pending'
);

-- Insert
INSERT INTO orders (status) VALUES ('pending');
INSERT INTO orders (status) VALUES ('shipped');
-- INSERT INTO orders (status) VALUES ('invalid');  -- Error!
```

---

## Tipe Data Geometric

Untuk data spasial dan geometric.

```sql
CREATE TABLE locations (
    location_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    coordinates POINT,
    area CIRCLE,
    boundaries POLYGON
);

-- Insert
INSERT INTO locations (name, coordinates) 
VALUES ('Office', POINT(106.8456, -6.2088));  -- Jakarta coordinates
```

---

## Tipe Data Binary

**BYTEA**
- Binary data
- Use case: Images, files (not recommended for large files)

```sql
CREATE TABLE files (
    file_id SERIAL PRIMARY KEY,
    file_name VARCHAR(255),
    file_data BYTEA
);
```

**Catatan:** Untuk file besar, lebih baik simpan di filesystem dan simpan path di database.

---

## Conversion dan Casting

### Explicit Casting

```sql
-- Syntax: CAST(value AS type)
SELECT CAST('123' AS INTEGER);
SELECT CAST('2025-01-15' AS DATE);

-- Syntax: value::type (PostgreSQL specific)
SELECT '123'::INTEGER;
SELECT '2025-01-15'::DATE;
SELECT '{"name": "John"}'::JSONB;
```

### Contoh Praktis

```sql
-- String to number
SELECT '123'::INTEGER + 10;  -- Result: 133

-- Number to string
SELECT 123::VARCHAR || ' items';  -- Result: '123 items'

-- Date formatting
SELECT TO_CHAR(CURRENT_DATE, 'DD-MM-YYYY');

-- String to date
SELECT TO_DATE('15-01-2025', 'DD-MM-YYYY');
```

---

## Best Practices

### Memilih Tipe Data yang Tepat

**Untuk ID:**
- Small table: SERIAL (INTEGER)
- Large table: BIGSERIAL
- Distributed system: UUID

**Untuk Money:**
- DECIMAL(10, 2) atau NUMERIC(10, 2)
- Jangan gunakan REAL atau DOUBLE PRECISION

**Untuk String:**
- Known max length: VARCHAR(n)
- Unknown/large: TEXT
- Fixed length codes: CHAR(n)

**Untuk Date/Time:**
- Date only: DATE
- Time only: TIME
- Date + time: TIMESTAMP
- With timezone: TIMESTAMPTZ (recommended)

**Untuk True/False:**
- BOOLEAN

**Untuk JSON Data:**
- JSONB (bukan JSON)

### Storage Considerations

```sql
-- Lihat ukuran tabel
SELECT 
    pg_size_pretty(pg_total_relation_size('table_name')) as size;

-- Lihat ukuran per tipe data
SELECT 
    column_name,
    data_type,
    character_maximum_length
FROM information_schema.columns
WHERE table_name = 'table_name';
```

### Performance Tips

1. Gunakan INTEGER bukan BIGINT jika tidak perlu
2. Gunakan VARCHAR dengan limit yang wajar
3. Gunakan JSONB bukan JSON untuk query
4. Index kolom yang sering di-query
5. Gunakan appropriate precision untuk DECIMAL

---

## Contoh Tabel dengan Berbagai Tipe Data

```sql
CREATE TABLE comprehensive_example (
    -- Numeric
    id BIGSERIAL PRIMARY KEY,
    small_number SMALLINT,
    regular_number INTEGER,
    big_number BIGINT,
    decimal_number DECIMAL(10, 2),
    
    -- String/Text
    short_text VARCHAR(50),
    medium_text VARCHAR(255),
    long_text TEXT,
    fixed_code CHAR(5),
    
    -- Date/Time
    birth_date DATE,
    appointment_time TIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    event_time TIMESTAMPTZ,
    duration INTERVAL,
    
    -- Boolean
    is_active BOOLEAN DEFAULT true,
    
    -- JSON
    metadata JSONB,
    
    -- Array
    tags TEXT[],
    scores INTEGER[],
    
    -- UUID
    uuid UUID DEFAULT uuid_generate_v4()
);
```

---

## Rangkuman

- PostgreSQL menyediakan banyak tipe data built-in
- Numeric: SMALLINT, INTEGER, BIGINT, SERIAL, DECIMAL, REAL, DOUBLE PRECISION
- String: CHAR(n), VARCHAR(n), TEXT
- Date/Time: DATE, TIME, TIMESTAMP, TIMESTAMPTZ, INTERVAL
- Boolean: BOOLEAN (TRUE/FALSE)
- JSON: JSON dan JSONB (gunakan JSONB)
- Array: Mendukung array untuk semua tipe data
- UUID: Untuk globally unique identifiers
- Pilih tipe data yang tepat untuk efisiensi dan integritas
- Gunakan DECIMAL untuk money, TIMESTAMPTZ untuk timestamp
- Cast dengan `::` atau `CAST()` function

---

**Sebelumnya:** [03. Database dan Table](03-database-table.md)  
**Selanjutnya:** [05. Basic Query - SELECT](05-basic-query-select.md)


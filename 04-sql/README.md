# Materi SQL PostgreSQL Dasar

Selamat datang di materi pembelajaran SQL PostgreSQL dasar! Materi ini disusun secara berurutan untuk memudahkan Anda mempelajari SQL PostgreSQL dari nol hingga mahir.

---

## Daftar Materi

### 1. [Pengenalan SQL dan PostgreSQL](01-pengenalan-sql-postgresql.md)
- Apa itu SQL dan PostgreSQL?
- Perbedaan SQL vs NoSQL
- Kapan menggunakan PostgreSQL?
- Konsep database relasional
- ACID properties
- Perintah SQL utama (DDL, DML, DCL, TCL)
- Keunggulan PostgreSQL

### 2. [Instalasi dan Konfigurasi PostgreSQL](02-instalasi-konfigurasi.md)
- Instalasi di Windows, macOS, dan Linux
- Konfigurasi awal PostgreSQL
- Koneksi ke PostgreSQL
- Perintah dasar psql
- File konfigurasi (postgresql.conf, pg_hba.conf)
- Instalasi pgAdmin
- Environment variables

### 3. [Database dan Table](03-database-table.md)
- Membuat dan mengelola database
- Membuat table dengan constraints
- Primary Key, Foreign Key, NOT NULL, UNIQUE, CHECK, DEFAULT
- Melihat struktur table
- ALTER TABLE (menambah, menghapus, mengubah kolom)
- DROP dan TRUNCATE table
- Best practices design database

### 4. [Tipe Data PostgreSQL](04-tipe-data.md)
- Tipe data numerik (INTEGER, BIGINT, SERIAL, DECIMAL, REAL)
- Tipe data string (VARCHAR, CHAR, TEXT)
- Tipe data date/time (DATE, TIME, TIMESTAMP, INTERVAL)
- Boolean
- JSON dan JSONB
- Array
- UUID
- Enum
- Type casting dan conversion

### 5. [Basic Query - SELECT](05-basic-query-select.md)
- SELECT statement dasar
- SELECT semua kolom vs kolom tertentu
- Alias dengan AS
- DISTINCT
- COUNT
- Expressions dan calculations
- String concatenation
- NULL handling (COALESCE, NULLIF)
- CASE statement
- LIMIT dan OFFSET
- Subqueries dalam SELECT

### 6. [Filtering Data - WHERE](06-filtering-where.md)
- Comparison operators (=, !=, >, <, >=, <=)
- Logical operators (AND, OR, NOT)
- BETWEEN
- IN dan NOT IN
- LIKE dan ILIKE (pattern matching)
- IS NULL / IS NOT NULL
- EXISTS
- ANY / ALL
- Filtering dengan date/time
- Performance tips

### 7. [Sorting dan Limiting](07-sorting-limiting.md)
- ORDER BY (ASC, DESC)
- Multiple column sorting
- ORDER BY dengan expressions
- ORDER BY dengan CASE
- NULLS FIRST / NULLS LAST
- LIMIT untuk membatasi hasil
- OFFSET untuk skip rows
- Pagination (LIMIT + OFFSET)
- FETCH (SQL standard)
- Performance optimization

### 8. [Insert, Update, Delete](08-insert-update-delete.md)
- INSERT single dan multiple rows
- INSERT dengan DEFAULT
- INSERT dengan RETURNING
- INSERT dari SELECT
- ON CONFLICT (UPSERT)
- UPDATE data
- UPDATE dengan CASE
- UPDATE dengan subquery
- DELETE data
- DELETE dengan subquery
- TRUNCATE vs DELETE
- Transactions (BEGIN, COMMIT, ROLLBACK)

### 9. [Join Tables](09-join-tables.md)
- INNER JOIN
- LEFT JOIN (LEFT OUTER JOIN)
- RIGHT JOIN (RIGHT OUTER JOIN)
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN
- Multiple JOINs
- JOIN dengan WHERE
- JOIN dengan aggregation
- USING clause
- Performance tips untuk JOIN

### 10. [Aggregate Functions dan GROUP BY](10-aggregate-functions.md)
- COUNT, SUM, AVG, MIN, MAX
- GROUP BY untuk mengelompokkan data
- GROUP BY dengan multiple columns
- HAVING untuk filter hasil GROUP BY
- WHERE vs HAVING
- Window functions (ROW_NUMBER, RANK, DENSE_RANK)
- Running totals
- Moving averages
- STRING_AGG dan ARRAY_AGG
- FILTER clause
- ROLLUP, CUBE, GROUPING SETS

---

## Cara Menggunakan Materi Ini

### Untuk Pemula:
1. **Baca berurutan** dari materi 1 hingga 10
2. **Practice** setiap query yang diajarkan
3. **Buat database test** untuk eksperimen
4. **Jangan skip** materi dasar meskipun terlihat mudah

### Untuk Review:
- Gunakan sebagai **reference** saat bekerja
- **Bookmark** materi yang sering digunakan
- **Explore** section troubleshooting untuk solve masalah

---

## Tips Belajar SQL

1. **Practice, practice, practice!**
   - SQL adalah skill yang harus dipraktekkan
   - Buat database test untuk eksperimen

2. **Mulai dari query sederhana**
   - Master SELECT, WHERE, ORDER BY dulu
   - Lalu lanjut ke JOIN dan aggregation

3. **Pahami data structure**
   - Lihat struktur tabel dengan `\d table_name`
   - Pahami relasi antar tabel

4. **Gunakan `EXPLAIN` untuk optimization**
   - Lihat execution plan
   - Identifikasi bottleneck

5. **Baca error message dengan teliti**
   - PostgreSQL memberikan error message yang informatif
   - Seringkali sudah include hint solusinya

---

## Query Penting yang Sering Digunakan

```sql
-- Lihat daftar database
\l

-- Connect ke database
\c database_name

-- Lihat daftar tabel
\dt

-- Describe tabel
\d table_name

-- Basic SELECT
SELECT * FROM table_name LIMIT 10;

-- SELECT dengan WHERE
SELECT column1, column2 
FROM table_name 
WHERE condition
ORDER BY column1;

-- COUNT
SELECT COUNT(*) FROM table_name;

-- JOIN
SELECT t1.column, t2.column
FROM table1 t1
JOIN table2 t2 ON t1.id = t2.foreign_id;

-- GROUP BY
SELECT category, COUNT(*), AVG(price)
FROM products
GROUP BY category;

-- INSERT
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);

-- UPDATE
UPDATE table_name
SET column1 = value1
WHERE condition;

-- DELETE
DELETE FROM table_name
WHERE condition;
```

---

## Resources Tambahan

### Dokumentasi Official
- [PostgreSQL Official Documentation](https://www.postgresql.org/docs/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

### Tools
- **psql** - Command-line interface
- **pgAdmin** - GUI tool untuk PostgreSQL
- **DBeaver** - Universal database tool
- **DataGrip** - IDE dari JetBrains
- **TablePlus** - Modern database client

### Online Practice
- [SQLZoo](https://sqlzoo.net/)
- [LeetCode Database](https://leetcode.com/problemset/database/)
- [HackerRank SQL](https://www.hackerrank.com/domains/sql)

### Cheat Sheets
- [PostgreSQL Cheat Sheet](https://www.postgresqltutorial.com/postgresql-cheat-sheet/)

---

## Butuh Bantuan?

Jika menemui kesulitan:

1. **Baca ulang materi** yang relevan
2. **Cek section troubleshooting** di setiap materi
3. **Gunakan `\h command`** untuk help di psql
4. **Search di Stack Overflow** untuk error spesifik
5. **Practice dengan dataset test** sebelum apply ke production

---

## Proyek Latihan

Untuk mengasah skill SQL, coba buat:

1. **Database Toko Online**
   - Tabel: customers, products, orders, order_items
   - Practice: JOIN, aggregation, reporting

2. **Database Perpustakaan**
   - Tabel: books, members, borrowings
   - Practice: date operations, status tracking

3. **Database Blog**
   - Tabel: users, posts, comments, tags
   - Practice: many-to-many relationships, hierarchical data

4. **Database HR**
   - Tabel: employees, departments, salaries
   - Practice: self-join, aggregation, reporting

---

## Best Practices

### Design Database

- Normalize data (minimal 3NF)
- Gunakan PRIMARY KEY di setiap tabel
- Index kolom yang sering di-query
- Gunakan FOREIGN KEY untuk referential integrity
- Definisikan constraints (NOT NULL, CHECK, UNIQUE)

### Writing Queries

- Select kolom spesifik, hindari SELECT *
- Gunakan WHERE untuk filter data
- Gunakan LIMIT saat testing
- Format query dengan rapi untuk readability
- Comment query yang complex

### Performance

- Index foreign keys
- Avoid function pada indexed column di WHERE
- Gunakan EXISTS daripada IN untuk subquery
- Batch operations untuk INSERT/UPDATE banyak data
- Monitor dengan EXPLAIN ANALYZE

### Security

- Jangan simpan password plaintext
- Gunakan parameterized queries (prevent SQL injection)
- Principle of least privilege untuk users
- Regular backup database
- Audit trail untuk perubahan penting

---

## Progress Tracking

- [ ] 1. Pengenalan SQL dan PostgreSQL
- [ ] 2. Instalasi dan Konfigurasi
- [ ] 3. Database dan Table
- [ ] 4. Tipe Data PostgreSQL
- [ ] 5. Basic Query - SELECT
- [ ] 6. Filtering Data - WHERE
- [ ] 7. Sorting dan Limiting
- [ ] 8. Insert, Update, Delete
- [ ] 9. Join Tables
- [ ] 10. Aggregate Functions dan GROUP BY

---

## Selamat Belajar!

SQL adalah skill fundamental untuk data analysis, backend development, dan banyak role lainnya. Dengan menguasai SQL PostgreSQL, Anda akan:

- Dapat mengelola data dengan efisien
- Membuat query complex untuk analysis
- Optimize database performance
- Memahami relational database design
- Siap untuk advanced topics

**Happy Learning!**

---

*Materi ini dibuat untuk membantu Anda belajar SQL PostgreSQL dari dasar hingga mahir.*


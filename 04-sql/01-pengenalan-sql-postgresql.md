# 1. Pengenalan SQL dan PostgreSQL

## Apa itu SQL?

SQL (Structured Query Language) adalah bahasa standar untuk mengelola dan memanipulasi database relasional. SQL digunakan untuk:

- Menyimpan data
- Mengambil data
- Memperbarui data
- Menghapus data
- Membuat database dan tabel baru
- Mengatur permission dan security

## Apa itu PostgreSQL?

PostgreSQL adalah sistem manajemen database relasional (RDBMS) open-source yang powerful dan advanced. PostgreSQL dikenal dengan:

- **Reliability** - Stabil dan dapat diandalkan
- **Feature Rich** - Mendukung fitur-fitur advanced
- **Open Source** - Gratis dan community-driven
- **ACID Compliant** - Menjamin integritas data
- **Extensible** - Dapat diperluas dengan extension

### Sejarah PostgreSQL

- Dimulai tahun 1986 di University of California, Berkeley
- Originally bernama POSTGRES
- Tahun 1996 berganti nama menjadi PostgreSQL
- Terus berkembang dengan dukungan komunitas global

## SQL vs NoSQL

| Aspek | SQL (PostgreSQL) | NoSQL (MongoDB, dll) |
|-------|------------------|---------------------|
| **Struktur** | Terstruktur (tabel, baris, kolom) | Fleksibel (document, key-value) |
| **Schema** | Schema tetap | Schema dinamis |
| **Relasi** | Mendukung JOIN | Terbatas atau tidak ada |
| **Skalabilitas** | Vertical scaling | Horizontal scaling |
| **Use Case** | Data terstruktur, transaksi | Data tidak terstruktur, Big Data |

## Kapan Menggunakan PostgreSQL?

PostgreSQL cocok untuk:

- Aplikasi enterprise yang kompleks
- E-commerce dan sistem pembayaran
- Sistem yang membutuhkan integritas data tinggi
- Aplikasi dengan relasi data kompleks
- Analitik dan reporting
- Geospatial data (dengan PostGIS extension)

## Konsep Database Relasional

### Database
Container yang menyimpan semua data dan objek database.

### Table (Tabel)
Struktur yang menyimpan data dalam baris dan kolom.

### Row (Baris)
Satu record atau entry data dalam tabel.

### Column (Kolom)
Field atau atribut dari data.

### Primary Key
Kolom atau kombinasi kolom yang unik mengidentifikasi setiap baris.

### Foreign Key
Kolom yang merujuk ke primary key di tabel lain (untuk relasi).

### Contoh Struktur Tabel

```
Tabel: users
+----+----------+------------------+---------------------+
| id | username | email            | created_at          |
+----+----------+------------------+---------------------+
| 1  | john_doe | john@example.com | 2025-01-01 10:00:00 |
| 2  | jane_doe | jane@example.com | 2025-01-02 11:30:00 |
+----+----------+------------------+---------------------+
```

## ACID Properties

PostgreSQL mendukung ACID properties:

### Atomicity (Atomik)
Transaksi berhasil sempurna atau gagal sempurna (tidak ada hasil separuh jalan).

### Consistency (Konsistensi)
Data selalu dalam keadaan valid sesuai aturan yang ditentukan.

### Isolation (Isolasi)
Transaksi concurrent tidak saling mengganggu.

### Durability (Ketahanan)
Data yang sudah di-commit tetap ada meskipun terjadi kegagalan sistem.

## Perintah SQL Utama

### DDL (Data Definition Language)
Perintah untuk mendefinisikan struktur database:
- `CREATE` - Membuat database, table, index
- `ALTER` - Mengubah struktur table
- `DROP` - Menghapus database atau table
- `TRUNCATE` - Menghapus semua data dalam table

### DML (Data Manipulation Language)
Perintah untuk manipulasi data:
- `SELECT` - Mengambil data
- `INSERT` - Menambah data baru
- `UPDATE` - Memperbarui data
- `DELETE` - Menghapus data

### DCL (Data Control Language)
Perintah untuk kontrol akses:
- `GRANT` - Memberikan permission
- `REVOKE` - Mencabut permission

### TCL (Transaction Control Language)
Perintah untuk kontrol transaksi:
- `BEGIN` - Memulai transaksi
- `COMMIT` - Menyimpan perubahan
- `ROLLBACK` - Membatalkan perubahan

## Keunggulan PostgreSQL

- **Open Source dan Gratis** - Tidak ada biaya lisensi
- **Standards Compliant** - Mengikuti standar SQL
- **Extensible** - Dapat menambahkan custom functions, data types
- **MVCC** - Multi-Version Concurrency Control untuk performa tinggi
- **Full Text Search** - Pencarian teks lengkap built-in
- **JSON Support** - Mendukung data JSON dan JSONB
- **PostGIS** - Extension untuk geospatial data
- **Replication** - Mendukung streaming replication
- **Community Support** - Komunitas besar dan aktif

## PostgreSQL vs MySQL

| Fitur | PostgreSQL | MySQL |
|-------|-----------|-------|
| **Lisensi** | PostgreSQL License | GPL |
| **Standards** | Sangat comply dengan SQL standard | Beberapa deviation |
| **Kompleksitas Query** | Sangat baik | Baik |
| **Concurrency** | Lebih baik (MVCC) | Baik |
| **Extensibility** | Sangat extensible | Terbatas |
| **JSON** | JSONB (binary JSON) | JSON |
| **Full Text Search** | Built-in | Terbatas |

## Arsitektur PostgreSQL

```
Client Application
       ↓
PostgreSQL Server
       ↓
    Postmaster (Main Process)
       ↓
  ┌────┴────┐
  ↓         ↓
Backend   Shared
Processes Memory
  ↓         ↓
Database Files
```

### Komponen Utama:

1. **Postmaster** - Process utama yang mengelola koneksi
2. **Backend Process** - Satu process per koneksi client
3. **Shared Memory** - Memory yang dibagi antar process
4. **WAL (Write-Ahead Log)** - Log untuk recovery
5. **Background Workers** - Process untuk maintenance

## Tools PostgreSQL

### Command Line
- **psql** - Interactive terminal untuk PostgreSQL
- **pg_dump** - Backup database
- **pg_restore** - Restore database
- **createdb** - Membuat database
- **dropdb** - Menghapus database

### GUI Tools
- **pgAdmin** - Tool administrasi official
- **DBeaver** - Universal database tool
- **DataGrip** - IDE dari JetBrains
- **TablePlus** - Modern database client
- **Postico** - PostgreSQL client untuk Mac

## Istilah Penting

- **Schema** - Namespace dalam database untuk mengelompokkan objek
- **Index** - Struktur data untuk mempercepat query
- **View** - Virtual table dari hasil query
- **Stored Procedure** - Function yang tersimpan di database
- **Trigger** - Function yang otomatis dijalankan saat event tertentu
- **Constraint** - Aturan untuk menjaga integritas data
- **Transaction** - Unit kerja yang terdiri dari satu atau lebih operasi

## Contoh Use Case

### E-Commerce
- Menyimpan data produk, user, order
- Transaksi pembayaran
- Inventory management
- Order history

### Banking
- Account management
- Transaction processing
- Balance tracking
- Audit trail

### Social Media
- User profiles
- Posts dan comments
- Followers/following
- Activity feeds

---

## Rangkuman

- SQL adalah bahasa standar untuk database relasional
- PostgreSQL adalah RDBMS open-source yang powerful
- Database relasional menggunakan tabel dengan baris dan kolom
- PostgreSQL mendukung ACID properties
- Ada 4 kategori perintah SQL: DDL, DML, DCL, TCL
- PostgreSQL cocok untuk aplikasi yang membutuhkan integritas data tinggi

---

**Selanjutnya:** [02. Instalasi dan Konfigurasi PostgreSQL](02-instalasi-konfigurasi.md)


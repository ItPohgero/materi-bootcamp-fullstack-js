# 2. Instalasi dan Konfigurasi PostgreSQL

## Instalasi PostgreSQL

### Windows

**Cara 1: PostgreSQL Installer (Rekomendasi)**

1. Download installer dari https://www.postgresql.org/download/windows/
2. Jalankan file installer
3. Ikuti wizard instalasi:
   - Pilih direktori instalasi
   - Pilih komponen (PostgreSQL Server, pgAdmin, Command Line Tools)
   - Pilih direktori data
   - Set password untuk user `postgres`
   - Pilih port (default: 5432)
4. Selesaikan instalasi

**Cara 2: Using Chocolatey**

```powershell
choco install postgresql
```

**Verifikasi Instalasi:**

```cmd
psql --version
```

### macOS

**Cara 1: Homebrew (Rekomendasi)**

```bash
# Install PostgreSQL
brew install postgresql@15

# Start service
brew services start postgresql@15

# atau start manual
pg_ctl -D /usr/local/var/postgres start
```

**Cara 2: Postgres.app**

1. Download dari https://postgresapp.com/
2. Drag ke Applications folder
3. Buka aplikasi
4. Initialize server

**Verifikasi:**

```bash
psql --version
```

### Linux

**Ubuntu/Debian:**

```bash
# Update package list
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Cek status service
sudo systemctl status postgresql

# Start service jika belum running
sudo systemctl start postgresql

# Enable auto-start on boot
sudo systemctl enable postgresql
```

**Fedora/RHEL:**

```bash
sudo dnf install postgresql-server postgresql-contrib
sudo postgresql-setup --initdb
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

**Arch Linux:**

```bash
sudo pacman -S postgresql
sudo su - postgres -c "initdb --locale=en_US.UTF-8 -D /var/lib/postgres/data"
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

## Konfigurasi Awal

### Set Password untuk User postgres

**Linux/macOS:**

```bash
# Switch ke user postgres
sudo -u postgres psql

# Di dalam psql, set password
ALTER USER postgres PASSWORD 'password_anda';

# Exit
\q
```

**Windows:**

Password sudah di-set saat instalasi.

### Membuat User Database Baru

```bash
# Dari terminal/command line
sudo -u postgres createuser --interactive

# atau dari psql
sudo -u postgres psql
CREATE USER myuser WITH PASSWORD 'mypassword';
```

### Memberikan Privileges

```sql
-- Buat database
CREATE DATABASE mydatabase;

-- Berikan semua privileges
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;

-- Atau berikan privileges spesifik
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO myuser;
```

---

## Koneksi ke PostgreSQL

### Menggunakan psql (Command Line)

**Syntax:**

```bash
psql -h hostname -p port -U username -d database
```

**Contoh:**

```bash
# Koneksi sebagai user postgres ke database postgres
psql -U postgres

# Koneksi ke database spesifik
psql -U postgres -d mydatabase

# Koneksi ke remote server
psql -h 192.168.1.100 -p 5432 -U myuser -d mydatabase

# Koneksi dengan URL
psql postgresql://username:password@localhost:5432/database
```

**Opsi psql:**

- `-h` : Hostname (default: localhost)
- `-p` : Port (default: 5432)
- `-U` : Username
- `-d` : Database name
- `-W` : Prompt password

---

## Perintah Dasar psql

### Meta-Commands (dimulai dengan backslash)

```sql
-- Help
\?                    -- List semua meta-commands
\h                    -- SQL command help
\h SELECT            -- Help untuk command tertentu

-- Database
\l                    -- List databases
\c database_name      -- Connect ke database
\dn                   -- List schemas

-- Tables
\dt                   -- List tables
\dt+                  -- List tables dengan detail
\d table_name         -- Describe table
\d+ table_name        -- Describe table dengan detail

-- Users dan Roles
\du                   -- List users/roles
\du+                  -- List users dengan detail

-- Views, Indexes, Sequences
\dv                   -- List views
\di                   -- List indexes
\ds                   -- List sequences

-- Functions
\df                   -- List functions

-- System
\timing               -- Toggle timing
\x                    -- Toggle expanded display
\q                    -- Quit psql
```

### Contoh Penggunaan

```sql
-- Lihat daftar database
\l

-- Connect ke database
\c mydatabase

-- Lihat daftar tabel
\dt

-- Describe tabel users
\d users

-- Keluar dari psql
\q
```

---

## File Konfigurasi PostgreSQL

### postgresql.conf

File konfigurasi utama PostgreSQL. Lokasi:

- **Linux**: `/etc/postgresql/[version]/main/postgresql.conf`
- **macOS (Homebrew)**: `/usr/local/var/postgres/postgresql.conf`
- **Windows**: `C:\Program Files\PostgreSQL\[version]\data\postgresql.conf`

**Setting Penting:**

```conf
# Connection Settings
listen_addresses = 'localhost'    # Host yang bisa connect
port = 5432                        # Port PostgreSQL

# Memory Settings
shared_buffers = 128MB             # Memory untuk caching
work_mem = 4MB                     # Memory untuk operasi sort/hash

# Query Planning
effective_cache_size = 4GB         # Total memory untuk caching

# Logging
log_destination = 'stderr'
logging_collector = on
log_directory = 'log'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
```

### pg_hba.conf

File untuk konfigurasi autentikasi. Lokasi sama dengan `postgresql.conf`.

**Format:**

```
TYPE  DATABASE  USER  ADDRESS      METHOD
```

**Contoh:**

```conf
# Local connections
local   all       all                 peer

# IPv4 local connections
host    all       all   127.0.0.1/32  md5

# IPv6 local connections
host    all       all   ::1/128       md5

# Allow remote connections
host    all       all   0.0.0.0/0     md5
```

**Authentication Methods:**

- `trust` - Izinkan tanpa password (tidak aman)
- `md5` - Password dengan MD5 hash
- `scram-sha-256` - Password dengan SCRAM-SHA-256 (lebih aman)
- `peer` - OS user authentication (Linux/macOS)
- `ident` - Ident server authentication

---

## Mengelola PostgreSQL Service

### Linux (systemd)

```bash
# Start service
sudo systemctl start postgresql

# Stop service
sudo systemctl stop postgresql

# Restart service
sudo systemctl restart postgresql

# Status service
sudo systemctl status postgresql

# Enable auto-start
sudo systemctl enable postgresql

# Disable auto-start
sudo systemctl disable postgresql
```

### macOS (Homebrew)

```bash
# Start service
brew services start postgresql@15

# Stop service
brew services stop postgresql@15

# Restart service
brew services restart postgresql@15

# List services
brew services list
```

### Windows

```cmd
# Via Services GUI
services.msc

# Via command line (run as Administrator)
net start postgresql-x64-15
net stop postgresql-x64-15
```

---

## Instalasi pgAdmin

pgAdmin adalah GUI tool untuk mengelola PostgreSQL.

### Windows/macOS

Biasanya sudah include dalam PostgreSQL installer.

### Linux

```bash
# Ubuntu/Debian
sudo apt install pgadmin4

# atau install web version
sudo apt install pgadmin4-web
```

### Akses pgAdmin

- **Desktop**: Buka aplikasi pgAdmin
- **Web**: `http://localhost/pgadmin4`

### Konfigurasi Server di pgAdmin

1. Klik kanan pada "Servers" → "Register" → "Server"
2. Tab "General":
   - Name: `Local PostgreSQL`
3. Tab "Connection":
   - Host: `localhost`
   - Port: `5432`
   - Username: `postgres`
   - Password: `your_password`
4. Save

---

## Environment Variables

Set environment variables untuk kemudahan:

### Linux/macOS

Tambahkan ke `~/.bashrc` atau `~/.zshrc`:

```bash
export PGHOST=localhost
export PGPORT=5432
export PGUSER=postgres
export PGDATABASE=mydatabase
export PGPASSWORD=mypassword  # Hati-hati dengan security
```

Atau gunakan `.pgpass` file yang lebih aman:

```bash
# Create .pgpass file
touch ~/.pgpass
chmod 0600 ~/.pgpass

# Format: hostname:port:database:username:password
echo "localhost:5432:*:postgres:mypassword" >> ~/.pgpass
```

### Windows

Set via System Properties → Environment Variables atau PowerShell:

```powershell
$env:PGHOST = "localhost"
$env:PGPORT = "5432"
$env:PGUSER = "postgres"
```

---

## Testing Koneksi

### Test dengan psql

```bash
psql -U postgres -c "SELECT version();"
```

### Test dengan Python (psycopg2)

```python
import psycopg2

try:
    conn = psycopg2.connect(
        host="localhost",
        port="5432",
        database="postgres",
        user="postgres",
        password="your_password"
    )
    print("Connected successfully!")
    conn.close()
except Exception as e:
    print(f"Error: {e}")
```

### Test dengan Node.js (pg)

```javascript
const { Client } = require('pg');

const client = new Client({
  host: 'localhost',
  port: 5432,
  database: 'postgres',
  user: 'postgres',
  password: 'your_password'
});

client.connect()
  .then(() => {
    console.log('Connected successfully!');
    client.end();
  })
  .catch(err => console.error('Connection error', err));
```

---

## Troubleshooting

### Problem: "psql: command not found"

**Solusi:**

Tambahkan PostgreSQL bin ke PATH.

**macOS:**

```bash
echo 'export PATH="/usr/local/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Linux:**

```bash
echo 'export PATH="/usr/lib/postgresql/15/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Problem: "Connection refused"

**Solusi:**

1. Cek apakah service running:
   ```bash
   sudo systemctl status postgresql
   ```

2. Cek port:
   ```bash
   sudo lsof -i :5432
   ```

3. Cek konfigurasi di `postgresql.conf`:
   ```conf
   listen_addresses = 'localhost'
   port = 5432
   ```

### Problem: "FATAL: Peer authentication failed"

**Solusi:**

Edit `pg_hba.conf`, ganti `peer` dengan `md5`:

```conf
# Sebelum
local   all   all   peer

# Sesudah
local   all   all   md5
```

Restart PostgreSQL.

### Problem: "password authentication failed"

**Solusi:**

Reset password:

```bash
sudo -u postgres psql
ALTER USER postgres PASSWORD 'newpassword';
```

---

## Best Practices

### Security

1. Jangan gunakan user `postgres` untuk aplikasi
2. Buat user spesifik dengan privileges minimal
3. Gunakan `scram-sha-256` authentication
4. Jangan expose port ke internet tanpa firewall
5. Backup `.pgpass` secara aman

### Performance

1. Tune `shared_buffers` (25% dari RAM)
2. Set `effective_cache_size` (50-75% dari RAM)
3. Adjust `work_mem` sesuai kebutuhan
4. Enable query logging untuk monitoring

### Maintenance

1. Enable auto-vacuum
2. Regular backup database
3. Monitor disk space
4. Update PostgreSQL secara berkala

---

## Rangkuman

- PostgreSQL dapat diinstall di Windows, macOS, dan Linux
- psql adalah command-line tool untuk berinteraksi dengan PostgreSQL
- pgAdmin adalah GUI tool untuk administrasi
- File konfigurasi utama: `postgresql.conf` dan `pg_hba.conf`
- Gunakan environment variables atau `.pgpass` untuk kemudahan akses
- Selalu perhatikan security dan best practices

---

**Sebelumnya:** [01. Pengenalan SQL dan PostgreSQL](01-pengenalan-sql-postgresql.md)  
**Selanjutnya:** [03. Database dan Table](03-database-table.md)


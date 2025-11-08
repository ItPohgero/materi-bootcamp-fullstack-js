# 3. Membuat Repository

## Apa itu Repository?

Repository (atau repo) adalah folder project yang dikelola oleh Git. Repository menyimpan seluruh history perubahan file dan folder di dalamnya.

---

## Dua Cara Membuat Repository

### 1. Git Init - Membuat Repository Baru

Membuat repository baru dari folder yang sudah ada.

### 2. Git Clone - Menyalin Repository yang Sudah Ada

Mengambil copy dari repository yang sudah ada (biasanya dari GitHub).

---

## Metode 1: Git Init

### Membuat Repository Lokal Baru

**Langkah 1: Buat folder project**

```bash
mkdir project-saya
cd project-saya
```

**Langkah 2: Inisialisasi Git**

```bash
git init
```

**Output:**

```
Initialized empty Git repository in /path/to/project-saya/.git/
```

**Apa yang terjadi?**

- Git membuat folder tersembunyi `.git` di dalam folder project
- Folder `.git` berisi semua informasi Git (history, configuration, dll)
- Repository sekarang siap digunakan

### Melihat Folder .git

```bash
# Linux/Mac
ls -la

# Windows (Git Bash)
ls -la

# Output akan menampilkan folder .git
```

⚠️ **PENTING:** Jangan hapus atau edit folder `.git` secara manual!

---

## Contoh Praktis: Membuat Project Baru

```bash
# 1. Buat folder project
mkdir website-portfolio
cd website-portfolio

# 2. Inisialisasi git
git init

# 3. Buat file baru
echo "# Website Portfolio Saya" > README.md
echo "<h1>Hello World</h1>" > index.html

# 4. Cek status
git status

# 5. Tambahkan file ke staging area
git add .

# 6. Buat commit pertama
git commit -m "Initial commit: tambah README dan index.html"
```

---

## Metode 2: Git Clone

### Menyalin Repository dari Remote

Git clone digunakan untuk mengambil copy lengkap dari repository yang sudah ada (biasanya dari GitHub, GitLab, atau Bitbucket).

### Syntax Dasar

```bash
git clone <url-repository>
```

### Contoh Clone dari GitHub

**1. Clone dengan HTTPS:**

```bash
git clone https://github.com/username/nama-repo.git
```

**2. Clone dengan SSH:**

```bash
git clone git@github.com:username/nama-repo.git
```

**3. Clone ke folder dengan nama custom:**

```bash
git clone https://github.com/username/nama-repo.git nama-folder-baru
```

### Contoh Praktis

```bash
# Clone repository React
git clone https://github.com/facebook/react.git

# Masuk ke folder repository
cd react

# Lihat history
git log --oneline

# Lihat remote
git remote -v
```

---

## Perbedaan Git Init vs Git Clone

| Aspek | Git Init | Git Clone |
|-------|----------|-----------|
| **Tujuan** | Membuat repo baru dari nol | Menyalin repo yang sudah ada |
| **Remote** | Tidak ada remote | Otomatis terhubung ke remote |
| **History** | Mulai dari kosong | Dapat semua history |
| **Kapan digunakan** | Project baru | Berkontribusi ke project yang ada |

---

## Struktur Folder .git

Setelah `git init` atau `git clone`, folder `.git` akan berisi:

```
.git/
├── HEAD              # Pointer ke branch saat ini
├── config            # Konfigurasi repository
├── description       # Deskripsi repository
├── hooks/            # Script yang dijalankan saat event tertentu
├── objects/          # Database Git (menyimpan commits, files, dll)
├── refs/             # Referensi ke commits (branches, tags)
└── index             # Staging area
```

---

## Melihat Status Repository

Setelah membuat repository, gunakan `git status` untuk melihat kondisi repository:

```bash
git status
```

**Output untuk repository baru:**

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

---

## Best Practices

### ✅ DO (Lakukan)

1. **Buat file .gitignore sejak awal**

   ```bash
   # Contoh untuk Node.js project
   echo "node_modules/" > .gitignore
   echo ".env" >> .gitignore
   ```

2. **Buat README.md**

   ```bash
   echo "# Nama Project" > README.md
   ```

3. **Commit initial setup**

   ```bash
   git add .
   git commit -m "Initial commit: setup project"
   ```

4. **Set default branch ke 'main'** (jika belum di konfigurasi global)

   ```bash
   git branch -M main
   ```

### ❌ DON'T (Jangan)

1. ❌ Jangan run `git init` di home directory atau folder parent
2. ❌ Jangan hapus folder `.git` (akan hilang semua history)
3. ❌ Jangan init git di dalam git repository lain (nested repo)
4. ❌ Jangan commit file sensitif (password, API keys)

---

## Menghubungkan Repository Lokal ke Remote

Setelah `git init`, Anda perlu menghubungkan ke remote repository (GitHub):

### Langkah-langkah

**1. Buat repository di GitHub** (lewat web browser)

**2. Tambahkan remote:**

```bash
git remote add origin https://github.com/username/nama-repo.git
```

**3. Push ke remote:**

```bash
git branch -M main
git push -u origin main
```

---

## Memeriksa Remote Repository

```bash
# Lihat remote yang terhubung
git remote -v

# Output:
# origin  https://github.com/username/repo.git (fetch)
# origin  https://github.com/username/repo.git (push)
```

---

## Menghapus Repository

### Menghapus Git dari Repository (tetapi file tetap ada)

```bash
rm -rf .git
```

### Menghapus seluruh folder repository

```bash
cd ..
rm -rf nama-folder-repository
```

⚠️ **HATI-HATI:** Ini akan menghapus semua file dan history!

---

## Troubleshooting

### Problem: "fatal: not a git repository"

**Solusi:** Pastikan Anda berada di folder yang sudah di-init Git, atau jalankan `git init`

### Problem: "fatal: remote origin already exists"

**Solusi:** Remote sudah ada. Hapus dulu atau gunakan nama lain:

```bash
git remote remove origin
git remote add origin <url-baru>
```

### Problem: Clone sangat lambat

**Solusi:** Clone hanya branch tertentu atau shallow clone:

```bash
# Shallow clone (hanya commit terakhir)
git clone --depth 1 https://github.com/username/repo.git

# Clone branch tertentu
git clone -b branch-name --single-branch https://github.com/username/repo.git
```

---

## 🎯 Rangkuman

- **`git init`** - Membuat repository baru dari folder kosong
- **`git clone <url>`** - Menyalin repository yang sudah ada
- Folder `.git` menyimpan semua informasi Git (jangan dihapus!)
- Gunakan `git status` untuk melihat kondisi repository
- Hubungkan repository lokal ke remote dengan `git remote add origin <url>`

---

## 📝 Latihan

1. Buat folder baru, init git, dan buat commit pertama
2. Clone repository public dari GitHub
3. Hubungkan repository lokal ke GitHub repository baru

---

**Sebelumnya:** [02. Instalasi dan Konfigurasi](02-instalasi-konfigurasi.md)  
**Selanjutnya:** [04. Basic Workflow Git](04-basic-workflow.md)

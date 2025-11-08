# 4. Basic Workflow Git

## Siklus Dasar Git

Git memiliki alur kerja yang konsisten untuk mengelola perubahan:

```
[Working Directory] --git add--> [Staging Area] --git commit--> [Repository]
        ↑                                                            ↓
        └─────────────────── git checkout ──────────────────────────┘
```

---

## Tiga Area Utama

### 1. **Working Directory**
- Folder tempat Anda bekerja dan mengedit file
- Perubahan belum dilacak oleh Git

### 2. **Staging Area (Index)**
- Area perantara untuk mempersiapkan commit
- File yang sudah "dipilih" untuk disimpan

### 3. **Repository (.git directory)**
- Tempat penyimpanan permanen
- Berisi semua commit history

---

## Status File di Git

File dalam Git dapat memiliki beberapa status:

- **Untracked** - File baru yang belum dilacak Git
- **Unmodified** - File tidak berubah sejak commit terakhir
- **Modified** - File sudah diubah tapi belum di-add
- **Staged** - File sudah di-add, siap untuk di-commit

---

## Command: git status

Melihat status file dalam repository.

### Syntax
```bash
git status
```

### Output Contoh
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        style.css

no changes added to commit (use "git add" and/or "git commit -a")
```

### Opsi Berguna
```bash
# Status singkat
git status -s
git status --short

# Output:
# M  index.html    (M = Modified)
# ?? style.css     (?? = Untracked)
# A  script.js     (A = Added/Staged)
```

---

## Command: git add

Menambahkan file ke staging area.

### Syntax Dasar
```bash
git add <nama-file>
```

### Contoh Penggunaan

```bash
# Tambah satu file
git add index.html

# Tambah beberapa file
git add index.html style.css script.js

# Tambah semua file di folder tertentu
git add css/

# Tambah semua file yang diubah
git add .
# atau
git add -A
# atau
git add --all

# Tambah semua file dengan ekstensi tertentu
git add *.js
git add *.css
```

### Opsi Tambahan

```bash
# Interactive staging (memilih perubahan yang akan di-add)
git add -i

# Patch mode (pilih bagian spesifik dari file)
git add -p
```

---

## Command: git commit

Menyimpan perubahan dari staging area ke repository.

### Syntax Dasar
```bash
git commit -m "Pesan commit"
```

### Contoh Penggunaan

```bash
# Commit dengan pesan singkat
git commit -m "Tambah halaman about"

# Commit dengan pesan detail (multi-line)
git commit
# Akan membuka editor untuk menulis pesan lengkap

# Commit semua file yang diubah (skip staging)
git commit -a -m "Update semua file"
# atau
git commit -am "Update semua file"

# Amend commit terakhir (edit commit sebelumnya)
git commit --amend -m "Pesan baru"
```

### Best Practices Pesan Commit

#### Good Commit Messages
```bash
git commit -m "Tambah fitur login dengan Google OAuth"
git commit -m "Fix bug pada form validasi email"
git commit -m "Update dependencies ke versi terbaru"
git commit -m "Refactor fungsi calculateTotal untuk readability"
```

#### Bad Commit Messages
```bash
git commit -m "update"
git commit -m "fix"
git commit -m "changes"
git commit -m "asdfasdf"
```

### Format Commit Message (Conventional Commits)

```
<type>: <subject>

<body> (optional)

<footer> (optional)
```

**Types:**
- `feat:` - Fitur baru
- `fix:` - Bug fix
- `docs:` - Perubahan dokumentasi
- `style:` - Perubahan formatting (tidak mengubah kode)
- `refactor:` - Refactoring kode
- `test:` - Menambah atau update test
- `chore:` - Update build tasks, dependencies, dll

**Contoh:**
```bash
git commit -m "feat: tambah fitur dark mode"
git commit -m "fix: perbaiki overflow pada mobile view"
git commit -m "docs: update README dengan petunjuk instalasi"
```

---

## Workflow Lengkap: Contoh Praktis

### Scenario: Membuat Website Sederhana

```bash
# 1. Buat dan masuk ke folder project
mkdir website-sederhana
cd website-sederhana

# 2. Inisialisasi Git
git init

# 3. Buat file index.html
cat > index.html << EOF
<!DOCTYPE html>
<html>
<head>
    <title>Website Saya</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
EOF

# 4. Cek status
git status
# Output: index.html adalah untracked file

# 5. Tambahkan ke staging area
git add index.html

# 6. Cek status lagi
git status
# Output: index.html ada di staging area

# 7. Commit
git commit -m "Initial commit: tambah index.html"

# 8. Buat file CSS
cat > style.css << EOF
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
}
EOF

# 9. Edit index.html (tambah link ke CSS)
echo '<link rel="stylesheet" href="style.css">' >> index.html

# 10. Cek status
git status
# Output: 
# - style.css untracked
# - index.html modified

# 11. Add semua file
git add .

# 12. Commit
git commit -m "Tambah styling dengan CSS"

# 13. Lihat history
git log --oneline
```

---

## Command: git diff

Melihat perbedaan antara perubahan.

### Syntax dan Penggunaan

```bash
# Lihat perubahan yang belum di-staging
git diff

# Lihat perubahan yang sudah di-staging
git diff --staged
# atau
git diff --cached

# Lihat perubahan pada file tertentu
git diff index.html

# Lihat perbedaan antara dua commit
git diff commit1 commit2
```

### Contoh Output
```diff
diff --git a/index.html b/index.html
index abc123..def456 100644
--- a/index.html
+++ b/index.html
@@ -1,5 +1,6 @@
 <!DOCTYPE html>
 <html>
 <head>
     <title>Website Saya</title>
+    <link rel="stylesheet" href="style.css">
 </head>
```

---

## Command: git rm

Menghapus file dari Git repository.

### Syntax
```bash
# Hapus file dari Git dan working directory
git rm nama-file.txt

# Hapus file dari Git tapi tetap di working directory
git rm --cached nama-file.txt

# Hapus folder
git rm -r nama-folder/
```

### Contoh
```bash
# Hapus file
git rm old-file.txt
git commit -m "Hapus old-file.txt yang tidak diperlukan"

# Unstage file (berhenti tracking)
git rm --cached .env
git commit -m "Stop tracking .env file"
```

---

## Command: git mv

Rename atau memindahkan file.

### Syntax
```bash
git mv old-name.txt new-name.txt
```

### Contoh
```bash
# Rename file
git mv index.htm index.html
git commit -m "Rename index.htm ke index.html"

# Pindah file ke folder lain
git mv style.css css/style.css
git commit -m "Pindah style.css ke folder css/"
```

---

## Shortcut dan Tips

### Commit Skip Staging (Hati-hati!)
```bash
# Commit langsung semua file yang modified (tidak termasuk untracked)
git commit -am "Pesan commit"
```

### Melihat Status Singkat
```bash
git status -s
```

### Ignore File Sementara
```bash
# Git akan mengabaikan perubahan pada file ini
git update-index --assume-unchanged file.txt

# Untuk undo:
git update-index --no-assume-unchanged file.txt
```

---

## Troubleshooting

### Problem: Salah add file
```bash
# Unstage file tertentu
git restore --staged nama-file.txt

# Unstage semua file
git restore --staged .
```

### Problem: Ingin batal commit (belum push)
```bash
# Undo commit terakhir, perubahan tetap ada
git reset --soft HEAD~1

# Undo commit dan staging, perubahan tetap ada
git reset HEAD~1

# Undo commit dan buang perubahan (HATI-HATI!)
git reset --hard HEAD~1
```

### Problem: Salah tulis commit message
```bash
# Edit commit message terakhir
git commit --amend -m "Pesan yang benar"
```

### Problem: Lupa add file ke commit terakhir
```bash
# Tambah file yang terlupa
git add file-terlupa.txt

# Amend ke commit terakhir
git commit --amend --no-edit
```

---

## Rangkuman

- **`git status`** - Cek status file (modified, staged, untracked)
- **`git add <file>`** - Tambahkan file ke staging area
- **`git commit -m "pesan"`** - Simpan perubahan ke repository
- **`git diff`** - Lihat perubahan yang belum di-commit
- **Workflow:** Edit → Add → Commit → Repeat
- Tulis commit message yang jelas dan deskriptif

---

## Latihan

1. Buat repository baru dan buat 3 file
2. Add dan commit file satu per satu dengan pesan yang jelas
3. Edit file, lihat diff, lalu commit perubahannya
4. Gunakan `git log` untuk melihat history commit Anda

---

**Sebelumnya:** [03. Membuat Repository](03-membuat-repository.md)  
**Selanjutnya:** [05. Melihat History](05-melihat-history.md)


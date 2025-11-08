# 5. Melihat History Git

## Command: git log

Melihat history commit dalam repository.

### Syntax Dasar
```bash
git log
```

### Output Default
```
commit 3f2a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a
Author: Wahyu Agus Arifin <wahyu@example.com>
Date:   Sat Nov 8 10:30:00 2025 +0700

    Tambah fitur login

commit 2e1d0c9b8a7f6e5d4c3b2a1f0e9d8c7b6a5f4e3d
Author: Wahyu Agus Arifin <wahyu@example.com>
Date:   Fri Nov 7 15:20:00 2025 +0700

    Initial commit
```

---

## Variasi git log

### 1. Log Singkat (Oneline)
```bash
git log --oneline
```

Output:
```
3f2a1b9 Tambah fitur login
2e1d0c9 Initial commit
```

### 2. Log dengan Graph
```bash
git log --oneline --graph --all
```

Output:
```
* 3f2a1b9 (HEAD -> main) Tambah fitur login
* 2e1d0c9 Initial commit
```

### 3. Log dengan Statistik
```bash
git log --stat
```

Output menampilkan file yang diubah dan jumlah line changes.

### 4. Log Detailed
```bash
git log -p
# atau
git log --patch
```

Menampilkan full diff untuk setiap commit.

### 5. Log N Commit Terakhir
```bash
# 5 commit terakhir
git log -5

# 3 commit terakhir dengan format oneline
git log --oneline -3
```

### 6. Log dengan Filtering

```bash
# Log dari author tertentu
git log --author="Wahyu"

# Log dalam rentang tanggal
git log --since="2025-01-01"
git log --after="2 weeks ago"
git log --before="2025-11-01"
git log --until="yesterday"

# Log untuk file tertentu
git log -- index.html
git log --follow -- style.css  # Follow renames

# Log dengan keyword di commit message
git log --grep="fix"
git log --grep="login" --oneline
```

### 7. Custom Format
```bash
# Format custom
git log --pretty=format:"%h - %an, %ar : %s"

# Output:
# 3f2a1b9 - Wahyu, 2 hours ago : Tambah fitur login
# 2e1d0c9 - Wahyu, 1 day ago : Initial commit
```

**Format Placeholders:**
- `%H` - Commit hash (full)
- `%h` - Commit hash (abbreviated)
- `%an` - Author name
- `%ae` - Author email
- `%ar` - Author date, relative
- `%ad` - Author date
- `%s` - Subject (commit message)
- `%b` - Body (commit message detail)

---

## Command: git show

Melihat detail commit tertentu.

### Syntax
```bash
# Show commit terakhir
git show

# Show commit tertentu
git show <commit-hash>

# Show file tertentu dari commit
git show <commit-hash>:path/to/file.txt
```

### Contoh
```bash
# Lihat detail commit terakhir
git show

# Lihat commit spesifik
git show 3f2a1b9

# Lihat file pada commit tertentu
git show 3f2a1b9:index.html

# Lihat perubahan pada file tertentu di commit terakhir
git show HEAD:style.css
```

---

## Command: git log dengan Grafik

### Grafik Sederhana
```bash
git log --graph --oneline
```

### Grafik Lengkap dengan Semua Branch
```bash
git log --graph --all --decorate --oneline
```

### Grafik dengan Warna dan Info Lengkap
```bash
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
```

### Alias untuk Log yang Cantik
```bash
# Set alias
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Gunakan
git lg
```

---

## Melihat History File Tertentu

### Git Log untuk File
```bash
# History file tertentu
git log -- index.html

# Dengan patch (diff)
git log -p -- style.css

# Follow file yang di-rename
git log --follow -- config.js
```

### Git Blame (Siapa yang Mengubah)
```bash
# Lihat siapa yang mengubah setiap line
git blame index.html

# Dengan format lebih readable
git blame -L 10,20 index.html  # Line 10-20 saja

# Ignore whitespace changes
git blame -w index.html
```

Output contoh:
```
3f2a1b9c (Wahyu 2025-11-08 10:30:00 +0700  1) <!DOCTYPE html>
3f2a1b9c (Wahyu 2025-11-08 10:30:00 +0700  2) <html>
2e1d0c9b (Budi  2025-11-07 15:20:00 +0700  3) <head>
```

---

## Referensi Commit

### Cara Merujuk Commit

```bash
# Hash lengkap
3f2a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a

# Hash pendek (7 karakter pertama biasanya cukup)
3f2a1b9

# HEAD - commit terakhir di branch saat ini
HEAD

# HEAD~1 atau HEAD^ - 1 commit sebelum HEAD
HEAD~1
HEAD^

# HEAD~2 - 2 commit sebelum HEAD
HEAD~2

# Nama branch
main
feature-login

# Nama tag
v1.0.0
```

### Contoh Penggunaan
```bash
# Show commit terakhir
git show HEAD

# Show 2 commit sebelumnya
git show HEAD~2

# Diff antara 2 commit
git diff HEAD~3 HEAD

# Log antara 2 commit
git log HEAD~5..HEAD
```

---

## Git Reflog

Melihat history semua perubahan HEAD (termasuk yang sudah di-reset).

### Syntax
```bash
git reflog
```

### Output
```
3f2a1b9 HEAD@{0}: commit: Tambah fitur login
2e1d0c9 HEAD@{1}: commit: Update styling
1d0c9b8 HEAD@{2}: commit (initial): Initial commit
```

### Kapan Menggunakan?
- Recover commit yang terhapus
- Undo reset yang salah
- Lihat history navigasi antar commit

### Contoh Recovery
```bash
# Misal Anda salah reset
git reset --hard HEAD~3

# Lihat reflog
git reflog

# Recovery commit yang hilang
git reset --hard HEAD@{1}
```

---

## Melihat Perubahan Antar Commit

### Git Diff untuk Commit
```bash
# Diff antara 2 commit
git diff commit1 commit2

# Diff antara HEAD dan 3 commit sebelumnya
git diff HEAD~3 HEAD

# Diff file tertentu antara 2 commit
git diff commit1 commit2 -- index.html

# Stat (ringkasan) perubahan
git diff --stat commit1 commit2
```

---

## Mencari dalam History

### Git Log dengan Grep
```bash
# Cari commit dengan keyword di message
git log --grep="login"

# Cari commit yang mengubah code tertentu
git log -S"function login"

# Cari commit yang mengubah regex pattern
git log -G"def.*function"
```

### Git Bisect (Binary Search)
Mencari commit yang menimbulkan bug.

```bash
# Start bisect
git bisect start

# Mark commit saat ini sebagai bad
git bisect bad

# Mark commit yang bagus
git bisect good <commit-hash>

# Git akan checkout commit tengah
# Test apakah masih ada bug

# Jika masih bug
git bisect bad

# Jika sudah bagus
git bisect good

# Ulangi sampai ketemu commit yang bermasalah

# Selesai
git bisect reset
```

---

## Shortlog (Summary per Author)

```bash
# Summary commit per author
git shortlog

# Dengan jumlah commit
git shortlog -n -s

# Output:
#    15  Wahyu Agus Arifin
#     8  Budi Santoso
#     3  Ani Wijaya
```

---

## Tips dan Alias Berguna

### Alias Recommended

```bash
# Log cantik
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Log singkat
git config --global alias.ls "log --oneline --graph --all"

# Show last commit
git config --global alias.last "log -1 HEAD --stat"

# Contributors
git config --global alias.contributors "shortlog -n -s"
```

### Gunakan Alias
```bash
git lg
git ls
git last
git contributors
```

---

## Rangkuman

- **`git log`** - Melihat history commit
- **`git log --oneline`** - History singkat
- **`git log --graph`** - History dengan visualisasi branch
- **`git show <commit>`** - Detail commit tertentu
- **`git blame <file>`** - Siapa yang mengubah tiap line
- **`git reflog`** - History semua perubahan HEAD
- **`git diff commit1 commit2`** - Perbedaan antar commit

---

## Latihan

1. Buat beberapa commit dan lihat history dengan berbagai format
2. Gunakan `git blame` pada file untuk lihat siapa yang mengubah
3. Set alias `git lg` untuk log yang lebih cantik
4. Coba cari commit dengan keyword tertentu menggunakan `--grep`

---

**Sebelumnya:** [04. Basic Workflow](04-basic-workflow.md)  
**Selanjutnya:** [06. Bekerja dengan Branch](06-bekerja-dengan-branch.md)


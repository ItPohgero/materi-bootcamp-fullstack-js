# 6. Bekerja dengan Branch

## Apa itu Branch?

Branch adalah cabang independen dari kode yang memungkinkan Anda mengembangkan fitur, memperbaiki bug, atau bereksperimen tanpa mempengaruhi kode utama (main/master).

### Analogi
Branch seperti **timeline paralel** dalam cerita. Anda bisa membuat timeline alternatif untuk mencoba sesuatu, dan jika berhasil, gabungkan kembali ke timeline utama.

---

## Mengapa Menggunakan Branch?

- **Isolasi Pekerjaan** - Kerjakan fitur baru tanpa ganggu kode yang sudah stabil  
- **Kolaborasi** - Beberapa developer bekerja pada fitur berbeda secara bersamaan  
- **Eksperimen** - Coba ide baru tanpa risiko merusak kode utama  
- **Bug Fixes** - Perbaiki bug di branch terpisah  
- **Code Review** - Review kode sebelum digabung ke main  

---

## Konsep Branch

```
main:      A --- B --- C --- F (merge)
                    \       /
feature:             D --- E
```

- `main` adalah branch utama
- `feature` adalah branch baru yang dibuat dari commit C
- Setelah selesai, branch `feature` di-merge kembali ke `main`

---

## Melihat Branch

### Lihat Branch yang Ada
```bash
# Lihat branch lokal
git branch

# Output:
# * main        <- tanda * = branch aktif saat ini
#   feature-login
#   bugfix-payment
```

### Lihat Semua Branch (Lokal + Remote)
```bash
git branch -a
# atau
git branch --all
```

### Lihat Branch dengan Commit Terakhir
```bash
git branch -v

# Output:
# * main           3f2a1b9 Tambah fitur login
#   feature-login  2e1d0c9 Update form validation
```

---

## Membuat Branch

### Syntax
```bash
# Buat branch baru
git branch <nama-branch>

# Buat dan pindah ke branch baru
git checkout -b <nama-branch>
# atau (Git 2.23+)
git switch -c <nama-branch>
```

### Contoh
```bash
# Buat branch "feature-login"
git branch feature-login

# Buat dan langsung pindah ke branch "feature-payment"
git checkout -b feature-payment

# Atau dengan git switch (lebih modern)
git switch -c feature-dashboard
```

### Buat Branch dari Commit Tertentu
```bash
# Buat branch dari commit tertentu
git branch feature-x <commit-hash>

# Contoh
git branch bugfix-typo 3f2a1b9
```

---

## Pindah Branch

### Syntax
```bash
# Cara lama (masih bisa digunakan)
git checkout <nama-branch>

# Cara baru (Git 2.23+, lebih jelas)
git switch <nama-branch>
```

### Contoh
```bash
# Pindah ke branch main
git checkout main
# atau
git switch main

# Pindah ke branch feature-login
git checkout feature-login
# atau
git switch feature-login

# Pindah ke branch sebelumnya
git checkout -
# atau
git switch -
```

### Pindah Branch dengan Perubahan Uncommitted

⚠️ **Penting:** Sebelum pindah branch, pastikan:
1. Commit perubahan Anda, atau
2. Stash perubahan Anda (lihat materi Git Stash)

```bash
# Jika ada perubahan uncommitted
git stash
git switch other-branch
# ... kerja di branch lain ...
git switch main
git stash pop
```

---

## Mengganti Nama Branch

### Rename Branch Aktif
```bash
# Ganti nama branch saat ini
git branch -m <nama-baru>

# Contoh: ganti nama branch saat ini ke "feature-awesome"
git branch -m feature-awesome
```

### Rename Branch Lain
```bash
# Ganti nama branch lain
git branch -m <nama-lama> <nama-baru>

# Contoh
git branch -m old-feature new-feature
```

---

## Menghapus Branch

### Hapus Branch Lokal
```bash
# Hapus branch yang sudah di-merge
git branch -d <nama-branch>

# Hapus branch paksa (meski belum di-merge)
git branch -D <nama-branch>
```

### Contoh
```bash
# Hapus branch feature-login (aman, hanya jika sudah di-merge)
git branch -d feature-login

# Hapus branch feature-old paksa
git branch -D feature-old
```

### Hapus Branch Remote
```bash
# Hapus branch di remote repository
git push origin --delete <nama-branch>

# Contoh
git push origin --delete feature-old
```

---

## Workflow Branch yang Umum

### 1. Feature Branch Workflow

```bash
# 1. Pastikan di branch main dan update
git checkout main
git pull origin main

# 2. Buat branch baru untuk fitur
git checkout -b feature-login

# 3. Kerjakan fitur (edit, add, commit)
# ... edit file ...
git add .
git commit -m "Tambah form login"

# 4. Push branch ke remote
git push -u origin feature-login

# 5. Buat Pull Request di GitHub/GitLab

# 6. Setelah di-merge, hapus branch lokal
git checkout main
git pull origin main
git branch -d feature-login
```

### 2. Bugfix Workflow

```bash
# 1. Buat branch untuk bugfix
git checkout -b bugfix-navbar-overlap

# 2. Fix bug
# ... edit file ...
git add .
git commit -m "Fix: perbaiki navbar overlap pada mobile"

# 3. Push dan buat PR
git push -u origin bugfix-navbar-overlap

# 4. Merge dan cleanup
git checkout main
git pull
git branch -d bugfix-navbar-overlap
```

---

## Branch Naming Conventions

### Best Practices

```bash
# Feature
feature/user-authentication
feature/payment-integration
feat/dark-mode

# Bugfix
bugfix/login-error
fix/navbar-responsive
hotfix/security-patch

# Experimental
experiment/new-algorithm
exp/performance-test

# Release
release/v1.0.0
release/2025-11-08
```

### Format Umum
```
<type>/<short-description>
```

**Types:**
- `feature/` atau `feat/` - Fitur baru
- `bugfix/` atau `fix/` - Perbaikan bug
- `hotfix/` - Perbaikan urgent di production
- `release/` - Persiapan release
- `experiment/` atau `exp/` - Eksperimen
- `refactor/` - Refactoring code
- `docs/` - Dokumentasi

---

## Melihat Perbedaan Antar Branch

### Git Diff untuk Branch
```bash
# Lihat perbedaan antara 2 branch
git diff branch1 branch2

# Lihat perbedaan branch saat ini dengan main
git diff main

# Lihat file yang berbeda (tanpa diff detail)
git diff --name-only main feature-login

# Lihat statistik perubahan
git diff --stat main feature-login
```

### Git Log untuk Branch
```bash
# Lihat commit yang ada di feature-login tapi tidak di main
git log main..feature-login

# Lihat commit yang ada di main tapi tidak di feature-login
git log feature-login..main

# Grafik visual
git log --oneline --graph main feature-login
```

---

## Protected Branches

Di platform seperti GitHub/GitLab, Anda bisa melindungi branch penting:

### Kenapa Melindungi Branch?
- Cegah push langsung ke main/master
- Wajibkan Pull Request + Review
- Wajibkan CI/CD pass sebelum merge
- Cegah force push
- Cegah penghapusan branch

### Setting di GitHub:
1. Repository Settings → Branches
2. Branch protection rules
3. Protect `main` branch
4. Require pull request reviews before merging
5. Require status checks to pass

---

## Tips dan Trik

### Lihat Branch yang Sudah Di-merge
```bash
# Branch yang sudah di-merge ke branch saat ini
git branch --merged

# Branch yang belum di-merge
git branch --no-merged
```

### Cleanup Branch yang Sudah Di-merge
```bash
# Hapus semua branch lokal yang sudah di-merge ke main
git branch --merged main | grep -v "main" | xargs git branch -d
```

### Switch ke Branch dan Auto-create
```bash
# Jika branch belum ada di lokal tapi ada di remote
git switch feature-x
# Git otomatis create branch lokal yang track remote branch
```

### Lihat Remote Branch
```bash
# Lihat branch di remote
git branch -r

# Update info remote branch
git fetch origin

# Checkout remote branch
git checkout -b feature-x origin/feature-x
# atau
git switch -c feature-x origin/feature-x
```

---

## Troubleshooting

### Problem: "Cannot switch branch because you have uncommitted changes"

**Solusi 1: Commit perubahan**
```bash
git add .
git commit -m "WIP: work in progress"
git switch other-branch
```

**Solusi 2: Stash perubahan**
```bash
git stash
git switch other-branch
# ... kerja di branch lain ...
git switch original-branch
git stash pop
```

### Problem: "Branch already exists"
```bash
# Jika ingin overwrite
git branch -D feature-x
git checkout -b feature-x
```

### Problem: Salah branch saat commit
```bash
# Pindahkan commit ke branch yang benar
git log --oneline -1  # catat hash commit

# Undo commit di branch saat ini (soft reset)
git reset --soft HEAD~1

# Pindah ke branch yang benar
git switch correct-branch

# Commit ulang
git commit -c ORIG_HEAD
```

---

## Rangkuman

- **`git branch`** - Lihat daftar branch
- **`git branch <name>`** - Buat branch baru
- **`git checkout <name>`** atau **`git switch <name>`** - Pindah branch
- **`git checkout -b <name>`** atau **`git switch -c <name>`** - Buat dan pindah branch
- **`git branch -d <name>`** - Hapus branch
- **`git branch -m <new-name>`** - Rename branch
- Gunakan branch untuk isolasi pekerjaan dan kolaborasi
- Ikuti naming convention yang konsisten

---

## Latihan

1. Buat branch baru untuk fitur atau bugfix
2. Pindah antar branch dan lihat perubahan file
3. Buat beberapa commit di branch baru
4. Lihat perbedaan antara branch main dan branch baru
5. Rename dan hapus branch yang tidak diperlukan

---

**Sebelumnya:** [05. Melihat History](05-melihat-history.md)  
**Selanjutnya:** [07. Merging Branches](07-merging-branches.md)


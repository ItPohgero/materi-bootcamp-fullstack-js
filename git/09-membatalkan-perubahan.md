# 9. Membatalkan Perubahan

## Overview

Git menyediakan berbagai cara untuk membatalkan perubahan tergantung pada situasi:
- Perubahan di working directory
- Perubahan di staging area
- Commit yang sudah dibuat
- Commit yang sudah di-push

---

## Membatalkan Perubahan di Working Directory

### Command: git restore

Mengembalikan file ke kondisi commit terakhir (Git 2.23+).

### Syntax
```bash
# Restore file tertentu
git restore <file>

# Restore semua file
git restore .

# Restore dengan source tertentu
git restore --source=<commit> <file>
```

### Contoh
```bash
# Edit index.html
echo "Wrong change" >> index.html

# Lihat status
git status
# Output: modified: index.html

# Batalkan perubahan
git restore index.html

# File kembali ke kondisi commit terakhir
```

### Command Lama: git checkout
```bash
# Cara lama (masih bisa digunakan)
git checkout -- <file>
git checkout -- index.html
```

⚠️ **HATI-HATI:** Perubahan akan hilang permanen!

---

## Membatalkan Perubahan di Staging Area

### Command: git restore --staged

Unstage file yang sudah di-add.

### Syntax
```bash
# Unstage file tertentu
git restore --staged <file>

# Unstage semua file
git restore --staged .
```

### Contoh
```bash
# Add file ke staging
git add style.css

# Lihat status
git status
# Output: Changes to be committed: style.css

# Unstage file
git restore --staged style.css

# File tetap modified tapi tidak di staging
git status
# Output: Changes not staged for commit: style.css
```

### Command Lama: git reset
```bash
# Cara lama
git reset HEAD <file>
git reset HEAD style.css
```

---

## Membatalkan Commit

### 1. Git Reset

Menghapus commit dan mengubah history.

#### Jenis-jenis Reset

**A. Soft Reset**
Hapus commit, tapi perubahan tetap di staging area.

```bash
git reset --soft HEAD~1

# Setelah ini:
# - Commit dihapus
# - Perubahan masih di staging (git add)
# - Bisa langsung git commit lagi
```

**B. Mixed Reset (Default)**
Hapus commit dan unstage perubahan.

```bash
git reset HEAD~1
# atau
git reset --mixed HEAD~1

# Setelah ini:
# - Commit dihapus
# - Perubahan di working directory (unstaged)
# - Perlu git add lagi sebelum commit
```

**C. Hard Reset**
Hapus commit dan semua perubahan.

```bash
git reset --hard HEAD~1

# Setelah ini:
# - Commit dihapus
# - Perubahan dihapus
# - Kembali ke kondisi commit sebelumnya
```

⚠️ **SANGAT HATI-HATI dengan `--hard`! Perubahan hilang permanen!**

#### Contoh Praktis

```bash
# Scenario: Salah commit
git add .
git commit -m "Salah commit"

# Opsi 1: Soft reset (keep changes staged)
git reset --soft HEAD~1
# Edit lagi jika perlu
git commit -m "Commit yang benar"

# Opsi 2: Mixed reset (keep changes unstaged)
git reset HEAD~1
# Edit lagi
git add .
git commit -m "Commit yang benar"

# Opsi 3: Hard reset (hapus semua)
git reset --hard HEAD~1
```

#### Reset ke Commit Tertentu

```bash
# Reset ke commit spesifik
git reset --hard 3f2a1b9

# Reset ke 3 commit sebelumnya
git reset --hard HEAD~3

# Reset ke branch tertentu
git reset --hard origin/main
```

---

### 2. Git Revert

Membuat commit baru yang membatalkan commit sebelumnya (safe untuk public branch).

#### Perbedaan Reset vs Revert

| Aspek | Reset | Revert |
|-------|-------|--------|
| **History** | Menghapus commit | Menambah commit baru |
| **Safe untuk public?** | Tidak | Aman |
| **Undo method** | Hapus dari history | Buat inverse commit |

#### Syntax
```bash
# Revert commit terakhir
git revert HEAD

# Revert commit tertentu
git revert <commit-hash>

# Revert tanpa auto-commit
git revert --no-commit HEAD
```

#### Contoh
```bash
# Scenario: Bug di commit terakhir (sudah di-push)

# 1. Lihat history
git log --oneline
# 3f2a1b9 Tambah fitur X (buggy)
# 2e1d0c9 Update styling

# 2. Revert commit buggy
git revert 3f2a1b9

# 3. Git akan buka editor untuk commit message
# Default: "Revert 'Tambah fitur X'"

# 4. Push
git push origin main
```

#### Revert Multiple Commits

```bash
# Revert 3 commit terakhir
git revert HEAD~3..HEAD

# Revert range dengan no-commit (combine jadi satu)
git revert --no-commit HEAD~3..HEAD
git commit -m "Revert changes from last 3 commits"
```

---

### 3. Git Commit --amend

Edit commit terakhir (ganti message atau tambah perubahan).

#### Syntax
```bash
# Edit commit message
git commit --amend -m "New message"

# Tambah file ke commit terakhir
git add forgotten-file.txt
git commit --amend --no-edit
```

#### Contoh

**Scenario 1: Typo di commit message**
```bash
git commit -m "Fix tpyo in README"

# Ups, typo! Amend:
git commit --amend -m "Fix typo in README"
```

**Scenario 2: Lupa add file**
```bash
# Commit
git add style.css
git commit -m "Update styling"

# Ups, lupa script.js!
git add script.js
git commit --amend --no-edit
```

⚠️ **Jangan amend commit yang sudah di-push ke public branch!**

---

## Membatalkan Perubahan yang Sudah di-Push

### Scenario 1: Private Branch (Anda sendiri)

Bisa gunakan reset + force push:

```bash
# 1. Reset local
git reset --hard HEAD~1

# 2. Force push
git push --force-with-lease origin feature-x
```

### Scenario 2: Public/Shared Branch

Gunakan revert (aman):

```bash
# 1. Revert commit
git revert HEAD

# 2. Push
git push origin main
```

---

## Git Reflog (Recovery)

Reflog mencatat semua perubahan HEAD. Berguna untuk recovery.

### Lihat Reflog
```bash
git reflog

# Output:
# 3f2a1b9 HEAD@{0}: commit: Tambah fitur X
# 2e1d0c9 HEAD@{1}: commit: Update styling
# 1d0c9b8 HEAD@{2}: reset: moving to HEAD~1
# 5e8d4c2 HEAD@{3}: commit: Fitur yang terhapus
```

### Recovery dengan Reflog

```bash
# Scenario: Salah reset --hard
git reset --hard HEAD~3
# Ups, commit penting hilang!

# 1. Lihat reflog
git reflog

# 2. Temukan commit yang hilang
# 5e8d4c2 HEAD@{3}: commit: Fitur yang terhapus

# 3. Reset kembali
git reset --hard 5e8d4c2

# Commit kembali!
```

---

## Git Clean

Menghapus untracked files.

### Syntax
```bash
# Dry run (lihat apa yang akan dihapus)
git clean -n

# Hapus untracked files
git clean -f

# Hapus untracked files + directories
git clean -fd

# Hapus termasuk ignored files
git clean -fdx
```

### Contoh
```bash
# Ada banyak file build yang tidak di-track
ls
# index.html  style.css  dist/  node_modules/  temp/

# Lihat apa yang akan dihapus
git clean -n
# Output: Would remove dist/ temp/

# Hapus
git clean -fd
```

⚠️ **HATI-HATI:** File yang dihapus tidak bisa di-recovery!

---

## Git Stash (Simpan Sementara)

Menyimpan perubahan sementara tanpa commit.

### Syntax Dasar
```bash
# Stash perubahan
git stash

# atau dengan pesan
git stash save "Work in progress: feature X"

# Lihat daftar stash
git stash list

# Apply stash terakhir
git stash pop

# Apply tanpa menghapus dari stash
git stash apply

# Apply stash tertentu
git stash apply stash@{1}
```

### Contoh Workflow
```bash
# Scenario: Lagi kerja di fitur, tiba-tiba ada bug urgent

# 1. Simpan pekerjaan saat ini
git stash save "WIP: feature payment"

# 2. Pindah branch untuk fix bug
git checkout main
git checkout -b hotfix-urgent-bug

# 3. Fix bug
# ... edit file ...
git add .
git commit -m "Fix: urgent bug"

# 4. Kembali ke pekerjaan semula
git checkout feature-payment
git stash pop

# 5. Lanjutkan kerja
```

### Perintah Stash Lainnya

```bash
# Lihat perubahan di stash
git stash show

# Lihat diff detail
git stash show -p

# Hapus stash tertentu
git stash drop stash@{0}

# Hapus semua stash
git stash clear

# Stash termasuk untracked files
git stash -u
```

---

## Matrix: Kapan Menggunakan Apa?

| Situasi | Command |
|---------|---------|
| Batalkan perubahan file (belum add) | `git restore <file>` |
| Unstage file | `git restore --staged <file>` |
| Edit commit message terakhir | `git commit --amend -m "new"` |
| Hapus commit terakhir (belum push) | `git reset --soft HEAD~1` |
| Hapus commit + perubahan | `git reset --hard HEAD~1` |
| Undo commit (sudah push) | `git revert HEAD` |
| Simpan perubahan sementara | `git stash` |
| Recovery commit yang hilang | `git reflog` + `git reset` |
| Hapus untracked files | `git clean -fd` |

---

## Best Practices

### DO (Lakukan)

1. **Backup sebelum reset hard**
   ```bash
   git branch backup-branch
   git reset --hard HEAD~3
   ```

2. **Gunakan revert untuk public branch**

3. **Test setelah undo**

4. **Baca git status sebelum action**

5. **Gunakan `--force-with-lease` bukan `--force`**
   ```bash
   git push --force-with-lease origin feature-x
   ```

### DON'T (Jangan)

1. Reset hard tanpa backup
2. Amend commit yang sudah di-push ke public
3. Force push ke main/master
4. Clean tanpa dry run (`-n`)
5. Reset public branch

---

## Troubleshooting

### Problem: Salah reset --hard
```bash
# Gunakan reflog untuk recovery
git reflog
git reset --hard HEAD@{1}
```

### Problem: Ingin undo amend
```bash
git reset --soft HEAD@{1}
```

### Problem: Conflict saat revert
```bash
# Resolve conflict seperti biasa
# ... edit file ...
git add .
git revert --continue
```

### Problem: Ingin undo push
```bash
# Jika private branch
git reset --hard HEAD~1
git push --force-with-lease origin feature-x

# Jika public branch
git revert HEAD
git push origin main
```

---

## Rangkuman

- **`git restore <file>`** - Batalkan perubahan di working directory
- **`git restore --staged <file>`** - Unstage file
- **`git reset`** - Hapus commit (ubah history)
- **`git revert`** - Buat commit baru yang undo commit lama
- **`git commit --amend`** - Edit commit terakhir
- **`git stash`** - Simpan perubahan sementara
- **`git reflog`** - Recovery commit yang hilang
- **`git clean`** - Hapus untracked files

---

## Latihan

1. Buat perubahan, lalu batalkan dengan `git restore`
2. Add file, lalu unstage dengan `git restore --staged`
3. Buat commit, lalu undo dengan berbagai jenis reset
4. Practice revert commit
5. Gunakan stash untuk menyimpan pekerjaan sementara

---

**Sebelumnya:** [08. Remote Repository](08-remote-repository.md)  
**Selanjutnya:** [10. Git Ignore](10-gitignore.md)


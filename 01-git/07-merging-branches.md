# 7. Merging Branches

## Apa itu Merge?

Merge adalah proses menggabungkan perubahan dari satu branch ke branch lain. Biasanya digunakan untuk menggabungkan feature branch kembali ke branch utama (main/master).

### Ilustrasi
```
main:      A --- B --- C --------------- F (merge commit)
                    \                   /
feature:             D --- E -----------
```

---

## Jenis-jenis Merge

### 1. Fast-Forward Merge
Terjadi ketika tidak ada commit baru di branch target.

```
Before merge:
main:      A --- B --- C
                        \
feature:                 D --- E

After merge:
main:      A --- B --- C --- D --- E
```

### 2. Three-Way Merge
Terjadi ketika ada commit baru di kedua branch.

```
Before merge:
main:      A --- B --- C --- F
                    \
feature:             D --- E

After merge:
main:      A --- B --- C --- F --- G (merge commit)
                    \             /
feature:             D --- E -----
```

---

## Command: git merge

### Syntax Dasar
```bash
# Merge branch ke branch saat ini
git merge <nama-branch>
```

### Contoh Praktis

```bash
# 1. Pastikan Anda di branch target (biasanya main)
git checkout main

# 2. Update branch main
git pull origin main

# 3. Merge feature branch
git merge feature-login

# 4. Push hasil merge
git push origin main
```

---

## Workflow Merge Lengkap

### Scenario: Merge Feature Branch ke Main

```bash
# === Di awal development ===

# 1. Buat dan pindah ke branch feature
git checkout -b feature-payment
# ... kerja di branch ini ...
# ... buat beberapa commit ...

# === Selesai development ===

# 2. Pastikan branch feature up to date
git checkout feature-payment
git status
git add .
git commit -m "Selesai: fitur payment"

# 3. Pindah ke branch main
git checkout main

# 4. Update main dari remote
git pull origin main

# 5. Merge feature-payment ke main
git merge feature-payment

# 6. Jika ada conflict, selesaikan (lihat section Conflict)

# 7. Push ke remote
git push origin main

# 8. Hapus feature branch (optional)
git branch -d feature-payment
git push origin --delete feature-payment
```

---

## Fast-Forward Merge

### Kapan Terjadi?
Ketika tidak ada perubahan baru di branch target sejak branch source dibuat.

### Contoh
```bash
# Buat branch dari main
git checkout main
git checkout -b feature-x

# Kerja di feature-x
echo "Feature X" > feature.txt
git add feature.txt
git commit -m "Tambah feature X"

# Merge ke main (akan fast-forward)
git checkout main
git merge feature-x

# Output:
# Updating 3f2a1b9..5e8d4c2
# Fast-forward
#  feature.txt | 1 +
#  1 file changed, 1 insertion(+)
```

### Disable Fast-Forward
Jika ingin selalu membuat merge commit:
```bash
git merge --no-ff feature-x
```

---

## Three-Way Merge

### Kapan Terjadi?
Ketika ada commit baru di kedua branch.

### Contoh
```bash
# Di branch main
git checkout main
echo "Update main" >> README.md
git add README.md
git commit -m "Update README di main"

# Merge feature-login (akan three-way merge)
git merge feature-login

# Git akan buat merge commit otomatis atau buka editor untuk commit message
```

### Merge Commit Message
Default format:
```
Merge branch 'feature-login' into main
```

Anda bisa edit message ini sesuai kebutuhan.

---

## Merge Conflict

### Apa itu Conflict?

Conflict terjadi ketika Git tidak bisa otomatis menggabungkan perubahan karena:
- Kedua branch mengubah baris yang sama di file yang sama
- Satu branch menghapus file yang diubah di branch lain

### Contoh Conflict

```bash
# Merge dengan conflict
git merge feature-login

# Output:
# Auto-merging index.html
# CONFLICT (content): Merge conflict in index.html
# Automatic merge failed; fix conflicts and then commit the result.
```

### Cek Status Conflict
```bash
git status

# Output:
# On branch main
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#         both modified:   index.html
```

---

## Menyelesaikan Conflict

### Step 1: Buka File yang Conflict

File dengan conflict akan terlihat seperti ini:

```html
<html>
<head>
<<<<<<< HEAD
    <title>Website Utama</title>
=======
    <title>Website Portfolio</title>
>>>>>>> feature-login
</head>
<body>
```

**Penjelasan:**
- `<<<<<<< HEAD` - Perubahan di branch saat ini (main)
- `=======` - Pemisah
- `>>>>>>> feature-login` - Perubahan di branch yang di-merge

### Step 2: Edit File

Pilih salah satu atau gabungkan kedua perubahan:

**Opsi 1: Pilih salah satu**
```html
<html>
<head>
    <title>Website Portfolio</title>
</head>
<body>
```

**Opsi 2: Gabungkan keduanya**
```html
<html>
<head>
    <title>Website Portfolio Utama</title>
</head>
<body>
```

**⚠️ Penting:** Hapus marker conflict (`<<<<<<<`, `=======`, `>>>>>>>`)

### Step 3: Stage File yang Sudah Diselesaikan
```bash
git add index.html
```

### Step 4: Commit Merge
```bash
git commit -m "Merge feature-login: selesaikan conflict di index.html"

# Atau biarkan default message
git commit
```

### Step 5: Verifikasi
```bash
git log --oneline --graph
```

---

## Tools untuk Resolve Conflict

### 1. Git Mergetool
```bash
# Buka mergetool
git mergetool

# Akan membuka tool visual seperti:
# - vimdiff (default)
# - meld
# - kdiff3
# - p4merge
```

### 2. Set Mergetool
```bash
# Set Visual Studio Code sebagai mergetool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Set Meld
git config --global merge.tool meld
```

### 3. Manual dengan Editor
Gunakan editor favorit Anda (VS Code, Sublime, Vim, dll)

---

## Abort Merge

Jika ingin membatalkan merge:

```bash
# Batalkan merge dan kembali ke sebelum merge
git merge --abort
```

---

## Strategi Merge

### 1. Recursive (Default)
```bash
git merge feature-x
# Otomatis menggunakan recursive strategy
```

### 2. Ours
Gunakan versi kita jika ada conflict.
```bash
git merge -X ours feature-x
```

### 3. Theirs
Gunakan versi mereka jika ada conflict.
```bash
git merge -X theirs feature-x
```

### 4. No Fast-Forward
Selalu buat merge commit.
```bash
git merge --no-ff feature-x
```

### 5. Squash
Gabungkan semua commit jadi satu.
```bash
git merge --squash feature-x
git commit -m "Merge feature-x sebagai satu commit"
```

---

## Rebase sebagai Alternatif Merge

### Perbedaan Merge vs Rebase

**Merge:**
```
main:      A --- B --- C ----------- F (merge commit)
                    \               /
feature:             D --- E -------
```

**Rebase:**
```
main:      A --- B --- C
                         \
feature:                  D' --- E'
```

### Kapan Menggunakan?
- **Merge**: Untuk public branch, preserving history
- **Rebase**: Untuk cleanup local branch, linear history

### Contoh Rebase
```bash
# Di branch feature
git checkout feature-x
git rebase main

# Jika ada conflict, selesaikan lalu
git add .
git rebase --continue

# Pindah ke main dan merge (fast-forward)
git checkout main
git merge feature-x
```

---

## Best Practices

### DO (Lakukan)

1. **Update target branch sebelum merge**
   ```bash
   git checkout main
   git pull origin main
   ```

2. **Test sebelum merge**
   ```bash
   # Di feature branch
   npm test
   npm run build
   ```

3. **Buat commit message yang jelas**
   ```bash
   git commit -m "Merge feature-login: tambah OAuth dan validation"
   ```

4. **Delete branch setelah merge**
   ```bash
   git branch -d feature-x
   ```

5. **Gunakan Pull Request untuk review**

### DON'T (Jangan)

1. Merge ke main tanpa test
2. Force push setelah merge (kecuali tahu apa yang dilakukan)
3. Merge branch yang belum selesai
4. Ignore conflict markers
5. Merge tanpa resolve semua conflict

---

## Melihat Merge History

### Git Log untuk Merge
```bash
# Lihat merge commits
git log --merges

# Lihat non-merge commits
git log --no-merges

# Grafik merge
git log --oneline --graph --all
```

### Lihat Parent dari Merge Commit
```bash
# Show parent commits dari merge
git show <merge-commit-hash>

# Diff dengan parent pertama
git diff <merge-commit>^1 <merge-commit>

# Diff dengan parent kedua
git diff <merge-commit>^2 <merge-commit>
```

---

## Undo Merge

### Jika Belum Push

**Opsi 1: Reset**
```bash
# Undo merge, hapus merge commit
git reset --hard HEAD~1
```

**Opsi 2: Reset tapi keep changes**
```bash
git reset --merge HEAD~1
```

### Jika Sudah Push

**Revert merge commit**
```bash
# Buat commit baru yang undo merge
git revert -m 1 <merge-commit-hash>
git push origin main
```

**Parameter `-m 1`:**
- `1` = keep changes from main
- `2` = keep changes from feature branch

---

## Troubleshooting

### Problem: "fatal: refusing to merge unrelated histories"
```bash
# Jika merge dua repository yang berbeda
git merge feature-x --allow-unrelated-histories
```

### Problem: Conflict yang sulit diselesaikan
```bash
# Abort merge dan coba lagi
git merge --abort

# Update branch
git fetch origin
git checkout feature-x
git rebase main

# Resolve conflict satu per satu
git rebase --continue
```

### Problem: Merge sudah selesai tapi ada bug
```bash
# Revert merge
git revert -m 1 <merge-commit-hash>

# Fix bug di feature branch
git checkout feature-x
# ... fix bug ...

# Merge lagi
git checkout main
git merge feature-x
```

---

## Rangkuman

- **`git merge <branch>`** - Merge branch ke branch saat ini
- **Fast-forward merge** - Tidak ada commit baru di target branch
- **Three-way merge** - Ada commit baru di kedua branch
- **Conflict** - Terjadi saat perubahan bentrok
- Selesaikan conflict: edit file → `git add` → `git commit`
- **`git merge --abort`** - Batalkan merge
- Gunakan Pull Request untuk code review sebelum merge

---

## Latihan

1. Buat dua branch dan merge salah satunya
2. Buat conflict dengan sengaja dan selesaikan
3. Coba merge dengan `--no-ff` dan tanpa `--no-ff`, lihat perbedaannya
4. Practice abort merge dan merge ulang

---

**Sebelumnya:** [06. Bekerja dengan Branch](06-bekerja-dengan-branch.md)  
**Selanjutnya:** [08. Remote Repository](08-remote-repository.md)


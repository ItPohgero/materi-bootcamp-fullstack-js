# 8. Remote Repository

## Apa itu Remote Repository?

Remote repository adalah versi repository yang di-host di server atau cloud (seperti GitHub, GitLab, Bitbucket). Remote memungkinkan kolaborasi dengan developer lain dan backup kode Anda.

### Analogi
Jika repository lokal adalah draft di komputer Anda, remote repository adalah copy yang dipublikasikan online yang bisa diakses siapa saja (atau tim Anda).

---

## Platform Remote Repository

### GitHub
- Platform paling populer
- Gratis untuk public dan private repos
- Fitur: Issues, Projects, Actions, Pages
- Website: https://github.com

### GitLab
- Open source option tersedia
- Built-in CI/CD
- Self-hosted option
- Website: https://gitlab.com

### Bitbucket
- Dari Atlassian
- Integrasi dengan Jira
- Gratis untuk tim kecil
- Website: https://bitbucket.org

---

## Command: git remote

Mengelola remote repository.

### Lihat Remote
```bash
# Lihat daftar remote
git remote

# Output:
# origin

# Lihat URL remote
git remote -v

# Output:
# origin  https://github.com/username/repo.git (fetch)
# origin  https://github.com/username/repo.git (push)
```

### Tambah Remote
```bash
# Tambah remote baru
git remote add <name> <url>

# Contoh:
git remote add origin https://github.com/username/repo.git

# Tambah remote lain (misal untuk fork)
git remote add upstream https://github.com/original/repo.git
```

### Hapus Remote
```bash
git remote remove <name>
# atau
git remote rm <name>

# Contoh:
git remote remove origin
```

### Ganti URL Remote
```bash
# Ganti URL remote
git remote set-url origin <url-baru>

# Contoh: ganti HTTPS ke SSH
git remote set-url origin git@github.com:username/repo.git
```

### Rename Remote
```bash
git remote rename <old-name> <new-name>

# Contoh:
git remote rename origin github
```

### Lihat Detail Remote
```bash
git remote show origin

# Output info detail:
# - URL
# - Branch yang di-track
# - Status push/pull
```

---

## Command: git clone

Menyalin repository dari remote.

### Syntax
```bash
git clone <url>
```

### Contoh
```bash
# Clone dengan HTTPS
git clone https://github.com/username/repo.git

# Clone dengan SSH
git clone git@github.com:username/repo.git

# Clone ke folder dengan nama custom
git clone https://github.com/username/repo.git my-project

# Clone hanya branch tertentu
git clone -b develop https://github.com/username/repo.git

# Shallow clone (hanya history terakhir)
git clone --depth 1 https://github.com/username/repo.git
```

---

## Command: git push

Mengirim commit lokal ke remote repository.

### Syntax
```bash
git push <remote> <branch>
```

### Contoh Dasar
```bash
# Push branch main ke remote origin
git push origin main

# Push branch feature-x
git push origin feature-x

# Push semua branch
git push --all origin

# Push semua tags
git push --tags
```

### First Push (Set Upstream)
```bash
# Push pertama kali dan set tracking
git push -u origin main
# atau
git push --set-upstream origin main

# Setelah ini, cukup:
git push
```

### Force Push (HATI-HATI!)
```bash
# Force push (overwrite remote)
git push -f origin main
# atau
git push --force origin main

# Safer force push (tidak overwrite jika ada push lain)
git push --force-with-lease origin main
```

⚠️ **Jangan force push ke branch public/shared kecuali sangat perlu!**

---

## Command: git fetch

Mengambil perubahan dari remote tanpa merge.

### Syntax
```bash
# Fetch dari remote origin
git fetch origin

# Fetch branch tertentu
git fetch origin main

# Fetch dari semua remote
git fetch --all

# Fetch dan hapus branch reference yang sudah dihapus di remote
git fetch --prune
# atau
git fetch -p
```

### Perbedaan Fetch vs Pull
- **`git fetch`** - Download perubahan, tapi tidak merge
- **`git pull`** - Download perubahan + auto merge

### Workflow dengan Fetch
```bash
# 1. Fetch perubahan
git fetch origin

# 2. Lihat perbedaan
git log origin/main..HEAD

# 3. Merge manual jika diperlukan
git merge origin/main
```

---

## Command: git pull

Mengambil perubahan dari remote dan auto-merge.

### Syntax
```bash
git pull <remote> <branch>
```

### Contoh
```bash
# Pull dari origin main
git pull origin main

# Jika sudah set upstream, cukup:
git pull

# Pull dengan rebase (bukan merge)
git pull --rebase origin main

# Pull dan stash perubahan uncommitted
git pull --autostash
```

### Git Pull = Fetch + Merge
```bash
# git pull setara dengan:
git fetch origin
git merge origin/main
```

---

## Workflow: Push ke Remote

### Scenario: Push Project Baru ke GitHub

```bash
# === Di GitHub ===
# 1. Buat repository baru di GitHub (via web browser)
#    - Nama: my-project
#    - Jangan initialize with README/gitignore/license

# === Di Local ===

# 2. Initialize Git
cd my-project
git init

# 3. Buat commit pertama
echo "# My Project" > README.md
git add .
git commit -m "Initial commit"

# 4. Tambah remote
git remote add origin https://github.com/username/my-project.git

# 5. Push ke remote
git branch -M main
git push -u origin main
```

### Scenario: Update Remote

```bash
# 1. Kerja di local
# ... edit file ...
git add .
git commit -m "Update feature X"

# 2. Push ke remote
git push
# atau
git push origin main
```

---

## Workflow: Pull dari Remote

### Scenario: Update Local dari Remote

```bash
# 1. Pull perubahan terbaru
git pull origin main

# 2. Jika ada conflict, selesaikan
# ... resolve conflict ...
git add .
git commit -m "Merge remote changes"

# 3. Push hasil merge (jika perlu)
git push origin main
```

### Scenario: Pull dengan Uncommitted Changes

```bash
# Opsi 1: Stash dulu
git stash
git pull origin main
git stash pop

# Opsi 2: Commit dulu
git add .
git commit -m "WIP: work in progress"
git pull origin main

# Opsi 3: Pull dengan autostash
git pull --autostash origin main
```

---

## Tracking Branches

### Apa itu Tracking Branch?

Tracking branch adalah local branch yang terhubung dengan remote branch.

### Set Tracking Branch
```bash
# Saat push pertama kali
git push -u origin main

# Set tracking untuk branch yang sudah ada
git branch --set-upstream-to=origin/main main
# atau
git branch -u origin/main main
```

### Lihat Tracking Branch
```bash
git branch -vv

# Output:
# * main    3f2a1b9 [origin/main: ahead 2, behind 1] Update feature
#   feature 2e1d0c9 [origin/feature] Work in progress
```

**Penjelasan:**
- `ahead 2` - Ada 2 commit di local yang belum di-push
- `behind 1` - Ada 1 commit di remote yang belum di-pull

---

## Fork dan Upstream

### Fork
Copy repository orang lain ke akun Anda (via GitHub UI).

### Setup Upstream
```bash
# 1. Clone fork Anda
git clone https://github.com/your-username/repo.git

# 2. Tambah upstream (original repo)
git remote add upstream https://github.com/original-owner/repo.git

# 3. Verifikasi
git remote -v

# Output:
# origin    https://github.com/your-username/repo.git (fetch)
# origin    https://github.com/your-username/repo.git (push)
# upstream  https://github.com/original-owner/repo.git (fetch)
# upstream  https://github.com/original-owner/repo.git (push)
```

### Sync Fork dengan Upstream
```bash
# 1. Fetch dari upstream
git fetch upstream

# 2. Checkout main
git checkout main

# 3. Merge upstream/main
git merge upstream/main

# 4. Push ke origin (fork Anda)
git push origin main
```

---

## Mengelola Remote Branches

### Lihat Remote Branches
```bash
# Lihat semua branch (local + remote)
git branch -a

# Lihat hanya remote branches
git branch -r
```

### Checkout Remote Branch
```bash
# Buat local branch dari remote branch
git checkout -b feature-x origin/feature-x

# atau (Git 2.23+)
git switch -c feature-x origin/feature-x

# atau otomatis:
git switch feature-x
# Git otomatis create local branch yang track remote
```

### Hapus Remote Branch
```bash
# Hapus branch di remote
git push origin --delete feature-old

# atau
git push origin :feature-old
```

### Prune Remote Branches
```bash
# Hapus references ke remote branches yang sudah dihapus
git fetch --prune
# atau
git fetch -p

# atau set otomatis:
git config --global fetch.prune true
```

---

## Authentication

### HTTPS vs SSH

| Aspek | HTTPS | SSH |
|-------|-------|-----|
| **URL** | https://github.com/user/repo.git | git@github.com:user/repo.git |
| **Setup** | Mudah | Perlu generate SSH key |
| **Security** | Username + Token/Password | SSH Key (lebih aman) |
| **Best for** | Quick clone, public repos | Regular work, private repos |

### Setup SSH

```bash
# 1. Generate SSH key
ssh-keygen -t ed25519 -C "your-email@example.com"

# 2. Start ssh-agent
eval "$(ssh-agent -s)"

# 3. Add key ke ssh-agent
ssh-add ~/.ssh/id_ed25519

# 4. Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy output

# 5. Add ke GitHub
# GitHub → Settings → SSH and GPG keys → New SSH key
# Paste public key

# 6. Test connection
ssh -T git@github.com
```

### Setup Personal Access Token (PAT)

Untuk HTTPS authentication:

```bash
# 1. Generate token di GitHub
# Settings → Developer settings → Personal access tokens → Generate new token

# 2. Saat git push, gunakan token sebagai password
# Username: your-username
# Password: ghp_xxxxxxxxxxxxx (token)

# 3. Save credential (agar tidak perlu input berulang)
git config --global credential.helper cache
# atau untuk permanent:
git config --global credential.helper store
```

---

## Best Practices

### DO (Lakukan)

1. **Pull sebelum Push**
   ```bash
   git pull origin main
   # ... resolve conflict jika ada ...
   git push origin main
   ```

2. **Commit message yang jelas**

3. **Gunakan branch untuk fitur baru**
   ```bash
   git checkout -b feature-x
   # ... kerja ...
   git push -u origin feature-x
   ```

4. **Regular backup dengan push**

5. **Setup SSH untuk keamanan**

### DON'T (Jangan)

1. Force push ke branch public/shared
2. Push credential atau sensitive data
3. Push directly ke main (gunakan PR)
4. Push tanpa test
5. Ignore merge conflict

---

## Troubleshooting

### Problem: "Updates were rejected"
```bash
# Remote memiliki commit yang tidak ada di local
# Solusi: pull dulu
git pull origin main
# resolve conflict jika ada
git push origin main
```

### Problem: "fatal: refusing to merge unrelated histories"
```bash
# Jika gabung dua repo berbeda
git pull origin main --allow-unrelated-histories
```

### Problem: "failed to push some refs"
```bash
# Solusi 1: Pull dulu
git pull --rebase origin main
git push origin main

# Solusi 2: Force (hati-hati!)
git push --force-with-lease origin main
```

### Problem: "Permission denied (publickey)"
```bash
# SSH key bermasalah
# Solusi: regenerate dan add SSH key
ssh-keygen -t ed25519 -C "email@example.com"
# add ke GitHub
```

---

## Rangkuman

- **`git remote`** - Kelola remote repository
- **`git clone <url>`** - Copy repository dari remote
- **`git push`** - Kirim commit ke remote
- **`git fetch`** - Download perubahan tanpa merge
- **`git pull`** - Download dan auto-merge perubahan
- Pull sebelum push untuk menghindari conflict
- Gunakan SSH untuk autentikasi yang lebih aman

---

## Latihan

1. Clone repository dari GitHub
2. Buat perubahan, commit, dan push ke remote
3. Simulate conflict: edit file di GitHub, edit yang sama di local, lalu pull
4. Setup SSH key dan switch dari HTTPS ke SSH
5. Fork repository dan sync dengan upstream

---

**Sebelumnya:** [07. Merging Branches](07-merging-branches.md)  
**Selanjutnya:** [09. Membatalkan Perubahan](09-membatalkan-perubahan.md)


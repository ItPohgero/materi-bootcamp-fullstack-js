# 10. Git Ignore

## Apa itu .gitignore?

`.gitignore` adalah file yang memberitahu Git file atau folder mana yang **tidak** perlu di-track. File ini mencegah file yang tidak perlu (seperti log, dependencies, credential) masuk ke repository.

---

## Mengapa Perlu .gitignore?

### File yang Tidak Perlu di-Track:

1. **Dependencies** - `node_modules/`, `vendor/`, `venv/`
2. **Build output** - `dist/`, `build/`, `*.exe`, `*.o`
3. **IDE/Editor files** - `.vscode/`, `.idea/`, `*.swp`
4. **OS files** - `.DS_Store`, `Thumbs.db`, `desktop.ini`
5. **Credentials** - `.env`, `config.local.js`, `*.key`
6. **Logs** - `*.log`, `logs/`
7. **Cache** - `.cache/`, `__pycache__/`, `*.pyc`
8. **Temporary files** - `*.tmp`, `*.temp`, `~*`

### Keuntungan:

- Repository lebih bersih dan kecil
- Clone dan pull lebih cepat
- Menghindari commit credential secara tidak sengaja
- Kolaborasi lebih mudah (tidak ada conflict di file yang tidak penting)

---

## Membuat .gitignore

### Lokasi File

File `.gitignore` harus berada di root folder repository (atau di subfolder untuk aturan spesifik folder tersebut).

```
my-project/
├── .git/
├── .gitignore       ← File gitignore utama
├── src/
│   └── .gitignore   ← Opsional: gitignore untuk folder ini
├── index.html
└── package.json
```

### Membuat File

```bash
# Buat file .gitignore
touch .gitignore

# atau dengan echo
echo "node_modules/" > .gitignore
```

---

## Syntax .gitignore

### 1. Ignore File Tertentu

```gitignore
# Ignore file spesifik
config.json
secret.key
database.db
```

### 2. Ignore Semua File dengan Ekstensi Tertentu

```gitignore
# Ignore semua file .log
*.log

# Ignore semua file .tmp
*.tmp

# Ignore semua file .env
*.env
```

### 3. Ignore Folder

```gitignore
# Ignore folder node_modules di root
node_modules/

# Ignore folder dist di mana saja
**/dist/

# Ignore folder .vscode
.vscode/
```

### 4. Negasi (Exception)

Gunakan `!` untuk tidak mengabaikan file tertentu:

```gitignore
# Ignore semua .log
*.log

# Tapi jangan ignore important.log
!important.log
```

### 5. Ignore di Root Saja

Gunakan `/` di awal:

```gitignore
# Hanya ignore /notes.txt di root
/notes.txt

# notes.txt di subfolder tetap di-track
```

### 6. Wildcard

```gitignore
# Ignore semua file yang dimulai dengan temp
temp*

# Ignore file yang berakhiran _backup
*_backup

# Ignore semua file di folder yang namanya dimulai test
test*/
```

### 7. Comment

```gitignore
# Ini adalah comment
# Comment untuk dokumentasi

# Dependencies
node_modules/

# Build output
dist/
build/
```

---

## Contoh .gitignore untuk Berbagai Project

### Node.js / JavaScript

```gitignore
# Dependencies
node_modules/
npm-debug.log
yarn-error.log
package-lock.json  # opsional

# Environment variables
.env
.env.local
.env.*.local

# Build output
dist/
build/
*.bundle.js

# IDE
.vscode/
.idea/
*.sublime-workspace

# OS
.DS_Store
Thumbs.db

# Logs
logs/
*.log

# Cache
.cache/
.parcel-cache/
.next/
.nuxt/

# Testing
coverage/
.nyc_output/
```

### Python

```gitignore
# Virtual environment
venv/
env/
ENV/
.venv/

# Python cache
__pycache__/
*.py[cod]
*$py.class
*.so

# Distribution / packaging
dist/
build/
*.egg-info/
.eggs/

# Testing
.pytest_cache/
.coverage
htmlcov/

# Jupyter Notebook
.ipynb_checkpoints/

# Environment
.env
*.env

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
```

### Java

```gitignore
# Compiled files
*.class
*.jar
*.war
*.ear

# Build output
target/
build/
out/

# IDE
.idea/
*.iml
.classpath
.project
.settings/

# Gradle
.gradle/
gradle-app.setting

# Maven
.mvn/
mvnw
mvnw.cmd

# Logs
*.log

# OS
.DS_Store
Thumbs.db
```

### PHP

```gitignore
# Dependencies
vendor/
composer.lock  # opsional

# Environment
.env
.env.local

# Logs
*.log

# Cache
bootstrap/cache/
storage/framework/cache/
storage/framework/sessions/
storage/framework/views/
storage/logs/

# IDE
.vscode/
.idea/

# OS
.DS_Store
```

### React / Next.js

```gitignore
# Dependencies
node_modules/

# Production
build/
.next/
out/

# Environment
.env
.env.local
.env.production.local

# Testing
coverage/

# Misc
.DS_Store
*.pem

# Debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Vercel
.vercel
```

---

## Menambahkan .gitignore ke Repository

### Scenario: Project Baru

```bash
# 1. Buat .gitignore
touch .gitignore

# 2. Edit .gitignore
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore

# 3. Add dan commit
git add .gitignore
git commit -m "Tambah .gitignore"

# 4. Add file lain (yang tidak di-ignore)
git add .
git commit -m "Initial commit"
```

### Scenario: Project yang Sudah Ada

```bash
# 1. Buat .gitignore
echo "node_modules/" > .gitignore

# 2. Remove file yang sudah di-track tapi seharusnya di-ignore
git rm -r --cached node_modules/

# 3. Add .gitignore
git add .gitignore

# 4. Commit
git commit -m "Tambah .gitignore dan hapus node_modules dari tracking"

# 5. Push
git push origin main
```

---

## File yang Sudah Di-track

⚠️ **Penting:** `.gitignore` hanya bekerja untuk **untracked files**.

Jika file sudah di-add/commit sebelumnya, Git akan tetap melacaknya meskipun sudah ada di `.gitignore`.

### Menghentikan Tracking File yang Sudah Ada

```bash
# Stop tracking file tapi tetap keep file di lokal
git rm --cached <file>

# Stop tracking folder
git rm -r --cached <folder>/

# Contoh: stop tracking .env
git rm --cached .env
git commit -m "Stop tracking .env"

# File .env tetap ada di lokal, tapi tidak di-track lagi
```

---

## Global .gitignore

Untuk ignore file di semua repository Anda.

### Setup Global Gitignore

```bash
# 1. Buat file gitignore global
touch ~/.gitignore_global

# 2. Set global config
git config --global core.excludesfile ~/.gitignore_global

# 3. Edit file
nano ~/.gitignore_global
```

### Contoh Global .gitignore

```gitignore
# OS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
Desktop.ini

# IDE
.vscode/
.idea/
*.sublime-project
*.sublime-workspace
*.swp
*.swo
*~

# Misc
.env
.env.local
*.log
```

---

## Template .gitignore

### Menggunakan GitHub Template

GitHub menyediakan template `.gitignore` untuk berbagai bahasa/framework:

https://github.com/github/gitignore

**Cara menggunakan:**

1. Kunjungi repository di atas
2. Pilih template (misal: `Node.gitignore`, `Python.gitignore`)
3. Copy isi file
4. Paste ke `.gitignore` Anda

### Generate dengan gitignore.io

https://www.toptal.com/developers/gitignore

**Cara menggunakan:**

```bash
# Install gig (gitignore.io CLI)
npm install -g gig

# Generate .gitignore
gig node > .gitignore

# Generate untuk beberapa tools
gig node,visualstudiocode,macos > .gitignore

# List available templates
gig list
```

---

## Melihat Ignored Files

### Cek File yang Di-ignore

```bash
# Lihat file yang di-ignore di folder saat ini
git status --ignored

# Check apakah file tertentu di-ignore
git check-ignore -v <file>

# Contoh:
git check-ignore -v node_modules/express
# Output: .gitignore:1:node_modules/    node_modules/express
```

### Debug .gitignore

```bash
# Lihat kenapa file di-ignore
git check-ignore -v style.css

# Output (jika di-ignore):
# .gitignore:10:*.css    style.css
```

---

## Best Practices

### DO (Lakukan)

1. **Buat .gitignore sejak awal project**
   ```bash
   git init
   touch .gitignore
   # Edit .gitignore
   git add .gitignore
   git commit -m "Initial commit with gitignore"
   ```

2. **Gunakan template yang sesuai**

3. **Commit .gitignore ke repository**
   ```bash
   git add .gitignore
   git commit -m "Update gitignore"
   ```

4. **Dokumentasi dengan comment**
   ```gitignore
   # Dependencies
   node_modules/
   
   # Environment variables (never commit!)
   .env
   ```

5. **Review .gitignore secara berkala**

### DON'T (Jangan)

1. Jangan commit credential (`.env`, `.key`, etc)
2. Jangan commit dependencies (`node_modules/`, `vendor/`)
3. Jangan ignore file penting project
4. Jangan add `.gitignore` itself ke .gitignore
5. Jangan lupa update .gitignore saat menambah dependencies baru

---

## Troubleshooting

### Problem: File masih di-track meskipun sudah di .gitignore

**Solusi:** Stop tracking file:
```bash
git rm --cached <file>
git commit -m "Stop tracking file"
```

### Problem: Tidak tahu file mana yang di-ignore

**Solusi:**
```bash
git status --ignored
```

### Problem: Ingin ignore file tapi keep di local

**Solusi:** Gunakan `git rm --cached`
```bash
git rm --cached config.local.js
# File tetap ada di local, tapi tidak di-track
```

### Problem: Accidentally commit .env

**Solusi:**
```bash
# 1. Add ke .gitignore
echo ".env" >> .gitignore

# 2. Remove from tracking
git rm --cached .env

# 3. Commit
git commit -m "Stop tracking .env"

# 4. IMPORTANT: Rotate credentials!
# Ganti semua password/token di .env karena sudah exposed
```

---

## Security: Credential yang Ter-commit

### ⚠️ SANGAT PENTING!

Jika Anda **accidentally commit credential** (password, API key, token):

1. **Jangan hanya remove dari commit terakhir!**
   ```bash
   # INI TIDAK CUKUP:
   git rm --cached .env
   git commit -m "Remove .env"
   # Credential masih ada di git history!
   ```

2. **Rotate credential immediately!**
   - Ganti password
   - Generate API key baru
   - Revoke token yang ter-expose

3. **Remove dari entire history** (advanced):
   ```bash
   # Gunakan git filter-branch atau BFG Repo-Cleaner
   # Contoh dengan BFG:
   bfg --delete-files .env
   git reflog expire --expire=now --all
   git gc --prune=now --aggressive
   ```

4. **Force push** (jika belum ada yang clone):
   ```bash
   git push --force-with-lease origin main
   ```

---

## Rangkuman

- **`.gitignore`** - File untuk specify file/folder yang tidak di-track Git
- Ignore dependencies, build output, credentials, logs, cache
- Syntax: `file.txt`, `*.log`, `folder/`, `!exception.txt`
- Gunakan template dari GitHub atau gitignore.io
- File yang sudah di-track perlu di-remove dengan `git rm --cached`
- Setup global gitignore untuk ignore OS/IDE files
- **NEVER commit credentials!**

---

## Latihan

1. Buat .gitignore untuk project Anda
2. Test dengan membuat file yang seharusnya di-ignore
3. Practice `git rm --cached` untuk file yang sudah di-track
4. Setup global .gitignore untuk OS dan IDE files
5. Explore template dari https://github.com/github/gitignore

---

**Sebelumnya:** [09. Membatalkan Perubahan](09-membatalkan-perubahan.md)

---

## Selamat!

Anda telah menyelesaikan materi Git dasar! Sekarang Anda sudah memahami:

1. Apa itu Git dan version control
2. Instalasi dan konfigurasi Git
3. Membuat repository (init & clone)
4. Basic workflow (add, commit, status)
5. Melihat history (log, show, blame)
6. Bekerja dengan branch
7. Merging branches
8. Remote repository (push, pull, fetch)
9. Membatalkan perubahan (reset, revert, restore)
10. Git ignore

### Langkah Selanjutnya:

- Practice dengan project nyata
- Belajar Git advanced topics (rebase, cherry-pick, submodules)
- Explore GitHub features (Pull Requests, Issues, Actions)
- Join open source projects

**Happy coding!**


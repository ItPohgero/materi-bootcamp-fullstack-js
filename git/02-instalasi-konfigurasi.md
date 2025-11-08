# 2. Instalasi dan Konfigurasi Git

## Instalasi Git

### Windows

1. **Download Git**
   - Kunjungi: <https://git-scm.com/download/win>
   - Download installer sesuai sistem (32-bit atau 64-bit)

2. **Install**
   - Jalankan file installer
   - Ikuti wizard instalasi (gunakan setting default jika ragu)
   - Pilihan yang direkomendasikan:
     - Use Git from Git Bash only / Git from the command line
     - Use OpenSSL library
     - Checkout Windows-style, commit Unix-style line endings

3. **Verifikasi Instalasi**

   ```bash
   git --version
   ```

### macOS

**Cara 1: Homebrew (Rekomendasi)**

```bash
# Install Homebrew jika belum ada
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git
```

**Cara 2: Download Installer**

- Kunjungi: <https://git-scm.com/download/mac>
- Download dan install

**Cara 3: Xcode Command Line Tools**

```bash
xcode-select --install
```

### Linux

**Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install git
```

**Fedora/RHEL:**

```bash
sudo dnf install git
```

**Arch Linux:**

```bash
sudo pacman -S git
```

---

## Konfigurasi Awal Git

Setelah instalasi, Anda perlu mengatur identitas Git Anda. Ini penting karena setiap commit menggunakan informasi ini.

### 1. Set Username dan Email

```bash
# Set username
git config --global user.name "Nama Anda"

# Set email
git config --global user.email "email@example.com"
```

**Contoh:**

```bash
git config --global user.name "Wahyu Agus Arifin"
git config --global user.email "wahyu@example.com"
```

### 2. Set Default Editor

```bash
# Visual Studio Code
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"

# Sublime Text
git config --global core.editor "subl -n -w"
```

### 3. Set Default Branch Name

```bash
# Set default branch ke 'main' (best practice terbaru)
git config --global init.defaultBranch main
```

### 4. Enable Colors

```bash
git config --global color.ui auto
```

---

## Melihat Konfigurasi

### Melihat Semua Konfigurasi

```bash
git config --list
```

### Melihat Konfigurasi Spesifik

```bash
# Lihat username
git config user.name

# Lihat email
git config user.email

# Lihat editor
git config core.editor
```

### Melihat Lokasi File Konfigurasi

```bash
git config --list --show-origin
```

---

## Level Konfigurasi Git

Git memiliki 3 level konfigurasi:

### 1. System Level

Berlaku untuk semua user di sistem

```bash
git config --system <key> <value>
```

File: `/etc/gitconfig` (Linux/Mac) atau `C:\ProgramData\Git\config` (Windows)

### 2. Global Level (User)

Berlaku untuk user saat ini di semua repository

```bash
git config --global <key> <value>
```

File: `~/.gitconfig` atau `~/.config/git/config`

### 3. Local Level (Repository)

Berlaku hanya untuk repository tertentu

```bash
git config --local <key> <value>
```

File: `.git/config` di dalam repository

**Prioritas:** Local > Global > System

---

## Konfigurasi Tambahan yang Berguna

### Set Alias (Shortcut)

```bash
# Shortcut untuk status
git config --global alias.st status

# Shortcut untuk commit
git config --global alias.ci commit

# Shortcut untuk checkout
git config --global alias.co checkout

# Shortcut untuk branch
git config --global alias.br branch

# Shortcut untuk log yang lebih cantik
git config --global alias.lg "log --oneline --graph --all --decorate"
```

### Konfigurasi Line Ending

**Windows:**

```bash
git config --global core.autocrlf true
```

**Mac/Linux:**

```bash
git config --global core.autocrlf input
```

### Credential Helper (Simpan Password)

**Windows:**

```bash
git config --global credential.helper wincred
```

**Mac:**

```bash
git config --global credential.helper osxkeychain
```

**Linux:**

```bash
git config --global credential.helper cache
# atau untuk menyimpan permanent:
git config --global credential.helper store
```

---

## Menghapus Konfigurasi

```bash
# Hapus konfigurasi global
git config --global --unset user.name

# Hapus konfigurasi local
git config --unset user.email
```

---

## Setup SSH Key untuk GitHub/GitLab

### 1. Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "email@example.com"
```

### 2. Lihat SSH Key

```bash
cat ~/.ssh/id_ed25519.pub
```

### 3. Copy dan Tambahkan ke GitHub

- Buka GitHub → Settings → SSH and GPG keys
- Click "New SSH key"
- Paste public key Anda
- Save

### 4. Test Koneksi

```bash
ssh -T git@github.com
```

---

## Rangkuman

- Install Git dari website resmi atau package manager
- Konfigurasi wajib: `user.name` dan `user.email`
- Ada 3 level konfigurasi: system, global, dan local
- Gunakan alias untuk mempercepat workflow
- Setup SSH key untuk autentikasi yang lebih aman

---

**Sebelumnya:** [01. Pengenalan Git](01-pengenalan-git.md)  
**Selanjutnya:** [03. Membuat Repository](03-membuat-repository.md)

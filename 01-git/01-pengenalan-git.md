# 1. Pengenalan Git

## Apa itu Git?

Git adalah sistem kontrol versi (Version Control System/VCS) yang terdistribusi dan open source. Git diciptakan oleh Linus Torvalds pada tahun 2005 untuk membantu pengembangan kernel Linux.

## Mengapa Menggunakan Git?

### 1. **Melacak Perubahan**

- Mencatat setiap perubahan yang dilakukan pada kode
- Mengetahui siapa, kapan, dan apa yang diubah
- Dapat kembali ke versi sebelumnya jika diperlukan

### 2. **Kolaborasi Tim**

- Beberapa developer dapat bekerja secara bersamaan
- Menggabungkan pekerjaan dari berbagai contributor
- Menghindari konflik saat bekerja bersama

### 3. **Backup dan Recovery**

- Setiap perubahan tersimpan dengan aman
- Dapat mengembalikan file yang terhapus
- Memiliki history lengkap dari project

### 4. **Branching dan Merging**

- Membuat cabang untuk fitur baru tanpa mengubah kode utama
- Eksperimen dengan aman
- Menggabungkan perubahan dengan mudah

## Konsep Dasar Version Control

### Lokal Repository

Repository yang tersimpan di komputer lokal Anda sendiri.

### Remote Repository

Repository yang tersimpan di server (seperti GitHub, GitLab, Bitbucket).

### Commit

Snapshot atau rekaman dari perubahan yang Anda simpan.

### Branch

Cabang independen dari kode yang memungkinkan pengembangan paralel.

## Git vs GitHub

| Git | GitHub |
|-----|--------|
| Version control system | Hosting service untuk Git repository |
| Dijalankan secara lokal | Platform berbasis web |
| Tool command line | Menyediakan GUI dan fitur kolaborasi |
| Gratis dan open source | Gratis untuk public repos, berbayar untuk private |

## Keunggulan Git

- **Cepat dan Efisien** - Operasi dilakukan secara lokal  
- **Distributed** - Setiap developer memiliki copy lengkap repository  
- **Data Integrity** - Git menggunakan SHA-1 hash untuk memastikan integritas data  
- **Staging Area** - Kontrol penuh atas apa yang akan di-commit  
- **Free dan Open Source** - Tidak ada biaya lisensi  

## Workflow Dasar Git

```bash
Working Directory → Staging Area → Local Repository → Remote Repository
      ↓                  ↓                ↓                  ↓
  git add          git commit        git push         (GitHub/GitLab)
```

## Istilah Penting

- **Repository (Repo)**: Folder project yang dikelola oleh Git
- **Commit**: Menyimpan perubahan ke repository
- **Push**: Mengirim perubahan ke remote repository
- **Pull**: Mengambil perubahan dari remote repository
- **Clone**: Menyalin repository dari remote ke lokal
- **Fork**: Menyalin repository orang lain ke akun Anda
- **Merge**: Menggabungkan branch
- **Conflict**: Konflik saat menggabungkan perubahan

## Tiga Area Utama Git

### 1. Working Directory

Area kerja Anda - folder tempat Anda mengedit file.

### 2. Staging Area (Index)

Area persiapan - file yang siap untuk di-commit.

### 3. Repository

Area penyimpanan - commit history tersimpan di sini.

---

## Rangkuman

- Git adalah version control system yang powerful dan gratis
- Membantu melacak perubahan, kolaborasi, dan backup kode
- Memiliki tiga area utama: Working Directory, Staging Area, dan Repository
- Git berbeda dengan GitHub (Git = tool, GitHub = hosting service)

---

**Selanjutnya:** [02. Instalasi dan Konfigurasi Git](02-instalasi-konfigurasi.md)

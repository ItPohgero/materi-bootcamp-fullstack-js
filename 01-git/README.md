# Materi Git Dasar

Selamat datang di materi pembelajaran Git dasar! Materi ini disusun secara berurutan untuk memudahkan Anda mempelajari Git dari nol hingga mahir.

---

## Daftar Materi

### 1. [Pengenalan Git](01-pengenalan-git.md)
- Apa itu Git dan Version Control System?
- Mengapa menggunakan Git?
- Konsep dasar dan istilah penting
- Git vs GitHub
- Workflow dasar Git

### 2. [Instalasi dan Konfigurasi](02-instalasi-konfigurasi.md)
- Instalasi Git di Windows, macOS, dan Linux
- Konfigurasi awal (username, email, editor)
- Level konfigurasi (system, global, local)
- Setup SSH key untuk GitHub/GitLab
- Alias dan konfigurasi berguna lainnya

### 3. [Membuat Repository](03-membuat-repository.md)
- Membuat repository baru dengan `git init`
- Menyalin repository dengan `git clone`
- Struktur folder `.git`
- Menghubungkan repository lokal ke remote
- Best practices

### 4. [Basic Workflow Git](04-basic-workflow.md)
- Siklus dasar Git (Working Directory → Staging → Repository)
- Command: `git status`, `git add`, `git commit`
- Status file di Git
- Best practices commit message
- `git diff`, `git rm`, `git mv`

### 5. [Melihat History](05-melihat-history.md)
- Command: `git log` dengan berbagai format
- `git show` untuk detail commit
- `git log` dengan filtering
- `git blame` untuk tracking perubahan
- `git reflog` untuk recovery
- Custom format dan alias

### 6. [Bekerja dengan Branch](06-bekerja-dengan-branch.md)
- Apa itu branch dan mengapa penting?
- Membuat, melihat, dan menghapus branch
- Pindah antar branch
- Rename branch
- Branch naming conventions
- Workflow branch yang umum

### 7. [Merging Branches](07-merging-branches.md)
- Apa itu merge?
- Fast-forward vs Three-way merge
- Menangani merge conflict
- Tools untuk resolve conflict
- Strategi merge (recursive, ours, theirs, squash)
- Merge vs Rebase
- Undo merge

### 8. [Remote Repository](08-remote-repository.md)
- Apa itu remote repository?
- Platform: GitHub, GitLab, Bitbucket
- Command: `git remote`, `git clone`, `git push`, `git pull`, `git fetch`
- Fork dan upstream
- Authentication (HTTPS vs SSH)
- Tracking branches
- Best practices

### 9. [Membatalkan Perubahan](09-membatalkan-perubahan.md)
- Membatalkan perubahan di working directory (`git restore`)
- Unstaging file (`git restore --staged`)
- Membatalkan commit (`git reset`, `git revert`)
- `git commit --amend`
- `git stash` untuk simpan sementara
- `git reflog` untuk recovery
- `git clean` untuk hapus untracked files

### 10. [Git Ignore](10-gitignore.md)
- Apa itu `.gitignore` dan mengapa penting?
- Syntax `.gitignore`
- Contoh `.gitignore` untuk berbagai project (Node.js, Python, Java, PHP, React)
- Global `.gitignore`
- Template dan generator
- Stop tracking file yang sudah di-commit
- Security: handling credential yang ter-commit

---

## Cara Menggunakan Materi Ini

### Untuk Pemula:
1. **Baca berurutan** dari materi 1 hingga 10
2. **Practice** setiap command yang diajarkan
3. **Buat project** kecil untuk latihan
4. **Jangan skip** materi dasar meskipun terlihat mudah

### Untuk Review:
- Gunakan sebagai **reference** saat bekerja
- **Bookmark** materi yang sering digunakan
- **Explore** section troubleshooting untuk solve masalah

---

## Tips Belajar Git

1. **Practice, practice, practice!** 
   - Git adalah skill yang harus dipraktekkan
   - Buat repository test untuk eksperimen

2. **Jangan takut salah**
   - Hampir semua kesalahan bisa di-undo
   - Gunakan `git reflog` untuk recovery

3. **Baca error message**
   - Git memberikan pesan error yang informatif
   - Seringkali sudah include solusinya

4. **Gunakan `git status` sesering mungkin**
   - Selalu cek status sebelum melakukan action
   - Membantu memahami kondisi repository

5. **Commit often dengan message yang jelas**
   - Small, focused commits lebih baik dari big commits
   - Message yang jelas membantu collaboration

---

## Command Git yang Paling Sering Digunakan

```bash
# Setup
git config --global user.name "Nama Anda"
git config --global user.email "email@example.com"

# Repository
git init                          # Buat repo baru
git clone <url>                   # Clone repo

# Basic Workflow
git status                        # Cek status
git add <file>                    # Stage file
git add .                         # Stage semua
git commit -m "message"           # Commit
git log --oneline                 # Lihat history

# Branch
git branch                        # Lihat branch
git branch <name>                 # Buat branch
git checkout <name>               # Pindah branch
git checkout -b <name>            # Buat + pindah branch
git merge <branch>                # Merge branch

# Remote
git remote -v                     # Lihat remote
git push origin <branch>          # Push ke remote
git pull origin <branch>          # Pull dari remote
git fetch origin                  # Fetch perubahan

# Undo
git restore <file>                # Batalkan perubahan
git restore --staged <file>       # Unstage
git reset --soft HEAD~1           # Undo commit (keep changes)
git reset --hard HEAD~1           # Undo commit (delete changes)
git revert <commit>               # Revert commit (safe)
```

---

## Resources Tambahan

### Dokumentasi Official
- [Git Official Docs](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2) (Gratis)

### Interactive Learning
- [Learn Git Branching](https://learngitbranching.js.org/) - Visual interactive
- [GitHub Learning Lab](https://lab.github.com/)
- [Git Immersion](http://gitimmersion.com/)

### Cheat Sheets
- [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Atlassian Git Cheat Sheet](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)

### Tools
- [GitHub Desktop](https://desktop.github.com/) - GUI untuk Git
- [GitKraken](https://www.gitkraken.com/) - Visual Git client
- [Sourcetree](https://www.sourcetreeapp.com/) - Free Git GUI

---

## Butuh Bantuan?

Jika menemui kesulitan:

1. **Baca ulang materi** yang relevan
2. **Cek section troubleshooting** di setiap materi
3. **Gunakan `git help <command>`** untuk bantuan command tertentu
4. **Search di Stack Overflow** untuk error spesifik
5. **Practice dengan project test** sebelum apply ke project penting

---

## Selamat Belajar!

Git adalah skill yang sangat penting untuk developer. Dengan menguasai Git, Anda akan:

- Lebih terorganisir dalam coding
- Bisa berkolaborasi dengan tim
- Lebih percaya diri dalam mengembangkan fitur baru
- Bisa kontribusi ke open source projects
- Memiliki backup otomatis untuk kode Anda

**Happy Learning!**

---

## Progress Tracking

- [ ] 1. Pengenalan Git
- [ ] 2. Instalasi dan Konfigurasi
- [ ] 3. Membuat Repository
- [ ] 4. Basic Workflow Git
- [ ] 5. Melihat History
- [ ] 6. Bekerja dengan Branch
- [ ] 7. Merging Branches
- [ ] 8. Remote Repository
- [ ] 9. Membatalkan Perubahan
- [ ] 10. Git Ignore

---

*Materi ini dibuat untuk membantu Anda belajar Git dari dasar hingga mahir.*


---
title: Git (Instalasi dan Penggunaan Umum)
sidebar_position: 4
---

## Preface

Rekayasa perangkat lunak, atau yang lebih sering dipanggil _software
engineering_, adalah proses mendesain, membangun, dan memperbarui solusi
berbasis perangkat lunak.

Proses ini sering kali melibatkan dua atau lebih developer yang membuat
perubahan secara serentak pada kode aplikasi. Apakah kalian pernah berpikir,
_"Gimana ya cara kerja sama ama orang lain buat ngerjain suatu projek? Masa
ngoding pake Google Drive?"_

Modul ini dibangun untuk menjawab pertanyaan tersebut. Setelah menyelesaikan
modul ini, kami harap bahwa kalian akan mendapatkan insight mengenai topik-topik
berikut:

1. Kerja sama dalam tim
2. Definisi version control system (VCS)
3. Menggunakan Git sebagai VCS
4. Penggunaan dasar Git
5. Workflow kerja sama menggunakan Git
6. Penggunaan dasar GitHub

## Teamwork: Expectations vs Reality

Secara teoretis, mengerjakan suatu tugas dalam kelompok berukuran `n` akan
menurunkan waktu pengerjaan tugas tersebut hingga hanya `1/n` dari apabila tugas
tersebut hanya dikerjakan satu orang.

Tentu saja, hal tersebut jarang kita observasi dalam dunia nyata. Fenomena ini
disebabkan banyak hal, seperti performa bervariasi antara anggota kelompok,
ketergantungan antara sub-tugas, atau pembagian tugas yang tidak teratur.

Oleh karena itu, dalam bentuk kerja sama apa pun, kita berusaha untuk
meningkatkan efektivitas penyelesaian tugas, serta meningkatkan kualitas
keseluruhan dari produk yang dihasilkan.

Dalam keseharian sebagai mahasiswa, kalian pasti sudah sering menggunakan alat
kolaborasi seperti Google Docs dan media sosial (e.g. LINE, WhatsApp, Discord)
untuk memfasilitasi kerja sama antar anggota kelompok.

Dalam mengerjakan projek berbasis kode, serta selama berjalannya kegiatan SBF,
kalian akan menggunakan suatu kategori alat bernama Version Control System (VCS)
sebagai media utama untuk kerja sama kalian selama mengerjakan tugas kelompok.

## Version Control System (VCS)

### Terminology

Untuk bagian ini, kata-kata berikut memiliki arti spesifik sesuai dengan daftar
berikut:

| Kosakata      | Definisi                                                 |
| ------------- | -------------------------------------------------------- |
| Branch        | Jalur pengembangan independen projek                     |
| Versi         | Tangkapan projek pada suatu titik waktu                  |
| Diff          | Perbandingan antara berkas-berkas dari dua versi projek  |
| Child branch  | Branch yang merupakan hasil branching dari suatu branch  |
| Parent branch | Branch yang merupakan sumber branching dari suatu branch |

### What is a VCS?

Version Control System adalah _software_ yang melacak perubahan pada suatu
projek. Fitur utama dari VCS adalah kemampuannya untuk melacak perubahan
berbagai versi dari projek tersebut—hence, **version** control.

Secara kasar, suatu VCS memiliki fungsionalitas dasar berikut:

1. Memulai projek dari satu branch utama (_main_ branch)
2. Membuat branch baru dari branch utama
3. Membuat branch baru dari branch non-utama
4. Melacak perubahan pada suatu branch
5. Melakukan diff (_diffing_) antara dua versi projek
6. Menggabungkan suatu branch dengan parent branchnya

:::info

_Technically_, fungsionalitas ini ada pada _distributed_ VCS, seperti Git.
Terdapat model _centralized_ VCS, dimana fungsionalitas ini belum tentu berlaku.
Model _centralized_ VCS seperti yang diimplementasikan pada CVS, Perforce, dan
SVN lebih jarang digunakan dalam development skala kecil, namun masih sering
digunakan di industri.

:::

## Git as a VCS

Git adalah sebuah VCS yang diciptakan oleh Linus Torvalds (pencipta Linux juga
loh :D) pada tahun 2005 untuk memfasilitasi pengembangan Linux Kernel. Sejak
waktu penciptaannya, Git telah berkembang dari versi awal yang relatif primitif,
hingga statusnya saat ini sebagai VCS yang dewasa dan penuh fitur.

**Pada modul ini, kalian akan mengunduh Git, lalu menjalankan beberapa _command_
dasar yang akan sering kalian gunakan saat menggunakan Git.**

### Installing Git

import Tabs from '@theme/Tabs'; import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="windows" label="Windows" default>
    Pada Windows, kami merekomendasikan untuk menggunakan Windows Package Manager (`winget`) untuk instalasi yang cepat. Buka PowerShell atau Command Prompt dan jalankan:
    ```bash
    winget install --id Git.Git -e
    ```
    Alternatifnya, Anda dapat mengunduh installer resmi dari [git-scm.com](https://git-scm.com/download/win) dan menjalankannya. Opsi instalasi default sudah cukup untuk sebagian besar pengguna dan akan menyertakan **Git Bash**, sebuah terminal yang sangat berguna.
  </TabItem>
  <TabItem value="linux" label="Linux">
    Gunakan package manager yang tersedia pada distribusi Linux Anda.
    - **Debian/Ubuntu:**
      ```bash
      sudo apt update && sudo apt install git
      ```
    - **Fedora:**
      ```bash
      sudo dnf install git
      ```
    - **Arch Linux:**
      ```bash
      sudo pacman -S git
      ```
    Jika Anda menggunakan [Homebrew](https://brew.sh/), Anda juga dapat menginstall Git dengan:
    ```bash
    brew install git
    ```
  </TabItem>
  <TabItem value="macos" label="macOS">
    Cara termudah untuk menginstall Git pada macOS adalah dengan menginstall Xcode Command Line Tools. Cukup jalankan `git` pada terminal, dan macOS akan memandu Anda untuk melakukan instalasi jika belum ada.

    Jika Anda adalah pengguna [Homebrew](https://brew.sh/), Anda dapat menjalankan:

    ```bash
    brew install git
    ```

  </TabItem>
</Tabs>

### Configuring Git

Setelah Git terinstall, ada satu langkah konfigurasi penting yang harus
dilakukan. Anda perlu memberitahu Git siapa Anda, sehingga setiap kontribusi
yang Anda buat akan tercatat dengan benar.

Jalankan perintah berikut, ganti `"Your Name"` dan `"youremail@example.com"`
dengan data diri Anda:

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

Konfigurasi ini akan disimpan secara global, jadi Anda hanya perlu melakukannya
sekali per komputer.

## Git Basic Usage

### Initialization

Kita dapat menggunakan Git untuk menerapkan version control pada folder apa pun.
Namun, kita perlu menginisialisasikan Git pada folder tersebut terlebih dahulu.
Untuk eksperimentasi, buatlah sebuah folder kosong dengan nama `git-test`.

Jalankan perintah berikut di PowerShell, bash, atau terminal Mac kalian:

```bash
mkdir git-test
```

Command ini akan membuat sebuah folder kosong dengan nama `git-test`. Saat ini,
Git masih belum dapat digunakan untuk melakukan version control pada folder ini.
Untuk menginisialisasikan Git pada folder `git-test`, jalankan perintah berikut:

```bash
cd git-test   # Berpindah ke folder git-test
git init      # Inisialisasi Git
```

Pause. Lihatlah isi dari folder `git-test` sekarang. Jalankan perintah berikut
untuk melihat isi suatu folder:

```bash
ls -al        # Mencetak daftar isi folder, termasuk file tersembunyi
```

Perhatikan bahwa hanya terdapat folder `.git` di dalam `git-test`. Folder `.git`
ini menunjukkan bahwa folder `git-test` sedang di-_track_ oleh Git, dan setiap
perubahan pada file di dalam folder `git-test` dapat di-_track_ oleh Git. Dengan
demikian, folder `git-test` sekarang dapat disebut sebagai sebuah **Git
repository**, atau yang sering disingkat menjadi **repo**.

Coba jalankan perintah `git status` pada terminal, dan perhatikan keluarannya.

```bash
$ git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Keluaran ini menyatakan bahwa:

1. Kita sedang di branch main
2. Belum ada commit pada branch main
3. Belum ada _tracked changes_ yang dapat kita commit

Demikian adalah status dari suatu repositori Git kosong. Kita akan mulai
mengeksplor fungsionalitas Git dari repositori kosong ini.

### General workflow

Pada dasarnya, menggunakan Git sebagai alat _version control_ terdiri atas 3
langkah.

1. Pertama, kita membuat perubahan pada kode. Saat ini, kita belum memberi tahu
   Git bahwa kode kita sudah berubah, jadi status perubahan pada kode kita masih
   bersifat **untracked**.

2. Kedua, kita memberi tahu Git bahwa kode kita sudah berubah, sehingga status
   perubahan pada kode kita sekarang bersifat **tracked**.

3. Ketiga, kita memberi tahu Git untuk membuat **versi** baru dari kode kita
   dari perubahan yang sudah bersifat _tracked_.

Setelah ketiga langkah ini, Git sudah membuat versi baru dari kode kamu
berdasarkan perubahan-perubahan pada langkah pertama.

Nah, saat menggunakan Git, kata **commit** digunakan untuk merujuk pada suatu
**versi** kode. Alhasil, kita biasa menuliskan ulang tiga langkah di atas
sebagai berikut:

1. Pertama, kita membuat perubahan pada kode. Perubahan pada kode saat ini belum
   di-**track** oleh Git (_create untracked changes_).

2. Kedua, kita **track** perubahan yang sudah dibuat pada kode (_track file
   changes_)

3. Ketiga, kita **commit** perubahan yang sudah di _track_, sehingga
   manghasilkan **commit** baru (_commit tracked changes_).

:::note

Perhatikan bahwa kata _commit_ di sini digunakan sebagai kata kerja dan kata
benda. Kata _commit_ sebagai kata kerja berarti "membuat versi baru", sedangkan
kata _commit_ sebagai kata benda merujuk pada "versi baru" hasil _commit_ (kata
kerja).

:::

### Applying what we know (so far)

Di folder `git-test` yang sudah kamu buat sebelumnya, jalankan perintah-perintah
berikut:

```bash
touch init.txt     # Membuat file baru

git status          # Cek status Git repository
```

Saat ini, file `init.txt` belum di-_track_ oleh Git. So, let's do just that!

```bash
git add init.txt   # Track file init.txt

git status
```

Setelah menjalankan command `git add init.txt`, Git mencatat semua perubahan
pada file `init.txt` (pada kasus ini, penciptaan file `init.txt`). Untuk membuat
_commit_ (read: versi) baru dari repositori `git-test`, yakni versi yang
memiliki file `init.txt`, kita perlu menjalankan command berikut:

```bash
git commit
```

Nah, suatu detail teknis disini yang belum kita sebutkan sebelumnya adalah bahwa
kita perlu menambahkan suatu **commit message** pada setiap commit.

Apabila kamu baru menjalankan `git commit` tanpa argumen tambahan, maka
kemungkinan besar bahwa komputer kamu baru membuka sebuah text editor, dan
sedang meminta kamu untuk memberikan commit tersebut sebuah _commit message_.

_Commit message_ ini merupakan suatu catatan mengenai _commit_ yang dibuat.
Biasanya, _commit message_ digunakan untuk mencatat perubahan yang dibuat pada
_commit_ tersebut.

Berikan commit tersebut suatu comment yang sederhana, lalu save dan tutup code
editor kamu.

Setelah menjalankan `git commit`, jalankan perintah `git log`.

```bash
$ git log
commit 2da6e0a54001637ee6c08c41bb5416b9878dab34 (HEAD -> main)
Author: thorbert <tobyas2005139@gmail.com>
Date:   Mon Sep 22 21:39:03 2025 +0700

    hello there, this is a commit message

```

Output di atas adalah hasil saya sendiri saat menjalankan `git log`. Perhatikan
struktur dari suatu _commit_ (ingat bahwa _commit_ merujuk pada suatu versi
projek).

Perhatikan struktur commit berikut:

```plaintext
                  Commit hash                    Current position
commit 2da6e0a54001637ee6c08c41bb5416b9878dab34   (HEAD -> main)

            Penulis commit
Author: thorbert <tobyas2005139@gmail.com>

      Tanggal pembuatan commit
Date:   Mon Sep 22 21:39:03 2025 +0700

               Commit message
    hello there, this is a commit message

```

Unsur yang perlu kalian perhatikan adalah **commit hash**. Setiap _commit_
memiliki sebuah _hash_ yang digunakan oleh Git sebagai pembeda (identifier)
untuk masing-masing _commit_.

Dengan menjalankan perintah-perintah sebelumnya, kamu telah menggunakan
fungsionalitas dasar Git untuk melacak perubahan pada suatu Git _repository_
pada satu branch.

### Git Branching

Untuk membuat sebuah _branch_ dari `main`, pastikan dulu kalau kamu sedang
berada pada _branch_ `main` dengan menjalankan perintah berikut:

```bash
git branch
```

Perintah ini mencetak semua branch yang ada pada Git _repository_ kamu.
Seharusnya, output dari perintah `git branch` di atas adalah sebagai berikut:

```plaintext
$ git branch
* main
```

Dari branch `main`, buat branch baru dengan nama `feat/poem`, yang menandakan
bahwa kita akan membuat suatu branch untuk mengerjakan fitur puisi pada projek
kita.

```bash
git branch feat/poem    # Buat branch dengan nama feat/poem
git switch feat/poem    # Berpindah dari branch main ke feat/poem
git branch              # Periksa current branch

# Perintah berikut adalah shortcut untuk membuat branch baru
git switch -c feat/poem
```

:::info

Terdapat dua perintah yang dapat digunakan untuk berpindah branch, yakni
`checkout` dan `switch`. Pada Git versi 2.23, perintah `switch` ditambahkan
karena perintah `checkout` dianggap mencakup terlalu banyak use case. Untuk
scope tutorial ini, perbedaan antara `switch` dan `checkout` hanyalah bahwa
penggunaan `checkout` akan meng-overwrite working directory apabila terdapat
_untracked files_, sedangkan `switch` tidak akan melakukan hal tersebut.

:::

### Branch merging

Setelah membuat perubahan pada suatu branch, sering kali, kita akan perlu
menyatukan perubahan yang dibuat pada branch tersebut ke _parent_ branch nya.

Berikut adalah perintah yang dapat digunakan untuk melakukan _merge_ dari _child
branch_ C ke _parent branch_ P:

```bash
git switch P  # Pindah ke parent branch
git merge C   # Gabungkan perubahan dari child branch ke parent branch
```

:::tip[Do it yourself]

Apabila kamu sudah membuat branch `feat/poem` di repositori `git-test` pada
bagian sebelumnya, cobalah untuk melakukan hal berikut:

1. Buat sebuah file dengan nama `poem.txt`.
2. Tulis sebuah puisi singkat dalam file tersebut.
3. Track dan commit perubahan dalam folder `git-test`.
4. Merge branch `feat/poem` ke branch `main`.

:::

### Resolving merge conflicts

Saat bekerja dalam tim, akan ada saatnya dimana dua orang mengubah baris kode
yang sama pada berkas yang sama. Saat Git mencoba untuk menggabungkan kedua
perubahan ini, Git tidak akan tahu versi mana yang benar. Fenomena ini disebut
sebagai **merge conflict**.

#### Simulating a merge conflict

Mari kita coba simulasikan sebuah _merge conflict_.

Pertama, pastikan kamu berada di branch `feat/poem`. Di dalam branch ini, ubah
isi dari `poem.txt`.

```bash
# Diasumsikan Anda berada di branch feat/poem

# TASK: Tambahkan satu baris puisi pada file poem.txt

git commit -am "feat: add first line of poem"
```

:::info

Command `git commit -am "..."` adalah _shortcut_ untuk `git add .` yang
dilanjutkan dengan `git commit -m "..."`. Flag `-a` akan secara otomatis
men-`stage` semua berkas yang sudah di-_track_ oleh Git sebelumnya.

:::

Sekarang, kembali ke branch `main` dan buat perubahan yang akan berkonflik
dengan perubahan di atas.

```bash
# 1. Berpindah ke branch main
git switch main

# 2. TASK: Ubah baris pertama dari file poem.txt

# 3. Commit perubahan pada branch main
git commit -am "feat: add a note to poem.txt"
```

Saat ini, histori dari branch `main` dan `feat/poem` telah berbeda (_diverged_).
Keduanya memiliki versi `poem.txt` yang berbeda. Mari kita coba gabungkan
(`merge`) branch `feat/poem` ke `main`.

```bash
git merge feat/poem
```

#### Identifying conflicts

Git akan menghentikan proses _merge_ dan memberikan output berikut:

```plaintext
Auto-merging poem.txt
CONFLICT (content): Merge conflict in poem.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git memberitahu kita bahwa ada konflik pada berkas `poem.txt`. Jika kita membuka
`poem.txt`, kita akan melihat penanda konflik yang ditambahkan oleh Git:

```plaintext
<<<<<<< HEAD
ini adalah sebuah catatan, bukan puisi
=======
baris pertama dari sebuah puisi
>>>>>>> feat/poem
```

- Teks di antara `<<<<<<< HEAD` dan `=======` adalah versi dari branch `main`
  (branch kita saat ini, atau `HEAD`).
- Teks di antara `=======` dan `>>>>>>> feat/poem` adalah versi dari branch
  `feat/poem` (branch yang ingin kita _merge_).

#### Resolving conflicts

Untuk menyelesaikan konflik, kita harus mengubah berkas tersebut secara manual
dan menghapus penanda-penanda yang dibuat oleh Git. Kita perlu memutuskan
bagaimana isi akhir dari berkas tersebut.

Misalnya, kita ingin menyimpan kedua perubahan tersebut. Kita dapat mengubah isi
`poem.txt` menjadi seperti berikut:

```plaintext
baris pertama dari sebuah puisi
ini adalah sebuah catatan, bukan puisi
```

#### Completing merges

Setelah kita menyimpan perubahan pada `poem.txt`, kita perlu memberitahu Git
bahwa konflik telah diselesaikan.

1. **Stage berkas yang sudah diselesaikan** Jalankan `git add` pada berkas
   tersebut untuk menandakan bahwa konflik sudah selesai.

   ```bash
   git add hello.txt
   ```

2. **Commit hasil merge** Jalankan `git commit` untuk membuat sebuah _merge
   commit_ baru yang menyatukan kedua histori branch.

   ```bash
   git commit
   ```

Saat menjalankan `git commit`, Git akan membuka text editor dengan pesan
_commit_ yang sudah terisi, contohnya `Merge branch 'feat/poem'`. Kamu hanya
perlu menyimpan pesan tersebut dan menutup editor untuk menyelesaikan proses
_merge_.

Selamat, kamu telah berhasil menyelesaikan sebuah _merge conflict_!

## Collaborating on GitHub

> GitHub is a code hosting platform for collaboration and version control.
>
> GitHub lets you (and others) work together on projects.
>
> — W3Schools

GitHub adalah platform kolaborasi dan penyimpanan kode yang menggunakan Git
sebagai VCS. Setiap perubahan yang dilakukan pada kode dilakukan lewat
perintah-perintah Git.

Bagian ini akan menjelaskan secara singkat alur kerja sama pada GitHub.

### Remote vs Local Git repositories

Pada langkah-langkah sebelumnya, kamu telah menggunakan Git sebagai alat untuk
mengatur versi repositori pada _device_ (laptop/desktop) kalian sendiri.

Git memiliki fitur _remote repositories_, yakni kemampuan untuk melakukan
_tracking_ pada _repository_ yang di-_host_ secara remote pada layanan seperti
GitHub atau GitLab.

Git memiliki command `git remote` yang dapat digunakan untuk mengelola
repositori-repositori _remote_ yang berhubungan dengan repository lokal.

### Creating a remote GitHub repository

Apabila kamu belum memiliki akun GitHub, buatlah sebuah akun sesuai dengan
username bebas.

Kemudian, klik profil kamu di sisi kanan atas, dan pergi ke halaman
**Repositories**. Pada halaman tersebut, buatlah sebuah repository baru dengan
nama `sbf-week-0`.

Seharusnya, kamu tidak perlu mengubah pengaturan apa-apa pada halaman pembuatan
repository. Langsung saja tekan tombol _Create repository_.

Pada titik ini, kalian sudah memiliki sebuah _remote repository_ di GitHub.

:::tip[Do it yourself]

Kamu bisa menyimpan pekerjaan kalian di folder `git-test` di _remote repository_
`sbf-week-0` nih.

Di halaman repositori GitHub `sbf-week-0`, kamu dapat melihat bahwa terdapat
command untuk mengunggah repositori lokal ke repositori _remote_ (_push an
existing repository from the command line_).

```bash
git remote add origin git@github.com:<username_kalian>/sbf-week-0.git
git branch -M main
git push -u origin main
```

Jalankan perintah tersebut di terminal lokal kalian dari folder `git-test`, lalu
_refresh_ halaman repositori `sbf-week-0` di GitHub. Kalian akan melihat bahwa
isi dari `git-test` sekarang sudah ada pada _branch_ `main` di repositori
GitHub.

:::

### Remote repository related commands

Terdapat beberapa terminologi yang digunakan saat menggunakan _remote
repository_ yang tidak kita gunakan saat menggunakan Git secara lokal.

#### <code style={{fontSize: 24}}>pull</code>

Perintah `git pull` digunakan untuk mengambil (fetch) dan mengintegrasikan
(merge) perubahan dari _remote repository_ ke _local repository_ Anda. Ini
adalah cara umum untuk memperbarui _local branch_ Anda dengan perubahan terbaru
dari rekan tim Anda.

Secara teknis, `git pull` adalah gabungan dari dua perintah lain: `git fetch`
diikuti oleh `git merge`.

- `git fetch`: Mengunduh konten (commit, file, dan referensi) dari _remote
  repository_ ke _local repository_ Anda, tetapi tidak secara otomatis
  mengintegrasikannya ke dalam _working directory_ Anda.
- `git merge`: Menggabungkan perubahan yang telah diunduh ke dalam _branch_ Anda
  saat ini.

**Cara Penggunaan:**

Sintaks paling umum untuk `git pull` adalah:

```bash
git pull <remote_name> <branch_name>
```

- `<remote_name>`: Nama dari _remote repository_. Secara _default_, namanya
  adalah `origin`.
- `<branch_name>`: Nama _branch_ di _remote repository_ yang ingin Anda tarik
  perubahannya.

**Contoh:**

Misalkan Anda ingin memperbarui _branch_ `main` lokal Anda dengan perubahan
terbaru dari _remote repository_ `origin`.

```bash
# Pastikan Anda berada di branch main
git switch main

# Tarik perubahan dari branch main di remote origin
git pull origin main
```

Perintah ini akan mengambil semua _commit_ baru dari _branch_ `main` di `origin`
dan menggabungkannya ke dalam _branch_ `main` lokal Anda.

#### <code style={{fontSize: 24}}>push</code>

Perintah `git push` digunakan untuk mengunggah konten dari _local repository_
Anda ke _remote repository_. Ini adalah cara Anda membagikan perubahan yang
telah Anda buat kepada anggota tim lainnya.

Setelah Anda membuat beberapa _commit_ secara lokal, Anda dapat "mendorong"
_commit-commit_ tersebut ke _remote repository_ agar orang lain dapat
melihatnya.

**Cara Penggunaan:**

Sintaks dasar untuk `git push` adalah:

```bash
git push <remote_name> <branch_name>
```

- `<remote_name>`: Nama dari _remote repository_ tujuan (biasanya `origin`).
- `<branch_name>`: Nama _branch_ yang ingin Anda dorong perubahannya.

**Contoh:**

Misalkan Anda telah selesai mengerjakan sebuah fitur di _branch_
`feat/new-feature` dan ingin mengunggahnya ke GitHub.

```bash
# Mendorong branch feat/new-feature ke remote origin
git push origin feat/new-feature
```

**Push Pertama Kali dan `-u` Flag:**

Saat Anda mendorong sebuah _branch_ untuk pertama kalinya, sangat disarankan
untuk menggunakan _flag_ `-u` (atau `--set-upstream`):

```bash
git push -u origin feat/new-feature
```

_Flag_ `-u` akan membuat hubungan (_tracking connection_) antara _branch_ lokal
Anda (`feat/new-feature`) dan _branch_ _remote_ (`origin/feat/new-feature`).
Setelah hubungan ini dibuat, Anda dapat menjalankan `git pull` dan `git push`
dari _branch_ tersebut tanpa harus menentukan _remote_ dan _branch_ setiap saat.
Cukup jalankan:

```bash
git push  # Akan otomatis mendorong ke origin/feat/new-feature
git pull  # Akan otomatis menarik dari origin/feat/new-feature
```

#### <code style={{fontSize: 24}}>Pull Request (PR)</code>

Saat bekerja secara lokal, setelah kita selesai membuat perubahan pada suatu
branch, kita biasa akan melakukan merge dari _branch_ perubahan ke _parent_
branch\_.

Namun, apabila kita sedang bekerja dalam suatu tim di suatu _remote repository_,
tentu saja kita tidak bisa menggabungkan kode secara semena-mena. Setiap
perubahan pada kode harus dapat diinspeksi dan ditinjau kembali oleh penanggung
jawab suatu projek.

Oleh karena itu, saat kita sudah selesai membuat perubahan pada suatu branch,
kita perlu membuat suatu **pull request**, yang secara kasar memiliki arti
'pengajuan untuk menggabungkan branch.'

Proses membuat sebuah _pull request_ adalah sebagai berikut:

1. `push` sebuah branch lokal ke repositori _remote_.
2. Pergi ke laman repositori _remote_ di GitHub.
3. Pergi ke branch yang sudah di-`push` di GitHub.
4. Tekan tombol `Contribute` yang ada di panel utama halaman.
5. Pastikan bahwa _base branch_ yang menjadi target _pull request_ sudah benar.
6. Buatlah pull request dengan komentar opsional yang sesuai.

## Closing remarks

Pada modul ini, kamu sudah belajar mengenai Version Control System,
perintah-perintah dasar Git, dan penggunaan GitHub untuk kolaborasi berbasis
Git. Modul ini akan kalian gunakan selama keseluruhan kegiatan SBF, dan dapat
kalian gunakan juga sebagai cheatsheet apabila kamu lupa atau ingin mendapatkan
intuisi mengenai cara menggunakan Git dengan baik.

Terima kasih banyak atas perhatiannya semua, semoga modul ini sudah memaparkan
materi dengan baik supaya kalian bisa bekerja sama dengan lebih baik yaa \<3

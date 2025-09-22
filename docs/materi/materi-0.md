---
title: Git Workflow
sidebar_position: 4
---

# Git (Installation, How to Handle Conflict, etc)

## Preface

Rekayasa perangkat lunak, atau yang lebih sering dipanggil Software Engineering,
adalah proses mendesain, membangun, dan memperbarui solusi berbasis perangkat
lunak.

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

<!-- TODO: FINISH INSTALLATION SECTION -->

import Tabs from '@theme/Tabs'; import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="windows" label="Windows" default>
    Dive into the world of knowledge with a captivating book 📚
  </TabItem>
  <TabItem value="linux" label="Linux">
    Admire the strokes of artistry on a beautiful painting 🖼️
  </TabItem>
  <TabItem value="macos" label="macOS">
    Let the soothing melodies of music transport you 🎶
  </TabItem>
</Tabs>

### Configuring Git

## Git Basic Usage

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

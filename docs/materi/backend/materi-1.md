---
title: Materi 1 
sidebar_position: 1
---

# Intro to Django (Installation, MVT, Structures, App)
Django adalah framework berbasis Python untuk membangun aplikasi web dengan cepat, aman, dan scalable.

## Tutorial Instalasi Django
**Langkah 1: Install Python**

`python --version`<br/>
Pastikan versionnya 3.x, kalau versionnya ga sesuai (kurang dari 3.x) download dari link:
https://www.python.org/downloads/

**Langkah 2: Instalasi Django**<br/>
`pip install django`

Cek versi<br/>
`django-admin --version`

**Langkah 3: Membuat Direktori dan Menyiapkan Virtual Environment**

Virtual environment (venv) adalah environment python terisolasi. Dibuat folder khusus berisi python dan library untuk satu projek. Jadi untuk tiap projek kalian dependenciesnya ga saling ganggu.

**Kenapa Perlu Virtual Env?**<br/>
Dengan virtual environment<br/>
✅ Setiap project punya library sendiri, minim konflik<br/>
✅ Environment project lebih rapi, gampang dipindah/di-deploy.<br/>
✅ Best practice.<br/>
✅ Pip list di dalam venv cuma berisi yang memang project itu butuh.

Tanpa virtual environment<br/>
⚠️ Semua package ke-install global cenderung numpuk, berantakan.<br/>
⚠️ Konflik version (project A butuh Django 4, project B butuh Django 5).<br/>
⚠️ Susah tracking dependency spesifik project.<br/>
⚠️ Kalau upgrade/downgrade 1 package, bisa buat project lain gabisa di run lokal.<br/>

1. Buat folder baru
2. Dalam direktori tersebut buka command prompt atau terminal. Atau dapat juga dibuka dengan VScode.

3. Jalankan command berikut.

Membuat env<br/>
`python -m venv env`

Mengaktifkan env<br/>
- windows<br/>
`env\Scripts\activate`<br/>
- linux<br/>
`source env/bin/activate`<br/>

Apabila env sudah menyala, akan terlihat dengan tulisan (env) di line terminal

Mematikan env<br/>
`deactivate`

**Langkah 4: Menyiapkan Dependencies dan Membuat Projek Django**

Dependencies adalah komponen atau modul yang diperlukan oleh suatu perangkat lunak untuk berfungsi, termasuk library, framework, atau package. Hal tersebut memungkinkan pengembang memanfaatkan kode yang telah ada, mempercepat pengembangan, tetapi juga memerlukan manajemen yang hati-hati untuk memastikan kompatibilitas versi yang tepat. Penggunaan virtual environment membantu mengisolasi dependencies antara proyek-proyek yang berbeda.

1. Pada direktori yang sama, buat file `requiirements.txt` dan tambahkan dependency berikut<br/>
```
django
gunicorn
whitenoise
psycopg2-binary
requests
urllib3
```

2. Lakukan instalasi dependencies yang ada dengan perintah berikut. (Jalankan venv sebelum menjalankan perintah ini)

`pip install -r requirements.txt`

3. Buat proyek Django dengan nama bebas, buat dengan perintah berikut.<br/>
`django-admin startproject nama_proyek .`<br/>
Note: Pastikan karakter `.` tertulis diakhir command<br/>

**Langkah 5: Konfigurasi dan Menjalankan Server**
1. Tambahkan string berikut pada ALLOWED_HOSTS di settings.py untuk keperluan deployment<br/>
```
...
ALLOWED_HOSTS = ["localhost", "127.0.0.1"]
...
```
Dalam konteks deployment, ALLOWED_HOSTS berfungsi sebagai daftar host yang diizinkan untuk mengakses aplikasi web. Disini kita meng set `localhost` dan ip `127.0.0.1` pada ALLOWED_HOSTS, artinya kita memberi izin ke peramban lokal kita untuk dapat meng run aplikasi secara lokal.<br/>

Note: Variabel inilah yang kita edit agar aplikasi kita dapat dibuka pada saat deployment.

2. Menjalankan Server<br/>
Pastikan bahwa berkas manage.py ada pada direktori yang aktif pada terminal kamu saat ini. Untuk menjalankan server Django, jalankan perintah berikut:
- windows<br/>
`python manage.py runserver`<br/>
- linux<br/>
`python3 manage.py runserver`<br/>

3. Buka server django pada browser favorit anda dengan memasukkan link http://localhost:8000. Apabila proyek Django di setup dengan benar, anda akan melihat animasi roket :D.

**Langkah 4: Mematikan Server**
1. Matikan server dengan menekan `ctrl + c` pada terminal anda
2. Matikan venv dengan menginput `deactivate`.

## Django MVT


## Structures

## App

This is page 1.
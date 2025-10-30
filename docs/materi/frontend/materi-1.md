---
title: Materi 1 
sidebar_position: 1
---

# Intro to HTML & CSS

## Pengenalan HTML

HyperText Markup Language (HTML) adalah bahasa standar untuk membuat struktur halaman web. HTML terdiri atas elemen-elemen yang digunakan untuk menampilkan berbagai jenis konten. Elemen-elemen ini ditulis menggunakan tag.

HTML **bukan bahasa pemrograman** karena tidak memiliki variabel, kondisi, maupun logika. HTML hanya berfungsi untuk menyusun struktur halaman.

### Cara Menjalankan File HTML

Setelah kita tahu bahwa HTML adalah bahasa untuk menyusun struktur halaman web, sekarang pertanyaan berikutnya adalah bagaimana cara melihat hasilnya? HTML tidak perlu di-*compile* seperti bahasa pemrograman, cukup dibuka dengan browser. Caranya yaitu buat file dengan ekstensi `.html`, misalnya `index.html`, lalu tulis struktur dasar HTML (di bawah). Kemudian buka file tersebut dengan browser (chrome, firefox, edge, atau safari). kamu bisa klik dua kali filenya atau klik kanan lalu pilih **open with → browser**. begitu terbuka, halaman HTML akan langsung ditampilkan.

Kamu juga bisa menjalankan file HTML lewat server lokal. Misalnya kalau kamu punya python, cukup jalankan perintah `python -m http.server` di terminal atau `python3 -m http.server` untuk Mac. Setelah itu buka alamat `http://localhost:8000` di browser, maka file HTML akan terbuka melalui server.

### Struktur Dasar HTML

Contoh kerangka dasar dokumen HTML:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello World</title>
</head>
<body>
  <h1>Hello World</h1>
  <p>Ini adalah sebuah paragraf.</p>
</body>
</html>
```

* `<!DOCTYPE html>` : mendefinisikan bahwa dokumen ini menggunakan HTML5.
* `<html>` : elemen induk dari seluruh isi halaman.
* `<head>` : bagian yang berisi metadata, judul, stylesheet, dan skrip.
* `<body>` : bagian utama yang berisi konten yang ditampilkan di halaman.

### Elemen Teks

#### Heading

Digunakan untuk membuat judul atau subjudul.

```html
<h1>Judul Utama</h1>
<h2>Subjudul</h2>
<h3>Sub-subjudul</h3>
<h4>Judul Level 4</h4>
<h5>Judul Level 5</h5>
<h6>Judul Level 6</h6>
```

* `<h1>` digunakan sekali sebagai judul utama halaman.
* `<h2>` hingga `<h6>` digunakan untuk hierarki subjudul.

#### Paragraf

```html
<p>Ini adalah sebuah paragraf.</p>
<p>Ini adalah paragraf kedua.</p>
```

Dipakai untuk teks dalam bentuk paragraf.

#### Span dan Div

```html
<div>
  <h2>Bagian Konten</h2>
  <p>Teks ini berada di dalam elemen div.</p>
</div>

<p>Teks ini mengandung <span style="color:red;">kata berwarna merah</span>.</p>
```

* `<div>` : digunakan sebagai wadah blok konten.
* `<span>` : digunakan sebagai wadah teks kecil dalam satu baris.

### Daftar (List)

#### Unordered List

```html
<ul>
  <li>DDP</li>
  <li>PSO</li>
  <li>PSD</li>
</ul>
```

Menampilkan daftar dengan simbol bullet.

#### Ordered List

```html
<ol>
  <li>Dek Depe</li>
  <li>Pak Eso</li>
  <li>Pak Esde</li>
</ol>
```

Menampilkan daftar dengan penomoran.

### Gambar

```html
<img src="foto.jpg" alt="Foto Profil" width="200">
<img src="https://picsum.photos/600/400" alt="Contoh Gambar">
```

* `src` : sumber gambar (lokal atau URL).
* `alt` : deskripsi gambar apabila gagal dimuat.
* `width` dan `height` : menentukan ukuran gambar.

### Tabel

```html
<table border="1">
  <tr>
    <th>Nama</th>
    <th>Umur</th>
  </tr>
  <tr>
    <td>Dek Depe</td>
    <td>18</td>
  </tr>
  <tr>
    <td>Pak Esde</td>
    <td>32</td>
  </tr>
</table>
```

* `<table>` : wadah tabel.
* `<tr>` : baris tabel.
* `<th>` : sel header (tulisan tebal).
* `<td>` : sel data.

## Pengenalan CSS

Cascading Style Sheets (CSS) adalah bahasa untuk mengatur tampilan dan gaya dari elemen HTML. CSS memungkinkan kamu mengatur warna, ukuran, posisi, serta tata letak elemen pada halaman.

### Cara Menyambungkan HTML dengan CSS
1. Buat file baru, namanya misalnya `style.css` (bisa juga nama lain, asal ekstensinya `.css`).
2. Simpan file itu di folder yang sama dengan `index.html`.
3. Di dalam `index.html`, tambahin baris berikut di bagian `<head>`:

```html
<head>
  ...
  <link rel="stylesheet" href="style.css">
</head>
```

4. isi `style.css` dengan aturan css kamu, misalnya:

```css
body {
  background-color: #FEFEFE;
  font-family: Arial, sans-serif;
}
h1 {
  color: black;
  text-align: center;
}
```

### _Selector_ pada CSS

Pada tutorial ini, kita akan mengenak tiga jenis _selector_: **_Element Selector_**, **ID _Selector_**, dan **_Class Selector_**.

1. **_Element Selector_**

    _Element Selector_ memungkinkan kita mengubah properti untuk semua elemen yang memiliki tag HTML yang sama.
    
    Contoh penggunaan _Element Selector_:
    ```html 
    <body>
      <div>
        <h1>Aku h1 ges</h1>
        <h2>Aku h2 ges, beda sama h1 :D</h2>
      </div>
      ...
    </body>
    ```
    
    Kita dapat menggunakan _element_ sebagai _selector_ dalam **_file_ CSS**. _Element selector_ menggunakan format *[id_name]* (tanpa diawali oleh sebuah simbol)
    
    ```html 
    h1 {
      color: #5bc933;
      font-family: "Times New Roman";
      font-style: normal;
    }
    ```
2. **ID _Selector_**

    ID _selector_ menggunakan ID pada _tag_ sebagai _selector_-nya. ID bersifat unik dalam satu halaman web. ID dapat ditambahkan pada halaman template HTML.
    
    Contoh penggunaan ID _Selector_ pada **_template_ HTML**:
    
    ```html 
    <body>
      <div id="header">
        <h1>Bermain bersama ID header</h1>
      </div>
      ...
    </body>
    ```
    
    Kemudian, kita dapat menggunakan ID tersebut sebagai _selector_ dalam **_file_ CSS**. ID _selector_ menggunakan format *#[id_name]* (selalu diawali #)
    ```html 
    #header {
      background-color: #a3b90e;
      margin-top: 0;
      padding: 20px 20px 20px 40px;
    }
    ```
    
3. **_Class Selector_**
    
    _Class Selector_ memungkinkan kita untuk mengelompokkan elemen dengan karakteristik yang sama.
    
    Contoh penggunaan _Class Selector_ pada **_template_ HTML**:
    
    ```html
    <div id="main">
      <div class="parent_content">
        <p class="date">Tanggal 27 September 2025</p>
        <h2>Subjek: Keseruan SBF</h2>
        <p id="content_1">Materi SBF sangat seru!</p>
      </div>

      <div class="parent_content">
        <p class="date">Tanggal 29 September 2025</p>
        <h2>Subjek: Review SBF</h2>
        <p id="content_2">Biar lebih gacor aku review materi SBF</p>
      </div>

      <div class="parent_content">
        <p class="date">Tanggal 29 September 2025</p>
        <h2>Subjek: Ikut Tutorial SBF</h2>
        <p id="content_3">Tutorial SBF sangat seru!</p>
      </div>
    </div>
    ```
    
    Kemudian, kita dapat menggunakan _Class_ tersebut sebagai _selector_ dalam **_file_ CSS**. _Class selector_ menggunakan format *.[class_name]* (diawali .)
    
    ```html
    .parent_content {
      background-color: #E0E0E0;
      margin-bottom: 10px;
      color: #0F0F0F;
      font-family: cursive;
      padding: 20px 20px 20px 40px;
      border-radius: 10px;
    }
    ```
    
Untuk lebih memahami terkait CSS _Selector Reference_, kamu dapat membaca referensi [berikut](https://www.w3schools.com/cssref/css_selectors.php).


### Properti Dasar CSS

#### Ukuran Font

```css
p {
  font-size: 20px; /* px, em, rem, atau % */
}
```

#### Ketebalan Font

```css
h1 {
  font-weight: bold; /* normal, bold, lighter, atau 100–900 */
}
```

#### Jenis Font

```css
body {
  font-family: Arial, Helvetica, sans-serif;
}
```

#### Font Miring

```css
p {
  font-style: italic;
}
```

#### Penataan Teks

```css
h1 {
  text-align: center;  /* left, right, center */
}
p {
  text-align: justify; /* teks rata kanan kiri */
}
```

#### Warna

```css
h2 {
  color: blue; /* warna teks */
  background-color: yellow; /* warna latar belakang */
}
```

#### Ukuran Elemen

```css
img {
  width: 300px;
  height: auto;
}
```

#### Box Model

<!-- ![CSS Box Model](/img/4-box-model-css.png) -->

*Box model* pada CSS pada dasarnya merupakan suatu *box* yang membungkus setiap elemen HTML dan terdiri atas:

* *Content*: isi dari *box* (tempat terlihatnya teks dan gambar)
* *Padding*: mengosongkan area di sekitar konten (transparan)
* *Border*: garis tepian yang membungkus konten dan *padding*-nya
* *Margin*: mengosongkan area di sekitar *border* (transparan)

#### Padding dan Margin

```css
div {
  margin: 20px;   /* jarak luar */
  padding: 10px;  /* jarak dalam */
}
```

#### Border

```css
p {
  border: 2px solid black;
}
```

### Layout dengan CSS

#### Display

```css
span {
  display: inline-block;
  width: 100px;
}
```

Nilai yang umum digunakan: `block`, `inline`, `inline-block`, `none`.

#### Position

```css
.box {
  position: absolute;
  top: 50px;
  left: 100px;
}
```

Jenis posisi: `static`, `relative`, `absolute`, `fixed`, `sticky`.

#### Flexbox

```css
.container {
  display: flex;
  justify-content: space-around;
}
```

#### Media Query

```css
@media (max-width: 600px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
}
```

Digunakan untuk menyesuaikan tampilan di layar kecil.

### Responsive Web Design

*Responsive web design* merupakan metode sistem desain web yang memiliki tujuan untuk menghasilkan tampilan web yang terlihat baik pada seluruh perangkat seperti *desktop*, *tablet*, *mobile*, dan sebagainya. *Responsive web design* tidak mengubah isi dari situs web, tetapi hanya mengubah tampilan dan penataan pada setiap perangkat agar sesuai dengan lebar layar dan kemampuan perangkat tersebut. Dalam *responsive web design* tampilan-tampilan tertentu membutuhkan bantuan CSS (seperti mengecilkan atau membesarkan) suatu elemen. 

Salah satu cara untuk menguji apakah suatu situs web menggunakan *responsive web design* adalah dengan mengakses situs web tersebut dan mengaktifkan fitur `Toggle Device Mode` pada *browser*. Fitur ini memungkinkan kamu untuk melihat bagaimana situs web tersebut ditampilkan pada berbagai perangkat, seperti komputer, tablet, atau *smartphone*, tanpa harus mengubah ukuran jendela *browser*. 

Berikut adalah cara untuk mengakses fitur tersebut pada *browser* **Google Chrome**.
* Windows/Linux: Tekan `CTRL + SHIFT + I` untuk membuka Dev Tools, lalu tekan `CTRL + SHIFT + M` untuk mengaktifkan `Toggle Device Mode`.
* Mac : `Command + Shift + M`
 
Cara lain yang lebih praktis adalah dengan melakukan klik kanan pada *browser* dan memilih *Inspect Element/Inspect* untuk membuka *Dev Tools* yang berguna.

Untuk mempelajari lebih lengkap mengenai *Reponsive Web Design*, kamu dapat membuka referensi [ini](https://web.dev/responsive-web-design-basics/).

#### Teknik Dasar Membuat Website Responsif

1. **Menggunakan Persentase (%) atau Unit Fleksibel (em, rem, vw, vh)**
  Alih-alih menetapkan ukuran dalam piksel tetap, gunakan ukuran relatif agar elemen menyesuaikan ukuran layar.

  ```css
  img {
    width: 80%;   /* bukan 800px */
    height: auto;
  }
  ```

2. **Menggunakan Flexbox**
  Dengan `display: flex;`, elemen dapat otomatis menyesuaikan tata letak.

  ```css
  .container {
    display: flex;
    flex-wrap: wrap; /* elemen akan turun jika layar sempit */
  }
  ```

3. **Menggunakan Media Queries**
  Digunakan untuk memberikan aturan CSS berbeda sesuai ukuran layar.

  ```css
  @media (max-width: 768px) {
    body {
      background-color: lightgray;
    }
    .container {
      flex-direction: column;
    }
  }
  ```

#### Prinsip Penting Responsif

* Gunakan **layout fleksibel** (misalnya `flexbox` atau `grid`).
* Gunakan **gambar yang skalanya menyesuaikan** (`max-width: 100%; height: auto;`).
* Gunakan **media query** untuk menyesuaikan tampilan di layar kecil.

### Contoh Kode yang Responsif
```html
/* HTML */
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Website Responsif</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header class="header">
    <h1>Halo, ini Web Responsif</h1>
    <nav class="nav">
      <a href="#">Home</a>
      <a href="#">Tentang</a>
      <a href="#">Kontak</a>
    </nav>
  </header>

  <main class="container">
    <div class="box">Konten 1</div>
    <div class="box">Konten 2</div>
    <div class="box">Konten 3</div>
  </main>

  <footer class="footer">
    <p>&copy; 2025 My Project</p>
  </footer>
</body>
</html>
```

```css
/* CSS */
body, h1, p {
  margin: 0;
  padding: 0;
}

html, body {
  height: 100%;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  display: flex;
  flex-direction: column;
}

.header {
  background-color: #333333;
  color: white;
  text-align: center;
  padding: 20px;
  position: sticky; /* selalu nempel di atas saat scroll */
  top: 0;
}

.nav a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
}

.container {
  display: flex;
  justify-content: space-around;
  margin: 20px;
  flex: 1; /* isi ruang kosong di antara header dan footer */
}

.box {
  background-color: salmon;
  color: white;
  width: 30%;
  height: 150px;
  text-align: center;
  line-height: 150px;
  border-radius: 10px;
}

.footer {
  background-color: #333333;
  color: white;
  text-align: center;
  padding: 15px;
}

/* responsive */
@media (max-width: 768px) {
  .container {
    flex-direction: column; /* kotak jadi ke bawah */
    align-items: center;
  }
  .box {
    width: 80%; /* biar memenuhi layar kecil */
    margin-bottom: 10px;
  }
}

```

## Hands-On Project: Portfolio Sederhana

### File `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>Portfolio</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Halo, Saya Fulan</h1>
  <p>Mahasiswa Ilmu Komputer yang sedang mengikuti SBF PTI 2025.</p>

  <h2>Data Diri</h2>
  <table>
    <tr><th>Nama</th><td>Fulan</td></tr>
    <tr><th>Umur</th><td>20</td></tr>
    <tr><th>Jurusan</th><td>Ilmu Komputer / Sistem Informasi</td></tr>
  </table>

  <h2>Hobi</h2>
  <ul>
    <li>Membaca</li>
    <li>Menulis</li>
    <li>Desain</li>
  </ul>

  <h2>Mata Kuliah yang Diambil</h2>
  <div class="container">
    <div class="kotak">Box 1</div>
    <div class="kotak">Box 2</div>
    <div class="kotak">Box 3</div>
  </div>
</body>
</html>
```

Coba tambahkan navbar dan footer pada web portofolio ini, lalu desainlah dengan kreasi kalian sendiri!

---
Sekian materi minggu ini! Selamat belajar, semoga bermanfaat, dan jangan lupa untuk mengerjakan checkpoint kalian, ya! ✨
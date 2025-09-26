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
  <title>Judul Halaman</title>
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
<img src="https://via.placeholder.com/150" alt="Contoh Gambar">
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
    ...
    <div id="main">
        <div class="parent_content">
            <p class="date">Tanggal 9 September 2024</p>
            <h2><a href="">Subjek: Keseruan PBP</a></h2>
            <p id="content_1">Materi PBP sangat seru!</p>
        </div>
        <div class="parent_content">
            <p class="date">Tanggal 10 September 2024</p>
            <h2><a href="">Subjek: Review PBP</a></h2>
            <p id="content_2">Biar lebih gacor aku review materi PBP</p>
        </div>
        <div class="parent_content">
            <p class="date">Tanggal 11 September 2024</p>
            <h2><a href="">Subjek: Ikut Tutorial PBP</a></h2>
            <p id="content_3">Tutorial PBP sangat seru!</p>
        </div>
    </div>
    ...
    ```
    
    Kemudian, kita dapat menggunakan _Class_ tersebut sebagai _selector_ dalam **_file_ CSS**. _Class selector_ menggunakan format *.[class_name]* (diawali .)
    
    ```html
    .content_section {
      background-color: #112a33;
      margin-bottom: 30px;
      color: #0F0F0F;
      font-family: cursive;
      padding: 20px 20px 20px 40px;
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

---

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

---

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
  <p>Mahasiswa Ilmu Komputer yang sedang mempelajari web development.</p>

  <h2>Foto</h2>
  <img src="https://via.placeholder.com/200" alt="Foto Saya">

  <h2>Data Diri</h2>
  <table>
    <tr><th>Nama</th><td>Fulan</td></tr>
    <tr><th>Umur</th><td>20</td></tr>
    <tr><th>Kampus</th><td>UI</td></tr>
  </table>

  <h2>Hobi</h2>
  <ul>
    <li>Membaca</li>
    <li>Menulis</li>
    <li>Desain</li>
  </ul>

  <div class="container">
    <div class="kotak">Box 1</div>
    <div class="kotak">Box 2</div>
    <div class="kotak">Box 3</div>
  </div>
</body>
</html>
```

### File `style.css`

```css
body {
  font-family: Arial, sans-serif;
  margin: 20px;
  background-color: #f9f9f9;
}

h1 {
  text-align: center;
  font-size: 36px;
  font-weight: bold;
}

h2 {
  margin-top: 30px;
  color: navy;
}

p {
  text-align: justify;
  font-size: 18px;
  font-style: italic;
}

table {
  width: 50%;
  margin: auto;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  text-align: center;
  border: 1px solid black;
}

.container {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.kotak {
  width: 100px;
  height: 100px;
  background-color: salmon;
  color: white;
  text-align: center;
  line-height: 100px;
  border-radius: 10px;
}

@media (max-width: 600px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
  .kotak {
    margin-bottom: 10px;
  }
}
```
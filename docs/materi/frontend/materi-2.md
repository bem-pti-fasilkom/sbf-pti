---
title: Materi 2
sidebar_position: 2
---

# Javascript Functions & ES6 Basics for React

---

## JavaScript Variables: var, let, dan const
Di JavaScript, variabel dideklarasikan menggunakan `var`, `let`, atau `const`. Meski fungsinya sama, ketiganya memiliki perbedaan mendasar pada **lingkup (scope)**, **kemampuan untuk diubah**, dan **hoisting**. 

### var 

`var` adalah deklarasi variabel asli di JavaScript dan memiliki **lingkup fungsi (function scope)**. Artinya, variabel `var` yang dideklarasikan di dalam `if` atau `for` tetap bisa diakses dari luar blok tersebut.

-   Nilainya bisa diubah dan bisa dideklarasi ulang.
-   Mengalami *hoisting*, di mana deklarasi "diangkat" ke atas dengan nilai awal `undefined`.

```javascript
var nama = "Andi";
var nama = "Budi"; // Bisa dideklarasi ulang
console.log(nama); // Output: Budi

if (true) {
  var skor = 100;
}
console.log(skor); // Output: 100 (bisa diakses dari luar blok)
````

-----

### let 

`let` diperkenalkan di ES6 untuk memperbaiki kelemahan `var`. `let` memiliki **lingkup blok (block scope)**, sehingga variabel hanya hidup di dalam blok `{...}` tempat ia dibuat.

  - Nilainya bisa diubah, tetapi tidak bisa dideklarasi ulang.
  - Mengalami *hoisting* tetapi masuk ke **Temporal Dead Zone (TDZ)**, yang berarti akan error jika diakses sebelum dideklarasikan.

<!-- end list -->

```javascript
let favorit = "Kopi";
// let favorit = "Teh"; // Error: 'favorit' sudah dideklarasikan
favorit = "Teh Manis"; // OK: nilainya bisa diubah
console.log(favorit); // Output: Teh Manis

if (true) {
  let minuman = "Susu";
}
// console.log(minuman); // Error: minuman tidak terdefinisi di sini
````

-----

### const 

`const` (konstanta) juga memiliki **lingkup blok**. Ini digunakan untuk nilai yang tidak akan pernah berubah. Aturan ini membuat kode lebih aman dan mudah ditebak.

  - Nilainya **tidak bisa diubah** dan tidak bisa dideklarasi ulang.
  - Wajib diinisialisasi saat deklarasi.
  - Sama seperti `let`, `const` juga memiliki perilaku **TDZ**.

<!-- end list -->

```javascript
const PI = 3.14;
// PI = 3.14159; // Error: tidak bisa mengubah nilai const

const PENGGUNA = { nama: "Alice" };
// PENGGUNA = { nama: "Bob" }; // Error: tidak bisa re-assign referensi

// Namun, isi dari objek/array BISA diubah:
PENGGUNA.nama = "Bob";
console.log(PENGGUNA.nama); // Output: Bob (ini diizinkan)
````
-----
| Keyword | Lingkup | Bisa Diubah? | Bisa Dideklarasi Ulang? | Kapan Dipakai? |
| :------ | :------------ | :----------: | :--------------------: | :---------------------------------------------------------------------------------------------------- |
| `var`   | Fungsi   |   Ya                 |   Ya                 | Sebaiknya **dihindari**. |
| `let`   | Blok   |   Ya                 |   Tidak                | Untuk nilai yang **perlu diubah** (misal: *counter*). |
| `const` | Blok   |   Tidak              |   Tidak                | **Gunakan secara default** untuk nilai yang tidak berubah. |
-----



### Latihan Soal 1: Hoisting dan TDZ

Apa output dari kode ini?

```javascript
console.log(nama);
var nama = "Budi";

console.log(kota);
let kota = "Jakarta";
```

#### Jawaban Soal 1

**Output:**

```
undefined
// Uncaught ReferenceError: Cannot access 'kota' before initialization
```

**Penjelasan:**

  - `var nama` di-*hoist* dengan nilai awal `undefined`, sehingga `console.log` pertama tidak error.
  - `let kota` juga di-*hoist* tetapi masuk ke TDZ. Mengaksesnya sebelum baris deklarasi akan langsung menyebabkan `ReferenceError`.



---

## Data Types

JavaScript memiliki beberapa tipe data penting yang sering digunakan dalam pemrograman sehari-hari. Berikut tipe data dasar yang perlu kamu kuasai:

### Number
Digunakan untuk merepresentasikan angka, baik bilangan bulat maupun pecahan.

```javascript
let age = 20;       // integer
let price = 19.99;  // float
````

---

### String

Digunakan untuk menyimpan teks. Bisa ditulis dengan `'`, `"`, atau menggunakan template literal dengan backticks ``.

```javascript
let name = "Alice";
let message = `Hello, ${name}`;
```

---

### Boolean

Digunakan untuk menyimpan nilai logika: `true` atau `false`.

```javascript
let isActive = true;
let isComplete = false;
```

---

### Array

Digunakan untuk menyimpan sekumpulan data dalam satu variabel. Elemen array diakses dengan **indeks** (dimulai dari 0).

```javascript
let numbers = [1, 2, 3, 4];
let fruits = ["apple", "banana", "cherry"];

console.log(fruits[0]); // "apple"
```

---

### Object

Digunakan untuk menyimpan data dalam bentuk pasangan **key–value**.

```javascript
let person = {
  name: "Alice",
  age: 25,
  isStudent: true
};

console.log(person.name);  // "Alice"
console.log(person["age"]); // 25
```
----

### `==` vs `===`

- `==` => membandingkan nilai dengan konversi tipe (loose).  
- `===` => membandingkan nilai *dan* tipe (strict).  

Contoh:
```javascript
5 == "5"   // true
5 === "5"  // false
0 == false // true
0 === false // false
```

### Latihan Soal 2: Data Types

Apa output dari kode berikut?

```javascript
let a = 10;
let b = "5";
let c = "";
let d = [1, 2];

console.log(a + b);
console.log(a - b);
console.log(Boolean(c));
console.log(d + 1);
```

#### Jawaban Soal 2

**Output:**

```
105
5
false
1,21
```

**Penjelasan:**

* `a + b`: number + string => string concatenation `"10" + "5" = "105"`.
* `a - b`: string `"5"` dikonversi ke number => `10 - 5 = 5`.
* `Boolean(c)`: string kosong `""` dianggap false => `false`.
* `d + 1`: array `[1,2]` dikonversi ke string `"1,2"` lalu ditambah `"1"` => `"1,21"`.

---


## Objects

Di JavaScript, **object** adalah struktur data yang menyimpan pasangan *key-value*. Object sangat fleksibel untuk merepresentasikan data nyata.

### Object Sederhana
Contoh object dengan 2 atribut:
```javascript
const mahasiswa = {
  nama: "Budi",
  umur: 21
};

console.log(mahasiswa.nama); // Budi
console.log(mahasiswa.umur); // 21
````

### Array of Objects

Kita bisa membuat array yang berisi banyak object.

```javascript
const daftarMahasiswa = [
  { nama: "Budi", umur: 21 },
  { nama: "Citra", umur: 22 },
  { nama: "Andi", umur: 20 }
];

console.log(daftarMahasiswa[1].nama); // Citra
```

### `map()` dan `filter()`

* **`map()`** menghasilkan array baru dengan memodifikasi tiap elemen.
* **`filter()`** menghasilkan array baru berisi elemen yang lolos kondisi tertentu.

```javascript
// Ambil semua nama mahasiswa
const namaMahasiswa = daftarMahasiswa.map(m => m.nama);
console.log(namaMahasiswa); // ["Budi", "Citra", "Andi"]

// Ambil mahasiswa yang umurnya >= 21
const dewasa = daftarMahasiswa.filter(m => m.umur >= 21);
console.log(dewasa); // [{ nama: "Budi", umur: 21 }, { nama: "Citra", umur: 22 }]
```

---

### Add/Edit Attributes dengan `map()`

Kode:

```javascript
const peopleLoveJakarta = people.map(person => {
  return {
    ...person,   // ambil semua atribut lama dari person
    city: "Jakarta"  // tambahkan/ubah atribut city jadi "Jakarta"
  };
});
```

Penjelasan:

* `people.map(...)` => kita buat array baru dari `people`.
* `...person` => spread operator, menyalin semua atribut dari object `person`.
* `city: "Jakarta"` => menambahkan atribut baru bernama `city`, atau kalau sudah ada, nilainya akan diganti jadi `"Jakarta"`.
* Hasilnya: setiap orang dalam array `people` sekarang punya atribut `city` = `"Jakarta"`.

**Contoh input/output:**

```javascript
const people = [
  { name: "Budi", age: 25 },
  { name: "Citra", age: 32 }
];

console.log(peopleLoveJakarta);
// [
//   { name: "Budi", age: 25, city: "Jakarta" },
//   { name: "Citra", age: 32, city: "Jakarta" }
// ]
```

---

### Conditional Handling dengan `map()`

Kode:

```javascript
const oldPeopleLoveDepok = people.map(person => {
  if (person.age > 30) {
    return {
      ...person,
      city: "Depok"
    };
  }
  return person; // kalau syarat tidak terpenuhi, kembalikan object asli
});
```

Penjelasan:

* Cek kondisi `if (person.age > 30)`.
* Kalau benar => kembalikan object baru dengan tambahan atribut `city: "Depok"`.
* Kalau salah => kembalikan object aslinya tanpa perubahan.

**Contoh input/output:**

```javascript
const people = [
  { name: "Budi", age: 25 },
  { name: "Citra", age: 32 }
];

console.log(oldPeopleLoveDepok);
// [
//   { name: "Budi", age: 25 },
//   { name: "Citra", age: 32, city: "Depok" }
// ]
```


---



### Latihan Soal 3: Objects

Apa output dari kode berikut?

```javascript
const people = [
  { name: "Budi", age: 25 },
  { name: "Citra", age: 32 },
  { name: "Andi", age: 28 }
];

const updated = people.map(person => {
  if (person.age > 30) {
    return { ...person, city: "Depok" };
  }
  return person;
});

console.log(updated[1].city);
console.log(updated[0].city);
console.log(people[1].city);
```

---

#### Jawaban Soal 3

**Output:**

```
Depok
undefined
undefined
```

**Penjelasan:**

* `map()` membuat array baru.
* Untuk `Citra` (age 32), kondisi `age > 30` terpenuhi → dikembalikan object baru dengan atribut tambahan `city: "Depok"`.
* Untuk `Budi` (25) dan `Andi` (28), kondisi tidak terpenuhi → dikembalikan object lama apa adanya (tanpa atribut `city`).
* `updated[1].city` => `"Depok"`.
* `updated[0].city` => `undefined` (tidak punya atribut `city`).
* `people[1].city` => tetap `undefined` karena object asli `people` tidak berubah, hanya `updated` yang dapat modifikasi.

---


## Conditional & Looping

###  Conditional (If–Else)


```javascript
let age = 18;

if (age >= 18) {
  console.log("Dewasa");
} else {
  console.log("Anak-anak");
}
```

> Bisa juga pakai `else if` untuk kondisi tambahan.

---

###  Looping


**For loop**:

```javascript
for (let i = 1; i <= 3; i++) {
  console.log("Hitungan ke-" + i);
}
```

**While loop**:

```javascript
let n = 1;
while (n <= 3) {
  console.log("Loop ke-" + n);
  n++;
}
```

**For…of (khusus array):**

```javascript
let fruits = ["Apple", "Mango", "Banana"];
for (let f of fruits) {
  console.log(f);
}
```

---

### Latihan Soal 4: Loop

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(`var i: ${i}`), 10);
}

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(`let j: ${j}`), 10);
}
```

#### Jawaban Soal 4

**Output:**

```
var i: 3
var i: 3
var i: 3
let j: 0
let j: 1
let j: 2
```

**Penjelasan:**

  - **`var`**: Karena `var` memiliki lingkup fungsi, `setTimeout` mengakses nilai `i` yang sama setelah loop selesai (yaitu **3**).
  - **`let`**: Karena `let` memiliki lingkup blok, setiap iterasi loop menciptakan variabel `j` yang baru, sehingga nilainya tetap terjaga (0, 1, dan 2).

----

## JavaScript: Bisa Apa Aja Sih?

JavaScript memungkinkan kita untuk membuat halaman web menjadi dinamis dan interaktif. Salah satu kekuatan utamanya adalah kemampuan untuk memanipulasi **Document Object Model (DOM)**, yaitu representasi struktur halaman HTML Anda.

Dengan JavaScript, Anda bisa melakukan banyak hal pada elemen-elemen di halaman Anda. Mari kita lihat beberapa contoh dasarnya!

---

### Mengubah Konten Elemen

Anda dapat dengan mudah mengubah teks atau konten di dalam sebuah elemen HTML. Caranya adalah dengan menyeleksi elemen tersebut (misalnya, menggunakan `document.getElementById`) lalu mengubah properti `.innerText` atau `.innerHTML`.

```javascript title="script.js"
// Menyeleksi elemen dengan id 'title' dan 'description'
const titleElement = document.getElementById('title');
const descriptionElement = document.getElementById('description');

// Mengubah konten teks di dalamnya
titleElement.innerText = 'SBF PTI keren';
descriptionElement.innerText = 'Kelas SBF PTI keren banget!';
````

-----

### Menambah Elemen Baru

Tidak hanya mengubah, Anda juga bisa membuat elemen HTML baru dari nol dan menambahkannya ke dalam halaman secara dinamis.

Prosesnya terdiri dari tiga langkah utama:

1.  **Buat elemennya** dengan `document.createElement()`.
2.  **Isi kontennya**, misalnya dengan `.innerText`.
3.  **Tambahkan ke halaman** menggunakan `appendChild()`.

<!-- end list -->

```javascript title="script.js"
// 1. Membuat elemen paragraf (<p>) baru
const newParagraph = document.createElement('p');

// 2. Mengisi konten teks untuk elemen baru tersebut
newParagraph.innerText = 'This is a new paragraph added dynamically!';

// 3. Menambahkan elemen baru sebagai anak terakhir dari <body>
document.body.appendChild(newParagraph);
```

-----

### Menghapus Elemen

Jika Anda ingin menghilangkan sebuah elemen dari halaman, caranya sangat mudah. Cukup seleksi elemen yang ingin dihapus, lalu panggil method `.remove()`.

```javascript title="script.js"
// Menyeleksi elemen dengan id 'description'
const description = document.getElementById('description');

// Menghapus elemen tersebut dari DOM
description.remove();
```

-----

### Bekerja dengan Custom Data Attributes

Terkadang kita perlu menyimpan informasi atau data tambahan pada sebuah elemen HTML yang tidak terlihat oleh pengguna. Untuk itu, kita bisa menggunakan *custom data attributes*, yang selalu diawali dengan `data-`.

#### Mengakses Nilai Data Attribute

Untuk membaca nilai dari *data attribute*, gunakan method `.getAttribute()`.

```html title="index.html"
<div id="item" data-user-id="123" data-status="active" data-role="admin">
  Ini adalah item produk.
</div>
```

```javascript title="script.js"
// Menyeleksi elemen
const item = document.getElementById('item');

// Mengambil nilai dari masing-masing data attribute
const userID = item.getAttribute('data-user-id');   // "123"
const status = item.getAttribute('data-status'); // "active"
const role = item.getAttribute('data-role');     // "admin"

console.log(`User ID: ${userID}, Status: ${status}, Role: ${role}`);
```

#### Mengubah Nilai Data Attribute

Untuk mengubah atau menambahkan *data attribute* baru, gunakan method `.setAttribute()`.

```javascript title="script.js"
const item = document.getElementById('item');

// Mengubah nilai 'data-status' dari 'active' menjadi 'inactive'
item.setAttribute('data-status', 'inactive');

console.log(`Updated Status: ${item.getAttribute('data-status')}`); // "inactive"
```

-----

### Event Listener: Membuat Halaman Jadi Interaktif

*Event Listener* adalah "pendengar" yang kita pasang pada sebuah elemen untuk menunggu terjadinya sebuah aksi (event), seperti klik, *hover*, atau input keyboard. Ketika aksi tersebut terjadi, kita bisa menjalankan fungsi JavaScript tertentu.

Ada **banyak sekali** jenis *event* yang bisa kita gunakan. Contoh paling umum adalah `click`.

```html title="index.html"
<button id="myButton">Klik Saya!</button>
```

```javascript title="script.js"
const button = document.getElementById('myButton');

button.addEventListener('click', function() {
  alert('Tombol berhasil diklik!');
});
```

Untuk daftar lengkap semua *event* yang ada di web, Anda bisa melihat referensi tepercaya seperti [**MDN Web Docs**](https://developer.mozilla.org/en-US/docs/Web/Events) atau [**W3Schools**](https://www.w3schools.com/jsref/dom_obj_event.asp).

<!-- end list -->

This is page 2.
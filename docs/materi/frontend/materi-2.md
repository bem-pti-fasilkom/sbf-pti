---
title: Materi 2
sidebar_position: 2
---

# Javascript Functions & ES6 Basics for React

# JavaScript Variables: var, let, dan const

Di JavaScript, variabel dideklarasikan menggunakan `var`, `let`, atau `const`. Meski fungsinya sama, ketiganya memiliki perbedaan mendasar pada **lingkup (scope)**, **kemampuan untuk diubah**, dan **hoisting**. Ini adalah kunci untuk clean code dan bebas dari *bug*.


---

## 1. var (Cara Lama)

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

## 2\. let (Cara Modern & Fleksibel)

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
```

-----

## 3\. const (Cara Modern & Aman)

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
```
-----
| Keyword | Lingkup | Bisa Diubah? | Bisa Dideklarasi Ulang? | Kapan Dipakai? |
| :------ | :------------ | :----------: | :--------------------: | :---------------------------------------------------------------------------------------------------- |
| `var`   | Fungsi   |   Ya                 |   Ya                 | Sebaiknya **dihindari** pada kode modern. |
| `let`   | Blok   |   Ya                 |   Tidak                | Untuk nilai yang **perlu diubah** (misal: *counter*). |
| `const` | Blok   |   Tidak              |   Tidak                | **Gunakan secara default** untuk nilai yang tidak berubah. |
-----

## Praktik Terbaik & Aturan Sederhana

1.  **Gunakan `const` secara default.** Ini mencegah perubahan nilai yang tidak disengaja.
2.  **Gunakan `let` hanya jika Anda tahu nilai variabel tersebut perlu diubah.**
3.  **Hindari `var`** untuk mencegah masalah terkait lingkup dan *hoisting*.

-----

## Latihan Soal

Tes ilmu Anda di sini.

### Soal 1: Hoisting dan TDZ

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


### Soal 2: Perbedaan Lingkup di dalam Loop

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(`var i: ${i}`), 10);
}

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(`let j: ${j}`), 10);
}
```

#### Jawaban Soal 2

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


<!-- end list -->

```
```
This is page 2.
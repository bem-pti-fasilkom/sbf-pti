---
title: Materi 3
sidebar_position: 3
---

# Pengantar React Js (Instalasi, Komponen Fungsional, Props, Lifecycle)

# Pengantar React.js - Panduan Lengkap untuk Pemula

## Apa itu React?

React adalah library JavaScript yang powerful yang dikembangkan oleh Facebook untuk membangun antarmuka pengguna, khususnya aplikasi web. Bayangkan React sebagai alat yang membantu Anda membuat website interaktif dengan memecahnya menjadi bagian-bagian yang dapat digunakan kembali yang disebut **komponen**.

### Mengapa React?

- **Berbasis Komponen**: Bangun komponen yang terkapsulasi dan mengelola state mereka sendiri
- **Dapat Digunakan Kembali**: Gunakan di mana saja dalam aplikasi Anda
- **Cepat**: Virtual DOM membuat pembaruan menjadi efisien
- **Populer**: Komunitas besar dan permintaan pasar kerja yang tinggi

---

## 1. Instalasi & Setup

### Prasyarat

Sebelum memulai dengan React, pastikan Anda memiliki:

- **Node.js** (versi 14 atau lebih tinggi) - Unduh dari [nodejs.org](https://nodejs.org)
- Pengetahuan dasar **HTML**, **CSS**, dan **JavaScript**

### Menggunakan Vite (Bagus untuk memulai)

```bash
# Buat proyek baru dengan Vite
npm create vite@latest week3-react-vite

# Masuk ke folder dan instal dependensi
cd week3-react-vite
npm install

# Jalankan server pengembangan
npm run dev
```

### Gambaran Struktur Proyek

```
aplikasi-pertama-saya/
├── public/
│   └── index.html
├── src/
│   ├── App.jsx
│   ├── App.css
│   ├── main.jsx
│   └── index.css
├── package.json
└── README.md
```

---

## 2. Komponen Fungsional

### Apa itu Komponen?

Komponen seperti fungsi JavaScript yang mengembalikan HTML (JSX). Mereka adalah blok bangunan dari aplikasi React.

### Komponen Fungsional Dasar

```jsx
// Komponen sederhana
function Selamat() {
  return <h1>Halo, Dunia!</h1>;
}

// Versi arrow function (DIREKOMENDASIKAN menurut Konvensi)
const Selamat = () => {
  return <h1>Halo, Dunia!</h1>;
};

// Export untuk digunakan di file lain
export default Selamat;
```

### JSX - JavaScript XML

JSX memungkinkan Anda menulis kode mirip HTML dalam JavaScript:

```jsx
function KomponenSaya() {
  const nama = "React";
  const sedangBelajar = true;

  return (
    <div>
      <h1>Belajar {nama}</h1>
      <p>{sedangBelajar ? "Kamu hebat!" : "Terus mencoba!"}</p>
      <button onClick={() => alert("Halo!")}>Klik saya</button>
    </div>
  );
}
```

### Aturan JSX

1. **Elemen Parent Tunggal**: Bungkus beberapa elemen dalam `<div>` atau React Fragment `<></>`
2. **Ekspresi JavaScript**: Gunakan `{}` untuk menyematkan JavaScript
3. **Atribut camelCase**: `className` bukan `class`, `onClick` bukan `onclick`
4. **Tag Self-Closing**: `<img />`, `<br />`, `<hr />`

### Contoh: Komponen Kartu Pengguna

```jsx
function KartuPengguna() {
  const pengguna = {
    nama: "Budi Santoso",
    umur: 25,
    lokasi: "Jakarta",
  };

  return (
    <div className="kartu-pengguna">
      <h2>{pengguna.nama}</h2>
      <p>Umur: {pengguna.umur}</p>
      <p>Lokasi: {pengguna.lokasi}</p>
    </div>
  );
}
```

---

## 3. Props (Properti)

### Apa itu Props?

Props adalah cara Anda mengoper data dari komponen parent ke komponen child. Anggap saja seperti parameter fungsi.

### Penggunaan Props Dasar

```jsx
// Komponen child yang menerima props
// Komponen child: menerima props sebagai object
const Salam = (props) => {
  return <h1>Halo, {props.nama}!</h1>;
};

// Komponen parent: mengoper props ke child
const App = () => {
  return (
    <div>
      <Salam nama="Alice" />
      <Salam nama="Bob" />
      <Salam nama="Charlie" />
    </div>
  );
};
```

### Destructuring Props (Sintaks Lebih Bersih)

```jsx
// Langsung "unpack" props di parameter → tidak perlu tulis props.nama
const ProfilPengguna = ({ nama, umur, email }) => {
  return (
    <div className="profil">
      <h2>{nama}</h2>
      <p>Umur: {umur}</p>
      <p>Email: {email}</p>
    </div>
  );
};

// Penggunaan di parent
const App = () => {
  return (
    <ProfilPengguna 
      nama="Sarah Wilson" 
      umur={28} 
      email="sarah@example.com" 
    />
  );
};
```

### Berbagai Tipe Props

```jsx
function KartuProduk({
  judul, // string
  harga, // number
  sedangDiskon, // boolean
  tag, // array
  onBeli, // function
}) {
  return (
    <div className="produk">
      <h3>{judul}</h3>
      <p>Rp{harga}</p>
      {sedangDiskon && <span className="badge-diskon">DISKON!</span>}
      <div>
        {tag.map((t, index) => (
          <span key={index} className="tag">
            {t}
          </span>
        ))}
      </div>
      <button onClick={onBeli}>Beli Sekarang</button>
    </div>
  );
}

// Penggunaan
function App() {
  const handlePembelian = () => {
    alert("Ditambahkan ke keranjang!");
  };

  return (
    <KartuProduk
      judul="Headphone Wireless"
      harga={999000}
      sedangDiskon={true}
      tag={["elektronik", "audio", "nirkabel"]}
      onBeli={handlePembelian}
    />
  );
}
```

### Props Default

```jsx
function Tombol({ teks = "Klik saya", warna = "blue", onClick }) {
  return (
    <button style={{ backgroundColor: warna }} onClick={onClick}>
      {teks}
    </button>
  );
}
```

---

## 4. Lifecycle Komponen 

### Pola useEffect Umum

#### 1. Component Did Mount (berjalan sekali)

```jsx
useEffect(() => {
  // Ambil data ketika komponen di-mount
  fetchDataPengguna();
}, []); // Array dependency kosong = berjalan sekali
```

#### 2. Component Did Update (berjalan pada perubahan tertentu)

```jsx
useEffect(() => {
  // Simpan ke localStorage ketika hitung berubah
  localStorage.setItem("hitung", hitung);
}, [hitung]); // Berjalan ketika hitung berubah
```

#### 3. Component Will Unmount (cleanup)

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Timer tick");
  }, 1000);

  return () => {
    clearInterval(timer); // Cleanup saat unmount
  };
}, []);
```

---

## 5. Best Practices untuk Pemula

### 1. Organisasi Komponen

- Jaga komponen tetap kecil dan fokus
- Gunakan nama yang deskriptif

### 2. Best Practices Props

- Destructure props untuk kode yang lebih bersih
- Berikan nilai default bila memungkinkan

### 3. Manajemen State

- Jangan duplikasi data di state
- Update state secara immutable

### 4. Pola Umum

```jsx
// Rendering kondisional
{sudahLogin ? <Dashboard /> : <Login />}

// Rendering list
{items.map(item => <Item key={item.id} data={item} />)}

// Event handling
<button onClick={(e) => handleClick(e, item.id)}>
```

---

## Sumber untuk Pembelajaran Lebih Lanjut

- **Dokumentasi Resmi React**: [reactjs.org](https://reactjs.org)
- **React DevTools**: Extension browser untuk debugging
- **freeCodeCamp React Course**: Tutorial komprehensif gratis

---
Sekian materi minggu ini! Selamat belajar, semoga bermanfaat, dan jangan lupa untuk mengerjakan checkpoint kalian, ya! ✨

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
npm create vite@latest aplikasi-react-saya -- --template react

# Masuk ke folder dan instal dependensi
cd aplikasi-react-saya
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
│   ├── App.js
│   ├── App.css
│   ├── index.js
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
function Salam(props) {
  return <h1>Halo, {props.nama}!</h1>;
}

// Komponen parent yang mengoper props
function App() {
  return (
    <div>
      <Salam nama="Alice" />
      <Salam nama="Bob" />
      <Salam nama="Charlie" />
    </div>
  );
}
```

### Destructuring Props (Sintaks Lebih Bersih)

```jsx
// Daripada props.nama, props.umur
function ProfilPengguna({ nama, umur, email }) {
  return (
    <div className="profil">
      <h2>{nama}</h2>
      <p>Umur: {umur}</p>
      <p>Email: {email}</p>
    </div>
  );
}

// Penggunaan
function App() {
  return <ProfilPengguna nama="Sarah Wilson" umur={28} email="sarah@example.com" />;
}
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

## 4. Lifecycle Komponen & Hooks

### Pengantar Hooks

Hooks adalah fungsi yang memungkinkan Anda "mengaitkan" ke fitur React. Mereka selalu dimulai dengan `use`.

### Hook useState - Mengelola State

```jsx
import { useState } from "react";

function Penghitung() {
  // Deklarasikan variabel state dengan nilai awal
  const [hitung, setHitung] = useState(0);

  return (
    <div>
      <p>Hitung: {hitung}</p>
      <button onClick={() => setHitung(hitung + 1)}>Tambah</button>
      <button onClick={() => setHitung(hitung - 1)}>Kurang</button>
      <button onClick={() => setHitung(0)}>Reset</button>
    </div>
  );
}
```

### Beberapa Variabel State

```jsx
function FormPengguna() {
  const [nama, setNama] = useState("");
  const [email, setEmail] = useState("");
  const [sedangSubmit, setSedangSubmit] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();
    setSedangSubmit(true);

    // Simulasi panggilan API
    setTimeout(() => {
      alert(`Halo ${nama}! Email: ${email}`);
      setSedangSubmit(false);
      setNama("");
      setEmail("");
    }, 1000);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Nama Anda"
        value={nama}
        onChange={(e) => setNama(e.target.value)}
      />
      <input
        type="email"
        placeholder="Email Anda"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button type="submit" disabled={sedangSubmit}>
        {sedangSubmit ? "Mengirim..." : "Kirim"}
      </button>
    </form>
  );
}
```

### Hook useEffect - Side Effects

```jsx
import { useState, useEffect } from "react";

function Timer() {
  const [detik, setDetik] = useState(0);
  const [berjalan, setBerjalan] = useState(false);

  // Effect berjalan setelah setiap render
  useEffect(() => {
    let interval = null;

    if (berjalan) {
      interval = setInterval(() => {
        setDetik((detik) => detik + 1);
      }, 1000);
    }

    // Fungsi cleanup (berjalan sebelum effect berikutnya atau unmount)
    return () => {
      if (interval) clearInterval(interval);
    };
  }, [berjalan]); // Array dependency - effect berjalan ketika berjalan berubah

  return (
    <div>
      <h2>Timer: {detik} detik</h2>
      <button onClick={() => setBerjalan(!berjalan)}>
        {berjalan ? "Pause" : "Mulai"}
      </button>
      <button onClick={() => setDetik(0)}>Reset</button>
    </div>
  );
}
```

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

## 5. Contoh Praktis

### Aplikasi Daftar Todo

```jsx
import { useState } from "react";

function AplikasiTodo() {
  const [todos, setTodos] = useState([]);
  const [nilaiInput, setNilaiInput] = useState("");

  const tambahTodo = () => {
    if (nilaiInput.trim()) {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          teks: nilaiInput,
          selesai: false,
        },
      ]);
      setNilaiInput("");
    }
  };

  const toggleTodo = (id) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, selesai: !todo.selesai } : todo
      )
    );
  };

  const hapusTodo = (id) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  return (
    <div className="aplikasi-todo">
      <h1>Daftar Todo</h1>

      <div className="tambah-todo">
        <input
          type="text"
          value={nilaiInput}
          onChange={(e) => setNilaiInput(e.target.value)}
          placeholder="Tambah todo baru..."
          onKeyPress={(e) => e.key === "Enter" && tambahTodo()}
        />
        <button onClick={tambahTodo}>Tambah</button>
      </div>

      <div className="daftar-todo">
        {todos.map((todo) => (
          <div key={todo.id} className="item-todo">
            <input
              type="checkbox"
              checked={todo.selesai}
              onChange={() => toggleTodo(todo.id)}
            />
            <span
              style={{
                textDecoration: todo.selesai ? "line-through" : "none",
              }}
            >
              {todo.teks}
            </span>
            <button onClick={() => hapusTodo(todo.id)}>Hapus</button>
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

## 6. Best Practices untuk Pemula

### 1. Organisasi Komponen

- Jaga komponen tetap kecil dan fokus
- Satu komponen per file
- Gunakan nama yang deskriptif

### 2. Best Practices Props

- Destructure props untuk kode yang lebih bersih
- Berikan nilai default bila diperlukan
- Validasi props dengan PropTypes (opsional)

### 3. Manajemen State

- Jaga state sesederhana mungkin
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

## 7. Langkah Selanjutnya

Setelah menguasai konsep-konsep ini, jelajahi:

1. **React Router** - Navigasi antar halaman
2. **Context API** - Manajemen state global
3. **Custom Hooks** - Logika yang dapat digunakan kembali
4. **Testing** - Jest dan React Testing Library
5. **Styling** - CSS Modules, Styled Components
6. **State Management** - Redux, Zustand

---

## Referensi Cepat

### Hooks Penting

- `useState()` - Mengelola state komponen
- `useEffect()` - Menangani side effects
- `useContext()` - Mengakses React context
- `useReducer()` - Logika state yang kompleks

### Pola Umum

```jsx
// Update state
const [state, setState] = useState(nilaiAwal);

// Effect dengan cleanup
useEffect(() => {
  // setup
  return () => {
    // cleanup
  };
}, [dependencies]);

// Event handler
const handleClick = (e) => {
  e.preventDefault();
  // tangani event
};
```

---

## Sumber untuk Pembelajaran Lebih Lanjut

- **Dokumentasi Resmi React**: [reactjs.org](https://reactjs.org)
- **React DevTools**: Extension browser untuk debugging
- **freeCodeCamp React Course**: Tutorial komprehensif gratis

---
Sekian materi minggu ini! Selamat belajar, semoga bermanfaat, dan jangan lupa untuk mengerjakan checkpoint kalian, ya! ✨

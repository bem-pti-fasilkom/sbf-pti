---
title: Checkpoint 4
sidebar_position: 4
---

# React JS Advanced  Routing, Hooks, Fetching

Ringkasan singkat
-----------------

Pada checkpoint ini kamu akan melanjutkan proyek FinTrack yang dibuat pada Checkpoint 3 (kamu seharusnya sudah membuat landing page / halaman depan). Tugasnya adalah menambahkan fitur inti aplikasi pengelolaan keuangan pribadi:

- Routing dan navigasi ke halaman utama aplikasi (Dashboard), halaman transaksi, dan halaman untuk menambah transaksi.
- Implementasi state/flow aplikasi menggunakan Hooks (mis. `useReducer` untuk form/transactions, `useContext` untuk state global opsional).
- Menyimpan dan mengambil data transaksi (bisa menggunakan `localStorage`, mock API lokal, atau `json-server`).
- Menampilkan ringkasan (total balance, total income, total expense) dan daftar transaksi.

Tujuan: dari landing page kamu akan punya aplikasi kecil FinTrack yang bisa menerima input transaksi, menyimpan data, menampilkan daftar, dan menghitung ringkasan.

Asumsi: kamu sudah memiliki project React (CRA/Vite) dan landing page dari Checkpoint 3.

Spesifikasi tugas (step-by-step)
--------------------------------

1) Setup awal
   - Pastikan project sudah siap (CRA/Vite). Jika belum, buat project dan salin landing page dari checkpoint-3.
   - Install dependency yang diperlukan (opsional): `react-router-dom@6`. Jika mau pakai server mock: `json-server`.

2) Routing & Layout
   - Tambahkan `react-router-dom@6`.
   - Buat route dan navigasi minimal:
     - `/` → Landing page (sudah ada)
     - `/app` → Dashboard (ringkasan + link ke Transactions)
     - `/transactions` → Daftar transaksi
     - `/transactions/new` → Form tambah transaksi
     - `/transactions/:id` → (opsional) detail/edit transaksi
   - Buat header/navbar sederhana untuk berpindah antar halaman tanpa reload.

3) Data model & persistence
   - Gunakan struktur data sederhana untuk transaksi, misalnya:

```js
// contoh transaksi
{
  id: 'uuid-or-timestamp',
  type: 'income' | 'expense',
  amount: 250000,
  description: 'Gaji' ,
  date: '2025-11-01'
}
```

   - Pilih satu metode persistence:
     - localStorage (paling mudah), atau
     - json-server lokal (lebih mirip API). Jika memilih API, berikan fallback ke localStorage bila tidak tersedia.

4) State management & Hooks
   - Buat context sederhana `TransactionsContext` (opsional) atau gunakan `useReducer` di parent (mis. `App` atau `TransactionsProvider`) untuk menangani state transactions: tambah, hapus, update.
   - Implementasikan form `AddTransaction` yang memanfaatkan `useReducer` atau `useState` dengan validasi sederhana (amount > 0, description tidak kosong).
   - Buat komponen `TransactionsList` yang menampilkan transaksi terurut (terbaru di atas).

5) Dashboard & ringkasan
   - Tampilkan ringkasan singkat di `/app`: total balance, total income, total expense — hitung dari daftar transaksi.
   - Tambahkan visual cue (hijau untuk income, merah untuk expense) dan formatting untuk rupiah.

6) Fetching & optional useFetch
   - Jika kamu menggunakan json-server atau API eksternal, buat custom hook `src/hooks/useFetch.js` (lihat hint di bawah). Hook harus menangani loading, error, dan pembatalan request (AbortController).
   - Jika hanya menggunakan `localStorage`, tunjukkan penggunaan `useEffect` untuk sinkronisasi ke localStorage.

7) UX & polish
   - Tampilkan loading state saat mengambil data dari API/mock.
   - Beri feedback setelah menambah transaksi (toast sederhana atau pesan singkat).
   - Pastikan form memiliki validasi dan handling error tersendiri.

Total: 100% (bonus opsional seperti grafik kecil atau edit/delete yang rapi dapat menambah nilai practical notes).

Struktur file yang direkomendasikan
----------------------------------

```
src/
  App.jsx
  index.jsx
  pages/
    Landing.jsx        # dari checkpoint-3
    Dashboard.jsx      # /app
    Transactions.jsx   # /transactions
    TransactionForm.jsx# /transactions/new
  context/
    TransactionsContext.jsx  (opsional)
  hooks/
    useFetch.js        # opsional, jika memakai API
  utils/
    currency.js        # helper formatting
  components/
    TransactionsList.jsx
    TransactionItem.jsx
    SummaryCard.jsx
```

Contoh ringkas `useFetch` (hint)
--------------------------------

Jika kamu memutuskan untuk memakai JSON server atau API publik, berikut contoh `useFetch` yang menangani pembatalan request:

```js
// src/hooks/useFetch.js
import { useState, useEffect } from 'react'

export function useFetch(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    if (!url) return
    const controller = new AbortController()
    setLoading(true)

    fetch(url, { signal: controller.signal })
      .then(res => {
        if (!res.ok) throw new Error('Network response was not ok')
        return res.json()
      })
      .then(json => setData(json))
      .catch(err => { if (err.name !== 'AbortError') setError(err) })
      .finally(() => setLoading(false))

    return () => controller.abort()
  }, [url])

  return { data, loading, error }
}
```

Tips & catatan penutup
---------------------

- Fokuskan dulu pada fungsi (menambah transaksi, menyimpan, menampilkan summary). UI boleh sederhana tapi harus jelas.
- Tulis README singkat di root project yang menjelaskan cara menjalankan app, apakah menggunakan `json-server` atau `localStorage` fallback.
- Jika mau, aku bisa bantu membuat starter template (komponen dasar + context + useFetch) atau commit perubahan ini langsung ke `feat/week4` — bilang saja mau aku kerjakan.

---

Selamat mengerjakan! Kalau mau aku bantu bikin starter template atau commit perubahan, beri tahu dan aku akan lanjutkan.
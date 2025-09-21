---
title: Materi 4
sidebar_position: 4
---

# React Js Advanced (Routing, Hooks, Fetching)

## Routing

**Routing** adalah proses navigasi antar halaman atau komponen dalam aplikasi web. Dalam konteks React, routing biasanya diimplementasikan menggunakan pustaka seperti React Router atau fitur bawaan dari framework seperti Next.js.

**Macam-macam routing:**

1. Based on file structure
2. Dynamic routing
3. Nested routing
4. Route groups
5. Not found page
6. Exclude files from routing
7. Static assets

**Routing in Next.js:**

1. File-based system
2. URL merepresentasikan struktur folder
3. Semua routing di dalam folder ‘app’
4. Routing berlaku untuk file page.tsx atau page.js

## Hooks

**Hooks** adalah fitur built-in dari React yang memungkinkan kita untuk menggunakan state dan fitur React lainnya tanpa menulis class. Hooks memungkinkan kita untuk mengelola state, efek samping, konteks, dan lainnya dalam komponen fungsional. Hooks yang paling umum digunakan adalah `useState` dan `useEffect`.

- `useState`: Digunakan untuk menambahkan state ke dalam komponen fungsional. Contoh penggunaan:

```javascript
import React, { useState } from "react";
const [count, setCount] = useState(0);
```

- `useEffect`: Digunakan untuk menangani efek samping dalam komponen fungsional, seperti fetching data, mengatur timer, atau berinteraksi dengan API eksternal. Contoh penggunaan:

```javascript
import React, { useEffect } from "react";
useEffect(() => {
  // kode
}, []);
```

## Fetching

Fetching adalah proses mengambil data dari sumber eksternal, seperti API atau database, dan menggunakannya dalam aplikasi kita. Dalam konteks React, fetching biasanya dilakukan dalam efek samping menggunakan hook seperti `useEffect` dan `useState`.

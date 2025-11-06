---
title: Materi 4
sidebar_position: 4
---

# React JS Advanced  Routing, Hooks, Fetching

> Materi ini membahas 3 topik penting di React modern: Routing (React Router v6), Hooks (built-in & custom), dan Data Fetching (fetch / axios / React Query). Di setiap bagian disertakan contoh implementasi langkah-demi-langkah agar bisa dicoba langsung.

---

## Ringkasan singkat

- Routing: membuat halaman berbeda di Single Page Application dengan React Router v6 (route, nested route, params, navigation).
- Hooks: penggunaan dan contoh implementasi useState, useEffect, useRef, useContext, useReducer, dan cara buat custom hook.
- Fetching: pola mengambil data dari API dengan fetch/axios, pembatalan request, penanganan loading/error, dan pengenalan React Query untuk caching/revalidation.

---

## Cara pakai materi ini

1. Tutorial ini ditulis sebagai panduan untuk project React (Create React App atau Vite).
2. Untuk mencoba contoh, ikuti bagian "Try it" di setiap topik atau langsung ke Mini-Project di bagian akhir.

---

## 1) Routing  penjelasan + implementasi

React Router v6 adalah library routing yang sering dipakai di React. Kita akan lihat cara membuat rute, parameter rute, navigation, dan nested routes.

Instalasi:

```bash
npm install react-router-dom@6
```

Implementasi minimal  langkah demi langkah

- File: `src/App.jsx`

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom'
import Home from './pages/Home'
import Movies from './pages/Movies'
import MovieDetail from './pages/MovieDetail'

export default function App() {
  return (
    <BrowserRouter>
      <header>
        <nav>
          <Link to="/">Home</Link> | <Link to="/movies">Movies</Link>
        </nav>
      </header>

      <main>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/movies" element={<Movies />} />
          <Route path="/movies/:id" element={<MovieDetail />} />
          <Route path="*" element={<h2>404 - Not Found</h2>} />
        </Routes>
      </main>
    </BrowserRouter>
  )
}
```

- File: `src/pages/Home.jsx`

```jsx
export default function Home() {
  return (
    <div>
      <h1>Welcome to Simple Movie App</h1>
      <p>Pilih menu Movies untuk melihat daftar film.</p>
    </div>
  )
}
```

- File: `src/pages/Movies.jsx` (list dengan Link ke detail)

```jsx
import { Link } from 'react-router-dom'
import { useEffect, useState } from 'react'

export default function Movies() {
  const [movies, setMovies] = useState([])

  useEffect(() => {
    // contoh data lokal / mock untuk memudahkan percobaan
    setMovies([
      { id: '1', title: 'Movie A' },
      { id: '2', title: 'Movie B' },
      { id: '3', title: 'Movie C' },
    ])
  }, [])

  return (
    <div>
      <h2>Movies</h2>
      <ul>
        {movies.map(m => (
          <li key={m.id}>
            <Link to={`/movies/${m.id}`}>{m.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

- File: `src/pages/MovieDetail.jsx` (mengambil param rute)

```jsx
import { useParams, useNavigate } from 'react-router-dom'
import { useEffect, useState } from 'react'

export default function MovieDetail() {
  const { id } = useParams()
  const navigate = useNavigate()
  const [movie, setMovie] = useState(null)

  useEffect(() => {
    // contoh ambil data lokal berdasarkan id
    const map = {
      '1': { id: '1', title: 'Movie A', description: 'Deskripsi Movie A' },
      '2': { id: '2', title: 'Movie B', description: 'Deskripsi Movie B' },
      '3': { id: '3', title: 'Movie C', description: 'Deskripsi Movie C' },
    }
    setMovie(map[id] || null)
  }, [id])

  if (!movie) return <p>Movie not found</p>

  return (
    <div>
      <h2>{movie.title}</h2>
      <p>{movie.description}</p>
      <button onClick={() => navigate(-1)}>Back</button>
    </div>
  )
}
```

Catatan penting:

- Gunakan `Link` untuk navigasi internal (menghindari reload penuh).
- `useParams()` mengembalikan object parameter rute.
- `useNavigate()` memberikan fungsi navigasi programatik (navigate('/path') atau navigate(-1) untuk back).

Try it  perintah singkat:

```bash
npx create-react-app movie-app
cd movie-app
# tambahkan file sesuai contoh di atas
npm start
```

---

## 2) Hooks  penjelasan + implementasi contoh

Di sini kita tunjukkan contoh implementasi yang bisa langsung dicoba.

### useState  counter sederhana

File: `src/components/Counter.jsx`

```jsx
import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>Tambah</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  )
}
```

### useEffect  fetch data sederhana + cleanup

File: `src/hooks/useFetch.js` (custom hook, nanti dipakai juga di fetching section)

```js
import { useState, useEffect } from 'react'

export function useFetch(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    const controller = new AbortController()
    setLoading(true)

    fetch(url, { signal: controller.signal })
      .then(res => {
        if (!res.ok) throw new Error('Network response was not ok')
        return res.json()
      })
      .then(json => setData(json))
      .catch(err => {
        if (err.name !== 'AbortError') setError(err)
      })
      .finally(() => setLoading(false))

    return () => controller.abort()
  }, [url])

  return { data, loading, error }
}
```

Penggunaan di komponen:

```jsx
import { useFetch } from '../hooks/useFetch'

function Users() {
  const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users')

  if (loading) return <p>Loading...</p>
  if (error) return <p>Error: {error.message}</p>

  return (
    <ul>
      {data.map(u => <li key={u.id}>{u.name} ({u.email})</li>)}
    </ul>
  )
}
```

### useRef  mengontrol fokus input

File: `src/components/TextFocus.jsx`

```jsx
import { useRef } from 'react'

export default function TextFocus() {
  const inputRef = useRef(null)

  return (
    <div>
      <input ref={inputRef} placeholder="Ketik..." />
      <button onClick={() => inputRef.current?.focus()}>Focus</button>
    </div>
  )
}
```

### useContext  tema sederhana

File: `src/context/ThemeContext.jsx`

```jsx
import { createContext, useContext, useState } from 'react'

const ThemeContext = createContext(null)

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light')
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export function useTheme() {
  return useContext(ThemeContext)
}
```

Contoh memakai `useTheme` di komponen:

```jsx
import { useTheme } from '../context/ThemeContext'

export default function ThemeSwitcher() {
  const { theme, setTheme } = useTheme()
  return (
    <div>
      <p>Current: {theme}</p>
      <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>Toggle</button>
    </div>
  )
}
```

### useReducer  form sederhana dengan validasi

File: `src/components/LoginForm.jsx`

```jsx
import { useReducer } from 'react'

function reducer(state, action) {
  switch (action.type) {
    case 'field':
      return { ...state, [action.field]: action.value }
    case 'submit':
      return { ...state, submitting: true }
    case 'done':
      return { ...state, submitting: false }
    default:
      return state
  }
}

export default function LoginForm() {
  const [state, dispatch] = useReducer(reducer, { email: '', password: '', submitting: false })

  return (
    <form onSubmit={(e) => { e.preventDefault(); dispatch({type: 'submit'}) }}>
      <input value={state.email} onChange={e => dispatch({type: 'field', field: 'email', value: e.target.value})} placeholder="Email" />
      <input value={state.password} onChange={e => dispatch({type: 'field', field: 'password', value: e.target.value})} placeholder="Password" />
      <button type="submit">{state.submitting ? 'Submitting...' : 'Login'}</button>
    </form>
  )
}
```

---

## 3) Fetching  pola & contoh implementasi

### Fetch + AbortController (pola recommended)

Contoh ada di `src/hooks/useFetch.js` (lihat di atas). Intinya:

- Gunakan AbortController untuk membatalkan request saat komponen unmount.
- Tangani state loading & error.

### axios contoh

Instal:

```bash
npm install axios
```

Contoh penggunaan di komponen:

```jsx
import axios from 'axios'
import { useEffect, useState } from 'react'

function Posts() {
  const [posts, setPosts] = useState([])
  useEffect(() => {
    const controller = new AbortController()
    axios.get('https://jsonplaceholder.typicode.com/posts', { signal: controller.signal })
      .then(res => setPosts(res.data))
      .catch(err => console.error(err))

    return () => controller.abort()
  }, [])

  return <div>{posts.slice(0,5).map(p => <p key={p.id}>{p.title}</p>)}</div>
}
```

> Catatan: beberapa environment/versi axios tidak mendukung `signal` penuh  kalau error, gunakan cancellation token axios lama atau pakai fetch.

### React Query (untuk caching & revalidation)

React Query (sekarang TanStack Query) membantu caching, refetch, background updates.

Instal:

```bash
npm install @tanstack/react-query
```

Contoh setup di `src/index.jsx`:

```jsx
import React from 'react'
import { createRoot } from 'react-dom/client'
import App from './App'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

createRoot(document.getElementById('root')).render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
)
```

Contoh pemakaian:

```jsx
import { useQuery } from '@tanstack/react-query'

function MoviesList() {
  const { data, isLoading, error } = useQuery(['movies'], () => fetch('https://api.sampleapis.com/movies/action').then(r => r.json()))

  if (isLoading) return <p>Loading...</p>
  if (error) return <p>Error</p>

  return <ul>{data.slice(0,10).map(m => <li key={m.id}>{m.title}</li>)}</ul>
}
```

React Query mengurangi boilerplate (loading, error, caching) dan cocok untuk aplikasi yang sering berkomunikasi ke server.

---

## Mini-Project lengkap: Simple Movie App (implementasi langkah-demi-langkah)

Tujuan: gabungkan routing, hooks, dan fetching menjadi satu aplikasi sederhana.

Rekomendasi: gunakan Create React App atau Vite. Contoh di sini pakai Create React App.

1) Buat project

```bash
npx create-react-app movie-app
cd movie-app
npm install react-router-dom@6
```

2) Struktur file (tambah/ubah di `src/`):

- `src/App.jsx`  seperti contoh routing di atas.
- `src/index.jsx`  kalau pakai React Query, wrap dengan QueryClientProvider.
- `src/pages/Home.jsx`, `src/pages/Movies.jsx`, `src/pages/MovieDetail.jsx`  isi sesuai contoh sebelumnya.
- `src/hooks/useFetch.js`  hook fetch dengan AbortController.

3) Jalankan

```bash
npm start
```

4) Cek di browser `http://localhost:3000`.

Catatan: kalau API publik yang dipakai down, gunakan mock data di `Movies.jsx` dan `MovieDetail.jsx` (seperti contoh) atau simpan sample JSON di `public/`.

---

## Latihan (Exercises)

1. Tambahkan halaman Search yang memfilter daftar film berdasarkan judul (client-side filter).
2. Implementasikan fitur favorite: simpan film favorit di `localStorage` dan tampilkan di halaman tersendiri menggunakan `useContext` untuk state global.
3. Ganti `useFetch` dengan React Query di project dan bandingkan ukuran kode untuk fetching + caching.

---

## Resources

- React Router: https://reactrouter.com/
- React Hooks: https://reactjs.org/docs/hooks-intro.html
- React Query (TanStack): https://tanstack.com/query/latest
- SWR: https://swr.vercel.app/

---

## Git: commit & push perubahan materi

Jika kamu ingin menyimpan perubahan di repo ini, contoh perintah PowerShell:

```powershell
git add docs/materi/frontend/materi-4.md
git commit -m "Add: Materi 4  React advanced (routing, hooks, fetching) with examples"
git push -u origin feat/week4
```

---

Kalau mau, aku bisa langsung commit dan push file ini ke branch `feat/week4` untuk kamu. Mau aku jalankan itu sekarang? Jika ya, konfirmasi dan aku akan melakukan add/commit/push. Jika tidak, kamu bisa menjalankan perintah di atas sendiri.

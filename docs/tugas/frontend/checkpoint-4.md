---
title: Checkpoint 4
sidebar_position: 4
---

# React JS Advanced Routing, Hooks, Fetching

Dari checkpoint sebelumnya, sekarang kita akan menambahkan routing, hooks, dan fetching data dari API.

- Tambahkan React Router ke halaman login dan homepage.
- Gunakan hooks (useState, useEffect) untuk fetch data dari API.
- Tampilkan data dari backend

### Contoh ringkas `useFetch` (hint)

Jika kamu memutuskan untuk memakai JSON server atau API publik, berikut contoh `useFetch` yang menangani pembatalan request:

```js
// src/hooks/useFetch.js
import { useState, useEffect } from "react";

export function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    if (!url) return;
    const controller = new AbortController();
    setLoading(true);

    fetch(url, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error("Network response was not ok");
        return res.json();
      })
      .then((json) => setData(json))
      .catch((err) => {
        if (err.name !== "AbortError") setError(err);
      })
      .finally(() => setLoading(false));

    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}
```

---

Selamat mengerjakan!

<!-- Kalau mau aku bantu bikin starter template atau commit perubahan, beri tahu dan aku akan lanjutkan. -->

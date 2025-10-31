---
title: Checkpoint 3
sidebar_position: 3
---

# Intro to React Js (Installation, Functional Component, Props, Lifecycle)

**Dari proyek landing page Checkpoint 2**, sekarang **konversi seluruhnya menjadi aplikasi React** menggunakan **Vite + React**.

## **Tujuan**
Mengubah struktur HTML + JS vanilla menjadi komponen React, sambil mempertahankan semua fungsi interaktif (Dark Mode & Hamburger Menu) tapi dengan state, props, dan komponen React.

---

Bagilah bagian" dari landing page kalian menjadi Components (Navbar.jsx, App.jsx, Footer.jsx).

## **Tips**
Untuk menerapkan fungsionalitas fitur checkpoint 2 ke dalam react, manfaatkan:
### **useState**  
Contoh : 
```jsx
const [isDarkMode, setIsDarkMode] = useState(false);
const [isMenuOpen, setIsMenuOpen] = useState(false);
```
  
### **Conditional Rendering**
Contoh:
```jsx
{isDarkMode ? '☀️ Mode Terang' : '🌙 Mode Gelap'}

className={`hamburger ${isMenuOpen ? 'active' : ''}`}
```

### Event Handlers
Contoh:
```jsx
<button onClick={toggleTheme}>
  {isDarkMode ? '☀️ Mode Terang' : '🌙 Mode Gelap'}
</button>
```

### Component-Based Architecture
Contoh:
```jsx
export default function FitTrackApp() {
  // All logic and UI in one component
  return (...)
}
```

## Tugas Utama
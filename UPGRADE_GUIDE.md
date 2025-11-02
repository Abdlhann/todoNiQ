# 🚀 Panduan Upgrade ke TodoNiQ Enhanced

## 📋 Langkah-Langkah Upgrade

### Opsi 1: Update main.js (Recommended)

**File:** `src/main.js`

**Dari:**
```javascript
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {}
});

export default app;
```

**Menjadi:**
```javascript
import App from './AppEnhanced.svelte';

const app = new App({
	target: document.body,
	props: {}
});

export default app;
```

### Opsi 2: Rename Files

Jika ingin tetap menggunakan nama `App.svelte`:

1. **Backup file lama:**
   ```bash
   mv src/App.svelte src/App.backup.svelte
   mv src/CardList.svelte src/CardList.backup.svelte
   mv src/TodoCard.svelte src/TodoCard.backup.svelte
   ```

2. **Rename file baru:**
   ```bash
   mv src/AppEnhanced.svelte src/App.svelte
   mv src/CardListEnhanced.svelte src/CardList.svelte
   mv src/TodoCardEnhanced.svelte src/TodoCard.svelte
   ```

3. **Update imports di App.svelte:**
   - Ubah `CardListEnhanced` → `CardList`
   - Ubah `TodoCardEnhanced` → `TodoCard`

---

## ✅ Verifikasi

Setelah upgrade, pastikan:

1. ✅ **Theme toggle** berfungsi (icon sun/moon di header)
2. ✅ **Statistics dashboard** muncul di atas
3. ✅ **Search bar** dan filter buttons tersedia
4. ✅ **Export/Import buttons** terlihat
5. ✅ **Advanced options** (+ button) di setiap card
6. ✅ **Priority badges** muncul di task cards
7. ✅ **No errors** di browser console

---

## 🔄 Migrasi Data

Data lama **tetap kompatibel**! 

Struktur data baru:
```javascript
{
  todo: "Task title",
  priority: "medium",      // NEW
  dueDate: "2024-12-31",  // NEW
  labels: ["work", "urgent"], // NEW
  subtasks: [             // NEW
    { text: "Subtask 1", completed: false }
  ],
  createdAt: "2024-10-31T..." // NEW
}
```

Task lama akan otomatis mendapat default values:
- `priority: "medium"`
- `dueDate: ""`
- `labels: []`
- `subtasks: []`

---

## 🎨 Customization

### Mengubah Default Theme

**File:** `src/AppEnhanced.svelte`

```javascript
// Line 17: Ubah default theme
let theme = localStorage.getItem('theme') || 'light'; // Ganti 'dark' ke 'light'
```

### Mengubah Warna Tema

**File:** `public/global.css`

Edit CSS variables di `:root` dan `[data-theme="light"]`

---

## 🐛 Troubleshooting

### Error: Module not found

**Solusi:**
```bash
npm install
npm run dev
```

### Theme tidak berubah

**Solusi:**
1. Clear browser cache
2. Check browser console untuk errors
3. Pastikan `data-theme` attribute ada di `<html>`

### Data hilang setelah upgrade

**Solusi:**
Data tersimpan di localStorage, tidak akan hilang.
Jika hilang, restore dari export backup.

### Styling tidak muncul

**Solusi:**
1. Hard refresh: `Ctrl + Shift + R`
2. Check `global.css` ter-load
3. Pastikan CSS variables defined

---

## 📦 File Structure Baru

```
src/
├── App.svelte                  # Original (backup)
├── AppEnhanced.svelte          # NEW - Main app dengan semua fitur
├── CardList.svelte             # Original (backup)
├── CardListEnhanced.svelte     # NEW - Enhanced card list
├── TodoCard.svelte             # Original (backup)
├── TodoCardEnhanced.svelte     # NEW - Enhanced todo card
├── Statistics.svelte           # NEW - Stats dashboard
├── TaskModal.svelte            # NEW - Add/Edit modal
└── main.js                     # Update import here
```

---

## 🔙 Rollback

Jika ingin kembali ke versi lama:

**File:** `src/main.js`

```javascript
import App from './App.svelte'; // Ubah dari AppEnhanced
```

Atau restore dari backup files.

---

## 🆘 Support

Jika ada masalah:

1. Check `FEATURES.md` untuk dokumentasi fitur
2. Check browser console untuk errors
3. Pastikan semua dependencies ter-install
4. Try clean install:
   ```bash
   rm -rf node_modules
   npm install
   npm run dev
   ```

---

## 🎉 Selamat!

TodoNiQ Enhanced siap digunakan dengan semua fitur profesional! 🚀

**Next Steps:**
1. Explore semua fitur baru
2. Export data untuk backup
3. Customize theme sesuai selera
4. Enjoy productivity boost! 💪

---

**Happy Task Managing! ✨**

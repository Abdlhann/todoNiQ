# 🚀 Panduan Deploy TodoNiQ ke GitHub Pages

## ❌ Masalah yang Terjadi

GitHub Pages menampilkan README.md repository, bukan aplikasi TodoNiQ.

**Penyebab:**
- Aplikasi belum di-build
- File build belum di-push ke branch `gh-pages`
- GitHub Pages tidak tahu file mana yang harus ditampilkan

---

## ✅ Solusi: Deploy dengan GitHub Actions (Otomatis)

### **Langkah 1: Setup GitHub Actions**

File `.github/workflows/deploy.yml` sudah dibuat! File ini akan:
- ✅ Otomatis build saat push ke main/master
- ✅ Deploy hasil build ke GitHub Pages
- ✅ Update otomatis setiap kali ada perubahan

### **Langkah 2: Enable GitHub Pages**

1. Buka repository di GitHub
2. Klik **Settings** (tab paling kanan)
3. Scroll ke bagian **Pages** (menu kiri)
4. Di **Source**, pilih:
   - Branch: `gh-pages`
   - Folder: `/ (root)`
5. Klik **Save**

### **Langkah 3: Push ke GitHub**

```bash
git add .
git commit -m "Add GitHub Actions deploy workflow"
git push origin main
```

### **Langkah 4: Tunggu Build Selesai**

1. Buka tab **Actions** di repository GitHub
2. Lihat workflow "Deploy to GitHub Pages" sedang running
3. Tunggu sampai selesai (✅ hijau)
4. Refresh halaman GitHub Pages Anda

**URL:** `https://abdlhann.github.io/todoNiQ/`

---

## 🔧 Solusi Alternatif: Deploy Manual

Jika GitHub Actions tidak jalan, deploy manual:

### **Opsi 1: Menggunakan gh-pages Package**

#### 1. Install gh-pages
```bash
npm install --save-dev gh-pages
```

#### 2. Update package.json
Tambahkan script deploy:
```json
{
  "scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w",
    "start": "sirv public --no-clear",
    "deploy": "npm run build && gh-pages -d public"
  }
}
```

#### 3. Deploy
```bash
npm run deploy
```

#### 4. Enable GitHub Pages
- Settings → Pages
- Source: `gh-pages` branch
- Folder: `/ (root)`

---

### **Opsi 2: Build & Push Manual**

#### 1. Build aplikasi
```bash
npm run build
```

#### 2. Buat branch gh-pages
```bash
git checkout --orphan gh-pages
```

#### 3. Hapus semua file kecuali public
```bash
git rm -rf .
```

#### 4. Copy isi folder public ke root
```bash
# Windows (PowerShell)
Copy-Item -Path public\* -Destination . -Recurse

# Linux/Mac
cp -r public/* .
```

#### 5. Commit & Push
```bash
git add .
git commit -m "Deploy to GitHub Pages"
git push origin gh-pages --force
```

#### 6. Kembali ke branch main
```bash
git checkout main
```

#### 7. Enable GitHub Pages
- Settings → Pages
- Source: `gh-pages` branch

---

## 🎯 Verifikasi Deploy Berhasil

### **Cek di GitHub:**
1. Tab **Actions** → Workflow harus ✅ hijau
2. Tab **Settings → Pages** → Harus ada URL aktif
3. Branch `gh-pages` harus ada di repository

### **Cek di Browser:**
1. Buka: `https://abdlhann.github.io/todoNiQ/`
2. Harus muncul aplikasi TodoNiQ, bukan README
3. Semua fitur harus berfungsi

---

## 🐛 Troubleshooting

### **1. Aplikasi Tidak Muncul**
**Penyebab:** Path tidak benar

**Solusi:** Update `rollup.config.js`:
```javascript
// Tambahkan base path
export default {
  // ... config lainnya
  output: {
    // ... output config
    // Tambahkan ini jika perlu:
    // assetFileNames: 'build/[name]-[hash][extname]'
  }
}
```

### **2. CSS/JS Tidak Load**
**Penyebab:** Path relatif salah

**Solusi:** Cek `public/index.html`:
```html
<!-- Pastikan path benar -->
<link rel='stylesheet' href='/global.css'>
<link rel='stylesheet' href='/build/bundle.css'>
<script defer src='/build/bundle.js'></script>
```

Atau gunakan path relatif:
```html
<link rel='stylesheet' href='./global.css'>
<link rel='stylesheet' href='./build/bundle.css'>
<script defer src='./build/bundle.js'></script>
```

### **3. Logo Tidak Muncul**
**Penyebab:** Folder images belum di-copy ke public

**Solusi:**
```bash
# Pastikan folder ini ada:
public/images/logo.jpeg
```

### **4. GitHub Actions Gagal**
**Penyebab:** Permissions atau token issue

**Solusi:**
1. Settings → Actions → General
2. Scroll ke **Workflow permissions**
3. Pilih **Read and write permissions**
4. Klik **Save**
5. Re-run workflow

### **5. 404 Error**
**Penyebab:** Branch atau folder salah

**Solusi:**
- Pastikan branch `gh-pages` ada
- Pastikan folder `/ (root)` dipilih di Settings → Pages
- Tunggu 1-2 menit untuk propagasi

---

## 📋 Checklist Deploy

- [ ] File `.github/workflows/deploy.yml` sudah ada
- [ ] Push ke GitHub (main/master branch)
- [ ] GitHub Actions workflow running
- [ ] Workflow selesai dengan status ✅
- [ ] Branch `gh-pages` terbuat otomatis
- [ ] GitHub Pages enabled di Settings
- [ ] Source: `gh-pages` branch dipilih
- [ ] URL aktif di Settings → Pages
- [ ] Aplikasi muncul di browser
- [ ] Semua fitur berfungsi
- [ ] Logo muncul
- [ ] CSS/JS load dengan benar

---

## 🎨 Custom Domain (Opsional)

Jika punya domain sendiri:

### 1. Tambahkan CNAME
Buat file `public/CNAME`:
```
yourdomain.com
```

### 2. Update DNS
Di provider domain Anda, tambahkan:
```
Type: CNAME
Name: www
Value: abdlhann.github.io
```

### 3. Enable di GitHub
Settings → Pages → Custom domain → Masukkan domain

---

## 🚀 Deploy ke Platform Lain

### **Netlify** (Recommended - Paling Mudah)

1. Buka: https://app.netlify.com/
2. Drag & drop folder `public` setelah build
3. Atau connect ke GitHub repository
4. Build command: `npm run build`
5. Publish directory: `public`

### **Vercel**

1. Buka: https://vercel.com/
2. Import GitHub repository
3. Build command: `npm run build`
4. Output directory: `public`
5. Deploy!

### **Surge.sh** (Simple)

```bash
npm install -g surge
npm run build
cd public
surge
```

---

## 💡 Tips

1. **Selalu build sebelum deploy:**
   ```bash
   npm run build
   ```

2. **Test lokal dulu:**
   ```bash
   npm run start
   # Buka http://localhost:8080
   ```

3. **Update otomatis:**
   - Dengan GitHub Actions, setiap push akan auto-deploy
   - Tidak perlu deploy manual lagi

4. **Monitor deploy:**
   - Cek tab Actions untuk status
   - Lihat log jika ada error

5. **Cache busting:**
   - Hard refresh: `Ctrl + Shift + R`
   - Clear cache jika update tidak muncul

---

## ✅ Kesimpulan

**Dengan GitHub Actions:**
- ✅ Push → Auto build → Auto deploy
- ✅ Tidak perlu command manual
- ✅ Selalu up-to-date

**Tanpa GitHub Actions:**
- ⚠️ Harus build manual
- ⚠️ Harus deploy manual
- ⚠️ Lebih ribet

**Rekomendasi:** Gunakan GitHub Actions! 🚀

---

**TodoNiQ siap online! 🎉**

URL: `https://abdlhann.github.io/todoNiQ/`

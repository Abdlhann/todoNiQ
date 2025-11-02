# 🎨 Panduan Update Logo TodoNiQ

## ✅ Perubahan yang Sudah Dilakukan

### 1. **Logo di Header Website**
- File: `src/App.svelte` dan `src/AppEnhanced.svelte`
- Logo icon Font Awesome diganti dengan image logo Anda
- Ukuran: 56x56px dengan border radius dan shadow

### 2. **Favicon (Logo di Tab Browser)**
- File: `public/index.html`
- Favicon diganti menggunakan logo Anda
- Support untuk desktop dan mobile devices

### 3. **Social Media Preview**
- Open Graph meta tags ditambahkan
- Logo akan muncul saat link di-share di social media
- Support Twitter Card

---

## 📋 Langkah yang Perlu Anda Lakukan

### **PENTING: Copy Folder Images ke Public**

Anda perlu memindahkan folder `images` ke dalam folder `public`:

#### **Cara Termudah - Via File Explorer:**
1. Buka folder: `d:\Hanzz\Project\Pemograman\todoNiQ`
2. **Copy** folder `images` (yang berisi `logo.jpeg`)
3. **Paste** ke dalam folder `public`
4. Hasil akhir: `d:\Hanzz\Project\Pemograman\todoNiQ\public\images\logo.jpeg`

#### **Atau Via Command Prompt:**
```cmd
cd d:\Hanzz\Project\Pemograman\todoNiQ
mkdir public\images
copy images\logo.jpeg public\images\
```

---

## 🎯 Hasil Setelah Update

### **1. Logo di Header**
```
┌─────────────────────────────────┐
│ [LOGO] TodoNiQ    🌙  🕐 Time   │
│   ✨ Produktivitas dimulai...   │
└─────────────────────────────────┘
```

**Fitur Logo:**
- ✅ Ukuran 56x56px
- ✅ Border radius 12px
- ✅ Shadow effect dengan warna tema
- ✅ Hover animation (scale + rotate)
- ✅ Berbeda di dark/light mode

### **2. Favicon di Browser Tab**
```
[LOGO] TodoNiQ - Task...
```

**Support:**
- ✅ Desktop browsers (Chrome, Firefox, Edge, Safari)
- ✅ Mobile browsers
- ✅ Apple Touch Icon (iOS)
- ✅ PWA icon

### **3. Social Media Preview**
Saat link di-share di:
- Facebook
- Twitter
- WhatsApp
- LinkedIn
- Discord

Logo Anda akan muncul sebagai preview image! 🎉

---

## 🔧 Technical Details

### **File yang Diubah:**

#### 1. `src/AppEnhanced.svelte`
```svelte
<div class="logo">
	<img src="/images/logo.jpeg" alt="TodoNiQ Logo" class="logo-image" />
	<h1 class="app-title">TodoNiQ</h1>
</div>
```

#### 2. `src/App.svelte`
```svelte
<div class="logo">
	<img src="/images/logo.jpeg" alt="TodoNiQ Logo" class="logo-image" />
	<h1 class="app-title">TodoNiQ</h1>
</div>
```

#### 3. `public/index.html`
```html
<!-- Favicons -->
<link rel='icon' type='image/jpeg' href='/images/logo.jpeg'>
<link rel='apple-touch-icon' href='/images/logo.jpeg'>
<link rel='shortcut icon' type='image/jpeg' href='/images/logo.jpeg'>

<!-- Open Graph -->
<meta property='og:image' content='/images/logo.jpeg'>
<meta name='twitter:image' content='/images/logo.jpeg'>
```

#### 4. `public/global.css`
```css
.logo-image {
	width: 56px;
	height: 56px;
	border-radius: 12px;
	object-fit: cover;
	box-shadow: 0 4px 15px rgba(78, 205, 196, 0.4);
	transition: all 0.3s ease;
	border: 2px solid rgba(78, 205, 196, 0.3);
}

.logo-image:hover {
	transform: scale(1.05) rotate(2deg);
	box-shadow: 0 6px 20px rgba(78, 205, 196, 0.6);
}

[data-theme="light"] .logo-image {
	box-shadow: 0 4px 15px rgba(8, 145, 178, 0.3);
	border: 2px solid rgba(8, 145, 178, 0.3);
}
```

---

## 🎨 Customization

### **Mengubah Ukuran Logo:**
Edit di `public/global.css`:
```css
.logo-image {
	width: 64px;    /* Ubah sesuai keinginan */
	height: 64px;   /* Ubah sesuai keinginan */
}
```

### **Mengubah Border Radius:**
```css
.logo-image {
	border-radius: 50%;  /* Untuk logo bulat */
	/* atau */
	border-radius: 8px;  /* Untuk sudut lebih tajam */
}
```

### **Menghilangkan Border:**
```css
.logo-image {
	border: none;
}
```

### **Mengubah Shadow:**
```css
.logo-image {
	box-shadow: 0 8px 25px rgba(78, 205, 196, 0.6);  /* Shadow lebih besar */
}
```

---

## 🚀 Testing

Setelah copy folder images, test dengan:

### **1. Logo di Header**
- ✅ Buka website
- ✅ Logo muncul di kiri atas
- ✅ Hover untuk lihat animasi

### **2. Favicon di Tab**
- ✅ Lihat tab browser
- ✅ Logo muncul di sebelah title
- ✅ Coba bookmark untuk test

### **3. Mobile**
- ✅ Buka di mobile browser
- ✅ Add to home screen
- ✅ Logo muncul sebagai app icon

### **4. Social Media**
- ✅ Share link di WhatsApp/Facebook
- ✅ Logo muncul sebagai preview

---

## 🐛 Troubleshooting

### **Logo tidak muncul di header:**
1. Pastikan folder `public/images/logo.jpeg` ada
2. Hard refresh: `Ctrl + Shift + R`
3. Check browser console untuk errors

### **Favicon tidak berubah:**
1. Clear browser cache
2. Hard refresh: `Ctrl + Shift + R`
3. Close dan buka tab baru
4. Restart browser

### **Logo pecah/blur:**
1. Gunakan logo dengan resolusi tinggi (minimal 512x512px)
2. Format PNG lebih baik untuk transparency
3. Optimize image dengan tools online

---

## 📁 Struktur File Akhir

```
todoNiQ/
├── images/
│   └── logo.jpeg              ← Original (opsional, bisa dihapus)
├── public/
│   ├── images/                ← FOLDER INI HARUS ADA!
│   │   └── logo.jpeg          ← LOGO ANDA DI SINI!
│   ├── global.css             ← Styling logo
│   ├── index.html             ← Favicon references
│   └── favicon.png            ← Old favicon (bisa dihapus)
└── src/
    ├── App.svelte             ← Logo di header
    └── AppEnhanced.svelte     ← Logo di header (enhanced)
```

---

## 💡 Tips

1. **Format Logo:**
   - PNG lebih baik (support transparency)
   - JPEG untuk file size lebih kecil
   - SVG untuk scalability terbaik

2. **Ukuran Optimal:**
   - Header logo: 56x56px - 128x128px
   - Favicon: 32x32px, 64x64px, 128x128px
   - Social media: 1200x630px (landscape)

3. **Multiple Sizes:**
   Untuk hasil terbaik, siapkan beberapa ukuran:
   ```
   public/images/
   ├── logo.jpeg          (original)
   ├── logo-32.png        (favicon small)
   ├── logo-128.png       (favicon large)
   └── logo-social.png    (1200x630 untuk social media)
   ```

---

## ✅ Checklist

- [ ] Copy folder `images` ke `public/images/`
- [ ] Refresh browser dengan `Ctrl + Shift + R`
- [ ] Verifikasi logo muncul di header
- [ ] Verifikasi favicon muncul di tab
- [ ] Test hover animation pada logo
- [ ] Test di mobile device
- [ ] Test share link di social media

---

**Logo Anda siap digunakan! 🎉**

Jika ada masalah, pastikan path file benar:
`/images/logo.jpeg` → `public/images/logo.jpeg`

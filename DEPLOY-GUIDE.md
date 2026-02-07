# Advanta Sales App

Sales management application untuk Advanta.

## 🚀 Deploy ke Netlify

### Cara 1: Deploy via GitHub (Recommended)

1. **Push code ke GitHub:**
   ```bash
   git add .
   git commit -m "Add public DGA form"
   git push origin main
   ```

2. **Login ke Netlify:**
   - Buka [https://app.netlify.com](https://app.netlify.com)
   - Login dengan akun GitHub Anda

3. **Import Project:**
   - Klik "Add new site" → "Import an existing project"
   - Pilih "GitHub"
   - Pilih repository `ADV-SALES`
   - Branch: `main`
   - Build settings biarkan kosong (auto-detect)
   - Klik "Deploy site"

4. **Selesai!** ✅
   - Netlify akan generate URL seperti: `https://random-name-123.netlify.app`
   - Kedua file bisa diakses:
     - `https://your-site.netlify.app/` → index.html
     - `https://your-site.netlify.app/public-dga-form.html` → form publik

### Cara 2: Deploy Manual (Drag & Drop)

1. **Siapkan folder:**
   - Pastikan `index.html` dan `public-dga-form.html` ada di root folder
   - File `netlify.toml` sudah ada

2. **Deploy via Netlify Dashboard:**
   - Login ke [https://app.netlify.com](https://app.netlify.com)
   - Drag & drop folder `ADV-SALES` ke dashboard
   - Tunggu proses upload selesai
   - Selesai! 🎉

### Cara 3: Deploy via Netlify CLI

```bash
# Install Netlify CLI (jika belum)
npm install -g netlify-cli

# Login ke Netlify
netlify login

# Deploy dari folder ini
netlify deploy --prod
```

## 📱 Setelah Deploy

### Akses Aplikasi:
- **Main App:** `https://your-site.netlify.app/`
- **Form Publik DGA:** `https://your-site.netlify.app/public-dga-form.html`

### Share Link ke BS:
1. Copy link form publik: `https://your-site.netlify.app/public-dga-form.html`
2. Share via WhatsApp/Email ke BS
3. BS bisa langsung input kegiatan mereka

### Custom Domain (Opsional):
- Di Netlify Dashboard → Domain settings
- Tambahkan custom domain Anda
- Update DNS records sesuai instruksi

## 🔄 Update Aplikasi

Jika ada perubahan code:

### Via GitHub:
```bash
git add .
git commit -m "Update features"
git push origin main
```
Netlify akan auto-deploy perubahan! ✅

### Via Manual:
- Drag & drop folder yang sudah diupdate ke Netlify dashboard
- Pilih "Deploy site"

## 📝 Catatan Penting

- ✅ **LocalStorage** akan tetap berfungsi di production
- ✅ **Kedua file HTML** bisa diakses secara terpisah
- ✅ **No backend needed** - pure static files
- ✅ **HTTPS otomatis** dari Netlify
- ✅ **Free hosting** untuk static sites

## 🛠 Troubleshooting

**Q: Form publik tidak bisa diakses?**
- Pastikan file `public-dga-form.html` ada di root folder
- Cek URL: `https://your-site.netlify.app/public-dga-form.html`

**Q: Data tidak sinkron antar device?**
- LocalStorage bersifat per-browser/device
- Untuk sinkronisasi, perlu backend database (future feature)

**Q: Custom domain tidak jalan?**
- Tunggu DNS propagation (bisa 24-48 jam)
- Pastikan DNS records sudah benar

## 📊 File Structure

```
ADV-SALES/
├── index.html              # Main aplikasi
├── public-dga-form.html    # Form publik untuk BS
├── netlify.toml           # Netlify config
└── README.md              # This file
```

## 💡 Tips

1. **Rename site di Netlify:**
   - Site settings → Change site name
   - Misal: `advanta-sales.netlify.app`

2. **Enable form notifications:**
   - Netlify Forms bisa capture submissions
   - Bisa terima email notifikasi

3. **Analytics:**
   - Netlify Analytics (berbayar)
   - Atau gunakan Google Analytics (gratis)

---

Made with ❤️ for Advanta Sales Team

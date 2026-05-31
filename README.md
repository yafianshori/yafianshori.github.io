# Website Pribadi — Yafi Anshori

Satu file `index.html`. Tanpa build, tanpa dependency lokal. Bilingual (ID/EN).

## Lihat lokal
Buka `index.html` langsung di browser — selesai.

Atau jalankan server kecil (disarankan agar font & semua aman):
```bash
python3 -m http.server 8765
# buka http://localhost:8765
```

## Cara update konten
Semua teks ada di dalam `index.html`:
- **Teks Indonesia** → langsung di markup HTML.
- **Teks Inggris** → di objek `EN = { ... }` dalam `<script>` (cocokkan `data-i` key-nya).
- **Angka metrik** → atribut `data-target` / `data-dec` / `data-prefix` pada elemen `.count`.
- **Email / link** → cari `mailto:yafians@gmail.com`, `linkedin.com/in/yafianshori`, `github.com/yafianshori`.

## Deploy gratis
**GitHub Pages**
1. Push repo ini ke GitHub.
2. Settings → Pages → Source: `main` / root. Selesai.

**Netlify / Vercel** — drag-drop folder ini, atau connect repo. Tidak perlu build command.

## Live
🔗 **https://yafianshori.github.io/** — di-deploy via GitHub Pages (branch `main`, root).
Setiap `git push` ke `main` otomatis memperbarui situs dalam ~1–2 menit.

## Aset gambar (folder `assets/`)
- `yafi.jpg` — foto profil (About). `yafi-duo.jpg` — foto duotone amber (hero). `yafi-sm.jpg` — cadangan kecil.
- `m-truck.jpg`, `m-logistics.jpg`, `m-warehouse.jpg` — foto mood logistik (duotone). Sumber: Unsplash (lisensi bebas).
- Ganti foto: timpa file di `assets/` dengan nama sama, atau ubah `src` di `index.html`.

## Catatan
- Gaya: Stripe-inspired — gradient aurora mesh, latar terang, aksen indigo (#533afd), Inter 300 (thin) + negative tracking, tabular figures untuk angka.
- Tailwind via CDN (cukup untuk situs statis pribadi).
- Animasi hero mati otomatis bila pengguna mengaktifkan *reduce motion*.
- Semua penghargaan dibingkai sebagai pencapaian produk, bukan klaim pribadi.

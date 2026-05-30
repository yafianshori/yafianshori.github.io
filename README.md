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

## Aset gambar (folder `assets/`)
- `yafi.jpg` — foto profil (About). `yafi-duo.jpg` — foto duotone amber (hero). `yafi-sm.jpg` — cadangan kecil.
- `m-truck.jpg`, `m-logistics.jpg`, `m-warehouse.jpg` — foto mood logistik (duotone). Sumber: Unsplash (lisensi bebas).
- Ganti foto: timpa file di `assets/` dengan nama sama, atau ubah `src` di `index.html`.

## Catatan
- Gaya: Editorial Swiss — grid 12 kolom, Playfair Display × Inter × JetBrains Mono, palet ink + aksen amber.
- Tailwind via CDN (cukup untuk situs statis pribadi).
- Animasi hero mati otomatis bila pengguna mengaktifkan *reduce motion*.
- Semua penghargaan dibingkai sebagai pencapaian produk, bukan klaim pribadi.

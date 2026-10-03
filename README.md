# Rekap Perpustakaan Al-Wafi

Aplikasi web untuk mendata koleksi buku keislaman perpustakaan sekolah.
Banyak anggota bisa mengisi data bersama, dan rekap bisa diunduh sebagai PDF.

**Link aplikasi:** https://claude.ai/artifact/DcQknDHBa78Vwnw1LLbHYS

## Fitur
- Tambah, ubah, hapus buku (judul, penulis, penerbit, tahun, kategori, rak, eksemplar, kondisi, catatan)
- 10 kategori keislaman (Al-Qur'an & Tafsir, Hadits, Fiqih, dll.)
- Cari & filter per kategori
- Ringkasan otomatis: total judul, total eksemplar, jumlah per kategori
- Data tersimpan di database bersama: semua anggota melihat data yang sama secara live
- Kolom "Diinput oleh" untuk melacak siapa yang memasukkan data
- **Download PDF** (A4 landscape) sesuai filter yang aktif, lengkap dengan tanggal Masehi & Hijriah

## Struktur folder
```
.
├── README.md                  ← file ini (ringkasan)
├── app/
│   └── index.html             ← seluruh aplikasi (style + markup + script)
└── docs/
    ├── PANDUAN-PENGGUNA.md    ← cara pakai & cara share ke anggota
    └── ARSITEKTUR.md          ← cara kerja kode & cara mengembangkan
```

## Mulai cepat (untuk developer)
1. Buka `app/index.html` langsung di browser → berjalan di **mode lokal** (data di localStorage, hanya di browser Anda).
2. Ubah kode, refresh browser untuk melihat hasilnya.
3. Untuk update versi online, publish ulang file tersebut sebagai Artifact (link tetap sama).

Baca `docs/ARSITEKTUR.md` sebelum mengubah kode.

# Arsitektur & Panduan Developer

## Gambaran umum
Aplikasi adalah **satu file** `app/index.html`, tanpa build tool. Urutannya:

| Bagian | Isi |
|---|---|
| `<style>` 1–7 | Design tokens (warna, font) → base → header → ringkasan → layout → form → tabel |
| Markup | Header, notice, ringkasan, form, toolbar + tabel |
| `<script>` 1–9 | Lihat tabel di bawah |

### Bagian script
| No | Nama | Tanggung jawab |
|---|---|---|
| 1 | `CONFIG` | Daftar kategori, kondisi, nama sekolah. **Ubah di sini dulu** |
| 2 | `state` | Data di memori: daftar buku, mode edit, cache nama |
| 3 | Util | Format tanggal Masehi/Hijriah, helper `h()` pembuat elemen |
| 4 | Storage adapter | `cloudStore` (database bersama) & `localStore` (localStorage) |
| 5 | Pengguna | Mengambil nama penginput dari id |
| 6 | Render | Ringkasan, tabel, tombol hapus 2 langkah |
| 7 | Form | Baca, validasi, simpan |
| 8 | Ekspor PDF | jsPDF + jspdf-autotable |
| 9 | Start | Pasang event, pilih adapter, cek izin tulis |

## Alur data
```
Form ──submit──▶ validate() ──▶ store.add/update ──▶ Database
                                                       │ (live)
Tabel & Ringkasan ◀── render() ◀── state.books ◀── store.subscribe
```
Halaman **tidak** mengubah tabel langsung saat simpan; tabel selalu digambar ulang dari data yang dikirim database.
Karena itu perubahan dari anggota lain otomatis muncul.

## Model data — collection `books`
| Field | Tipe | Keterangan |
|---|---|---|
| `judul` | string | wajib |
| `penulis`, `penerbit`, `rak`, `catatan` | string | opsional (`rak` disimpan huruf besar) |
| `tahun` | number \| null | 1000–2100 |
| `kategori` | string | salah satu dari `CONFIG.kategori` |
| `eksemplar` | number | ≥ 1 |
| `kondisi` | string | salah satu dari `CONFIG.kondisi` |
| `inputBy` | string | id pengguna penginput (bukan nama) |
| `createdAt`, `updatedAt` | string ISO | untuk urutan terbaru |

## Dua mode berjalan
| Mode | Kapan | Penyimpanan |
|---|---|---|
| Online (Artifact) | Dibuka lewat link claude.ai | Database bersama (`claude.use("db")`) |
| Lokal | File dibuka langsung di browser | `localStorage` (hanya browser itu) |

Pemilihan otomatis di bagian 9 (`start()`), berdasarkan ada/tidaknya `window.claude`.

## Library eksternal
- `jspdf@2.5.1` dan `jspdf-autotable@3.8.2` dari jsDelivr (untuk PDF)
- Font Google: Amiri (judul) & Plus Jakarta Sans (teks)

## Resep pengembangan
- **Tambah kategori**: tambahkan string di `CONFIG.kategori`. Selesai.
- **Tambah kolom baru** (mis. ISBN):
  1. Tambah `<input name="isbn">` di form.
  2. Tambah `isbn: str("isbn")` di `readForm()`.
  3. (Opsional) aturan di `validate()`.
  4. Tampilkan di `renderTable()` dan kolom PDF di `buildPdf()`.
- **Ubah warna**: edit token di `:root` (dan blok dark mode).

## Publish ulang
Publish `app/index.html` sebagai Artifact ke URL yang sama dengan capability:
`{ db: {}, user: { scopes: ["profile"] }, downloads: true }`. Data di database tidak hilang saat publish ulang.

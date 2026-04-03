# Dokumentasi Fitur E-Course - Syuraa

## 1. Struktur Database (Google Sheets)
Tabel `Courses` pada Spreadsheet memiliki kolom-kolom berikut:

| Nama Kolom | Tipe Data | Deskripsi |
|------------|-----------|-----------|
| id | String | ID Unik (CRS-timestamp) |
| title | String | Judul Video (Maks 100 karakter) |
| description| String | Deskripsi (Mendukung format HTML/Rich Text) |
| youtube_url| String | URL YouTube asli |
| video_id | String | 11 karakter ID video YouTube |
| category | String | Kategori untuk pengelompokan |
| status | String | `Published` atau `Draft` |
| thumbnail_url| String | URL Thumbnail (Otomatis atau Manual) |
| created_at | DateTime | Waktu pembuatan (ISO String) |
| updated_at | DateTime | Waktu pembaruan terakhir |
| hits | Number | Jumlah penayangan video |
| is_deleted | Boolean | Flag Soft Delete (`true`/`false`) |
| embed_url | String | URL format embed YouTube |

---

## 2. Dokumentasi API (Google Apps Script)
Seluruh API dipanggil melalui metode `POST` dengan payload JSON.

### Admin Operations
- **`get_courses`**: Mengambil daftar semua course yang tidak dihapus.
- **`save_course`**: Menambah atau memperbarui data course.
  - Payload: `{ id, title, description, youtube_url, video_id, category, status, thumbnail_url, is_edit }`
- **`delete_course`**: Menghapus course secara soft delete.
  - Payload: `{ id }`

### Member Operations
- **`get_member_courses`**: Mengambil daftar course yang dipublish untuk member yang memiliki produk lunas.
  - Payload: `{ email, page, limit }`
- **`track_course_hit`**: Menambah hit counter pada video.
  - Payload: `{ id }`

---

## 3. User Manual untuk Admin

### Menambah E-Course Baru
1. Masuk ke **Admin Area** > Menu **Input E-Course**.
2. Klik tombol **Tambah Video Course**.
3. Masukkan **Judul** (Maks 100 karakter).
4. Pilih atau ketik **Kategori**.
5. Masukkan **YouTube URL**. Sistem akan otomatis mengambil Video ID dan Thumbnail.
6. (Opsional) Masukkan **Thumbnail Khusus** jika tidak ingin menggunakan thumbnail bawaan YouTube.
7. Isi **Deskripsi**. Gunakan toolbar (B, I, Enter) untuk format teks sederhana.
8. Pilih status **Published** untuk langsung menampilkan ke member, atau **Draft** untuk disimpan sementara.
9. Klik **Simpan Course**.

### Mengelola Course
- **Edit**: Klik ikon pensil pada tabel untuk mengubah data.
- **Hapus**: Klik ikon sampah untuk menghapus (data tetap ada di database dengan flag `is_deleted`).
- **Pencarian**: Gunakan kotak pencarian di tabel admin untuk memfilter berdasarkan judul atau kategori.

---

## 4. Keamanan & Optimasi
- **XSS Protection**: Deskripsi dibersihkan dari tag `<script>` dan atribut `on*` di sisi server.
- **Lazy Loading**: Gambar thumbnail menggunakan atribut `loading="lazy"`.
- **Pagination**: Daftar course di halaman member dibatasi 6 per halaman untuk performa optimal.
- **Responsive**: UI menggunakan Tailwind CSS yang adaptif untuk Mobile, Tablet, dan Desktop.

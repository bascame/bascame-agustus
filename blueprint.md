# Blueprint: Panel Keuangan HUT RI ke-80

## Ringkasan Proyek

Aplikasi ini adalah panel admin berbasis web untuk mengelola dan memantau keuangan sebuah acara (HUT RI ke-80). Aplikasi ini memungkinkan pengguna dengan peran berbeda (admin, editor, visitor) untuk melihat, menambah, mengedit, dan menghapus data transaksi keuangan (uang masuk dan uang keluar) secara real-time menggunakan Firebase.

---

## Fitur dan Desain yang Diimplementasikan

### 1. Otentikasi dan Manajemen Peran
- **Login Aman:** Menggunakan Firebase Authentication (Email & Password) untuk login admin dan editor.
- **Login Visitor:** Terdapat opsi untuk masuk sebagai "visitor" dengan akses hanya-baca (read-only).
- **Manajemen Peran Berbasis Firestore:** Peran pengguna (`admin`, `editor`, `visitor`) disimpan dalam koleksi `users` di Firestore, di mana ID dokumen sesuai dengan UID pengguna dari Firebase Authentication.
- **Manajemen Sesi:** Menggunakan `onAuthStateChanged` untuk mengelola sesi login dan logout secara otomatis.

### 2. Antarmuka Pengguna (UI) dan Fungsionalitas
- **Dashboard Utama:** Menampilkan ringkasan total uang masuk, total uang keluar, dan saldo akhir.
- **Manajemen Data:** Halaman terpisah untuk "Uang Masuk" dan "Uang Keluar" dengan tabel data dan form untuk input.
- **UI Adaptif Berdasarkan Peran:**
  - **Admin:** Akses penuh (CRUD - Create, Read, Update, Delete) ke semua data. Dapat menyetujui atau menolak permintaan penghapusan dari editor.
  - **Editor:** Dapat membuat, membaca, dan mengedit data. Penghapusan data memerlukan persetujuan dari admin (data ditandai `pending_deletion`).
  - **Visitor:** Hanya dapat melihat data (read-only), tidak bisa mengubah atau menambah data.
- **Real-time Update:** Data di tabel dan dashboard diperbarui secara otomatis saat ada perubahan di database Firestore berkat `onSnapshot`.
- **Pencarian Data:** Fitur pencarian real-time di halaman "Uang Masuk" dan "Uang Keluar" untuk memfilter data berdasarkan keterangan.
- **Desain Modern:** Menggunakan Tailwind CSS untuk tata letak yang bersih, responsif, dan modern. Dilengkapi dengan indikator loading untuk memberikan feedback visual kepada pengguna.

### 3. Alur Kerja CRUD (Create, Read, Update, Delete)
- **Create/Update:** Admin dan editor dapat menambah atau mengedit data melalui form yang tersedia.
- **Delete Workflow:**
  - **Editor:** Saat menekan "Hapus", status data diubah menjadi `pending_deletion`.
  - **Admin:** Melihat data yang `pending_deletion` dan memiliki opsi untuk "Setujui" (menghapus permanen) atau "Tolak" (mengembalikan status ke `active`).

---
 
## Rencana Saat Ini: Menambahkan Fitur Pencarian

Rencana ini mengimplementasikan fungsionalitas pencarian untuk meningkatkan usabilitas aplikasi.

1.  **Menambahkan Input Pencarian:** Menambahkan elemen `<input>` di atas tabel "Uang Masuk" dan "Uang Keluar" pada file `index.html`.
2.  **Implementasi Logika Filter:**
    -   Membuat variabel state baru (`searchTermMasuk`, `searchTermKeluar`) untuk menyimpan query pencarian.
    -   Menambahkan event listener `input` pada kolom pencarian untuk memperbarui state dan memicu render ulang tabel.
    -   Memodifikasi fungsi `renderTableMasuk` dan `renderTableKeluar` untuk memfilter data berdasarkan `searchTerm` sebelum menampilkannya.
3.  **Memperbarui Dokumentasi:** Mencatat fitur baru ini di dalam file `blueprint.md`.

Fitur pencarian sekarang sudah terintegrasi dan berfungsi dengan baik.
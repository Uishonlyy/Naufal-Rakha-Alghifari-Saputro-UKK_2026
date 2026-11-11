# Debugging

Dokumen ini berisi kendala yang ditemui selama pembuatan aplikasi **Peminjaman Buku**, serta kendala umum yang perlu diantisipasi beserta cara mengatasinya.

## A. Masalah yang Dialami Selama Pengembangan

### 1. Error saat menjalankan Perancangan Algoritma CRUD 

| | |
|---|---|
| **Gejala** | Stok buku pada database kadang menjadi minus atau tidak sinkron ketika transaksi peminjaman dilakukan secara bersamaan, atau siswa bisa meminjam buku yang stoknya sudah habis. |
| **Penyebab** | Kurangnya validasi kondisi stok (stock > 0) sebelum query insert dijalankan, serta proses pengurangan stok dan pencatatan transaksi tidak dilakukan secara atomik. |
| **Solusi** | Menambahkan pengecekan logika kondisi stok di Controller/Filament Action serta membungkus proses pembuatan data peminjaman dan pembaruan stok menggunakan DB::transaction() agar eksekusi database aman dan sinkron. |
| **Hasil** | Validasi stok berjalan akurat, peminjaman buku yang habis berhasil ditolak oleh sistem, dan stok berkurang secara otomatis dengan konsisten. |

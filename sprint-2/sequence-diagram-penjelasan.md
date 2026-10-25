# Penjelasan Sequence Diagram — PinjamKuy

> Diagram untuk tiap skenario ada di file terpisah: [sequence-diagram.md](sequence-diagram.md)

### Peserta yang muncul di diagram
| Peserta | Jenis | Peran |
|---|---|---|
| Peminjam | Aktor | Pengguna yang mencari dan meminjam barang |
| Pemilik Barang | Aktor | Pengguna yang memiliki barang: verifikasi pembayaran dan penyelesaian peminjaman |
| Admin | Aktor | Pengguna dengan role Admin, diarahkan ke dashboard admin saat login |
| Antarmuka Web | Boundary | Halaman yang dilihat dan dipakai pengguna |
| Backend | Control | Menjalankan aturan bisnis dan mengatur alur proses |
| Database | Entity | Menyimpan data (user, kategori, barang, peminjaman, pembayaran, pesan, ulasan) |
| Payment Gateway | Sistem eksternal | Layanan pembayaran pihak ketiga |
| Fitur Pesan | Sistem internal | Pengirim notifikasi ke peminjam lewat kelas `Pesan` |

### 3.1 Skenario A: Peminjaman dan Pembayaran oleh Peminjam

**Tujuan.** Menunjukkan bagaimana peminjam mengajukan peminjaman barang dan mengirim bukti pembayaran.

**Prasyarat.** Peminjam sudah login dan sedang membuka detail barang yang ingin dipinjam.

**Alur utama (barang tersedia)**
1. Langkah 1-2: peminjam memilih tanggal mulai dan tanggal selesai, menekan Ajukan, lalu antarmuka mengirim data barang dan tanggal ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah barang masih berstatus Tersedia (`cekKetersediaan`). Pengecekan ini mencegah satu barang dipinjam dua orang pada waktu yang sama.
3. Langkah 5-6: karena tersedia, backend menyimpan `Peminjaman` dengan status Menunggu pembayaran, lalu mengubah status barang menjadi Dipesan agar tidak bisa diajukan orang lain.
4. Langkah 7-8: backend meminta instruksi pembayaran ke Payment Gateway dan menerima balasannya.
5. Langkah 9-10: peminjaman berstatus Menunggu pembayaran, dan peminjam melihat instruksi serta total biaya yang harus dibayar.
6. Langkah 11-13: peminjam membayar, mengunggah bukti, dan backend menyimpan data `Pembayaran` dengan status Menunggu verifikasi.
7. Langkah 14-15: sistem memberi tahu peminjam bahwa bukti sudah terkirim.

**Alur alternatif (barang tidak tersedia).** Langkah 16-17: backend menolak pengajuan karena barang sedang dipinjam atau sudah dipesan orang lain, lalu peminjam diminta memilih barang atau tanggal lain. Tidak ada data peminjaman yang tersimpan.

**Hasil.** Peminjaman tersimpan dan menunggu verifikasi pemilik. Status peminjaman berubah dari *Menunggu pembayaran* ke *Menunggu verifikasi*, dan status barang menjadi *Dipesan*.

**Terkait:** class Peminjaman, Barang, Pembayaran, User (method `ajukan()` dan `cekKetersediaan()`).

### 3.2 Skenario B: Verifikasi Pembayaran dan Penyelesaian Peminjaman oleh Pemilik Barang

**Tujuan.** Menunjukkan proses pemilik memeriksa pembayaran lalu menyelesaikan peminjaman setelah barang dikembalikan, sehingga transaksi selesai.

**Prasyarat.** Pemilik sudah login dan ada pembayaran berstatus Menunggu verifikasi (hasil dari Skenario A).

**Alur utama**
1. Langkah 1-5: pemilik membuka daftar pembayaran masuk, dan backend mengambil data pembayaran berstatus menunggu atas barang miliknya dari database lalu menampilkannya.
2. Langkah 6-7: pemilik memeriksa bukti transfer, menekan Verifikasi, dan antarmuka mengirim permintaan ke backend.
3. Langkah 8-10: backend mengubah status pembayaran menjadi Terverifikasi, status peminjaman menjadi Disetujui (`setujui()`), dan status barang menjadi Dipinjam.
4. Langkah 11-12: backend meminta Fitur Pesan mengirim notifikasi, dan peminjam menerima pesan bahwa pembayarannya terverifikasi dan peminjaman disetujui.
5. Langkah 13-16: setelah barang dikembalikan, pemilik menekan Selesaikan. Backend mengubah status peminjaman menjadi Selesai (`selesaikan()`) dan status barang menjadi Tersedia.
6. Langkah 17-19: backend mengirim pesan penutup ke peminjam berisi ajakan memberi ulasan, lalu pemilik mendapat konfirmasi berhasil.

**Alur alternatif.** Jika bukti tidak valid, pemilik menekan Tolak (`tolak()`). Pembayaran tidak diverifikasi dan peminjam diminta mengunggah ulang bukti. Alur ini tidak digambar agar diagram tetap ringkas.

**Hasil.** Barang sudah kembali, peminjaman Selesai, dan barang tercatat Tersedia sehingga bisa dipinjam lagi.

**Terkait:** class Peminjaman (`setujui()`, `tolak()`, `selesaikan()`), Pembayaran, Barang, Pesan (`kirim()`).

### 3.3 Skenario C: Registrasi Pengguna

**Tujuan.** Menunjukkan proses pengunjung membuat akun pengguna.

**Prasyarat.** Pengunjung belum memiliki akun dan membuka halaman registrasi.

**Alur utama (email belum terdaftar)**
1. Langkah 1-2: pengunjung mengisi nama, email, nomor HP, dan password, lalu data dikirim ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah email sudah dipakai.
3. Langkah 5: karena belum ada, backend menyimpan user baru. Password disimpan dalam bentuk hash, bukan teks asli, demi keamanan.
4. Langkah 6-7: sistem memberi tahu registrasi berhasil dan mengarahkan pengguna ke halaman login.

**Alur alternatif (email sudah terdaftar).** Langkah 8-9: backend menolak registrasi dan menampilkan pesan bahwa email sudah dipakai, disertai saran untuk login.

**Hasil.** Satu data user tersimpan dan siap dipakai untuk login (Skenario D).

**Terkait:** class User (method `register()`).

### 3.4 Skenario D: Login (Pengguna dan Admin)

**Tujuan.** Menunjukkan proses autentikasi dan pengarahan pengguna ke halaman yang sesuai role, baik User maupun Admin.

**Prasyarat.** Pengguna sudah terdaftar.

**Alur utama (kredensial valid)**
1. Langkah 1-2: pengguna mengisi email dan password, lalu data dikirim ke backend.
2. Langkah 3-4: backend mengambil data user dari database, yaitu hash password dan role.
3. Langkah 5: backend membandingkan password dengan hash. Jika cocok, backend mengirim token sesi beserta role.
4. Langkah 6 atau 7: antarmuka memakai role itu untuk menentukan tujuan. Admin diarahkan ke dashboard admin (langkah 6), sedangkan User ke beranda katalog barang (langkah 7).

**Alur alternatif (kredensial salah).** Langkah 8-9: backend menolak login dan antarmuka menampilkan pesan "email atau password salah". Pesan sengaja tidak menyebut mana yang salah agar tidak membantu penyerang menebak akun.

**Hasil.** Pengguna memiliki sesi aktif dan hanya melihat menu sesuai role-nya.

**Terkait:** class User (method `login()` dan atribut `role`).

### 3.5 Skenario E: Beri Ulasan

**Tujuan.** Menunjukkan proses peminjam memberi ulasan, dengan aturan bahwa hanya peminjam yang peminjamannya sudah selesai yang boleh mengulas.

**Prasyarat.** Peminjam sudah login dan memiliki riwayat peminjaman.

**Alur utama (ulasan diizinkan)**
1. Langkah 1-2: peminjam membuka riwayat peminjaman, memilih Beri ulasan, lalu mengirim peminjaman, rating, dan komentar ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah peminjaman itu milik peminjam tersebut dan berapa statusnya.
3. Langkah 5-6: karena peminjaman berstatus Selesai dan belum pernah diulas, backend menyimpan ulasan lalu mengonfirmasi ke antarmuka.
4. Langkah 7: ulasan tampil di halaman detail barang.

**Alur alternatif (ulasan ditolak).** Langkah 8-9: jika peminjaman belum selesai atau sudah pernah diulas, backend menolak dan antarmuka menampilkan alasannya. Aturan ini menjaga ulasan tetap jujur dan mencegah ulasan ganda, sesuai relasi satu peminjaman paling banyak satu ulasan (0..1) pada class diagram.

**Hasil.** Satu data Ulasan baru tersimpan dan terhubung ke peminjamannya.

**Terkait:** class Ulasan (`simpan()`), Peminjaman, Barang; peminjaman harus berstatus Selesai.

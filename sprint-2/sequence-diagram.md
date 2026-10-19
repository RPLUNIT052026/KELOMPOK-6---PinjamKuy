# 🔁 Sequence Diagram PinjamKuy

**Tujuan:** memahami urutan komunikasi dalam suatu skenario. Sequence diagram menunjukkan siapa mengirim pesan ke siapa, dan dalam urutan apa.

## 1. Skenario Login

```mermaid
sequenceDiagram
    autonumber
    actor Pengguna
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL

    Pengguna->>UI: Isi email dan password
    UI->>Server: POST login.php
    Server->>DB: Cari pengguna berdasarkan email
    DB-->>Server: Data pengguna dan hash password
    Server->>Server: Verifikasi password

    alt Password benar
        Server->>Server: Buat sesi login
        Server-->>UI: Arahkan ke katalog
        UI-->>Pengguna: Tampilkan katalog barang
    else Password salah
        Server-->>UI: Pesan email atau password salah
        UI-->>Pengguna: Tampilkan pesan error
    end
```

## 2. Skenario Pengajuan Peminjaman

```mermaid
sequenceDiagram
    autonumber
    actor Peminjam
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL
    actor Pemilik

    Peminjam->>UI: Pilih barang dan tanggal pinjam
    UI->>Server: POST ajukan pinjam
    Server->>DB: Cek status barang
    DB-->>Server: Status barang

    alt Barang tersedia
        Server->>DB: Simpan peminjaman dengan status menunggu
        DB-->>Server: Berhasil disimpan
        Server-->>UI: Pengajuan terkirim
        UI-->>Peminjam: Tampilkan konfirmasi
        Server-->>Pemilik: Pengajuan baru masuk
        Pemilik->>Server: Setujui atau tolak
        Server->>DB: Perbarui status peminjaman
        DB-->>Server: Berhasil diperbarui
        Server-->>Peminjam: Status pengajuan diperbarui
    else Barang sedang dipinjam
        Server-->>UI: Pesan barang tidak tersedia
        UI-->>Peminjam: Tampilkan pesan
    end
```

## 3. Skenario Melihat Katalog dan Mencari Barang

```mermaid
sequenceDiagram
    autonumber
    actor Pengguna
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL

    Pengguna->>UI: Buka halaman katalog
    UI->>Server: GET katalog.php
    Server->>DB: Ambil daftar barang dan kategori
    DB-->>Server: Data barang
    Server-->>UI: Halaman katalog
    UI-->>Pengguna: Tampilkan kartu barang

    Pengguna->>UI: Ketik kata kunci dan pilih kategori
    UI->>Server: GET katalog.php dengan parameter pencarian
    Server->>DB: Query barang sesuai filter
    DB-->>Server: Hasil pencarian
    Server-->>UI: Halaman katalog terfilter
    UI-->>Pengguna: Tampilkan hasil pencarian
```

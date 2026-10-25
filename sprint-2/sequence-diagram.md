# 🔁 Sequence Diagram PinjamKuy

**Tujuan:** memahami urutan komunikasi dalam suatu skenario. Sequence diagram menunjukkan siapa mengirim pesan ke siapa, dan dalam urutan apa.

## 1. Skenario A: Login Pengguna

```mermaid
sequenceDiagram
    autonumber
    actor P as Pengguna
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL

    P->>UI: Buka halaman login
    P->>UI: Isi email dan password
    UI->>Server: POST login.php (email, password)
    Server->>DB: Cari pengguna berdasarkan email
    DB-->>Server: Data pengguna dan hash password

    alt Pengguna ditemukan
        Server->>Server: Verifikasi password

        alt Password benar
            Server->>Server: Buat sesi login
            Server-->>UI: Login berhasil, redirect ke katalog
            UI-->>P: Tampilkan katalog barang
        else Password salah
            Server-->>UI: Error: email atau password salah
            UI-->>P: Tampilkan pesan error
        end

    else Pengguna tidak ditemukan
        Server-->>UI: Error: email atau password salah
        UI-->>P: Tampilkan pesan error
    end

## 2. Skenario Pengajuan Peminjaman

```mermaid
sequenceDiagram
    autonumber
    actor P as Peminjam
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL
    actor O as Pemilik

    P->>UI: Pilih barang dan tanggal pinjam
    UI->>Server: POST ajukan-pinjam.php (barang, tanggal)
    Server->>DB: Cek status dan ketersediaan barang
    DB-->>Server: Status barang

    alt Barang tersedia
        Server->>DB: Simpan pengajuan (status Menunggu)
        DB-->>Server: Pengajuan berhasil disimpan
        Server-->>UI: Pengajuan peminjaman terkirim
        UI-->>P: Tampilkan konfirmasi pengajuan

        Server-->>O: Notifikasi pengajuan peminjaman baru
        O->>UI: Buka daftar pengajuan
        UI->>Server: GET pengajuan-pemilik.php
        Server->>DB: Ambil daftar pengajuan
        DB-->>Server: Daftar pengajuan
        Server-->>UI: Tampilkan daftar pengajuan
        UI-->>O: Tampilkan detail pengajuan

        O->>UI: Pilih setujui atau tolak
        UI->>Server: POST keputusan pengajuan
        Server->>DB: Periksa status pengajuan terkini

        alt Pengajuan masih Menunggu
            alt Pengajuan disetujui
                Server->>DB: Ubah status menjadi Disetujui
                DB-->>Server: Status berhasil diperbarui
                Server-->>UI: Persetujuan berhasil
                Server-->>P: Notifikasi pengajuan disetujui
            else Pengajuan ditolak
                Server->>DB: Ubah status menjadi Ditolak
                DB-->>Server: Status berhasil diperbarui
                Server-->>UI: Penolakan berhasil
                Server-->>P: Notifikasi pengajuan ditolak
            end
        else Pengajuan sudah diproses
            Server-->>UI: Pengajuan sudah diproses
        end

    else Barang tidak tersedia
        Server-->>UI: Error: barang tidak tersedia
        UI-->>P: Tampilkan pesan barang tidak tersedia
    end

## 3. Skenario Melihat Katalog dan Mencari Barang

```mermaid
sequenceDiagram
    autonumber
    actor P as Pengguna
    participant UI as Browser
    participant Server as Server PHP
    participant DB as Database MySQL

    P->>UI: Buka halaman katalog
    UI->>Server: GET katalog.php
    Server->>DB: Ambil daftar barang dan kategori
    DB-->>Server: Data barang dan kategori
    Server-->>UI: Halaman katalog
    UI-->>P: Tampilkan kartu barang dan kategori

    P->>UI: Ketik kata kunci dan pilih kategori
    UI->>Server: GET katalog.php (kata kunci, kategori)
    Server->>DB: Cari barang sesuai kata kunci dan kategori
    DB-->>Server: Hasil pencarian

    alt Barang ditemukan
        Server-->>UI: Tampilkan hasil pencarian
        UI-->>P: Tampilkan barang sesuai filter
    else Barang tidak ditemukan
        Server-->>UI: Hasil pencarian kosong
        UI-->>P: Tampilkan pesan barang tidak ditemukan
    end                    

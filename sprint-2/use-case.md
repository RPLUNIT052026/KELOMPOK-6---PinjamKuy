# 🎯 Use Case Diagram PinjamKuy

**Tujuan:** memahami tujuan dan interaksi aktor dengan sistem. Use case diagram menunjukkan apa saja yang bisa dilakukan tiap aktor terhadap sistem.

## Diagram

```mermaid
@startuml
title Use Case Diagram - PinjamKuy

left to right direction
skinparam backgroundColor #121212
skinparam shadowing false
skinparam defaultFontName Arial
skinparam defaultFontColor #EEEEEE
skinparam ArrowColor #AAAAAA
skinparam actor {
    Color #DDDDDD
    BorderColor #DDDDDD
}
skinparam usecase {
    BackgroundColor #1F2D16
    BorderColor #739A36
    FontColor #EEEEEE
}
skinparam rectangle {
    BorderColor #739A36
    FontColor #EEEEEE
}

actor "Pengguna" as User
actor "Peminjam" as Borrower
actor "Pemilik Barang" as Owner
actor "Admin" as Admin

rectangle "Sistem Website PinjamKuy" {
    usecase "Registrasi" as UC_Register
    usecase "Login" as UC_Login
    usecase "Logout" as UC_Logout
    usecase "Lihat Katalog Barang" as UC_Catalog
    usecase "Cari dan Filter Barang" as UC_Search
    usecase "Lihat Detail Barang" as UC_Detail
    usecase "Lihat Riwayat Peminjaman" as UC_History
    usecase "Chat" as UC_Chat

    usecase "Ajukan Peminjaman" as UC_Request
    usecase "Beri Ulasan dan Rating" as UC_Review

    usecase "Tambah dan Kelola Barang" as UC_ManageItems
    usecase "Setujui atau Tolak Pengajuan" as UC_Approve
    usecase "Konfirmasi Pengembalian" as UC_Return

    usecase "Kelola Pengguna" as UC_ManageUsers
    usecase "Kelola Kategori" as UC_ManageCategories
}

' Generalisasi aktor
Borrower --|> User
Owner --|> User

' Relasi Pengguna
User -- UC_Register
User -- UC_Login
User -- UC_Logout
User -- UC_Catalog
User -- UC_Detail
User -- UC_History
User -- UC_Chat

' Relasi Peminjam
Borrower -- UC_Request
Borrower -- UC_Review

' Relasi Pemilik Barang
Owner -- UC_ManageItems
Owner -- UC_Approve
Owner -- UC_Return

' Relasi Admin
Admin -- UC_ManageUsers
Admin -- UC_ManageCategories

' Relasi include dan extend
UC_Request ..> UC_Login : <<include>>
UC_Review ..> UC_Login : <<include>>
UC_ManageItems ..> UC_Login : <<include>>
UC_Search ..> UC_Catalog : <<extend>>

@enduml
```

> Mermaid tidak punya diagram use case bawaan, jadi aktor dan use case digambar dengan flowchart. Garis lurus adalah hubungan aktor dengan use case, garis putus-putus adalah relasi `include` dan `extend`.

## Daftar Aktor

| Aktor | Keterangan |
|-------|------------|
| Pengguna | Aktor umum, semua pengguna yang sudah punya akun |
| Peminjam | Pengguna yang meminjam barang |
| Pemilik Barang | Pengguna yang memasang barang untuk dipinjamkan |
| Admin | Pengelola sistem |

## Deskripsi Use Case Utama

### UC-02 Login

| Bagian | Isi |
|--------|-----|
| Aktor | Pengguna |
| Prakondisi | Pengguna sudah punya akun |
| Alur utama | 1. Pengguna membuka halaman login. 2. Mengisi email dan password. 3. Sistem memeriksa data. 4. Sistem membuka halaman katalog. |
| Alur alternatif | Data salah: sistem menampilkan pesan error dan pengguna mengisi ulang. |
| Pascakondisi | Pengguna masuk dan sesi tersimpan |

### UC-07 Ajukan Peminjaman

| Bagian | Isi |
|--------|-----|
| Aktor | Peminjam |
| Prakondisi | Peminjam sudah login dan barang berstatus tersedia |
| Alur utama | 1. Peminjam membuka detail barang. 2. Memilih tanggal mulai dan selesai. 3. Menekan tombol ajukan. 4. Sistem menyimpan pengajuan dengan status menunggu. 5. Pemilik menerima pengajuan. |
| Alur alternatif | Barang sedang dipinjam: sistem menolak pengajuan dan menampilkan pesan. |
| Pascakondisi | Pengajuan tersimpan dengan status menunggu |

### UC-11 Setujui atau Tolak Pengajuan

| Bagian | Isi |
|--------|-----|
| Aktor | Pemilik Barang |
| Prakondisi | Ada pengajuan berstatus menunggu |
| Alur utama | 1. Pemilik membuka daftar pengajuan. 2. Memilih pengajuan. 3. Menekan setujui atau tolak. 4. Sistem memperbarui status. |
| Pascakondisi | Status pengajuan menjadi disetujui atau ditolak |

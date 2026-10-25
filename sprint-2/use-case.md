# 🎯 Use Case Diagram PinjamKuy

**Tujuan:** memahami tujuan dan interaksi aktor dengan sistem. Use case diagram menunjukkan apa saja yang bisa dilakukan tiap aktor terhadap sistem.

## Diagram

```mermaid
%%{init: {"theme": "base", "flowchart": {"curve": "linear", "nodeSpacing": 30, "rankSpacing": 70}, "themeVariables": {"fontFamily": "Arial", "lineColor": "#888888", "edgeLabelBackground": "#121212"}}}%%
flowchart LR
    PG(["Pengguna"])
    PM(["Peminjam"])
    PB(["Pemilik Barang"])
    AD(["Admin"])

    PM -->|"generalisasi"| PG
    PB -->|"generalisasi"| PG

    subgraph SISTEM["Sistem Website PinjamKuy"]
        subgraph UMUM["Fitur Umum"]
            UC1(["Registrasi"])
            UC2(["Login"])
            UC3(["Logout"])
            UC4(["Lihat Katalog Barang"])
            UC6(["Lihat Detail Barang"])
            UC8(["Lihat Riwayat Peminjaman"])
            UC13(["Chat"])
        end
        subgraph PINJAM["Fitur Peminjam"]
            UC7(["Ajukan Peminjaman"])
            UC9(["Beri Ulasan dan Rating"])
        end
        subgraph PEMILIK["Fitur Pemilik Barang"]
            UC10(["Tambah dan Kelola Barang"])
            UC11(["Setujui atau Tolak Pengajuan"])
            UC12(["Konfirmasi Pengembalian"])
        end
        subgraph ADMIN["Fitur Admin"]
            UC14(["Kelola Pengguna"])
            UC15(["Kelola Kategori"])
        end
        UC5(["Cari dan Filter Barang"])
    end

    PG --- UC1 & UC2 & UC3 & UC4 & UC6 & UC8 & UC13
    PM --- UC7 & UC9
    PB --- UC10 & UC11 & UC12
    AD --- UC14 & UC15

    UC5 -.->|"«extend»"| UC4
    UC7 -.->|"«include»"| UC2
    UC9 -.->|"«include»"| UC2
    UC10 -.->|"«include»"| UC2

    classDef actor fill:#121212,stroke:#DDDDDD,color:#EEEEEE,stroke-width:2px
    classDef usecase fill:#1F2D16,stroke:#739A36,color:#EEEEEE
    classDef group fill:#181818,stroke:#4A6324,color:#CCCCCC,stroke-dasharray:4 3

    class PG,PM,PB,AD actor
    class UC1,UC2,UC3,UC4,UC5,UC6,UC7,UC8,UC9,UC10,UC11,UC12,UC13,UC14,UC15 usecase
    class UMUM,PINJAM,PEMILIK,ADMIN group
    style SISTEM fill:#121212,stroke:#739A36,color:#EEEEEE,stroke-width:2px
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

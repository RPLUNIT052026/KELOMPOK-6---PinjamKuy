# 🧱 Class Diagram PinjamKuy

**Tujuan:** memahami struktur statis dan relationship antar-entity. Class diagram menunjukkan kelas-kelas dalam sistem, atribut dan fungsinya, serta hubungan antar kelas. Kelas-kelas ini sejalan dengan tabel di [ERD](../sprint-1/erd.md).

## Diagram

```mermaid
classDiagram
    class User {
        +int id
        +String nama
        +String email
        +String password
        +String noHp
        +String role
        +DateTime createdAt
        +register()
        +login()
        +logout()
        +editProfil()
    }

    class Kategori {
        +int id
        +String namaKategori
        +tambahKategori()
        +ubahKategori()
    }

    class Barang {
        +int id
        +String namaBarang
        +String deskripsi
        +String kondisi
        +int hargaPerHari
        +String foto
        +String status
        +tambahBarang()
        +ubahBarang()
        +hapusBarang()
        +cekKetersediaan()
    }

    class Peminjaman {
        +int id
        +Date tanggalMulai
        +Date tanggalSelesai
        +String status
        +DateTime createdAt
        +ajukan()
        +setujui()
        +tolak()
        +selesaikan()
    }

    class Pesan {
        +int id
        +String isiPesan
        +DateTime createdAt
        +kirim()
    }

    class Ulasan {
        +int id
        +int rating
        +String komentar
        +simpan()
    }

    User "1" --> "0..*" Barang : memiliki
    Kategori "1" --> "0..*" Barang : mengelompokkan
    User "1" --> "0..*" Peminjaman : meminjam
    Barang "1" --> "0..*" Peminjaman : dipinjam dalam
    Peminjaman "1" --> "0..1" Ulasan : diulas
    User "1" --> "0..*" Pesan : mengirim
    User "1" --> "0..*" Pesan : menerima
```

## Penjelasan Relasi

| Relasi | Arti |
|--------|------|
| User 1 ke banyak Barang | Satu pengguna bisa memiliki banyak barang |
| Kategori 1 ke banyak Barang | Satu kategori berisi banyak barang |
| User 1 ke banyak Peminjaman | Satu pengguna bisa melakukan banyak peminjaman |
| Barang 1 ke banyak Peminjaman | Satu barang bisa dipinjam berkali-kali di waktu berbeda |
| Peminjaman 1 ke 0 atau 1 Ulasan | Satu peminjaman punya paling banyak satu ulasan |
| User 1 ke banyak Pesan | Satu pengguna bisa mengirim dan menerima banyak pesan |

## Keterangan Simbol

| Simbol | Arti |
|--------|------|
| `+` | Atribut atau fungsi bersifat public |
| `1` | Tepat satu |
| `0..*` | Nol atau lebih |
| `0..1` | Nol atau satu |

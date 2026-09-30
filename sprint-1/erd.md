# 🗄️ ERD PinjamKuy

ERD (Entity Relationship Diagram) adalah rancangan tabel database beserta hubungan antar tabelnya.

## Diagram

```mermaid
erDiagram
    USERS ||--o{ ITEMS : "memiliki"
    USERS ||--o{ LOANS : "meminjam"
    CATEGORIES ||--o{ ITEMS : "mengelompokkan"
    ITEMS ||--o{ LOANS : "dipinjam dalam"
    USERS ||--o{ MESSAGES : "mengirim"
    LOANS ||--o| REVIEWS : "diulas"

    USERS {
        int id PK
        string nama
        string email
        string password
        string no_hp
        string role
        datetime created_at
    }

    CATEGORIES {
        int id PK
        string nama_kategori
    }

    ITEMS {
        int id PK
        int owner_id FK
        int category_id FK
        string nama_barang
        text deskripsi
        string kondisi
        int harga_per_hari
        string foto
        string status
    }

    LOANS {
        int id PK
        int item_id FK
        int borrower_id FK
        date tanggal_mulai
        date tanggal_selesai
        string status
        datetime created_at
    }

    MESSAGES {
        int id PK
        int sender_id FK
        int receiver_id FK
        text isi_pesan
        datetime created_at
    }

    REVIEWS {
        int id PK
        int loan_id FK
        int rating
        text komentar
    }
```

## Penjelasan Tabel

| Tabel | Fungsi |
|-------|--------|
| `USERS` | Menyimpan data pengguna (pemilik barang dan peminjam). Kolom `role` berisi `user` atau `admin`. |
| `CATEGORIES` | Kategori barang, misalnya Elektronik, Outdoor, Buku. |
| `ITEMS` | Barang yang dipasang pemilik untuk dipinjamkan. `status` berisi `tersedia` atau `dipinjam`. |
| `LOANS` | Data pengajuan dan transaksi peminjaman. `status` berisi `menunggu`, `disetujui`, `ditolak`, `dipinjam`, atau `selesai`. |
| `MESSAGES` | Chat antar pengguna. |
| `REVIEWS` | Rating dan ulasan setelah peminjaman selesai. |

## Penjelasan Hubungan

- Satu **user** bisa memiliki banyak **barang** (1 ke banyak).
- Satu **kategori** berisi banyak **barang** (1 ke banyak).
- Satu **user** bisa punya banyak **peminjaman** (1 ke banyak).
- Satu **barang** bisa dipinjam berkali-kali, di waktu yang berbeda (1 ke banyak).
- Satu **peminjaman** punya paling banyak satu **ulasan** (1 ke 0 atau 1).

## Catatan Sprint 1

Tabel yang dipakai di Sprint 1 adalah `USERS`, `CATEGORIES`, dan `ITEMS` (untuk registrasi, login, katalog, dan detail barang). Tabel `LOANS`, `MESSAGES`, dan `REVIEWS` dipakai di sprint berikutnya.

**Keterangan:** PK = Primary Key (penanda unik tiap baris), FK = Foreign Key (penghubung ke tabel lain).

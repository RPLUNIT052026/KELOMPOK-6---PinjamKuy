# Sequence Diagram — PinjamKuy

Sequence diagram menggambarkan **urutan pesan antar peserta** dalam satu skenario, dibaca dari atas ke bawah. Nomor pada tiap panah (autonumber) menunjukkan urutan kejadian. Panah penuh adalah permintaan, panah putus-putus adalah balasan, dan kotak `alt` adalah percabangan kondisi.

> Seluruh peserta, data, dan fungsi mengacu pada [Class Diagram PinjamKuy](class-diagram.md): `User`, `Kategori`, `Barang`, `Peminjaman`, `Pesan`, dan `Ulasan`, ditambah kelas `Pembayaran` yang perlu dimasukkan ke class diagram (relasi: `Peminjaman` 1 ke 0..* `Pembayaran`).

### 3.1 Skenario A: Peminjaman dan Pembayaran oleh Peminjam
```mermaid
sequenceDiagram
    autonumber
    actor P as Peminjam
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database
    participant PG as Payment Gateway

    P->>UI: Buka detail barang, pilih tanggal, tekan Ajukan
    UI->>API: POST /peminjaman (barangId, tanggalMulai, tanggalSelesai)
    API->>DB: Cek status barang (cekKetersediaan)
    DB-->>API: Status barang
    alt Barang tersedia
        API->>DB: Simpan Peminjaman, status Menunggu pembayaran
        API->>DB: Ubah status barang menjadi Dipesan
        API->>PG: Buat permintaan pembayaran
        PG-->>API: Instruksi pembayaran
        API-->>UI: Pengajuan dibuat (Menunggu pembayaran)
        UI-->>P: Tampilkan instruksi dan total biaya
        P->>UI: Upload bukti pembayaran
        UI->>API: POST /pembayaran (peminjamanId, bukti)
        API->>DB: Simpan Pembayaran, status Menunggu verifikasi
        API-->>UI: Berhasil
        UI-->>P: Bukti terkirim, menunggu verifikasi
    else Barang tidak tersedia
        API-->>UI: Error: barang sedang tidak tersedia
        UI-->>P: Pesan error dan saran pilih barang atau tanggal lain
    end
```

### 3.2 Skenario B: Verifikasi Pembayaran dan Penyelesaian Peminjaman oleh Pemilik Barang
```mermaid
sequenceDiagram
    autonumber
    actor O as Pemilik Barang
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database
    participant PS as Fitur Pesan
    actor P as Peminjam

    O->>UI: Buka daftar pembayaran masuk
    UI->>API: GET /pembayaran/masuk?status=menunggu
    API->>DB: Ambil data pembayaran atas barang milik pemilik
    DB-->>API: Daftar pembayaran
    API-->>UI: Daftar pembayaran
    O->>UI: Periksa bukti, tekan Verifikasi
    UI->>API: PATCH /pembayaran/{id} (valid)
    API->>DB: Ubah status pembayaran menjadi Terverifikasi
    API->>DB: Ubah status peminjaman menjadi Disetujui (setujui)
    API->>DB: Ubah status barang menjadi Dipinjam
    API->>PS: Kirim notifikasi ke peminjam
    PS-->>P: Pesan: pembayaran terverifikasi, peminjaman disetujui
    O->>UI: Tekan Selesaikan setelah barang dikembalikan
    UI->>API: PATCH /peminjaman/{id}/selesaikan
    API->>DB: Ubah status peminjaman menjadi Selesai
    API->>DB: Ubah status barang menjadi Tersedia
    API->>PS: Kirim notifikasi ke peminjam
    PS-->>P: Pesan: peminjaman selesai, silakan beri ulasan
    API-->>UI: Berhasil
```

### 3.3 Skenario C: Registrasi Pengguna
```mermaid
sequenceDiagram
    autonumber
    actor P as Pengguna
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database

    P->>UI: Isi form registrasi (nama, email, noHp, password)
    UI->>API: POST /register
    API->>DB: Cek apakah email sudah terdaftar
    DB-->>API: Hasil pengecekan
    alt Email belum terdaftar
        API->>DB: Simpan user baru (password di-hash)
        API-->>UI: Registrasi berhasil
        UI-->>P: Arahkan ke halaman login
    else Email sudah terdaftar
        API-->>UI: Error: email sudah dipakai
        UI-->>P: Tampilkan pesan dan saran login
    end
```

### 3.4 Skenario D: Login (Pengguna dan Admin)
```mermaid
sequenceDiagram
    autonumber
    actor U as Pengguna / Admin
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database

    U->>UI: Isi email dan password
    UI->>API: POST /login
    API->>DB: Ambil user berdasarkan email
    DB-->>API: Data user (hash password, role)
    alt Kredensial valid
        API-->>UI: Token sesi dan role
        alt Role = Admin
            UI-->>U: Buka dashboard admin
        else Role = User
            UI-->>U: Buka beranda katalog barang
        end
    else Kredensial salah
        API-->>UI: Error: email atau password salah
        UI-->>U: Tampilkan pesan error
    end
```

### 3.5 Skenario E: Beri Ulasan
```mermaid
sequenceDiagram
    autonumber
    actor P as Peminjam
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database

    P->>UI: Buka riwayat peminjaman, pilih Beri ulasan
    UI->>API: POST /ulasan (peminjamanId, rating, komentar)
    API->>DB: Cek peminjaman milik user tersebut
    DB-->>API: Data peminjaman dan status
    alt Peminjaman berstatus Selesai dan belum diulas
        API->>DB: Simpan ulasan
        API-->>UI: Ulasan tersimpan
        UI-->>P: Ulasan tampil di detail barang
    else Peminjaman belum selesai atau sudah diulas
        API-->>UI: Error: ulasan tidak diizinkan
        UI-->>P: Tampilkan alasan penolakan
    end
```

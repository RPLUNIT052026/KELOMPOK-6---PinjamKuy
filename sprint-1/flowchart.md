# 🔀 Flowchart PinjamKuy

Flowchart menggambarkan alur langkah-langkah proses di aplikasi.

## 1. Flowchart Registrasi dan Login

```mermaid
flowchart TD
    A([Mulai]) --> B[Buka halaman login]
    B --> C{Sudah punya akun?}
    C -- Belum --> D[Isi form registrasi]
    D --> E[Simpan data ke database]
    E --> B
    C -- Sudah --> F[Input email dan password]
    F --> G{Data valid?}
    G -- Tidak --> H[Tampilkan pesan error]
    H --> F
    G -- Ya --> I[Masuk ke halaman katalog]
    I --> J([Selesai])
```

## 2. Flowchart Peminjaman Barang

```mermaid
flowchart TD
    A([Mulai]) --> B[Login]
    B --> C[Lihat katalog barang]
    C --> D[Pilih barang dan lihat detail]
    D --> E{Barang tersedia?}
    E -- Tidak --> C
    E -- Ya --> F[Ajukan pinjam dan pilih tanggal]
    F --> G[Pemilik menerima pengajuan]
    G --> H{Pemilik menyetujui?}
    H -- Tidak --> I[Status ditolak]
    I --> Z([Selesai])
    H -- Ya --> J[Status disetujui]
    J --> K[Barang diserahkan dan dipakai]
    K --> L[Barang dikembalikan]
    L --> M[Status selesai dan beri ulasan]
    M --> Z
```

## 3. Flowchart Pemilik Menambah Barang

```mermaid
flowchart TD
    A([Mulai]) --> B[Login sebagai pemilik]
    B --> C[Buka form tambah barang]
    C --> D[Isi nama, kategori, harga, deskripsi, foto]
    D --> E{Data lengkap?}
    E -- Tidak --> F[Tampilkan pesan error]
    F --> D
    E -- Ya --> G[Simpan barang ke database]
    G --> H[Barang tampil di katalog]
    H --> I([Selesai])
```

## Keterangan Simbol

| Simbol | Arti |
|--------|------|
| Oval | Mulai atau selesai |
| Persegi panjang | Proses atau langkah |
| Belah ketupat | Keputusan (ya atau tidak) |

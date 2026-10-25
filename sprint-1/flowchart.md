# 🔀 Flowchart PinjamKuy

Flowchart menggambarkan alur langkah-langkah proses di aplikasi.

## 1. Flowchart Registrasi dan Login

```mermaid
flowchart TD
    A([Mulai]) --> B{Sudah punya akun?}
    B -- Belum --> C[Registrasi]
    C --> D[Login]
    B -- Sudah --> D
    D --> E{Data valid?}
    E -- Tidak --> D
    E -- Ya --> F[Halaman katalog]
    F --> G([Selesai])
```

## 2. Flowchart Peminjaman Barang

```mermaid
flowchart TD
    A([Mulai]) --> B[Pilih barang]
    B --> C{Tersedia?}
    C -- Tidak --> B
    C -- Ya --> D[Ajukan pinjam]
    D --> E{Disetujui pemilik?}
    E -- Tidak --> F([Selesai])
    E -- Ya --> G[Pakai dan kembalikan barang]
    G --> H[Beri ulasan]
    H --> F
```

## 3. Flowchart Pemilik Menambah Barang

```mermaid
flowchart TD
    A([Mulai]) --> B[Isi form barang]
    B --> C{Data lengkap?}
    C -- Tidak --> B
    C -- Ya --> D[Simpan dan tampil di katalog]
    D --> E([Selesai])
```

## Keterangan Simbol

| Simbol | Arti |
|--------|------|
| Oval | Mulai atau selesai |
| Persegi panjang | Proses atau langkah |
| Belah ketupat | Keputusan (ya atau tidak) |

# 🔀 Flowchart PinjamKuy

Flowchart menggambarkan alur langkah-langkah proses di aplikasi.

## 1. Flowchart Registrasi dan Login

```mermaid
flowchart TD
    A([Mulai]) --> B{Sudah punya akun?}
    B --> J(( ))
    J -- Sudah --> D[Login]
    J -- Belum --> C[Registrasi]
    C --> D
    D --> E{Data valid?}
    E --> K(( ))
    K -- Ya --> F[Halaman katalog]
    K -- Tidak --> D
    F --> G([Selesai])

    classDef dot width:8px,height:8px,fill:#888,stroke:#888
    class J,K dot
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

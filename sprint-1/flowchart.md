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
    C --> J1(( ))
    J1 -- Ya --> D[Ajukan pinjam]
    J1 -- Tidak --> B
    D --> E{Disetujui pemilik?}
    E --> J2(( ))
    J2 -- Ya --> G[Pakai dan kembalikan barang]
    J2 -- Tidak --> F([Selesai])
    G --> H[Beri ulasan]
    H --> F

    classDef dot width:8px,height:8px,fill:#888,stroke:#888
    class J1,J2 dot
```

## 3. Flowchart Pemilik Menambah Barang

```mermaid
flowchart TD
    A([Mulai]) --> B[Isi form barang]
    B --> C{Data lengkap?}
    C --> J(( ))
    J -- Ya --> D[Simpan dan tampil di katalog]
    J -- Tidak --> B
    D --> E([Selesai])

    classDef dot width:8px,height:8px,fill:#888,stroke:#888
    class J dot
```

## Keterangan Simbol

| Simbol | Arti |
|--------|------|
| Oval | Mulai atau selesai |
| Persegi panjang | Proses atau langkah |
| Belah ketupat | Keputusan (ya atau tidak) |

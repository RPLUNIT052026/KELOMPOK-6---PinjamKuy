# 🔄 State Diagram PinjamKuy

**Tujuan:** memahami perubahan perilaku sebagai respons terhadap event. State diagram menunjukkan keadaan (state) suatu objek dan peristiwa (event) yang membuatnya berpindah keadaan.

## 1. State Peminjaman

Status ini sama dengan kolom `status` di tabel `loans`.

```mermaid
stateDiagram-v2
    [*] --> Menunggu : Peminjam mengajukan pinjam
    Menunggu --> Disetujui : Pemilik menyetujui
    Menunggu --> Ditolak : Pemilik menolak
    Disetujui --> Dipinjam : Barang diserahkan
    Dipinjam --> Selesai : Barang dikembalikan
    Ditolak --> [*]
    Selesai --> [*]
```

## 2. State Barang

Status ini sama dengan kolom `status` di tabel `items`.

```mermaid
stateDiagram-v2
    [*] --> Tersedia : Pemilik menambahkan barang
    Tersedia --> Dipinjam : Pengajuan disetujui dan barang diserahkan
    Dipinjam --> Tersedia : Barang dikembalikan
    Tersedia --> Dihapus : Pemilik menghapus barang
    Dihapus --> [*]
```

## 3. State Sesi Pengguna

```mermaid
stateDiagram-v2
    [*] --> BelumLogin : Pengguna membuka aplikasi
    BelumLogin --> BelumLogin : Login gagal
    BelumLogin --> SudahLogin : Login berhasil
    BelumLogin --> BelumLogin : Registrasi berhasil
    SudahLogin --> BelumLogin : Logout
    SudahLogin --> [*] : Sesi berakhir
```

## Keterangan

| State | Arti |
|-------|------|
| Menunggu | Pengajuan sudah dikirim, belum ada keputusan pemilik |
| Disetujui | Pemilik setuju, barang belum diserahkan |
| Ditolak | Pemilik menolak pengajuan |
| Dipinjam | Barang sedang dipakai peminjam |
| Selesai | Barang sudah kembali, peminjaman tuntas |

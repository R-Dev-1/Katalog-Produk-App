# Katalog Produk App

Aplikasi desktop manajemen katalog produk berbasis **JavaFX** dan **SQLite**. Aplikasi ini menerapkan arsitektur berlapis (*View, Controller, Repository, Database*) untuk mengelola data produk secara lokal dengan fitur **CRUD** (*Create, Read, Update, Delete*) yang interaktif.

## Fitur Utama

* **Inisialisasi Database Otomatis:** Folder penyimpanan dan file database SQLite (`data/katalog.db`) beserta tabel `produk` otomatis dibuat saat aplikasi pertama kali dijalankan.

* **Tambah Produk (Create):** Formulir input di bagian atas untuk menambahkan nama produk, harga, dan memilih gambar produk dari daftar yang tersedia (`roti.png`, `susu.png`, `kopi.png`).

* **Tampilan Grid Katalog (Read):** Menampilkan seluruh produk dalam bentuk kartu responsif di dalam area gulir (`ScrollPane` + `TilePane`) lengkap dengan penghitung total produk secara *real-time*.

* **Edit Langsung di Kartu (Inline Update):** Mengubah data nama, harga, maupun foto langsung di dalam kartu produk yang dipilih tanpa berpindah halaman.

* **Hapus dengan Konfirmasi (Delete):** Menghapus produk langsung dari kartu disertai dialog konfirmasi (`Alert`) untuk mencegah penghapusan data yang tidak disengaja.

## Struktur Proyek

```
katalog-produk-upi/
├── data/
│   └── katalog.db                  # File database SQLite (dibuat otomatis saat aplikasi jalan)
├── screenshots/                    # Folder penyimpanan tangkapan layar aplikasi
│   ├── halaman-utama.png
│   ├── inline-edit.png
│   └── konfirmasi-hapus.png
└── src/
    ├── main/
    │   └── MainApp.java            # Titik masuk utama aplikasi JavaFX
    ├── controller/
    │   └── KatalogController.java  # Pengendali logika UI, validasi, & event handler
    ├── data/
    │   └── ProdukRepository.java   # Lapisan akses data SQL (CRUD)
    ├── db/
    │   └── Database.java           # Pengelola koneksi JDBC SQLite & pembuatan tabel
    ├── model/
    │   └── Produk.java             # Kelas model entitas Produk
    └── resources/
        ├── view/
        │   └── katalog.fxml        # Tata letak antarmuka pengguna (FXML)
        ├── styles/
        │   └── app.css             # CSS tampilan aplikasi
        └── image/
            ├── roti.png
            ├── susu.png
            └── kopi.png

```

## Prasyarat Sistem

Sebelum menjalankan aplikasi, pastikan perangkat Anda telah terpasang:

1. **Java Development Kit (JDK)** versi **17** atau **20+** (disarankan JDK 20 sesuai konfigurasi FXML).

2. **JavaFX SDK** versi **20.0.1** atau yang kompatibel dengan versi JDK Anda.

3. **SQLite JDBC Driver** (`sqlite-jdbc`, misalnya versi `3.42.0.0` atau terbaru).

## Cara Menjalankan Aplikasi

1. Buka proyek ini di IDE pilihan Anda.

2. Pastikan pustaka **JavaFX SDK** dan **SQLite JDBC (`sqlite-jdbc.jar`)** sudah ditambahkan ke dalam *Project Libraries / Dependencies*.

3. Buka file `src/main/MainApp.java`, lalu klik tombol **Run** pada metode `main`.

## Lampiran

### 1. Halaman Utama & Form Tambah Produk

Menampilkan formulir input produk di bagian atas dan daftar kartu produk di bagian tengah.

**Pratinjau Tata Letak Layar:**

```
+-----------------------------------------------------------------------+
| Katalog Produk                                                        |
| Isi form, simpan, lalu lihat katalog.                                 |
| Nama produk   : [ Contoh: Roti Cokelat                              ] |
| Harga (Rp)    : [ Contoh: 10000                                     ] |
| Foto contoh   : [ roti.png                                      ▼ ] |
| [ Simpan ]                                                            |
| Produk berhasil disimpan.                                             |
| Jumlah produk: 3                                                      |
+-----------------------------------------------------------------------+
|  +--------------------+  +--------------------+  +--------------------+ |
|  |   [Gambar Roti]    |  |   [Gambar Susu]    |  |   [Gambar Kopi]    | |
|  | Roti Cokelat       |  | Susu Segar         |  | Kopi Hitam         | |
|  | Rp10000            |  | Rp15000            |  | Rp12000            | |
|  | ID: 1              |  | ID: 2              |  | ID: 3              | |
|  | [ Edit ] [ Hapus ] |  | [ Edit ] [ Hapus ] |  | [ Edit ] [ Hapus ] | |
|  +--------------------+  +--------------------+  +--------------------+ |
+-----------------------------------------------------------------------+

```

### 2. Mode Inline Edit pada Kartu Produk

Ketika tombol **Edit** pada salah satu kartu diklik, kartu tersebut berubah menjadi formulir pengeditan instan.

**Pratinjau Kartu Saat Mode Edit:**

```
+--------------------+
| Edit ID: 1         |
| [ Roti Cokelat   ] |
| [ 10000          ] |
| [ roti.png     ▼ ] |
| [ Simpan ] [Batal] |
+--------------------+

```

### 3. Dialog Konfirmasi Hapus Produk

Muncul saat tombol **Hapus** ditekan untuk memastikan pengguna benar-benar ingin menghapus data produk dari database.

## Skema Database SQLite

Tabel `produk` dibuat secara otomatis oleh kelas `Database.java` dengan struktur berikut:

```
CREATE TABLE IF NOT EXISTS produk (
    id INTEGER PRIMARY KEY,
    nama TEXT NOT NULL CHECK(length(trim(nama)) > 0),
    harga INTEGER NOT NULL CHECK(harga > 0),
    foto TEXT NOT NULL CHECK(length(trim(foto)) > 0)
);

```
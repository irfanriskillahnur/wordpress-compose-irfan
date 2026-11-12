# Docker Compose - WordPress dan MariaDB

**Nama:** Irfan Riskillah Nur  
**Kelas:** XI TJAT 3  
**Nomor Absen:** 10  
**Sekolah:** SMK Telkom Sidoarjo

## 1. Deskripsi Project

Project ini menjalankan WordPress yang terhubung ke database MariaDB menggunakan Docker Compose.

Docker Compose membuat dua layanan bisa dinyalakan sekaligus dengan satu perintah, jadi tidak perlu menjalankan container satu per satu.

Project ini memakai tiga layanan: WordPress sebagai website, MariaDB sebagai tempat menyimpan data, dan phpMyAdmin untuk melihat isi database lewat browser.

## 2. Teknologi yang Digunakan

- Docker Desktop
- Docker Compose
- WordPress
- MariaDB
- phpMyAdmin
- Visual Studio Code
- Git dan GitHub

## 3. Struktur Folder

```text
wordpress-compose-irfan/
├── compose.yaml
├── .env
├── .env.example
├── .gitignore
├── README.md
└── bukti-running/
    ├── docker-ps.png
    ├── wordpress-browser.png
    ├── artikel-uji.png
    └── data-persistensi.png
```

File `.env` hanya ada di laptop dan tidak diunggah ke GitHub.

## 4. Cara Menjalankan Project

### Persiapan

Pastikan Docker Desktop sudah menyala dan Docker Compose sudah tersedia. Cek dengan `docker --version` dan `docker compose version`.

### Langkah menjalankan

1. Clone repository atau buka folder project.
2. Salin `.env.example` menjadi `.env` dengan perintah `copy .env.example .env` (Windows).
3. Isi nilai di file `.env` dengan nama database, user, dan password milik sendiri.
4. Buka terminal di folder project.
5. Periksa konfigurasi:

```bash
docker compose config
```

6. Jalankan container:

```bash
docker compose up -d
```

7. Periksa status container:

```bash
docker compose ps
```

8. Buka WordPress di browser: http://localhost:8080
9. Buka phpMyAdmin di browser: http://localhost:8081
10. Selesaikan instalasi WordPress, lalu buat satu artikel pengujian.

## 5. Perintah Docker Compose

| Perintah | Fungsi |
|---|---|
| `docker compose config` | Memeriksa isi konfigurasi dan memastikan variabel dari `.env` terbaca |
| `docker compose up -d` | Membuat dan menjalankan semua container di latar belakang |
| `docker compose ps` | Melihat status container |
| `docker compose logs wordpress` | Melihat catatan aktivitas container WordPress |
| `docker compose down` | Menghentikan dan menghapus container, volume tetap ada |
| `docker compose down` lalu `docker compose up -d` | Menguji apakah data tetap tersimpan |

## 6. Jawaban Tugas Eksplorasi

### 1. Apa tujuan Docker Compose dan mengapa WordPress membutuhkan database terpisah?

Docker Compose seperti tombol satu klik untuk menyalakan beberapa aplikasi (container) sekaligus: cukup satu file dan satu perintah. WordPress sendiri tidak punya tempat menyimpan data. Semua isi websitenya, seperti artikel, akun, dan komentar, disimpan di database. Dipisah supaya urusan tampilan website dan urusan penyimpanan data tidak tercampur.

### 2. Jelaskan fungsi service db dan service wordpress!

Service `db` adalah gudang data: MariaDB menyimpan semua data WordPress. Service `wordpress` adalah websitenya, yaitu aplikasi yang dibuka di browser untuk menulis artikel dan mengatur situs, dan ia menyimpan serta mengambil datanya dari `db`.

### 3. Jelaskan arti image, container_name, restart, ports, environment, depends_on, dan volumes!

- Image: cetakan atau paket jadi yang dipakai untuk membuat container.
- Container_name: nama yang kita berikan ke container supaya mudah dikenali.
- Restart: aturan kalau container mati. Dengan `unless-stopped`, container menyala lagi sendiri kecuali kita yang mematikannya.
- Ports: pintu penghubung antara laptop dan container. `8080:80` berarti `localhost:8080` diteruskan ke port 80 di dalam container.
- Environment: pengaturan yang dikirim ke container, misalnya nama database, user, dan password.
- Depends_on: urutan menyalakan, WordPress dinyalakan setelah database mulai dinyalakan.
- Volumes: tempat menyimpan data di luar container, jadi data aman walaupun container dihapus.

### 4. Mengapa WORDPRESS_DB_HOST menggunakan db:3306, bukan localhost:3306?

Setiap container punya alamatnya sendiri. Kalau WordPress memakai `localhost`, ia mencari database di dalam dirinya sendiri, padahal database ada di container lain, sehingga tidak ketemu. Di jaringan Docker Compose, container saling mengenal lewat nama service, jadi WordPress cukup memanggil `db`. Angka 3306 adalah nomor port MariaDB.

### 5. Jelaskan perbedaan .env, .env.example, dan .gitignore!

File `.env` berisi pengaturan dan password asli, dan hanya ada di laptop. File `.env.example` adalah contoh kosongnya, berisi nama variabel tanpa password, sehingga aman diunggah ke GitHub. File `.gitignore` adalah daftar file yang tidak boleh diunggah; dengan menuliskan `.env` di sana, password tidak ikut terkirim ke GitHub.

### 6. Apa yang dibuktikan oleh percobaan docker compose down lalu docker compose up -d?

Setelah `down`, container dihapus. Tetapi setelah dinyalakan lagi dengan `up -d`, artikel yang dibuat sebelumnya masih ada. Ini membuktikan data tidak hilang bersama container, karena disimpan di volume yang terpisah.

### 7. Pilih satu masalah yang ditemui selama setup!

Masalah yang saya temui: perintah `docker --version` gagal di komputer Ubuntu sekolah dengan pesan `Command 'docker' not found`.

Penyebab: Docker belum terinstal di komputer tersebut.

Langkah perbaikan: (isi dengan langkah yang benar-benar Anda lakukan, misalnya mengerjakan di laptop sendiri dan memakai Docker Desktop).

Hasil: (isi hasil yang sebenarnya, misalnya perintah `docker --version` berhasil dan project dapat berjalan).

### 8. Jelaskan satu modifikasi yang dilakukan sendiri!

Saya menambahkan service phpMyAdmin pada port 8081. Dengan itu isi database bisa dilihat lewat browser di http://localhost:8081, sehingga terbukti WordPress benar-benar menyimpan data ke database.

Saya juga memindahkan nama database, user, dan password ke file `.env` agar tidak terlihat di `compose.yaml` dan tidak ikut terunggah ke GitHub.

## 7. Bukti Hasil Running

### A. Status Container

![Docker PS](bukti-running/docker-ps.png)

Menampilkan status container WordPress, MariaDB, dan phpMyAdmin yang sedang berjalan.

### B. Tampilan WordPress

![WordPress Browser](bukti-running/wordpress-browser.png)

Menampilkan halaman WordPress yang berhasil dibuka melalui browser.

### C. Artikel Pengujian

![Artikel Uji](bukti-running/artikel-uji.png)

Menampilkan artikel pengujian yang berhasil dibuat di WordPress.

### D. Pengujian Persistensi Data

![Data Persistensi](bukti-running/data-persistensi.png)

Menunjukkan bahwa artikel tetap ada setelah container dihentikan dan dijalankan kembali.

## 8. Kesimpulan

Dari project ini saya belajar bahwa Docker Compose memudahkan menjalankan WordPress dan database sekaligus. Saya juga paham fungsi environment variable untuk menyimpan pengaturan dan password, jaringan Docker supaya container saling terhubung, serta volume supaya data tidak hilang.

## 9. Repository GitHub

Tautan Repository: https://github.com/[(https://github.com/irfanriskillahnur)]/wordpress-compose-irfan

## 10. Catatan Penggunaan AI

Saya menggunakan bantuan AI untuk memahami langkah pengerjaan dan struktur project. Penjelasan pada README ini sudah saya pahami dan saya tulis ulang dengan bahasa sendiri.

---

**Project Individu - Docker Compose WordPress dan MariaDB**  
SMK Telkom Sidoarjo
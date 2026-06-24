# WEB1
# 📰 Lab11 - Sistem Manajemen Artikel dengan CodeIgniter 4

> Proyek praktikum berbasis **CodeIgniter 4** yang mengimplementasikan sistem manajemen artikel lengkap dengan autentikasi session, REST API, dan fitur AJAX interaktif.

---

## 📋 Deskripsi Proyek

Aplikasi web ini dibangun sebagai bagian dari praktikum pemrograman web menggunakan framework **CodeIgniter 4 (CI4)**. Fitur utama meliputi:

- Manajemen artikel (CRUD) dengan upload gambar
- Kategori artikel
- Autentikasi berbasis **Session CI4**
- **REST API** dengan autentikasi token Bearer
- Operasi **AJAX** tanpa reload halaman
- Filter, pencarian, dan pengurutan artikel

---

## 🏗️ Struktur Direktori Proyek

```
ci4/
├── app/
│   ├── Cells/
│   │   └── ArtikelTerkini.php          # View Cell: artikel terbaru di sidebar
│   ├── Config/
│   │   ├── Routes.php                  # Definisi semua route aplikasi
│   │   ├── Filters.php                 # Registrasi filter (authFilter, apiauth)
│   │   └── AuthFilter.php              # Konfigurasi custom filter
│   ├── Controllers/
│   │   ├── Artikel.php                 # CRUD artikel (publik & admin)
│   │   ├── User.php                    # Login & logout session
│   │   ├── Post.php                    # REST API endpoint artikel
│   │   ├── AjaxController.php          # Controller operasi AJAX
│   │   ├── Page.php                    # Halaman statis (about, contact)
│   │   └── Api/
│   │       └── Auth.php                # API endpoint login (token)
│   ├── Filters/
│   │   ├── AuthFilter.php              # Filter autentikasi session (admin)
│   │   └── ApiAuthFilter.php           # Filter autentikasi token Bearer (API)
│   ├── Models/
│   │   ├── ArtikelModel.php            # Model tabel artikel
│   │   ├── KategoriModel.php           # Model tabel kategori
│   │   └── UserModel.php               # Model tabel user
│   ├── Database/
│   │   └── Seeds/
│   │       └── UserSeeder.php          # Seeder data user awal
│   └── Views/
│       ├── layout/
│       │   ├── main.php                # Layout halaman publik
│       │   └── admin.php               # Layout halaman admin
│       ├── template/
│       │   ├── header.php              # Header publik
│       │   ├── footer.php              # Footer publik
│       │   ├── admin_header.php        # Header admin
│       │   └── admin_footer.php        # Footer admin
│       ├── artikel/
│       │   ├── index.php               # Daftar artikel publik
│       │   ├── detail.php              # Detail artikel
│       │   ├── admin_index.php         # Daftar artikel admin (AJAX)
│       │   ├── form_add.php            # Form tambah artikel
│       │   └── form_edit.php           # Form edit artikel
│       ├── user/
│       │   └── login.php               # Halaman login
│       └── ajax/
│           └── index.php               # Halaman operasi AJAX
├── public/
│   ├── index.php                       # Entry point aplikasi
│   └── gambar/                         # Folder upload gambar artikel
└── writable/
    ├── logs/                           # Log aplikasi
    ├── session/                        # File session
    └── cache/                          # Cache aplikasi
```

---

## ✨ Fitur Aplikasi

### 🌐 Halaman Publik
| Fitur | Route | Keterangan |
|-------|-------|------------|
| Daftar Artikel | `GET /artikel` | Filter, search, dan sort artikel |
| Detail Artikel | `GET /artikel/{slug}` | Tampil detail artikel berdasarkan slug |
| Halaman About | `GET /about` | Informasi tentang aplikasi |
| Halaman Contact | `GET /contact` | Halaman kontak |

### 🔐 Autentikasi Session
| Fitur | Route | Keterangan |
|-------|-------|------------|
| Login | `GET/POST /login` | Form login dengan session CI4 |
| Logout | `GET /logout` | Hapus session & redirect ke login |

### 🛠️ Panel Admin *(Wajib Login)*
| Fitur | Route | Keterangan |
|-------|-------|------------|
| Daftar Artikel | `GET /admin/artikel` | Tabel artikel dengan AJAX filter/search/sort/pagination |
| Tambah Artikel | `GET/POST /admin/artikel/add` | Form dengan validasi & upload gambar |
| Edit Artikel | `GET/POST /admin/artikel/edit/{id}` | Edit artikel & ganti gambar |
| Hapus Artikel | `GET /admin/artikel/delete/{id}` | Hapus artikel & file gambar |
| AJAX Demo | `GET /admin/ajax` | Halaman CRUD artikel via AJAX |

### 🔌 REST API
| Method | Endpoint | Auth | Keterangan |
|--------|----------|------|------------|
| `GET` | `/post` | ❌ | Ambil semua artikel |
| `GET` | `/post/{id}` | ❌ | Ambil artikel berdasarkan ID |
| `POST` | `/post` | ✅ Token | Buat artikel baru |
| `PUT` | `/post/{id}` | ✅ Token | Update artikel |
| `DELETE` | `/post/{id}` | ✅ Token | Hapus artikel |
| `POST` | `/api/login` | ❌ | Login & dapatkan token |

---

## ⚙️ Persyaratan Sistem

- **PHP** >= 8.1
- **MySQL** >= 5.7 / MariaDB >= 10.4
- **Composer**
- **Web Server**: Apache / Nginx (atau PHP built-in server)

---

## 🚀 Cara Instalasi & Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/username/lab11-ci4.git
cd lab11-ci4/ci4
```

### 2. Install Dependency

```bash
composer install
```

### 3. Konfigurasi Environment

Salin file `.env` dan sesuaikan:

```bash
cp .env .env.bak
```

Edit file `.env`:

```env
CI_ENVIRONMENT = development

database.default.hostname = localhost
database.default.database = lab_ci4
database.default.username = root
database.default.password = 
database.default.DBDriver = MySQLi
```

### 4. Buat Database & Tabel

Buat database `lab_ci4` di MySQL, lalu jalankan SQL berikut:

```sql
-- Tabel kategori
CREATE TABLE kategori (
    id_kategori INT AUTO_INCREMENT PRIMARY KEY,
    nama_kategori VARCHAR(100) NOT NULL
);

-- Tabel artikel
CREATE TABLE artikel (
    id INT AUTO_INCREMENT PRIMARY KEY,
    judul VARCHAR(200) NOT NULL,
    isi TEXT,
    gambar VARCHAR(200),
    status TINYINT(1) DEFAULT 0,
    slug VARCHAR(200),
    id_kategori INT,
    FOREIGN KEY (id_kategori) REFERENCES kategori(id_kategori)
);

-- Tabel user
CREATE TABLE user (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100) NOT NULL UNIQUE,
    useremail VARCHAR(100),
    userpassword VARCHAR(255) NOT NULL
);
```

### 5. Jalankan Seeder (Data User Awal)

```bash
php spark db:seed UserSeeder
```

### 6. Jalankan Aplikasi

```bash
php spark serve
```

Akses di browser: **http://localhost:8080**

---

## 🔑 Akun Default

Setelah menjalankan seeder:

| Username | Password |
|----------|----------|
| `admin` | `admin123` |

---

## 📡 Panduan REST API

### Login untuk Mendapatkan Token

```bash
curl -X POST http://localhost:8080/api/login \
  -d "username=admin&password=admin123"
```

**Response:**
```json
{
  "status": 200,
  "error": null,
  "messages": "Login Berhasil",
  "data": {
    "id": 1,
    "username": "admin",
    "token": "VE9LRU4tU0VDUkVULWFkbWlu"
  }
}
```

### Menggunakan Token pada Request yang Membutuhkan Auth

```bash
# Buat artikel baru
curl -X POST http://localhost:8080/post \
  -H "Authorization: Bearer VE9LRU4tU0VDUkVULWFkbWlu" \
  -d "judul=Artikel Baru&isi=Isi artikel&id_kategori=1"

# Update artikel
curl -X PUT http://localhost:8080/post/1 \
  -H "Authorization: Bearer VE9LRU4tU0VDUkVULWFkbWlu" \
  -d "judul=Judul Diperbarui"

# Hapus artikel
curl -X DELETE http://localhost:8080/post/1 \
  -H "Authorization: Bearer VE9LRU4tU0VDUkVULWFkbWlu"
```

### Endpoint Tanpa Auth

```bash
# Ambil semua artikel
curl http://localhost:8080/post

# Ambil artikel berdasarkan ID
curl http://localhost:8080/post/1
```

---

## 🛡️ Sistem Autentikasi

Aplikasi menggunakan **dua lapisan autentikasi**:

### 1. Session Filter (`AuthFilter`)
Melindungi route `/admin/*`. Redirect ke `/login` jika belum login.

### 2. API Token Filter (`ApiAuthFilter`)
Melindungi endpoint `POST/PUT/DELETE /post`. Menggunakan skema **Bearer Token** di header `Authorization`.

---

## 📸 Screenshot

### Halaman Publik - Daftar Artikel
Menampilkan daftar artikel dengan fitur pencarian berdasarkan judul, filter kategori, dan opsi pengurutan (terbaru, terlama, A-Z, Z-A).

### Panel Admin - Manajemen Artikel
Tabel artikel dengan pagination, filter, dan search yang bekerja secara **AJAX** tanpa reload halaman penuh.

### Form Tambah/Edit Artikel
Form lengkap dengan validasi server-side, upload gambar (JPG/PNG/GIF/WebP, maks 2MB), dan pemilihan kategori.

---

## 🧰 Teknologi yang Digunakan

| Teknologi | Versi | Keterangan |
|-----------|-------|------------|
| CodeIgniter | 4.x | Framework PHP utama |
| PHP | >= 8.1 | Bahasa pemrograman |
| MySQL | >= 5.7 | Database |
| Bootstrap | 5.x | Framework CSS |
| JavaScript (Vanilla) | ES6+ | Operasi AJAX |

---

## 📚 Materi Praktikum yang Dicakup

Proyek ini mencakup implementasi dari beberapa sesi praktikum:

- **Lab 7** — Controller, View, dan Routing dasar CI4
- **Lab 8** — Model dan integrasi Database
- **Lab 9** — Autentikasi berbasis Session & Filter
- **Lab 10** — REST API dengan `ResourceController`
- **Lab 11** — AJAX, View Cell, dan fitur lanjutan
- **Lab 13** — API Authentication dengan Bearer Token
- **Lab 14** — Keamanan endpoint API

---

## 👤 Informasi Praktikan

| | |
|-|-|
| **Nama** | *[Nama Mahasiswa]* |
| **NIM** | *[NIM]* |
| **Kelas** | *[Kelas]* |
| **Mata Kuliah** | Pemrograman Web |
| **Dosen** | *[Nama Dosen]* |

---

## 📄 Lisensi

Proyek ini dibuat untuk keperluan **akademik / praktikum**. Lihat file [LICENSE](LICENSE) untuk detail lebih lanjut.

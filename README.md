# README - CodeIgniter 4 CRUD Artikel

## Pendahuluan
Repositori ini berisi proyek CodeIgniter 4 yang mengimplementasikan sistem CRUD (Create, Read, Update, Delete) untuk mengelola artikel. Proyek ini dikembangkan berdasarkan modul praktikum pemrograman web menggunakan CodeIgniter 4.

## Prasyarat
Sebelum menjalankan proyek ini, pastikan Anda telah menginstal:
- **XAMPP** (untuk Apache dan MySQL)
- **Composer** (untuk manajemen dependensi PHP)
- **CodeIgniter 4** (framework PHP yang digunakan dalam proyek ini)
- **VSCode atau Text Editor lainnya** (untuk pengeditan kode)

## Instalasi
1. **Clone repositori ini:**
   ```sh
   git clone https://github.com/username/Lab7Web.git
   ```
2. **Masuk ke folder proyek:**
   ```sh
   cd Lab7Web
   ```
3. **Instal dependensi menggunakan Composer:**
   ```sh
   composer install
   ```
4. **Konfigurasi database di `.env`:**
   ```env
   database.default.hostname = localhost
   database.default.database = lab_ci4
   database.default.username = root
   database.default.password =
   database.default.DBDriver = MySQLi
   ```
5. **Jalankan migrasi database untuk membuat tabel yang dibutuhkan:**
   ```sh
   php spark migrate
   ```
6. **Jalankan server CodeIgniter:**
   ```sh
   php spark serve
   ```
   Server akan berjalan di `http://localhost:8080`

## Struktur Database
Buat database dan tabel menggunakan perintah SQL berikut:
```sql
CREATE DATABASE lab_ci4;
USE lab_ci4;

CREATE TABLE artikel (
    id INT(11) AUTO_INCREMENT PRIMARY KEY,
    judul VARCHAR(200) NOT NULL,
    isi TEXT,
    gambar VARCHAR(200),
    status TINYINT(1) DEFAULT 0,
    slug VARCHAR(200)
);
```

## Konfigurasi Database di CodeIgniter
Konfigurasi database dapat dilakukan dengan dua cara:
1. **Melalui file `app/Config/Database.php`**
2. **Menggunakan file `.env` (lebih direkomendasikan dalam proyek ini)**

## Fitur CRUD
1. **Menampilkan Semua Artikel** - `GET /artikel`
   - Artikel yang telah ditambahkan ke dalam database akan ditampilkan dalam halaman utama.
2. **Menampilkan Detail Artikel** - `GET /artikel/{slug}`
   - Menampilkan isi lengkap dari artikel yang dipilih berdasarkan slug-nya.
3. **Menambah Artikel** - `POST /admin/artikel/add`
   - Form untuk menambahkan artikel baru.
4. **Mengedit Artikel** - `POST /admin/artikel/edit/{id}`
   - Mengubah isi dari artikel yang sudah ada.
5. **Menghapus Artikel** - `GET /admin/artikel/delete/{id}`
   - Menghapus artikel berdasarkan ID.

## Langkah-Langkah Praktikum
### 1. Membuat Model Artikel
Model digunakan untuk mengelola data dalam tabel `artikel`.
```php
namespace App\Models;
use CodeIgniter\Model;

class ArtikelModel extends Model {
    protected $table = 'artikel';
    protected $primaryKey = 'id';
    protected $useAutoIncrement = true;
    protected $allowedFields = ['judul', 'isi', 'status', 'slug', 'gambar'];
}
```
### 2. Membuat Controller Artikel
Controller mengatur bagaimana data ditampilkan dan diproses.
```php
namespace App\Controllers;
use App\Models\ArtikelModel;

class Artikel extends BaseController {
    public function index() {
        $model = new ArtikelModel();
        $artikel = $model->findAll();
        return view('artikel/index', ['artikel' => $artikel, 'title' => 'Daftar Artikel']);
    }
}
```
### 3. Membuat View Artikel
View digunakan untuk menampilkan data di dalam tampilan web.
```php
<?= $this->include('template/header'); ?>
<?php foreach($artikel as $row): ?>
<article>
    <h2><a href="<?= base_url('/artikel/' . $row['slug']); ?>"> <?= $row['judul']; ?> </a></h2>
    <p><?= substr($row['isi'], 0, 200); ?></p>
</article>
<hr>
<?php endforeach; ?>
<?= $this->include('template/footer'); ?>
```
### 4. Membuat Routing
Tambahkan routing di `app/Config/Routes.php`:
```php
$routes->get('/artikel', 'Artikel::index');
$routes->get('/artikel/(:any)', 'Artikel::view/$1');
$routes->group('admin', function($routes) {
    $routes->get('artikel', 'Artikel::admin_index');
    $routes->add('artikel/add', 'Artikel::add');
    $routes->add('artikel/edit/(:any)', 'Artikel::edit/$1');
    $routes->get('artikel/delete/(:any)', 'Artikel::delete/$1');
});
```
### 5. Menambahkan Data Dummy ke Database
Tambahkan beberapa artikel contoh ke dalam database:
```sql
INSERT INTO artikel (judul, isi, slug) VALUES  
('Artikel Pertama', 'Lorem Ipsum adalah teks dummy...', 'artikel-pertama'),
('Artikel Kedua', 'Lorem Ipsum telah digunakan sejak...', 'artikel-kedua');
```

## Screenshot dan Tampilan
**1. Tampilan daftar artikel**
_(Tambahkan screenshot dari daftar artikel)_

**2. Tampilan detail artikel**
_(Tambahkan screenshot detail artikel)_

**3. Tampilan form tambah artikel**
_(Tambahkan screenshot form tambah artikel)_

**4. Tampilan halaman admin**
_(Tambahkan screenshot halaman admin)_

## Deployment
Untuk deployment di hosting atau server, pastikan:
- Konfigurasi database telah diatur dengan benar.
- Folder `writable` memiliki izin tulis.
- `public/.htaccess` diaktifkan untuk pengaturan routing.

## Kontributor
Dikerjakan oleh [Sena Dwi Ilham Santoso] sebagai bagian dari Praktikum Pemrograman Web 2 di Universitas Pelita Bangsa.

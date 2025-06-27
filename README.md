# ProManage: Product Management System

ProManage adalah aplikasi manajemen produk berbasis desktop yang dirancang untuk menyederhanakan dan mengoptimalkan seluruh siklus hidup produk, mulai dari perencanaan hingga peluncuran. Aplikasi ini menyediakan alat yang efisien untuk mengelola data produk, kategori, pelanggan, pemasok, serta transaksi masuk dan keluar barang. Dengan antarmuka yang intuitif dan fitur yang kuat, ProManage membantu Anda memastikan setiap produk mencapai potensinya secara maksimal.

## Fitur Utama

*   **Manajemen Pengguna**:
    *   Sistem login dan registrasi pengguna.
    *   Fungsi CRUD (Create, Read, Update, Delete) untuk data pengguna.
*   **Manajemen Kategori**:
    *   Menambah, mengedit, dan menghapus kategori produk.
*   **Manajemen Produk**:
    *   Menambah, mengedit, dan menghapus detail produk, termasuk nama, kategori, deskripsi, stok, dan harga.
    *   Produk terhubung dengan kategori yang relevan.
*   **Manajemen Pelanggan**:
    *   Menambah, mengedit, dan menghapus informasi pelanggan (nama, alamat, email, telepon).
*   **Manajemen Pemasok**:
    *   Menambah, mengedit, dan menghapus informasi pemasok (nama, alamat, email, telepon).
*   **Manajemen Transaksi Barang Masuk (Inbound)**:
    *   Mencatat penerimaan produk dari pemasok.
    *   Secara otomatis memperbarui jumlah stok produk.
*   **Manajemen Transaksi Barang Keluar (Outbound)**:
    *   Mencatat pengiriman produk kepada pelanggan.
    *   Secara otomatis mengurangi jumlah stok produk.
*   **Integrasi Database MySQL**: Aplikasi terhubung dengan database MySQL untuk penyimpanan data yang persisten.
*   **Antarmuka Pengguna Modern**: Dibangun dengan Guna.UI2.WinForms untuk pengalaman pengguna yang responsif dan menarik.

## Teknologi yang Digunakan

*   **Bahasa Pemrograman**: C#
*   **Framework**: .NET 8.0 (Windows Forms)
*   **Database**: MySQL
*   **Konektor Database**: MySql.Data (MySQL Connector/NET)
*   **Komponen UI**: Guna.UI2.WinForms

## Prasyarat

Sebelum menjalankan proyek ini, pastikan Anda memiliki hal-hal berikut terinstal di sistem Anda:

*   **Visual Studio** (disarankan versi terbaru dengan dukungan .NET 8.0 dan pengembangan aplikasi desktop).
*   **MySQL Server** (misalnya, XAMPP, WAMP, atau instalasi MySQL mandiri).
*   **MySQL Connector/NET** (pastikan versi yang kompatibel dengan instalasi MySQL Anda).

## Panduan Instalasi dan Setup

Ikuti langkah-langkah di bawah ini untuk menyiapkan dan menjalankan proyek di lingkungan lokal Anda:

1.  **Kloning Repositori**:
    ```bash
    git clone <URL_REPOSITORI_ANDA>
    cd ManagementProduct
    ```

2.  **Setup Database MySQL**:
    *   Buka MySQL client (misalnya, MySQL Workbench, phpMyAdmin, atau command line).
    *   Buat database baru dengan nama `management_product`.
    *   Jalankan skrip SQL berikut untuk membuat tabel yang diperlukan:

    ```sql
    -- Tabel users
    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        username VARCHAR(255) NOT NULL UNIQUE,
        password VARCHAR(255) NOT NULL,
        email VARCHAR(255),
        phone VARCHAR(20),
        image LONGBLOB
    );

    -- Tabel categories
    CREATE TABLE categories (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL UNIQUE
    );

    -- Tabel products
    CREATE TABLE products (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        category_id INT,
        description TEXT,
        stock INT DEFAULT 0,
        price DECIMAL(10, 2),
        FOREIGN KEY (category_id) REFERENCES categories(id)
    );

    -- Tabel customer
    CREATE TABLE customer (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        address TEXT,
        email VARCHAR(255),
        phone VARCHAR(20)
    );

    -- Tabel supplier
    CREATE TABLE supplier (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        address TEXT,
        email VARCHAR(255),
        phone VARCHAR(20)
    );

    -- Tabel inbounds
    CREATE TABLE inbounds (
        id INT AUTO_INCREMENT PRIMARY KEY,
        product_id INT,
        supplier_id INT,
        quantity INT,
        date DATE,
        FOREIGN KEY (product_id) REFERENCES products(id),
        FOREIGN KEY (supplier_id) REFERENCES supplier(id)
    );

    -- Tabel outbounds
    CREATE TABLE outbounds (
        id INT AUTO_INCREMENT PRIMARY KEY,
        product_id INT,
        customer_id INT,
        quantity INT,
        date DATE,
        FOREIGN KEY (product_id) REFERENCES products(id),
        FOREIGN KEY (customer_id) REFERENCES customer(id)
    );

    -- Contoh data awal (opsional)
    INSERT INTO users (username, password, email, phone) VALUES ('admin', 'admin123', 'admin@example.com', '1234567890');
    INSERT INTO categories (name) VALUES ('Electronics'), ('Clothing');
    INSERT INTO products (name, category_id, description, stock, price) VALUES ('Laptop', 1, 'Powerful laptop for work and gaming', 10, 1200.00);
    INSERT INTO customer (name, address, email, phone) VALUES ('John Doe', '123 Main St', 'john@example.com', '0987654321');
    INSERT INTO supplier (name, address, email, phone) VALUES ('Tech Supplies', '456 Industrial Rd', 'tech@example.com', '1122334455');
    ```

3.  **Konfigurasi Koneksi Database**:
    *   Buka proyek di Visual Studio.
    *   Periksa file-file kelas di folder `ManagementProduct/Class/` (misalnya, `Barangs.cs`, `Users.cs`, dll.).
    *   Pastikan detail koneksi database di metode `InitializeDB()` sesuai dengan konfigurasi MySQL Anda. Secara default, ini diatur ke:
        ```csharp
        server = "localhost";
        database = "management_product";
        uid = "root";
        password = ""; // Sesuaikan jika Anda memiliki kata sandi untuk pengguna root MySQL Anda
        ```

4.  **Pulihkan Paket NuGet**:
    *   Di Visual Studio, klik kanan pada solusi `ManagementProduct.sln` di Solution Explorer.
    *   Pilih "Restore NuGet Packages" atau "Manage NuGet Packages for Solution..." dan pastikan semua paket (terutama `Guna.Charts.WinForms`, `Guna.UI2.WinForms`) telah terinstal.

5.  **Bangun Proyek**:
    *   Bangun proyek dengan menekan `Ctrl+Shift+B` atau melalui menu `Build > Build Solution`.

6.  **Jalankan Aplikasi**:
    *   Jalankan aplikasi dengan menekan `F5` atau `Debug > Start Debugging`.
    *   Aplikasi akan dimulai dengan layar login. Anda dapat menggunakan kredensial default (`admin`/`admin123`) atau mendaftar pengguna baru.

## Penggunaan Aplikasi

Setelah aplikasi berjalan:

1.  **Login**: Masuk menggunakan kredensial yang valid.
2.  **Navigasi**: Gunakan menu di sisi kiri untuk beralih antara modul manajemen (Kategori, Produk, Karyawan, Barang Masuk, Barang Keluar, Pelanggan, Pemasok).
3.  **Operasi CRUD**: Di setiap modul, Anda dapat menambah data baru, mengedit data yang sudah ada, atau menghapus entri.
4.  **Manajemen Stok**: Transaksi barang masuk akan menambah stok produk, sementara transaksi barang keluar akan mengurangi stok produk secara otomatis.

## Kontribusi

Jika Anda ingin berkontribusi pada proyek ini, silakan fork repositori dan buat Pull Request dengan perubahan Anda.

## Lisensi

Proyek ini dilisensikan di bawah Lisensi MIT. Lihat file `LICENSE` untuk detail lebih lanjut.

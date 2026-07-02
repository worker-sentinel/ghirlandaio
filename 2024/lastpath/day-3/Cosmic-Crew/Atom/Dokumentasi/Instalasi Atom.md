## Install Dependensi

1. Database: MySQL / Percona Server

AtoM 2.10 membutuhkan minimal MySQL 8.0 karena memanfaatkan fitur Common Table Expressions (CTE). Disini kita bisa menggunakan Percona Server 8.0 sebagai alternatif.

Langkah Installasi

```
sudo apt update
sudo apt install -y mysql-server
```

Keamanan Dasar:
Jalankan skrip berikut untuk mengamankan instalasi bawaan MySQL

```
sudo mysql_secure_installation
```

Konfigurasi Mode MySQL:
Buat file baru di /etc/mysql/conf.d/mysqld.cnf dan masukkan konfigurasi berikut

```
[mysqld]
sql_mode=ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
optimizer_switch='block_nested_loop=off'
```

Restart Layanan:

```
sudo systemctl restart mysql
```

2. Elasticsearch (OSS 7.x)
Elasticsearch digunakan sebagai mesin pencari utama di AtoM. Versi yang digunakan adalah versi Open Source Software (OSS) cabang 7.x.

Instalasi Java (OpenJDK 11) & Dependensi:

```
sudo apt install -y openjdk-11-jre-headless apt-transport-https software-properties-common
```

Menambahkan Repositori Elasticsearch:

```
# Unduh GPG Key
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

# Tambahkan ke Source List
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/oss-7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

Instalasi & Menjalankan Layanan:

```
sudo apt update
sudo apt install -y elasticsearch-oss
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

3. Runtime & Ekstensi: PHP 8.3

Ubuntu 24.04 secara bawaan menyediakan PHP 8.3. Berikut adalah perintah untuk menginstal PHP beserta seluruh ekstensi yang dibutuhkan oleh AtoM:


Instalasi PHP & Ekstensi Utama:

```
sudo apt install -y php-common php8.3-common php8.3-cli php8.3-curl php-json php8.3-ldap php8.3-mysql php8.3-opcache php8.3-readline php8.3-xml php8.3-mbstring php8.3-xsl php8.3-zip php-apcu
```

Ekstensi Tambahan (Opsional - Jika menggunakan Memcached):

```
sudo apt install -y php-memcache
```

4. Gearman Job Server

Gearman diperlukan untuk menangani proses latar belakang (background jobs) pada AtoM versi 2.2 ke atas.

```
sudo apt install -y gearman-job-server
```

5. Paket Pendukung & Pemrosesan Media

Pembuatan PDF (Apache FOP)

Parameter ``` --no-install-recommends ``` digunakan secara sengaja agar sistem tidak menginstal ``` openjdk-8 ``` yang tidak diperlukan, karena kita sudah menginstal OpenJDK 11 sebelumnya.

```
sudo apt install -y --no-install-recommends fop libsaxon-java
```

Penting: Pastikan sistem menggunakan Java 11 sebagai default dengan menjalankan perintah ini:

```
sudo update-java-alternatives -s java-1.11.0-openjdk-amd64
```

Pemrosesan Objek Digital (Gambar, PDF, Video)

Paket-paket ini bersifat opsional namun sangat direkomendasikan agar AtoM dapat membuat pratinjau (thumbnails), memproses gambar JPEG, mengekstrak teks dari PDF, atau memproses video:

```
sudo apt install -y imagemagick ghostscript poppler-utils ffmpeg
```

## Install Atom

1. Unduh Source Code (Pilih salah satu)

Opsi A: Menggunakan Tarball (Direkomendasikan untuk Produksi)

```
wget https://storage.accesstomemory.org/releases/atom-latest.tar.gz
sudo mkdir -p /usr/share/nginx/atom
sudo tar xzf atom-latest.tar.gz -C /usr/share/nginx/atom --strip 1
```

Opsi B: Menggunakan Git Clone (Direkomendasikan untuk Pengembangan)

```
sudo apt install -y git
sudo mkdir -p /usr/share/nginx/atom
sudo git clone -b stable/2.10.x --depth 1 http://github.com/artefactual/atom.git /usr/share/nginx/atom
```

2. Instalasi Dependensi PHP & Tema

Masuk ke direktori root AtoM:

```
cd /usr/share/nginx/atom
```

Instal Pustaka PHP (Composer):

```
# Untuk Produksi:
sudo composer install --no-dev

# Untuk Pengembangan:
sudo composer install
```

Kompilasi Tema (Hanya jika menggunakan Opsi B / Git):

```
sudo apt install -y npm
sudo npm install
sudo npm run build
```

3. Konfigurasi Basis Data MySQL

Eksekusi perintah berikut di terminal untuk membuat database, user baru, dan mengatur hak akses:

```
sudo mysql -h localhost -u root -p -e "CREATE DATABASE atom CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;"
sudo mysql -h localhost -u root -p -e "CREATE USER 'atom'@'localhost' IDENTIFIED BY '12345';"
sudo mysql -h localhost -u root -p -e "GRANT ALL PRIVILEGES ON atom.* TO 'atom'@'localhost';"
```

4. Eksekusi CLI Installer

Jalankan perintah instalasi otomatis via Symfony:

```
cd /usr/share/nginx/atom
sudo php symfony tools:install
```

# Data Input Isian Konfigurasi (Untuk Dokumentasi GitHub)

Saat proses ``` tools:install ``` berjalan, isi terminal dengan parameter berikut:

Kredensial Sistem & Pencarian

Database Host: ``` localhost ```

Database Port: ``` 3306 ```

Database Name: ``` atom ```

Database User: ``` atom ```

Database Password: ``` 12345 ```

Search Host: ``` localhost ```

Search Port: ``` 9200 ```

Search Index: ``` atom ```

Kredensial Admin Pembuat (Rekomendasi Aman)

Penting: Demi keamanan repositori publik di GitHub, jangan gunakan kata sandi default yang mudah ditebak seperti admin/admin123. Gunakan rekomendasi berikut atau buat variasi serupa:

Site Title: ``` Pusat Arsip Digital ``` (Silakan sesuaikan dengan nama institusi/perpustakaan)

Site Base URL: ``` http://localhost ``` (atau domain Anda)

Admin Email: ``` admin@domain.local ```

Admin Username: ``` atom_admin ```

Admin Password: ``` At0M_S3cur3_2026! ```


# Buat Basis Data

Buat database atom dengan charset utf8mb4, menggunakan kata sandi yang dibuat saat [instalasi MySQL](https://www.accesstomemory.org/en/docs/2.10/admin-manual/installation/ubuntu/#installation-ubuntu-dependencies-mysql).

```
sudo mysql -h localhost -u root -p -e "CREATE DATABASE atom CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;"
```

Jika password tidak disertakan setelah `-p`, akan diminta saat perintah dijalankan. Jika disertakan, tulis tanpa spasi (`-pPASSWORD`), meski cara ini kurang aman karena bisa tersimpan di `.bash_history`. Pastikan database dalam kondisi kosong; database lama bisa dihapus dulu dengan `DROP DATABASE` sebelum dibuat ulang.

Buat user MySQL khusus untuk AtoM.

```
sudo mysql -h localhost -u root -p -e "CREATE USER 'atom'@'localhost' IDENTIFIED BY '12345';"
```

Berikan hak akses penuh user tersebut ke database atom.

```
sudo mysql -h localhost -u root -p -e "GRANT ALL PRIVILEGES ON atom.* TO 'atom'@'localhost';"
```

Hak akses `INDEX`, `CREATE`, dan `ALTER` hanya diperlukan saat instalasi/upgrade, dan bisa dicabut setelahnya atau diganti user lain di [config.php](https://www.accesstomemory.org/en/docs/2.10/admin-manual/customization/config-files/#config-config-php).


# Jalankan Installer
Masuk ke direktori AtoM.

```
cd /usr/share/nginx/atom
```

Jalankan installer AtoM.

```
sudo php symfony tools:install
```

Installer akan meminta konfigurasi database dan pencarian. Isi sesuai setup di atas:

- Host basis data: `localhost`
- Port basis data: `3306`
- Nama basis data: `atom`
- Pengguna basis data: `atom`
- Kata sandi basis data: `12345`
- Cari host: `localhost`
- Port pencarian: `9200`
- Indeks pencarian: `atom`

Nilai-nilai ini akan berbeda jika MySQL/Elasticsearch berjalan di server terpisah. Konfigurasi lain (judul situs, deskripsi situs, URL dasar, email admin, username admin, password admin) bisa diisi bebas dan diubah kembali kapan saja lewat Admin > Pengaturan > [Informasi situs](https://www.accesstomemory.org/en/docs/2.10/user-manual/administer/settings/#site-information) atau [edit user](https://www.accesstomemory.org/en/docs/2.10/user-manual/administer/manage-user-accounts/#edit-user). Detail lengkap dan cara otomatisasi ada di [halaman CLI tools](https://www.accesstomemory.org/en/docs/2.10/admin-manual/maintenance/cli-tools/#cli-installer).

## Konfigurasi dan Pertimbangan Keamanan

# Konfigurasi

>Terdapat berbagai pengaturan yang hanya dapat dikonfigurasi melalui baris perintah  misalnya, zona waktu dan budaya default aplikasi.

>Tergantung pada kebutuhan lokal Anda, mungkin lebih baik untuk mengkonfigurasi beberapa di antaranya sekarang.

>Untuk informasi lebih lanjut tentang pengaturan ini, lihat dokumentasi resmi: Kelola file konfigurasi Atom.

# Pertimbangan Keamanan

>Setelah AtoM dikonfigurasi dan diinstal, luangkan waktu sejenak untuk membaca bagian keamanan berikut, yang menunjukkan cara mengkonfigurasi firewall di Ubuntu dan opsi lain dalam konfigurasi AtoM: Bagian Keamanan Atom.

>Catatan penting: Kami sangat menganjurkan pengguna untuk mengkonfigurasi firewall karena beberapa layanan yang dikonfigurasi tidak boleh diekspos ke publik. Contohnya, Elasticsearch tidak dirancang untuk dapat diakses dari jaringan yang tidak tepercaya, dan ini merupakan vektor serangan yang umum.

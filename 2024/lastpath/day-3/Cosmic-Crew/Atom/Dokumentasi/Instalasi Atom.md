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

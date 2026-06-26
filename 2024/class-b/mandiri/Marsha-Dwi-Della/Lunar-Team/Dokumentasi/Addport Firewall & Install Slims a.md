---
# Addport Firewall & Install Slims

## 1. Akses Server & Hak Akses Root
Langkah pertama adalah masuk ke server target melalui SSH dan beralih ke akun administrator (`root`) untuk mengizinkan eksekusi perintah sistem.

```bash
# Masuk ke server melalui SSH
ssh lunarserver@192.168.1.12

# Beralih ke hak akses superuser (root)
sudo su

```
## 2. Konfigurasi Firewall Server
Mengonfigurasi dan membuka beberapa port krusial agar layanan web server (HTTP/HTTPS) dan *database* (MySQL/MariaDB) dapat diakses dari jaringan luar.
```bash
# Membuka layanan HTTP (port 80) secara permanen
firewall-cmd --zone=public --add-service=http --permanent

# Membuka akses database MySQL/MariaDB (port 3306) secara permanen
firewall-cmd --zone=public --add-port=3306/tcp --permanent

# Membuka layanan HTTPS (port 443) secara permanen
firewall-cmd --zone=public --add-port=443/tcp --permanent

# Memuat ulang aturan firewall agar perubahan langsung diterapkan
firewall-cmd --reload

```
## 3. Instalasi Dependensi Sistem
Lakukan instalasi beberapa *package* pembantu dasar seperti wget untuk mengunduh berkas, unzip untuk ekstraksi arsip, serta pengaktifan layanan mesin kontainer podman.
```bash
# Memasang aplikasi wget dan unzip melalui package manager (Pacman)
pacman -S wget unzip

# Mengaktifkan layanan podman agar berjalan secara global saat sistem booting
systemctl enable --global podman

```
## 4. Mengunduh & Mempersiapkan Source Code SLiMS
Mengunduh struktur repositori berkas konfigurasi *Docker Compose* khusus SLiMS dari GitHub, kemudian melakukan ekstraksi serta penyesuaian direktori.
```bash
# Membuat direktori kerja baru
mkdir -p .conf/lunar
cd .conf/lunar

# Mengunduh repositori boilerplate Docker/Podman compose SLiMS
wget -c wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip

# Mengekstrak berkas master.zip
unzip master.zip

# Mengubah nama direktori hasil ekstrak agar lebih ringkas
mv docker-compose-for-slims-master compose
cd compose

```

# Set Kernel

nano /etc/sysctl.d/[nama file].conf

masukkan :
```
net.ipv4.ip_unprivileged_port_start=80
```
Lalu konfirmasi dengan :
```
sysctl --system
```

# Rename

```
mv docker-compose.yaml docker-compose.yaml.[nama rename]
```

```
mv docker-compose-redis.yaml docker-compose.yaml
```

## 5. Konfigurasi Hak Akses & Konfigurasi Kontainer
Sebelum menjalankan kontainer, sesuaikan izin akses direktori aplikasi agar kontainer Apache/PHP dapat memodifikasi berkas SLiMS di dalamnya, serta lakukan penyesuaian berkas .yaml.
```bash
# Memberikan hak akses penuh ke direktori aplikasi SLiMS agar tidak terkendala permission error
chmod -R 777 app/slims

# (Opsional) Mengonfigurasi berkas docker-compose jika diperlukan penyesuaian port atau nama database
nano docker-compose.yaml

```
## 6. Menjalankan Layanan SLiMS Berbasis Kontainer
Menjalankan seluruh arsitektur layanan (Web Server, PHP, dan Database MariaDB) di latar belakang (*background/detached mode*) menggunakan perintah podman compose.
```bash
# Mengaktifkan seluruh kontainer yang didefinisikan dalam docker-compose.yaml
podman compose up -d

# Memeriksa status dan memastikan seluruh kontainer telah berjalan dengan benar
podman ps -a

```

## 7. Masuk ke website
masukkan ip server ke website :80
.
lihat ip :
```
ip a
```

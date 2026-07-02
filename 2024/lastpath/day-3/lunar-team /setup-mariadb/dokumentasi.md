# Dokumentasi setup mariadb

## 1. Masuk ke mode root
```
sudo su
```
Berpindah dari user biasa (syifa) menjadi root, karena instalasi paket dan konfigurasi sistem butuh hak akses superuser. Prompt berubah dari [syifa@assyifa ~]$ menjadi [root@assyifa syifa]

## 2. Update sistem
```
pacman -Syyu
```
Sinkronisasi database paket dan upgrade seluruh sistem. Ini penting dilakukan sebelum instal paket baru supaya tidak ada masalah dependensi/mirror yang belum update.

## 3. Install MariaDB
```
pacman -S mariadb
```
Menginstal MariaDB beserta dependensinya 

## 4.  Inisialisasi data directory MariaDB
```
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```
Membuat struktur tabel sistem dasar MariaDB (wajib dilakukan sebelum service pertama kali dijalankan).
--user=mysql → dijalankan sebagai user sistem mysql
--basedir=/usr → lokasi binary MariaDB
--datadir=/var/lib/mysql → lokasi penyimpanan data database

## 5. Aktifkan dan jalankan service MariaDB
```
systemctl enable --now mariadb
```
Mengaktifkan MariaDB agar jalan otomatis saat boot (enable) dan langsung menyalakannya sekarang (--now).

## 6. Restart service agar konfigurasi baru diterapkan
```
sudo systemctl restart mariadb
```
Restart MariaDB supaya perubahan di server.cnf langsung berlaku.

## 7.  Login ke MariaDB shell
```
sudo mariadb -u root
```
Masuk ke MariaDB monitor sebagai root database, memakai autentikasi unix_socket lewat sudo (tanpa perlu password).

## 8. Buat database
```
sqlCREATE DATABASE slimsdb;
CREATE DATABASE atomdb;
```
Membuat dua database baru: slimsdb dan atomdb.

## 9. Buat user database baru
```
sqlCREATE USER 'lunar'@'%' IDENTIFIED BY '123';
```
Membuat user lunar dengan password bisa login dari host manapun.

## 10. Berikan hak akses ke database
```
sqlGRANT ALL PRIVILEGES ON slimsdb.* TO 'lunar'@'%';
GRANT ALL PRIVILEGES ON atomdb.* TO 'lunar'@'%';
```
Memberi user lunar akses penuh (semua hak) ke database slimsdb dan atomdb.

## 11. Terapkan perubahan privilege
```
sqlFLUSH PRIVILEGES;
```
Memuat ulang tabel privilege supaya perubahan langsung aktif.

## 12. Keluar dari MariaDB shell
```
sqlEXIT;
```
Keluar dari monitor MariaDB, kembali ke terminal.

## 13. Cek apakah MariaDB listening di port yang benar
```
ss -tulnp | grep 3306
```
Mengecek apakah service MariaDB benar-benar listening di port defaultnya, yaitu 3306 (bukan 3706 seperti di log aslinya).

## . Keluar dari sesi root & shell
```
exit
exit
```
Keluar dari sesi root kembali ke user biasa, lalu keluar dari terminal sepenuhnya

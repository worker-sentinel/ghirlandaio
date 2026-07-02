# SERVE ATOM
## izin sistem file
Perintah di bawah ini digunakan untuk mengatur hak akses folder agar aplikasi AtoM dapat berjalan lancar di bawah server web Nginx.
```
sudo chown -R www-data:www-data /usr/share/nginx/atom
```
Mengubah kepemilikan folder instalasi AtoM beserta seluruh isinya secara berulang (recursive) menjadi milik pengguna dan grup www-data (pengguna default Nginx). Ini wajib agar server web bisa menulis dan membaca file aplikasi.
```
sudo chmod o= /usr/share/nginx/atom
```
Menghapus semua hak akses (read, write, execute) untuk pengguna lain (others) demi menjaga keamanan sistem, terutama jika berada di lingkungan server bersama (shared environment).
## Manajemen Pekerja Asinkron (Gearman Worker via Systemd)
Perintah ini digunakan untuk mendaftarkan dan menjalankan worker (tugas latar belakang) menggunakan systemd setelah Anda membuat file konfigurasi atom-worker.service.
```
sudo systemctl daemon-reload
```
Memuat ulang konfigurasi manajer systemd agar sistem mengenali file layanan baru (atom-worker.service) yang baru saja dibuat atau diubah.
```
sudo systemctl enable atom-worker
```
Mengatur agar layanan AtoM worker otomatis berjalan setiap kali server dinyalakan (booting).
```
sudo systemctl start atom-worker
```
Menjalankan layanan AtoM worker saat ini juga di latar belakang untuk mulai memproses tugas-tugas asinkron aplikasi.
## Konfigurasi dan Manajemen PHP-FPM
Langkah-langkah untuk menginstal PHP-FPM, menguji konfigurasinya, dan mengelola pool PHP khusus untuk AtoM.
```
sudo apt install -y php-fpm
```
Mengunduh dan menginstal paket pengelola proses PHP (PHP-FPM) pada sistem operasi Ubuntu.
```
sudo systemctl enable php8.3-fpm
```
Mengatur agar layanan PHP 8.3-FPM otomatis berjalan saat server pertama kali dinyalakan.
```
sudo systemctl start php8.3-fpm
```
Mengaktifkan atau menjalankan layanan PHP 8.3-FPM sekarang juga.
```
sudo php-fpm8.3 --test
```
## Konfigurasi dan Manajemen Nginx
Perintah untuk menginstal Nginx, mengatur VirtualHost (Server Block) khusus AtoM, serta mengaktifkan situsnya.
```
sudo apt install -y nginx
```
Mengunduh dan menginstal server web Nginx di sistem Ubuntu.
```
sudo touch /etc/nginx/sites-available/atom
```
Membuat sebuah file kosong baru bernama atom di direktori situs yang tersedia untuk nantinya diisi dengan konfigurasi blok server AtoM.
```
sudo ln -sf /etc/nginx/sites-available/atom /etc/nginx/sites-enabled/atom
```
Membuat tautan simbolis (symbolic link/symlink) dari file konfigurasi di sites-available ke folder sites-enabled. Perintah ini berfungsi untuk mengaktifkan situs AtoM pada Nginx.
```
sudo rm /etc/nginx/sites-enabled/default
```
Menghapus tautan konfigurasi situs bawaan (default) Nginx agar tidak bentrok dengan konfigurasi situs AtoM yang baru.
```
sudo systemctl enable nginx
```
Mengatur agar server web Nginx otomatis berjalan setiap kali sistem operasi dinyalakan.
```
sudo systemctl reload nginx
```
Memuat ulang konfigurasi Nginx untuk menerapkan pengaturan blok server AtoM yang baru tanpa perlu mematikan koneksi yang sedang berjalan (tanpa downtime).

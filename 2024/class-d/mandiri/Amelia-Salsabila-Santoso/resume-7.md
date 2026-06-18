# Resume Kelompok 7
## Nama: Amelia Salsabila Santoso
## NIM: 12402051030089

## Kernel: 
Merupakan inti dari sistem operasi yang bertugas mengubah instruksi menjadi bahasa mesin (biner) agar dapat diproses oleh hardware. Linux adalah kernel, bukan sistem operasi secara keseluruhan. Penggunaan initramfs melalui Booster membantu memuat driver dan konfigurasi yang diperlukan oleh kernel saat proses booting, termasuk untuk membuka partisi terenkripsi.

## LUKS & LVM:
LUKS adalah metode enkripsi tingkat perangkat keras (disk encryption). Jika harddisk dicuri atau dibongkar, data tidak akan terbaca tanpa kata sandi/kunci. Sementara itu, LVM (Logical Volume Management) digunakan untuk menyatukan kapasitas dari beberapa harddisk fisik menjadi satu kesatuan logikal yang utuh, sehingga tidak terbatasi oleh ukuran fisik satu disk saja.

## Manajemen Password (KeePassXC & Secret Service):
Password harus bersifat unik. KeePassXC dan Secret Service berfungsi sebagai manajer password terpusat untuk menyimpan kredensial sensitif. Keduanya memastikan keamanan autentikasi tanpa pengguna harus mengingat banyak password.

## Automasi & Kontainer (Podman):
Podman digunakan untuk menjalankan aplikasi dalam kontainer yang terisolasi. Ini mencegah konflik antar-aplikasi (misalnya port web server yang bertabrakan) dan mendukung otomatisasi (deploy dan run otomatis) sehingga memudahkan pengelolaan layanan server.

## Antarmuka Pengguna (GUI vs CLI): 
Server pada umumnya menggunakan CLI (Command Line Interface) untuk efisiensi. Untuk mempermudah navigasi file berbasis terminal, digunakan Superfile (seperti Windows Explorer tetapi untuk terminal). Sedangkan XFCE4 adalah Desktop Environment (GUI) yang ringan, cocok jika sistem tetap membutuhkan antarmuka grafis.
​
## Keamanan Jaringan (Firewall & CIS):
iptables adalah firewall untuk mengatur lalu lintas jaringan dengan menentukan port mana yang terbuka atau tertutup guna membatasi akses yang tidak sah. CIS (Center for Internet Security) digunakan sebagai panduan standar konfigurasi keamanan sistem agar server memiliki tingkat keamanan yang diakui.

## Remote Access (OpenSSH): 
Protokol standar untuk mengakses server secara jarak jauh dengan aman melalui koneksi terenkripsi.
​Multimedia (MPD, MPC, MPV): Komponen untuk mengelola multimedia; MPD sebagai daemon pemutar di latar belakang, MPC sebagai client pengontrol, dan MPV sebagai pemutar media player.

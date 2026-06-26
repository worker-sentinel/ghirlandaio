## Resume Kelompok 7
## Nama: Puput Trimaililah
## Nim: 12402051010022
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.

### Superfile

Superfile adalah file manager berbasis terminal (TUI) yang memudahkan pengelolaan file dan direktori. Dengan tampilan yang lebih visual dibandingkan perintah terminal biasa, pengguna dapat melakukan operasi seperti menyalin, memindahkan, menghapus, dan mengganti nama file dengan lebih cepat dan efisien.

### LVM on LUKS dan LUKS on LVM

LVM on LUKS adalah konfigurasi di mana partisi fisik dienkripsi terlebih dahulu menggunakan LUKS, kemudian LVM dibuat di atasnya. Dengan cara ini, cukup satu kali memasukkan kunci enkripsi untuk mengakses seluruh logical volume. Pendekatan ini lebih sederhana dan cocok untuk sistem yang seluruh datanya diakses secara bersamaan.

LUKS on LVM merupakan kebalikannya, yaitu LVM dibuat terlebih dahulu lalu setiap logical volume dienkripsi secara terpisah menggunakan LUKS. Setiap volume memiliki kunci enkripsi sendiri sehingga memberikan kontrol akses yang lebih spesifik dan keamanan yang lebih tinggi untuk data sensitif.

### MPD, MPV, dan MPC

MPD (Music Player Daemon) adalah layanan pemutar musik yang berjalan di latar belakang dan mengelola koleksi audio. MPC (Music Player Client) digunakan untuk mengontrol MPD melalui terminal, seperti memutar atau mengganti lagu. Sementara itu, MPV adalah pemutar media yang mendukung berbagai format audio dan video. Ketiganya sering digunakan untuk kebutuhan multimedia pada sistem Linux, baik desktop maupun server minimalis.

### Podman

Podman adalah platform container yang digunakan untuk menjalankan aplikasi dalam lingkungan terisolasi. Berbeda dengan Docker, Podman bersifat daemonless dan mendukung mode rootless sehingga lebih aman. Podman cocok digunakan untuk menjalankan berbagai layanan server secara bersamaan tanpa konflik serta dapat diintegrasikan dengan systemd untuk otomatisasi.

### XFCE4

XFCE4 adalah desktop environment Linux yang ringan dan hemat sumber daya. Dibandingkan GNOME atau KDE, XFCE4 membutuhkan RAM dan CPU yang lebih sedikit tetapi tetap menyediakan antarmuka yang nyaman digunakan. Karena itu, XFCE4 sering dipilih untuk komputer berspesifikasi rendah maupun server yang memerlukan antarmuka grafis.

### iptables

iptables adalah utilitas firewall Linux yang digunakan untuk mengatur lalu lintas jaringan melalui framework Netfilter. Administrator dapat menentukan port, alamat IP, dan protokol yang diizinkan atau diblokir. Dengan konfigurasi yang tepat, iptables membantu melindungi server dari akses tidak sah dan berbagai serangan jaringan.

### KeePassXC dan Secret Service

KeePassXC adalah password manager open source yang menyimpan kata sandi dalam database terenkripsi. Sementara itu, Secret Service (seperti GNOME Keyring atau KDE Wallet) merupakan standar Linux untuk menyimpan kredensial secara aman agar dapat digunakan oleh berbagai aplikasi. Keduanya dapat bekerja bersama untuk mengelola password, kunci enkripsi, maupun kredensial login secara aman dan otomatis.

### OpenSSH

OpenSSH adalah implementasi open source dari protokol SSH yang memungkinkan akses jarak jauh ke server secara aman melalui koneksi terenkripsi. Selain remote login, OpenSSH juga mendukung transfer file menggunakan SCP dan SFTP serta port forwarding. Untuk keamanan yang lebih baik, penggunaan autentikasi SSH key lebih disarankan dibandingkan password.

### CIS (Center for Internet Security)

CIS adalah organisasi yang menyediakan standar dan panduan keamanan IT melalui CIS Benchmarks. Panduan ini berisi rekomendasi konfigurasi keamanan untuk berbagai sistem, termasuk Linux, seperti pengaturan firewall, manajemen pengguna, enkripsi, dan audit sistem. CIS banyak dijadikan acuan untuk meningkatkan keamanan dan kepatuhan infrastruktur teknologi informasi.

### Booster

Booster adalah alat pembuat initramfs yang dirancang lebih cepat dan ringan dibandingkan alternatif lain seperti mkinitcpio atau dracut. Booster membantu proses booting Linux dengan menyediakan komponen yang dibutuhkan sejak awal, termasuk dukungan untuk membuka partisi terenkripsi LUKS sebelum sistem utama dijalankan.



# Nama : Hana Maulida
# Kelas : 4D
# NIM : 12402051050152
# Mata Kuliah : Perpustakaan dan Arsip Digital
# Resume Materi Kelompok 7

# Podman

Podman adalah *container runtime* yang bersifat *daemonless* dan mendukung penggunaan tanpa hak root (*rootless*). Dibandingkan Docker, Podman menawarkan keamanan yang lebih baik karena tidak memerlukan daemon yang berjalan terus-menerus. Podman sering digunakan untuk menjalankan beberapa layanan seperti MySQL atau SLiMS dalam container yang terisolasi dan dapat diotomatisasi menggunakan *systemd*.

# LVM on LUKS dan LUKS on LVM

LVM on LUKS dan LUKS on LVM merupakan dua metode menggabungkan enkripsi LUKS dengan manajemen volume LVM. Pada **LVM on LUKS**, partisi fisik dienkripsi terlebih dahulu menggunakan LUKS, kemudian LVM dibuat di atasnya. Dengan cara ini, cukup satu kali membuka enkripsi untuk mengakses seluruh logical volume. Metode ini cocok untuk sistem yang seluruh datanya digunakan secara bersamaan.

Sebaliknya, pada **LUKS on LVM**, LVM dibuat terlebih dahulu lalu setiap logical volume dienkripsi secara terpisah menggunakan LUKS. Setiap volume memiliki kunci enkripsi sendiri sehingga memberikan tingkat keamanan yang lebih rinci. Metode ini sesuai untuk penyimpanan data sensitif yang memerlukan pemisahan akses antar volume.

# KeePassXC dan Secret Service

KeePassXC adalah password manager *open source* yang menyimpan kata sandi dalam database terenkripsi. Sementara itu, Secret Service merupakan standar pada Linux yang memungkinkan aplikasi menyimpan dan mengambil kredensial secara aman. Keduanya dapat digunakan untuk mengelola password maupun kunci enkripsi. KeePassXC juga mendukung integrasi dengan Secret Service sehingga aplikasi lain dapat mengakses kredensial secara otomatis dan aman.

# XFCE4

XFCE4 adalah *desktop environment* Linux yang ringan dan hemat sumber daya. Dibandingkan GNOME atau KDE, XFCE4 membutuhkan RAM dan CPU yang lebih sedikit namun tetap menyediakan antarmuka grafis yang nyaman. Karena itu, XFCE4 cocok digunakan pada komputer lama maupun server yang memerlukan tampilan desktop sederhana.


# iptables

iptables merupakan utilitas *firewall* Linux yang digunakan untuk mengatur lalu lintas jaringan melalui Netfilter. Administrator dapat menentukan port, alamat IP, dan protokol yang diizinkan atau diblokir. Dengan konfigurasi yang tepat, iptables membantu melindungi server dari akses tidak sah, *port scanning*, dan berbagai serangan jaringan.

# Superfile

Superfile adalah *file manager* berbasis terminal (TUI) yang mempermudah pengelolaan file tanpa antarmuka grafis. Aplikasi ini menyediakan navigasi direktori yang lebih nyaman serta mendukung operasi seperti *copy*, *move*, *rename*, *delete*, dan *preview* file langsung dari terminal, sehingga meningkatkan produktivitas administrator server.

# OpenSSH

OpenSSH adalah implementasi *open source* dari protokol SSH yang memungkinkan akses jarak jauh ke server secara aman. Seluruh komunikasi dienkripsi sehingga terlindung dari penyadapan. Selain *remote login*, OpenSSH juga menyediakan fitur transfer file melalui SCP dan SFTP serta mendukung autentikasi menggunakan password maupun SSH key.

# CIS (Center for Internet Security)

CIS adalah organisasi yang menyediakan standar dan panduan keamanan untuk berbagai sistem IT. CIS Benchmarks berisi rekomendasi konfigurasi keamanan, mulai dari firewall, manajemen pengguna, enkripsi, hingga audit sistem. Standar ini banyak digunakan oleh institusi dan perusahaan sebagai acuan penerapan keamanan yang baik.

# MPD, MPV, dan MPC

MPD, MPV, dan MPC merupakan tools multimedia yang sering digunakan pada Linux. MPD berfungsi sebagai server musik yang berjalan di latar belakang, MPC digunakan untuk mengontrol MPD melalui terminal, sedangkan MPV adalah pemutar media yang mendukung berbagai format audio dan video. Ketiganya dapat digunakan bersama untuk mengelola multimedia pada sistem desktop maupun server minimalis.

# Booster

Booster adalah alat pembuat *initramfs* yang lebih ringan dan cepat dibandingkan mkinitcpio atau dracut. Tool ini membantu proses *booting* Linux dengan menyediakan komponen yang diperlukan sebelum sistem file utama dimuat. Booster juga mendukung pembukaan partisi LUKS saat *boot* sehingga cocok digunakan pada sistem dengan enkripsi disk.


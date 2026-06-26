##### Nama        : Zaskia Syahda Hafizha Kurniawan
##### NIM         : 12402051050151
##### Kelas       : 4D
##### Mata Kuliah : Perpustakaan dan Arsip Digital

## **RANGKUMAN MATERI KELOMPOK 7**

### **LVM on LUKS dan LUKS on LVM**
LVM on LUKS dan LUKS on LVM sama-sama digunakan untuk menggabungkan enkripsi LUKS dengan manajemen storage LVM, tapi cara kerjanya berbeda. LVM on LUKS berarti partisi dienkripsi dulu menggunakan LUKS, baru di dalamnya dibuat LVM. Jadi cukup buka satu enkripsi untuk mengakses seluruh logical volume yang ada. Model ini lebih praktis dan cocok digunakan pada sistem seperti perpustakaan digital yang membutuhkan akses data secara cepat dan bersamaan. LUKS on LVM berarti LVM dibuat terlebih dahulu, lalu setiap logical volume dienkripsi secara terpisah menggunakan LUKS. Karena setiap volume memiliki enkripsi sendiri, tingkat keamanannya lebih tinggi dan cocok digunakan pada sistem arsip yang membutuhkan perlindungan data lebih detail.

### **KeePassXC dan Secret Service**
KeePassXC dan Secret Service berfungsi untuk menyimpan serta mengelola password secara aman. Password yang disimpan dapat digunakan sebagai kunci enkripsi (encryption key) maupun kunci autentikasi (authentication key). Dengan adanya tools ini, pengguna tidak perlu mengingat banyak password sekaligus karena semuanya tersimpan dalam sistem yang terenkripsi dan lebih aman.

### **Podman**
Podman merupakan tools untuk menjalankan aplikasi dalam container yang terisolasi. Podman membantu menjalankan berbagai layanan seperti MySQL atau SLiMS tanpa terjadi konflik antar aplikasi, terutama pada penggunaan port. Selain itu, Podman juga mendukung otomatisasi sehingga container dapat berjalan otomatis ketika server dinyalakan.

### **XFCE4**
XFCE4 adalah desktop environment atau desktop manager pada Linux yang terkenal ringan dan hemat resource. Karena penggunaan RAM dan CPU yang relatif kecil, XFCE4 cocok digunakan pada komputer dengan spesifikasi rendah maupun server yang tetap membutuhkan antarmuka grafis.

### **Superfile**
Superfile adalah file manager berbasis terminal yang mempermudah pengelolaan file di server. Dengan Superfile, pengguna dapat melakukan navigasi, copy, move, rename, maupun menghapus file dengan lebih mudah dibandingkan hanya menggunakan perintah terminal biasa.

### **iptables**
iptables merupakan firewall pada Linux yang digunakan untuk mengatur lalu lintas jaringan. Administrator dapat menentukan port mana yang boleh dibuka dan diakses, serta memblokir port yang tidak diperlukan. Dengan begitu, iptables membantu mencegah akses tidak sah, serangan, maupun upaya peretasan terhadap server.

### **CIS**
CIS (Center for Internet Security) adalah standar dan panduan keamanan yang digunakan sebagai acuan dalam mengamankan sistem. CIS berisi berbagai rekomendasi konfigurasi keamanan yang perlu diterapkan agar server atau sistem dapat mencapai tingkat keamanan tertentu sesuai standar yang berlaku.

### **OpenSSH**
OpenSSH digunakan untuk mengakses komputer atau server lain dari jarak jauh dengan aman. Semua komunikasi yang dilakukan melalui OpenSSH menggunakan enkripsi sehingga data yang dikirimkan lebih terlindungi. Selain remote access, OpenSSH juga dapat digunakan untuk transfer file dan berbagai kebutuhan administrasi server lainnya.

### **Booster**
Booster adalah tools pembuat initramfs yang digunakan saat proses booting Linux. Booster membantu menyiapkan driver dan konfigurasi yang diperlukan kernel ketika sistem dinyalakan. Selain itu, Booster juga mendukung penggunaan partisi terenkripsi LUKS sehingga sistem dapat membuka partisi tersebut saat proses boot berlangsung.

### **MPD, MPV, dan MPC**
MPD, MPV, dan MPC merupakan tools multimedia pada Linux.
1. MPD (Music Player Daemon) berfungsi sebagai layanan pemutar musik yang berjalan di background.
2. MPC (Music Player Client) digunakan untuk mengontrol MPD melalui terminal.
3. MPV merupakan media player yang dapat digunakan untuk memutar musik, video, maupun gambar.
Ketiga tools ini memungkinkan pengelolaan dan pemutaran multimedia secara efisien, baik pada server maupun desktop Linux.

##### Nama : Luthfah Nurussalamah
##### NIM : 12402051030106
##### Kelas : 4D

## **Resume Materi Kelompok 7**

### **Podman**
Platform untuk menjalankan container yang kompatibel dengan banyak workform Docker. Tidak menggunakan daemon pusat dan mendukung mode rootles, sehingga container dapat berjalan tanpa akses root. Sering dipilih karena pendekatan keamanannya yang lebih ketat.

### **Superfiles**
File manager berbais terminal dengan antarmuka modern dan navigasi keyboard yang cepat. Mendukung preview file, pencarian, copy-paste, dan berbagai fitur yang mempermudah pengelolaan file dari CLI.

### **XFCE4**
Desktop environtment Linux yang ringan, stabil, dan hemat RAM. Menyediakan fitur desktop lengkap tanpa banyak efek visual. Cocok untuk komputer lama maupun pengguna yang mengutamakan efisiensi.

### **KeePassXC**
Manajer kata sandi open-source yang menyimpan seluruh kredensial dalam database terenskripsi. Mendukung password, TOTP, SSH key, dan integrasi browser. Semua data tetap berada di perangkat pengguna.

### **Secret Service**
Standar penyimpanan rahasia di Linux yang memungkinkan aplikasi menyimpan password, token, dan kredensial secara aman. Biasanya dijalankan oleh GNOME Keyring atau KWallet dan diakses melalui D-Bus.

### **Iptables**
Firewall Linux yang berfungsi mengatur lalulintas jaringan. Firewall menentukan port mana yang boleh diakses dan port mana yang harus ditutup, shingga mengurangi resiko akses tidak sah ke sistem.

### **CIS**
Organisasi yang membuat panduan keamanan seperti CIS Benchmarks dan CIS Controls. Dokumen ini digunakan sebagai standar hardening server, sistem operasi, dan infrastruktur jaringan.

### **OpenSSH**
Implementasi SSH yang digunakan untuk mengakses server dari jarak jauh secara aman. Mendukung login terminal, transfer file, manajemen server, dan autentikasi menggunakan password maupun SSH key.

### **Booster**
Generator initramfs yang bertugas membuat image boot Linux. Image ini berisi komponen yang dibutuhkan kernel untuk mulai menjalankan sistem sebelum sistem operasi sepenuhnya aktif.

### **LVM on LUKS**
Disk dienkripsi terlebih dahulu menggunakan LUKS, lalu ruang terenkripsi tersebut dikelola oleh LVM. Semua logical volume yang dibuat didalamnya otomatis ikut terenskripsi. Ini yang paling umum dipakai untuk desktop dan server.

### **LUKS on LVM**
LVM dibuat terlebih dahulu, kemudian logical volume tertentu dienkripsi menggunakan LUKS. Cocok jika hanya sebagian volume yang perlu enkripsi atau ingin menggunakan password berbeda untuk volume yang berbeda.

### **MPD, MPV, MPC**
1. MPD : Server musik yang berjalan di latar belakang dan mengelola perpustakaan dan pemutaran audio. MPD tidak memiliki antarmuka sendiri dan biasanya dikendalikan melalui client.
2. MPV : Pemutar media ringan yang mendukung hampir semua format audio dan video modern. Dikenal karena performa tinggi, konfigurasi fleksibel, dan sering digunakan sebagai backend pemutar multimedia.
3. MPC : Client command-line untuk mengontrol MPD. Denagn MPC, pengguna dapat memutar, menghentikan, mengganti lagu, mengatur play;ist, dan mengubah volume langsung dari terminal.

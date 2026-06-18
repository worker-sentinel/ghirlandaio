# Resume Materi Pertemuan ke-7
> Sistem Operasi & Keamanan Linux — Infrastruktur Server
 
---
 
## LVM on LUKS dan LUKS on LVM
 
LVM on LUKS dan LUKS on LVM adalah dua pendekatan berbeda dalam menggabungkan enkripsi disk menggunakan LUKS (Linux Unified Key Setup) dengan manajemen volume logis menggunakan LVM (Logical Volume Manager). Pada konfigurasi LVM on LUKS, partisi fisik dienkripsi terlebih dahulu menggunakan LUKS, kemudian LVM dibangun di atasnya. Ini berarti hanya diperlukan satu kunci enkripsi untuk membuka seluruh partisi sekaligus, sehingga semua logical volume di dalamnya dapat langsung diakses tanpa perlu membuka satu per satu. Konfigurasi ini lebih cocok untuk lingkungan seperti sistem perpustakaan digital, di mana semua data umumnya diakses secara bersamaan dan tidak memerlukan isolasi keamanan yang ketat antar volume.
 
Sebaliknya, pada konfigurasi LUKS on LVM, LVM dibuat terlebih dahulu di atas partisi fisik, kemudian setiap logical volume dienkripsi secara individual menggunakan LUKS masing-masing. Artinya, setiap volume memiliki kunci enkripsinya sendiri dan harus dibuka secara terpisah satu per satu. Pendekatan ini memberikan keamanan yang jauh lebih granular karena kompromi pada satu volume tidak akan langsung mengekspos volume lainnya. Oleh karena itu, LUKS on LVM lebih cocok digunakan pada sistem arsip atau penyimpanan data sensitif, di mana setiap kelompok data memerlukan perlindungan dan kontrol akses yang independen satu sama lain.
 
---
 
## KeePassXC dan Secret Service
 
KeePassXC adalah aplikasi password manager lokal berbasis open source yang dirancang untuk menyimpan dan mengelola kata sandi secara aman dalam sebuah database terenkripsi. Secret Service, seperti GNOME Keyring atau KDE Wallet, adalah antarmuka standar pada sistem Linux yang memungkinkan berbagai aplikasi menyimpan dan mengambil kredensial secara terproteksi dari sistem tanpa harus mengeksposnya dalam bentuk teks biasa. Keduanya memiliki fungsi yang saling melengkapi, yaitu menjaga dan menyimpan password agar dapat digunakan baik sebagai encryption key untuk membuka volume terenkripsi seperti partisi LUKS, maupun sebagai authentication key untuk login ke aplikasi, layanan SSH, atau database. KeePassXC bahkan dapat berintegrasi langsung dengan Secret Service API sehingga aplikasi lain di sistem dapat mengambil kredensial secara otomatis dan aman tanpa intervensi manual dari pengguna setiap saat.
 
---
 
## Podman
 
Podman adalah container runtime yang digunakan untuk menjalankan aplikasi di dalam container yang terisolasi satu sama lain. Berbeda dengan Docker, Podman bersifat daemonless dan mendukung mode rootless, sehingga lebih aman untuk digunakan di lingkungan server produksi. Dalam konteks pengelolaan server, Podman sangat berguna untuk menjalankan layanan seperti MySQL atau SLiMS (Senayan Library Management System) secara bersamaan tanpa menimbulkan konflik antar port, karena setiap layanan berjalan di dalam container dengan konfigurasi port mapping masing-masing yang terisolasi. Podman juga mendukung otomasi melalui integrasi dengan systemd, sehingga container dapat dikonfigurasi untuk berjalan secara otomatis setiap kali server dinyalakan tanpa perlu intervensi manual dari administrator.
 
---
 
## XFCE4
 
XFCE4 adalah desktop environment untuk sistem operasi Linux yang dikenal ringan dan efisien dalam penggunaan sumber daya sistem seperti RAM dan CPU. Dibandingkan dengan desktop environment lain yang lebih berat seperti GNOME atau KDE, XFCE4 menawarkan antarmuka grafis yang fungsional dan responsif bahkan pada mesin dengan spesifikasi terbatas. Hal ini menjadikannya pilihan yang tepat untuk digunakan pada server yang juga memerlukan antarmuka visual, atau pada komputer lama yang masih ingin dioperasikan secara optimal. XFCE4 banyak digunakan pada distribusi Linux seperti Debian, Xubuntu, dan Kali Linux sebagai desktop environment default atau alternatif.
 
---
 
## Superfile
 
Superfile adalah terminal file manager modern yang berbasis TUI (Text User Interface), dirancang untuk memudahkan navigasi dan pengelolaan file langsung dari dalam terminal. Di lingkungan server yang umumnya tidak memiliki antarmuka grafis, mengelola file hanya dengan perintah dasar seperti `ls`, `cp`, dan `mv` bisa menjadi kurang efisien terutama saat berhadapan dengan struktur direktori yang kompleks. Superfile hadir untuk mengatasi hal tersebut dengan menyediakan tampilan visual berbasis panel yang intuitif, lengkap dengan dukungan operasi file umum seperti copy, move, delete, rename, dan preview langsung di dalam terminal. Dengan demikian, administrator server dapat bekerja lebih cepat dan produktif tanpa harus meninggalkan lingkungan terminal.
 
---
 
## iptables
 
iptables adalah utilitas Linux yang digunakan untuk mengkonfigurasi firewall berbasis kernel melalui framework Netfilter, dengan tujuan mengontrol lalu lintas jaringan yang masuk dan keluar dari server. Dengan iptables, administrator dapat mendefinisikan aturan (rules) secara presisi untuk menentukan port mana yang boleh diakses, dari alamat IP mana, dan melalui protokol apa. Port yang tidak diperlukan dapat diblokir secara default untuk mencegah akses tidak sah, sementara port layanan yang memang dibutuhkan seperti port 22 untuk SSH atau port 80 dan 443 untuk layanan web dapat dibuka secara selektif. Dengan konfigurasi yang tepat, iptables menjadi lapisan pertahanan pertama yang efektif dalam mencegah upaya peretasan, port scanning, dan eksploitasi layanan yang tidak seharusnya terekspos ke publik. iptables juga kerap digunakan bersama tools seperti fail2ban untuk memberikan perlindungan tambahan terhadap serangan brute force.
 
---
 
## CIS (Center for Internet Security)
 
CIS atau Center for Internet Security adalah organisasi yang menyediakan kumpulan benchmark dan panduan keamanan berbasis standar industri yang dirancang untuk membantu administrator mengamankan infrastruktur IT mereka. CIS Benchmarks mencakup rekomendasi konfigurasi keamanan yang sangat rinci untuk berbagai sistem operasi, termasuk Linux, mencakup aspek seperti pengaturan firewall, manajemen user dan hak akses, enkripsi data, hingga auditing dan logging aktivitas sistem. Panduan ini bersifat berbasis enkripsi dan harus diikuti secara sistematis agar sistem dapat mencapai tingkat keamanan (compliance level) tertentu yang diakui secara internasional. CIS Benchmarks banyak dijadikan acuan oleh institusi pemerintah, lembaga pendidikan, dan perusahaan untuk memastikan infrastruktur mereka memenuhi standar keamanan yang telah teruji dan diakui.
 
---
 
## OpenSSH
 
OpenSSH adalah implementasi open source dari protokol SSH (Secure Shell) yang memungkinkan pengguna mengakses komputer atau server lain dari jarak jauh melalui jaringan secara aman dan terenkripsi. Seluruh komunikasi yang terjadi melalui OpenSSH dienkripsi secara end-to-end, sehingga terlindungi dari ancaman penyadapan atau serangan man-in-the-middle. Selain untuk remote login, OpenSSH juga mendukung transfer file aman melalui perintah `scp` dan `sftp`, serta fitur port forwarding yang dapat digunakan untuk mengamankan koneksi aplikasi lain melalui tunnel SSH. Dalam hal autentikasi, OpenSSH mendukung dua metode utama yaitu password dan SSH key pair, di mana penggunaan SSH key pair jauh lebih direkomendasikan karena memberikan keamanan yang lebih kuat sekaligus kemudahan akses tanpa perlu memasukkan password setiap saat.
 
---
 
## Booster
 
Booster adalah alat pembuat initramfs (initial RAM filesystem) yang dirancang untuk bekerja lebih cepat dan lebih ringan dibandingkan tools serupa seperti mkinitcpio atau dracut. initramfs sendiri adalah sistem file sementara yang dimuat oleh kernel Linux pada tahap awal proses booting, berisi driver dan tools minimal yang diperlukan sebelum sistem file root utama dapat di-mount. Booster berperan penting dalam skenario penggunaan enkripsi disk, karena ia mendukung proses pembukaan partisi LUKS saat booting — memungkinkan sistem meminta passphrase dari pengguna sebelum melanjutkan proses boot ke sistem operasi utama. Konfigurasi Booster relatif lebih sederhana dan proses pembuatan image initramfs-nya jauh lebih cepat, menjadikannya pilihan yang efisien terutama pada sistem yang menggunakan enkripsi penuh berbasis LUKS.
 
---
 
## MPD, MPV, dan MPC
 
MPD (Music Player Daemon), MPV, dan MPC (Music Player Client) adalah tiga tools yang membentuk ekosistem multimedia di lingkungan Linux, khususnya untuk digunakan di terminal atau server tanpa antarmuka grafis penuh. MPD adalah server musik yang berjalan di background sebagai daemon, bertugas mengelola library musik dan menangani proses pemutaran audio secara terus-menerus tanpa memerlukan interaksi langsung dari pengguna. Untuk mengontrol MPD, digunakan MPC yang merupakan klien berbasis command-line, memungkinkan pengguna menjalankan perintah seperti play, pause, skip, atau mengatur playlist langsung dari terminal dengan cepat dan efisien. Sementara itu, MPV adalah pemutar media serbaguna yang mendukung berbagai format video, audio, maupun gambar, dan dapat dijalankan baik dari terminal maupun antarmuka grafis sederhana. Kombinasi ketiga tools ini memungkinkan pengelolaan dan pemutaran multimedia secara lengkap di lingkungan server maupun desktop minimalis berbasis Linux.

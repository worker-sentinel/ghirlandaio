# Menginstall Linux Kelompok 7
## Nama: Cinta Husnadzan Dzikri
## NIM: 12402051050150
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum
### LVM on LUKS dan LUKS on LVM

LVM on LUKS dan LUKS on LVM merupakan dua metode berbeda untuk menggabungkan teknologi enkripsi disk LUKS (Linux Unified Key Setup) dengan manajemen volume logis menggunakan LVM (Logical Volume Manager). Pada metode **LVM on LUKS**, partisi fisik terlebih dahulu dienkripsi menggunakan LUKS, kemudian LVM dibuat di atas partisi yang sudah terenkripsi tersebut. Dengan cara ini, pengguna hanya perlu memasukkan satu kunci enkripsi untuk membuka seluruh partisi sehingga semua logical volume di dalamnya dapat langsung digunakan. Pendekatan ini cocok diterapkan pada lingkungan seperti perpustakaan digital karena seluruh data biasanya diakses secara bersamaan dan tidak membutuhkan pemisahan keamanan yang terlalu detail antar volume.

Sebaliknya, pada metode **LUKS on LVM**, LVM dibuat terlebih dahulu pada partisi fisik, lalu setiap logical volume dienkripsi secara terpisah menggunakan LUKS. Setiap volume memiliki kunci enkripsinya sendiri sehingga harus dibuka satu per satu. Pendekatan ini memberikan tingkat keamanan yang lebih rinci karena apabila satu volume berhasil diakses oleh pihak yang tidak berwenang, volume lainnya tetap terlindungi. Oleh sebab itu, metode ini lebih sesuai untuk sistem arsip atau penyimpanan data sensitif yang membutuhkan pengelolaan akses dan perlindungan secara terpisah.

### KeePassXC dan Secret Service

KeePassXC merupakan aplikasi manajemen kata sandi berbasis open source yang menyimpan berbagai kredensial dalam database terenkripsi di komputer lokal. Sementara itu, Secret Service seperti GNOME Keyring atau KDE Wallet menyediakan standar penyimpanan kredensial pada sistem Linux agar aplikasi dapat menyimpan dan mengambil data autentikasi secara aman tanpa menampilkan informasi tersebut dalam bentuk teks biasa. Keduanya saling melengkapi dalam menjaga keamanan password yang digunakan baik sebagai kunci enkripsi untuk membuka volume terenkripsi seperti LUKS maupun sebagai kredensial autentikasi untuk aplikasi, layanan SSH, atau database. KeePassXC juga dapat terhubung dengan Secret Service API sehingga aplikasi lain dapat menggunakan kredensial yang tersimpan secara otomatis dan aman tanpa perlu memasukkannya berulang kali.

### Podman

Podman adalah container runtime yang berfungsi untuk menjalankan aplikasi di dalam container yang terisolasi. Berbeda dengan Docker yang menggunakan daemon, Podman bekerja secara daemonless dan mendukung mode rootless sehingga dianggap lebih aman untuk lingkungan server produksi. Dalam pengelolaan server, Podman memudahkan administrator menjalankan beberapa layanan seperti MySQL dan SLiMS (Senayan Library Management System) secara bersamaan tanpa terjadi konflik port karena setiap layanan berada dalam container dengan pengaturan port masing-masing. Selain itu, Podman dapat diintegrasikan dengan systemd sehingga container dapat dijalankan otomatis saat server dinyalakan.

### XFCE4

XFCE4 adalah desktop environment Linux yang terkenal ringan dan hemat penggunaan sumber daya seperti RAM maupun CPU. Jika dibandingkan dengan GNOME atau KDE yang lebih berat, XFCE4 mampu memberikan pengalaman antarmuka grafis yang tetap cepat dan responsif meskipun dijalankan pada perangkat dengan spesifikasi rendah. Karena keunggulan tersebut, XFCE4 sering dipilih untuk server yang membutuhkan tampilan grafis atau komputer lama yang masih digunakan. Desktop environment ini banyak ditemukan pada distribusi Linux seperti Debian, Xubuntu, dan Kali Linux.

### Superfile

Superfile merupakan file manager modern berbasis TUI (Text User Interface) yang dirancang untuk mempermudah pengelolaan file langsung melalui terminal. Pada lingkungan server yang umumnya tidak memiliki antarmuka grafis, penggunaan perintah dasar seperti `ls`, `cp`, atau `mv` terkadang kurang praktis ketika harus menangani struktur direktori yang kompleks. Superfile menyediakan tampilan visual berbasis panel yang lebih intuitif dan mendukung berbagai operasi file seperti menyalin, memindahkan, menghapus, mengganti nama, serta melihat pratinjau file secara langsung dari terminal. Dengan demikian, pekerjaan administrasi file dapat dilakukan dengan lebih cepat dan efisien.

### iptables

iptables adalah utilitas Linux yang digunakan untuk mengatur firewall melalui framework Netfilter di kernel Linux. Fungsinya adalah mengontrol lalu lintas jaringan yang masuk maupun keluar dari server. Dengan iptables, administrator dapat membuat aturan yang menentukan port, alamat IP, dan protokol yang diizinkan untuk mengakses sistem. Port yang tidak diperlukan dapat ditutup, sedangkan port yang dibutuhkan seperti port 22 untuk SSH atau port 80 dan 443 untuk layanan web dapat dibuka sesuai kebutuhan. Jika dikonfigurasi dengan baik, iptables menjadi lapisan keamanan awal yang efektif untuk mengurangi risiko peretasan, port scanning, maupun eksploitasi layanan yang tidak seharusnya dapat diakses. Penggunaannya juga sering dipadukan dengan tools seperti fail2ban guna meningkatkan perlindungan terhadap serangan brute force.

### CIS (Center for Internet Security)

CIS atau Center for Internet Security adalah organisasi yang menyediakan standar dan panduan keamanan berbasis praktik terbaik untuk membantu pengamanan infrastruktur teknologi informasi. Salah satu produknya, yaitu CIS Benchmarks, berisi rekomendasi konfigurasi keamanan yang rinci untuk berbagai sistem operasi, termasuk Linux. Panduan tersebut mencakup pengaturan firewall, pengelolaan pengguna dan hak akses, penggunaan enkripsi, hingga proses audit dan pencatatan aktivitas sistem. Rekomendasi ini perlu diterapkan secara sistematis agar sistem dapat mencapai tingkat kepatuhan keamanan tertentu yang diakui secara internasional. Karena kredibilitasnya, CIS Benchmarks banyak dijadikan acuan oleh instansi pemerintah, lembaga pendidikan, maupun perusahaan.

### OpenSSH

OpenSSH merupakan implementasi open source dari protokol SSH (Secure Shell) yang memungkinkan akses jarak jauh ke komputer atau server melalui jaringan dengan aman. Semua komunikasi yang berlangsung melalui OpenSSH dienkripsi sehingga terlindungi dari penyadapan maupun serangan man-in-the-middle. Selain digunakan untuk remote login, OpenSSH juga mendukung transfer file yang aman melalui SCP dan SFTP, serta menyediakan fitur port forwarding untuk membuat tunnel komunikasi yang aman bagi aplikasi lain. Dalam proses autentikasi, OpenSSH mendukung penggunaan password maupun pasangan SSH key. Namun, penggunaan SSH key lebih disarankan karena menawarkan keamanan yang lebih baik sekaligus kemudahan akses tanpa perlu mengetik password setiap kali terhubung.

### Booster

Booster adalah alat pembuat initramfs (initial RAM filesystem) yang dirancang agar lebih ringan dan cepat dibandingkan solusi lain seperti mkinitcpio atau dracut. Initramfs sendiri merupakan sistem file sementara yang dimuat kernel Linux pada tahap awal proses booting dan berisi driver serta utilitas dasar yang diperlukan sebelum sistem file utama dapat diakses. Dalam sistem yang menggunakan enkripsi disk, Booster berperan penting karena mendukung proses pembukaan partisi LUKS saat boot berlangsung. Dengan demikian, pengguna dapat memasukkan passphrase terlebih dahulu sebelum sistem operasi utama dijalankan. Selain cepat, konfigurasi Booster juga relatif sederhana sehingga menjadi pilihan yang efisien untuk sistem Linux dengan enkripsi penuh berbasis LUKS.

### MPD, MPV, dan MPC

MPD (Music Player Daemon), MPV, dan MPC (Music Player Client) merupakan tiga perangkat lunak yang sering digunakan bersama untuk kebutuhan multimedia di lingkungan Linux, terutama pada sistem berbasis terminal atau server. MPD berfungsi sebagai server musik yang berjalan di latar belakang dan bertugas mengelola koleksi musik serta proses pemutaran audio secara berkelanjutan. Untuk mengendalikan MPD, digunakan MPC sebagai klien berbasis command-line yang memungkinkan pengguna menjalankan perintah seperti memutar, menghentikan sementara, melewati lagu, atau mengatur playlist langsung dari terminal. Sementara itu, MPV adalah pemutar media serbaguna yang mendukung berbagai format audio, video, dan gambar serta dapat digunakan melalui terminal maupun antarmuka grafis sederhana. Kombinasi ketiganya memungkinkan pengelolaan dan pemutaran multimedia yang lengkap pada sistem Linux minimalis maupun server.


# Resume Kelompok 7

## Nama : Khadizah Hasna Tsabita
## NIM : 12402051030099

### Resume 
Linux adalah kernel, bukan sistem operasi sepenuhnya. Kernel berfungsi sebagai penghubung anatar hardware dan software. Penggunaan CPU, memori, penyimpanan, dan komunikasi perangkat keras lainnya diatur oleh kernel. Arch Linux, Ubuntu, Debian atau Fedora adalah contoh distribusi Linux yang terbentuk ketika kernel Linux dikombinasikan dengan berbagai utitlitas GNU, package manager, dan aplikasi pendukung.

Selanjutnya manajemen disk dan paartisi merupakan langkah penting dalam instalisasi Arch Linux, pada tahap ini pengguna harus membagi kapasitas penyimpanan menjadi beberapa bagian sesuai kebutuhan sistem. Menurut materi yang dipaparkan oleh kelompok 7, konfigurasi terdiri dari paartisi root sebagai tempat sistem operasi dan partisi EFI untuk bootloader. Dalam kebanyakan kasus, pertintah yang digunakan adalah lsblk untuk melihat disk yang tersedia dan cfdisk untuk membuat dan mengelola partisi. Tujuan pemisahan partisi adalah untuk membuat data dan sistem lebih teorganisir dan membuatnya lebih mudah untuk mengelola dan memperbaiki sistem ketika terjadi masalah.

Setelah sistem dasar diinstal, pengguna dapat memilik antarmuka dekstop untuk digunakan sebagai antarmuka grafis. Antarmuka dekstop terdiri dari sejumlah komponen yang menawarkan tampilan visual, pengelolaan jendela, panel, menu, dan aplikasi bawaah. Beberapa dekstop environment yang umum digunakan diLinux antara lain :

1. GNOME
2. KDE Plasma
3. XFCE
4. LCQt
5. Cinnamon
6. MATE
7. Budgie

khusus bagi para pengguna Arch Linux, pemilihan dekstop environment dapat disesuaikan dengan kebutuhan dan kemampuan perangkat keras yang dimiliki.

Lalu, Layanan jaringan yang disebut sebagai server diperlukan agar komputer dapat berkomunikasi dengan perangkat lain melalui internet. Server adalah komputer atau pogram menyediakan layanan kepada klien. Komunikasi ini berlangsung melalui nomor identitas yang disebut port. Salah satu port paling umum digunakan adalah port 443, yang merupakan port standar protokol HTTPS. Ketika pengguna menggunakan HTTPS untuk membuka situs web, browser akan mengirimkan permintaan ke server melalui port 443, yang memungkinkan data yang dikirim dienskripsi menggunakan SSL/TLS. Dalam kasus di mana pengguna menggunakan HTTPS untuk mengakses situs web perpustakaan digital, browser akan terhubung ke server web melalui port 443, yang memungkinkan mereka untuk mengambil halaman web dan data yang terkandung didalamnya.

Dalam proses komunikasi jaringan tersebut, keamanan menjadi aspek yang sangat penting. Firewall, sistem keamanan yang mirip dnegan satpam dipintu masuk gedung, digunakan untuk mencapai tujuan ini. Firewakk bertanggung jawab untuk mengontrol lalu lintas jaringan yang dapat masuk atau keluar sistem. Firewall dapat memberikan akses ke port jika dianggap aman dan diperlukan jika prot tersebut tidak memerlukan atau berpotensi menimbulkan risiko keamanan, firewall dapat menilak akses ke port tersebut.

Iptables adalah salah satu firewall linux yang paling sering digunakan kaena bekerja dengan membuat aturan yang mengatur paket data berdasarkan alamat IP, port, protokol dan rute lalu lintas jaringan. Administrator dapat mengatur iptables agar hanya memungkinkan koneksi HTTPS melalui port 443 dan menutup port lain yang tidak digunakan. Dengan demikian, sistem dilindungi dari masuknya orang yang tidak diinginkan dan serangan jaringan.

Dalam pengelolaan sistem Linux, semua ide ini berhubungan satu sama lain. Partisi mengatur penyimpanan data dan sistem, dekstop environment menyediakan antarmuka pengguna, dan kernel Linux menyediakan dasar sstem operasi. Port dan server memungkinkan komunikasi jaringan, dan firewall seperti iptables menjaga keamanan komunikasi. Seluruh komponen Arch Linux dapat dikonfigurasikan secara manual, memberikan pengguna pemahaman yang lebih baik mengenai tata cara sistem operasi Linux secara keseluruhan.

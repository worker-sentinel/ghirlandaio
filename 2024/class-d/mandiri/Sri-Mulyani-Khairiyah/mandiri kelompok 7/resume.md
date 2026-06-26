##### Nama        : Sri Mulyani Khairiyah
##### NIM         : 12402051050154
##### Kelas       : 4D
##### Mata Kuliah : Perpustakaan dan Arsip Digital

## RESUME KELOMPOK 7 
### 1. Konsep Kernel Sistem Operasi
Kernel dino yaitu sebuah aplikasi kecil yang bertugas mengubah instruksi dari bahasa pemrograman menjadi bahasa biner agar dapat dibaca dan dieksekusi langsung oleh prosesor, jaringan, serta perangkat keras lainnya. Dalam praktiknya, setiap sistem operasi memiliki image kernelnya sendiri yang didistribusikan dalam bentuk pre based unit, seperti halnya pada sistem windows maupun penggunaan sistem berbasi FreeBSD

### 2. Manajemen Password, Secrets, dan Otomasi
Terdapat aplikasi bernama Keeppass yang berfungsi sebagai basis data manajemen kata sandi terpusat. Aplikasi ini digunakan untuk menyimpan secara aman berbagai kredensial sensitif seperti kata sandi akun, media sosial, kunci digital sektor dll. Penggunaan basis data ini dinilai sangat efisien karena pengguna cukup memasukkan data ke dalam database lalu file basis data tersebut dapat dipindahkan dan diakses dari komputer lain asalkan aplikasi Keeppass sudah terinstall 

Selain Keeppass, adajuga secrets yang memiliki fungsi yang sama namun dioptimalkan untuk kebutuhan otomatisasi. Aplikasi secrets bekerja untuk menyimpan kunci SSH sehingga ketika pengembang melakukan proses push, sitem dapat menjalankan proses build aplikasi secara otomatis tanpa mewajibkan pengguna memasukkan kunci secaraa manual, aplikasi ini juga mendukung algoritma untuk menjalankan fungsi authenticator terintegrasi dan enkripsi SSL dalam satu tempat

### 3. GUI vs CLI
GUI (Graphic User Interface) yang berbasis visual dan klik , serta CLI (Command Line Interface) yang sepenuhnya berbasis baris perintah teks di terminal. untuk memenuhi kebutuhan eksplorasii file di server berbasis terminal tanpa tampilan grafis bisa memakai Superfile yaitu sebuah alat seperti Windows Explorer yang dapat dijalankan langsung dari terminal.

Docker juga sebagai solusi mempermudah instalasi dan konfigurasi otomatis aplikasi atau basis data melalui satu baris perintah saja. untuk mengendalikan komputer server dari jarak jauh secara aman melalui jalur CLI lalu menggunakan protokol OpenSSH. Pengendalian jarak jauh dari komputer workstation ke server memerlukan konfigurasi nama dan kata sandi yang kuat.

### 4. Proteksi Jaringan Melalui Firewall
Firewall bertugas menjaga sistem operasi dengan mengatur port mana yang diizinkan terbuka dan tertutup. setiap layanan dan aplikasi memiliki klasifikasi nomor port tertentu, contoh akses terhadap situs web aman berbasis HTTPS akan otomatis mebuka port 443. Ketika pengguna mengakses platform seperti Instagram atau Facebook melalui HTTPS sebenarnya sedang membuka port 443 dari server target tersebut. Pengaturan firewall yang baik yaitu wajib membatasi port yang terbuka dan memblokir jaringan pada port yang tidak digunakan unutuk menutup celah serangan dari pihak luar

### 5. LUKS
LUKS digunakan dalam dunia security untuk melkaukan enkripsi dan deskripsi pada tingkat perangkat keras (harddisk). fitur kemananan seperti grid setup lewat LUKS format ini sangat krusial jika harddisk dilepas dan dimasukkan ke komputer lain oleh pihak luar, data di dalamnya tidak akan bisa terbaca tanpa adanya kunci atau kata sandi yang tepat

### 6. LPM
Dalam infrastruktur server yang menggunakan banyak harddisk fisik secara terpisah misalnya perangkat sda, sdb, sdc terdapat resiko dimana slaah satu kapasitas hardisk disik tersebut akan penuh lebih cepet dibandingkan yang lain. fungsi dari LPM yaitu menyatukan kapasitas dari beberapa hardisk fisik tersebut secara digital seolah olah menjadi satu kesatuan hardisk yang utuh


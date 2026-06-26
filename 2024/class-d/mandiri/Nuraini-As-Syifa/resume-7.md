## Resume Kelompok 7
## Nama: Nuraini As Syifa
## NIM: 12402051030088
## Dosen Pengampu: Al Muhdil Karim, S.Ip., M.Hum
## Analisis
Resume ini dibuat untuk mengetahui aplikasi pada sistem linux
## Konteks
Resume ini dibuat berdasarkan penjelasan Pak Al Muhdil Karim, S.Ip., M.Hum
## KeePass
KeePass merupakan aplikasi manajemen kata sandi yang digunakan untuk menyimpan berbagai password dalam satu database terenkripsi. Aplikasi ini sangat berguna bagi administrator sistem yang harus mengelola banyak server dengan password yang berbeda-beda. Dengan KeePass, pengguna cukup mengingat satu password utama untuk mengakses seluruh data password yang tersimpan secara aman. Selain password server, KeePass juga dapat digunakan untuk menyimpan SSH key, token autentikasi, maupun data rahasia lainnya.
## Secrets
Secrets memiliki fungsi yang hampir sama dengan KeePass, yaitu menyimpan berbagai kunci keamanan dan informasi autentikasi. Perbedaannya, Secrets lebih berfokus pada penyimpanan kunci yang akan digunakan secara otomatis oleh sistem. Misalnya ketika pengguna melakukan koneksi ke server menggunakan SSH, sistem dapat langsung mengambil kunci yang tersimpan tanpa perlu memasukkan password atau key secara manual. Hal ini membuat proses autentikasi menjadi lebih cepat dan efisien.
## Superfile
Superfile adalah aplikasi manajemen file berbasis terminal. Superfile dapat dianggap sebagai versi terminal dari Windows Explorer. Jika pada Windows pengguna mengelola file melalui tampilan grafis, maka pada server Linux pengelolaan file umumnya dilakukan melalui Command Line Interface (CLI). Dengan Superfile, pengguna dapat melihat, memindahkan, menyalin, maupun menghapus file langsung dari terminal tanpa memerlukan tampilan grafis. Aplikasi ini sangat berguna dalam pengelolaan server yang biasanya tidak memiliki antarmuka GUI.
## Podman
Podman adalah alat otomatisasi yang digunakan untuk mempermudah pengelolaan layanan server. Dengan Podman, administrator dapat melakukan instalasi, menjalankan, maupun mengelola aplikasi server hanya melalui beberapa perintah sederhana. Kehadiran Webman membantu mengurangi konfigurasi manual sehingga proses pengelolaan server menjadi lebih cepat dan efisien.
## iptables
iptables berfungsi sebagai firewall pada sistem Linux. Firewall berperan mengatur lalu lintas jaringan dengan menentukan port mana yang boleh dibuka dan port mana yang harus ditutup. Ibaratkan port sebagai pintu pada sebuah rumah. Jika terlalu banyak pintu yang terbuka, maka risiko keamanan akan meningkat. Oleh karena itu, iptables digunakan untuk memastikan hanya port yang benar-benar dibutuhkan saja yang dapat diakses dari luar.
##  LUKS
LUKS (Linux Unified Key Setup) digunakan untuk mengenkripsi media penyimpanan. Berbeda dengan enkripsi file biasa, LUKS mengenkripsi seluruh isi hard disk sehingga data tidak dapat dibaca tanpa password yang benar. Teknologi ini sangat penting dalam keamanan data, terutama jika perangkat hilang, dicuri, atau hard disk dipindahkan ke komputer lain. Dengan LUKS, meskipun hard disk berhasil diambil oleh orang lain, data di dalamnya tetap tidak dapat diakses tanpa kunci dekripsi.
## LVM
LVM (Logical Volume Manager) adalah teknologi yang memungkinkan beberapa hard disk fisik digabungkan menjadi satu ruang penyimpanan logis. Misalnya terdapat lima hard disk berkapasitas 1 TB, maka melalui LVM seluruh kapasitas tersebut dapat terlihat sebagai satu penyimpanan sebesar 5 TB. Teknologi ini memudahkan administrator dalam mengelola kapasitas penyimpanan server tanpa harus memperhatikan batas masing-masing hard disk secara terpisah.
## Open SSH
OpenSSH merupakan standar yang digunakan untuk mengakses komputer atau server lain secara aman melalui jaringan. Dengan OpenSSH, administrator dapat mengelola server dari jarak jauh tanpa harus berada di lokasi yang sama. Selain untuk mengakses server, OpenSSH juga dapat digunakan untuk transfer file dan berbagai kebutuhan administrasi sistem lainnya. Keamanan OpenSSH terletak pada penggunaan enkripsi sehingga komunikasi antara komputer dan server tidak mudah disadap.
## CIS
CIS merupakan standar keamanan dalam teknologi informasi. CIS berisi berbagai panduan dan rekomendasi konfigurasi keamanan, seperti pengaturan password, penggunaan port, pengelolaan akun pengguna, hingga konfigurasi kernel. Dengan mengikuti standar CIS, administrator dapat memastikan bahwa sistem yang dikelola memiliki tingkat keamanan yang lebih baik dan konfigurasi yang konsisten di seluruh server.
## MPC, MPD, MPV
MPC adalah klien berbasis baris perintah untuk mengendalikan MPD. MPD berguna untuk aplikasi memutar, menjeda, melewati lagu, atau mengatur playlist langsung dari terminal. MPV adalah berguna untuk melengkapi keduanya sebagai pemutar media serbaguna yang mendukung berbagai format video, audio, dan gambar. Kombinasi ketiganya membentuk ekosistem multimedia yang komprehensif untuk lingkungan server atau desktop minimalis Linux.
## Booster
Booster adalah alat pembuat initramfs yang lebih cepat dan ringan dibandingkan mkinitcpio atau dracut. initramfs sendiri adalah sistem berkas sementara yang dimuat kernel pada fase awal booting sebelum sistem berkas root dapat dipasang. Booster berperan penting dalam skenario enkripsi disk karena mendukung pembukaan partisi LUKS saat booting, dengan konfigurasi yang lebih sederhana dan proses pembangunan image yang lebih efisien.

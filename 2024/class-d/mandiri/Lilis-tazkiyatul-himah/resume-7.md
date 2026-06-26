# Resume Materi Kelompok 7
## Nama: Lilis Tazkiyatul Himah
## NIM: 12402051030092
## Kelas: 4D_Ilmu Perpustakaan
# Rangkuman
## LVM on LUKS
adalah kombinasi dua teknologi di Linux yang digunakan untuk mengatur penyimpanan data sekaligus memberikan keamanan tingkat tinggi.
LUKS (Linux Unified Key Setup) berfungsi sebagai sistem enkripsi disk, artinya seluruh isi hard disk akan dikunci dan hanya bisa diakses jika 
pengguna memasukkan password yang benar saat proses booting. Dengan begitu, data didalamnya tidak bisa dibaca oleh orang lain meskipun perangkat 
dicabut atau dicuri. Sementara LVM (Logical Volume Manager) adalah sistem yang digunakan untuk mengelola partisi secara lebih fleksibel. Dengan LVM,
pengguna tidak lagi terikat pada pembagian partisi yang kaku seperti pada sistem konvensional. Kapasitas penyimpanan bisa diatur ulang, diperbesar,
diperkecil, atau dibagi menjadi beberapa volume sesuai kebutuhan tanpa install ulang sistem operasi. Ketika keduanya digabung menjadi LVM on LUKS,
maka alurnya adalah disk terlebih dahulu dienkripsi menggunakan LUKS, kemudian didalamnya dibuat LVM untuk mengatur pembagian storage.

## LUKS on LVM
adalah metode pengaturan storage di Linux yang dimana LVM dibuat terlebih dahulu untuk mengelola partisi secara fleksibel, kemudian LUKS digunakan
untuk mengenkripsi salah satu atau beberapa logical volume di dalamnya. Artinya tidak semua disk dienkripsi, hanya bagian tertentu saja yang dipilih
untuk diamankan. Biasanya, metode ini digunakan ketika pengguna ingin melindungi data penting seperti folder /home , sementara sistem operasi tetap
bisa berjalan tanpa harus membuka enkripsi seluruh disk saat boot.

## KeePassXC dan Secret Service
adalah aplikasi pengelola kata sandi di Linux yang digunakan untuk menyimpan password dalam satu database yang sudah dienkripsi. Pengguna hanya
perlu mengingat satu master password untuk membuka semua data didalamnya, sehingga lebih aman dan praktis untuk mengelola banyak akun. Sementara 
Secret Service adalah sistem bawaan di Linux yang berfungsi sebagai layanan penyimpanan rahasia untuk aplikasi. Melalui layanan ini, aplikasi bisa 
menyimpan dan mengambil password secara otomatis lewat sistem seperti GNOME Keyring, tanpa harus mengelola enkripsi sendiri.

## Podman
adalah sebuah tools di Linux yang digunakan untuk menjalankan dan mengelola container, yaitu lingkungan terisolasi untuk menjalankan aplikasi.
Podman mirip seperti Docker, tetapi memiliki kelebihan karena bisa dijalankan tanpa membutuhkan layanan utaman (daemon) dan bisa digunakan tanpa 
hak akses root, sehingga lebih aman. Dengan Podman, pengguna bisa membuat, menjalankan, menghentikan, dan menghapus container dengan mudah untuk 
berbagai keperluan seperti testing aplikasi, development, atau deployment.

## XFCE4
adalah salah satu desktop environment di Linux yang digunakan untuk menyediakan tampilan grafis seperti desktop, menu, taskbar, dan pengelola jendela.
XFCE4 dikenal ringan, cepat, dan tidak memerlukan banyak sumber daya, sehingga cocok digunakan di komputer dengan spesifikasi rendah atau sistem 
yang mengutamakan performa. Meskipun ringan, XFCE4 tetap menyediakan fitur yang cukup lengkap untuk penggunaan sehari-hari seperti mengelola file,
menjalankan aplikasi, dan mengatur tampilan sistem.

## Superfile
adalah sebuah file manager berbasis terminal di Linux yang digunakan untuk mengelola file dan folder melalui tampilan teks (CLI), bukan antarmuka 
grafis seperti file manager pada umumnya. Dengan Superfile, pengguna bisa melakukan operasi seperti membuka, memindahkan, menyalin, menghapus, dan 
mengorganisasi file secara cepat langsung dari terminal. Aplikasi ini biasanya dipilih oleh pengguna Linux yang sudah terbiasa dengan command line
karena lebih ringan, cepat, dan efisien. 

## iptables
adalah sebuah tools di Linux yang digunakan untuk mengatur dan mengontrol lalu lintas jaringan (network traffic) yang masuk dan keluar dari sistem.
Dengan iptables, pengguna bisa membuat aturan firewall untuk mengizinkan, memblokir, atau membatasi koneksi berdasarkan alamat IP, port, atau jenis 
protokol. 

## CIS
(Center for Internet Security) adalah sebuah organisasi yang menyusun standar dan panduan keamanan untuk sistem komputer, jaringan, dan aplikasi 
agar lebih aman dari serangan siber. CIS juga dikenal karena membuat CIS Benchmarks, yaitu panduan konfigurasi keamanan terbaik yang bisa digunakan 
untuk mengamankan berbagai sistem operasi dan software.

## OpenSSH
adalah sebuah tools di Linux yang digunakan untuk melakukan koneksi jarak jauh (remote access) secara aman ke komputer atau server lain melalui 
jaringan. OpenSSH mengenkripsi semua komunikasi yang terjadi, sehingga data seperti password dan perintah tidak bisa dengan mudah disadap oleh pihak 
lain. Selain untuk login ke server dari jarak jauh, OpenSSH juga bisa digunakan ubtuk transfer file secara aman dan menjalankan perintah di 
komputer lain.

## Booster
adalah istilah umum yang biasanya merujuk pada sesuatu yang berfungsi untuk meningkatkan atau memperkuat kinerja suatu sistem atau aplikasi.
Dalam konsteks teknologi, booster sering dipakai untuk aplikasi atau fitur yang mengoptimalkan perangkat, misalnya mempercepat sistem, membersihkan 
memori, atau meningkatkan performa jaringan.

## MPD, MPV, dan MPC
MPD (Music Player Daemon)
adalah layanan pemutar musik yang berjalan di background (daemon) di Linux. MPD tidak memiliki tampilan sendiri, jadi biasanya dikontrol lewat 
aplikasi lain (client). Fungsinya untuk memutar musik secara efisien di server atau sistem minimal.

MPV (Media Player Video)
adalah aplikasi pemutar media yang ringan dan fleksibel untuk memutar video dan audio. MPV dikenal karena performanya yang cepat, kualitas playback 
yang baik, serta bisa digunakan lewat terminal maupun GUI.

MPC (Music Player Client)
adalah aplikasi kecil berbasis terminal yang digunakan untuk mengontrol MPD. Jadi MPC tidak memutar musik langsung, tetapi menjadi remote untuk 
menjalankan dan mengatur MPD seperti play, pause, atau ganti lagu.

# Resume Kelompok 7
## Nama: Nayla Putri Andriyani
## NIM:12402051030093
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum

LVM on LUKS dan LUKS on LVM

LVM dan LUKS dapat dikombinasikan melalui dua metode. Pada LVM on LUKS, partisi dienkripsi terlebih dahulu menggunakan LUKS kemudian dibangun struktur LVM di atasnya. Metode ini hanya memerlukan satu kunci saat booting sehingga seluruh logical volume dapat diakses sekaligus. Sementara itu, pada LUKS on LVM, LVM dibuat terlebih dahulu lalu setiap logical volume dienkripsi secara terpisah. Metode ini membutuhkan pembukaan volume satu per satu, tetapi memberikan keamanan yang lebih tinggi karena kerusakan atau kebocoran pada satu volume tidak memengaruhi volume lainnya.

Keepassxc

KeePassXC merupakan aplikasi pengelola kata sandi berbasis open source yang menyimpan kredensial dalam database terenkripsi, sedangkan Secret Service adalah standar Linux yang memungkinkan aplikasi menyimpan dan mengambil kredensial secara aman. Keduanya bekerja sama untuk melindungi berbagai jenis kunci, baik untuk enkripsi maupun autentikasi. Integrasi KeePassXC dengan Secret Service memungkinkan aplikasi lain memperoleh kredensial secara otomatis tanpa harus meminta pengguna memasukkannya berulang kali.

Podman

Podman adalah container runtime yang digunakan untuk menjalankan aplikasi dalam container yang terisolasi. Berbeda dengan Docker, Podman tidak memerlukan daemon yang berjalan terus-menerus dan mendukung mode rootless sehingga lebih aman. Podman memungkinkan berbagai layanan berjalan bersamaan tanpa konflik konfigurasi karena setiap layanan ditempatkan dalam container tersendiri. Selain itu, integrasi dengan systemd memungkinkan container berjalan otomatis saat sistem dinyalakan.

Xfce4

XFCE4 merupakan desktop environment Linux yang dirancang ringan dan hemat sumber daya. Dibandingkan GNOME atau KDE, XFCE4 membutuhkan penggunaan RAM dan CPU yang lebih rendah namun tetap menyediakan antarmuka yang lengkap dan responsif. Karena efisiensinya, XFCE4 banyak digunakan pada server yang memerlukan tampilan grafis maupun komputer dengan spesifikasi rendah, serta menjadi pilihan pada berbagai distribusi Linux populer.

SUuperfile

Superfile adalah file manager berbasis Text User Interface (TUI) yang memudahkan pengelolaan berkas langsung dari terminal. Aplikasi ini menyediakan tampilan panel yang lebih intuitif dibandingkan penggunaan perintah terminal dasar seperti ls, cp, dan mv. Dengan Superfile, pengguna dapat melakukan operasi seperti menyalin, memindahkan, menghapus, mengganti nama, dan melihat isi file dengan lebih cepat tanpa memerlukan antarmuka grafis.

Iptable

iptables adalah utilitas firewall Linux yang memanfaatkan framework Netfilter untuk mengatur lalu lintas jaringan masuk dan keluar. Administrator dapat menentukan aturan akses berdasarkan port, alamat IP, maupun protokol tertentu. Port yang tidak diperlukan biasanya ditutup untuk mengurangi risiko keamanan, sedangkan layanan penting dibuka sesuai kebutuhan. Konfigurasi yang tepat menjadikan iptables sebagai lapisan pertahanan awal terhadap berbagai ancaman jaringan, termasuk pemindaian port dan serangan brute force.

CIS 

CIS merupakan organisasi yang menyediakan CIS Benchmarks, yaitu panduan keamanan standar industri untuk berbagai sistem dan perangkat lunak. Panduan ini mencakup konfigurasi firewall, pengelolaan pengguna, enkripsi, audit, serta pencatatan aktivitas sistem. Dengan menerapkan rekomendasi CIS secara sistematis, organisasi dapat meningkatkan tingkat kepatuhan keamanan dan memastikan infrastrukturnya memenuhi standar yang diakui secara internasional.

Openssh

OpenSSH adalah implementasi open source dari protokol SSH yang digunakan untuk mengakses dan mengelola server dari jarak jauh secara aman. Seluruh komunikasi yang dilakukan melalui OpenSSH dienkripsi sehingga terlindung dari penyadapan dan serangan man-in-the-middle. Selain mendukung remote login, OpenSSH juga menyediakan fasilitas transfer file melalui SCP dan SFTP serta port forwarding. Untuk autentikasi, OpenSSH mendukung password maupun SSH key, dengan SSH key dianggap lebih aman dan praktis.

Booster

Booster merupakan utilitas pembuat initramfs yang dirancang lebih cepat dan ringan dibandingkan alternatif lain seperti mkinitcpio atau dracut. Initramfs berfungsi sebagai sistem berkas sementara yang digunakan saat proses booting awal sebelum root filesystem utama dimuat. Booster sangat penting pada sistem yang menggunakan enkripsi LUKS karena mendukung proses pembukaan partisi terenkripsi selama tahap awal booting. Dengan konfigurasi yang sederhana dan proses pembuatan image yang cepat, Booster membantu mempercepat waktu startup sistem.

MPD, MPV, dan MPC

MPD, MPV, dan MPC merupakan perangkat lunak multimedia yang saling melengkapi di lingkungan Linux. MPD berperan sebagai server musik yang mengelola pustaka dan pemutaran audio di latar belakang. MPC berfungsi sebagai klien berbasis terminal untuk mengendalikan MPD, seperti memutar, menghentikan, atau mengelola playlist. Sementara itu, MPV adalah pemutar media yang mendukung berbagai format audio, video, dan gambar. Kombinasi ketiganya menghasilkan sistem multimedia yang efisien dan ringan untuk digunakan pada server maupun desktop minimalis.

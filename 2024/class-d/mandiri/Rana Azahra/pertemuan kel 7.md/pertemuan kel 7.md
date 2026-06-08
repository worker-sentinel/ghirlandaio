# pertemuan kel 7
# Nama : Rana Azahra
# mata kuliah : Perpustakaan dan Arsip Digital
# dosen pengampu : Al Muhdil Karim
# XFCE4
XFCE4 adalah lingkungan desktop linux yang ringan dan efisien dalam konsumsi RAM maupun CPU. Dibandingkan GNOME atau
KDE, XCFE4 tetapp responsif pada peragkat keras terbatas sekalipun,menjadikannya pilihan ideal untuk server yang memerlukan antarmuka visual atau computer lama. 
Distribusi seperti DEblan, Xubuntu dan Kali Linux menggunakannya sebagai desktop default atau alternatif.
# Superfile
Superfile manajer file berbasis TUI untuk terminal yang menghadirkan tampilan panel visual intuitif lengkap dengan
operasi sperti salin, pindah hapus Ganti nma dan pratinjau file. Alat ini sangat membantu administrator server yang
berhadapan dengan hierarki direktori komplekstanpa antarmuka grafis, sehingga pekerjaan dapaat dilakukan lebih efisien langsung dari terminal

# CIS 
organisasi yang menyediakan panduan dan tolok ukur keamanan berbasis standar industri untuk membantu administrator mengamankan 
infrastruktur IT. CIS Benchmarks mencakup rekomendasi konfigurasi rinci untuk sistem. operasi Linux, meliputi firewall, manajemen akses, enkripsi, 
serta auditing sistem. Panduan ini banyak dijadikan acuan oleh institusi pemerintah, lembaga pendidikan, dan korporasi dalam memenuhi 
standar keamanan yang diakui secara internasional.
# OPenSSH
adalah implementasi open source protokol SSH yang memungkinkan akses jarak jauh ke server melalui saluran komunikasi terenkripsi end-to-end, 
terlindung dari penyadapan dan serangan man-in-the-middle. Selain remote login,OpenSSH mendukung transfer file aman via scp dan sftp, serta 
fitur port forwarding. Metode autentikasi berbasis pasangan kunci SSH lebih direkomendasikan daripada kata sandi karena menawarkan keamanan lebih 
tinggi dan kemudahan akses.
# Podman
container runtime yang bersifat daemonless dan mendukung mode rootless, menjadikannya lebih aman dari Docker untuk lingkungan server produksi. 
Podman memungkinkan berbagai layanan seperti MySQL atau SLiMS berjalan secara bersamaan dalam kontainer terisolasi tanpa konflik port. 
Integrasinya dengan systemd juga memungkinkan kontainer aktif secara otomatis saat server dinyalakan.
# MPD, MPV, dan MPC
server musik yang berjalan sebagai daemon di latar belakang untuk mengelola pustaka dan pemutaran audio. MPC adalah klien berbasis baris 
perintah untuk mengendalikan MPD — memutar, menjeda, melewati lagu, atau mengatur playlist langsung dari terminal. MPV melengkapi keduanya 
sebagai pemutar media serbaguna yang mendukung berbagai format video, audio, dan gambar. Kombinasi ketiganya membentuk ekosistem multimedia 
yang komprehensif untuk lingkungan server atau desktop minimalis Linux.
# iptables
utilitas firewall berbasis kernel Linux melalui framework Netfilter yang memungkinkan administrator mengatur lalu lintas jaringan 
secara presisi — menentukan port, alamat IP, dan protokol yang diizinkan. Port tidak penting diblokir secara default, sementara port 
esensial seperti 22, 80, dan 443 dibuka selektif. iptables juga sering dikombinasikan dengan fail2ban untuk perlindungan tambahan terhadap
serangan brute force.
# KeePassXC dan Secret Service
manajer kata sandi lokal berbasis open source yang menyimpan kredensial dalam basis data terenkripsi. Secret Service seperti GNOME 
Keyring atau KDE Wallet adalah antarmuka standar Linux untuk menyimpan kredensial secara terproteksi. Keduanya berfungsi komplementer 
sebagai penyedia kunci enkripsi maupun autentikasi. KeePassXC bahkan dapat berintegrasi langsung dengan Secret Service API, sehingga aplikasi 
lain dapat mengambil kredensial secara otomatis tanpa intervensi manual pengguna.
# Booster
Booster adalah alat pembuat initramfs yang lebih cepat dan ringan dibandingkan mkinitcpio atau dracut. initramfs sendiri adalah sistem 
berkas sementara yang dimuat kernel pada fase awal booting sebelum sistem berkas root dapat dipasang. Booster berperan penting dalam 
skenario enkripsi disk karena mendukung pembukaan partisi LUKS saat booting, dengan konfigurasi yang lebih sederhana dan proses pembangunan
image yang lebih efisien. 
# LVM on LUKS dan LUKS on LVM
LVM on LUKS mengenkripsi partisi fisik terlebih dahulu, lalu LVM dibangun di atasnya — sehingga cukup satu kunci untuk membuka 
semua logical volume sekaligus. Pendekatan ini cocok untuk sistem yang mengakses seluruh data secara bersamaan. Sebaliknya, LUKS on
LVM membangun LVM lebih dulu, kemudian mengenkripsi tiap volume secara independen dengan kunci masing-masing. Ini menghasilkan 
isolasi keamanan yang lebih granular, sehingga lebih tepat untuk sistem penyimpanan data sensitif yang memerlukan kontrol akses per volume.

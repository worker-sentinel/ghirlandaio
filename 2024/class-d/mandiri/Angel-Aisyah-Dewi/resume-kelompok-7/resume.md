# Resume Kelompok 7
## Nama: Angel Aisyah Dewi
## NIM: 12402051030095
## Kelas: 4D
## Mata kuliah: Arsip dan Perpustakaan Digital

### LVM on LUKS vs LUKS on LVM
Dua cara berbeda buat gabungin enkripsi disk (LUKS) sama manajemen volume (LVM). LVM on LUKS: enkrip dulu baru dibagi, cukup satu kunci buat semua. LUKS on LVM: dibagi dulu baru tiap bagian dienkrip sendiri, jadi lebih aman tapi lebih ribet.

### KeePassXC dan Secret Service
KeePassXC buat nyimpen password secara lokal dan aman. Secret Service (GNOME Keyring/KDE Wallet) adalah sistem Linux buat aplikasi ambil kredensial otomatis. Keduanya bisa nyambung, jadi gak perlu masukin password terus-terusan.

### Podman
Alat buat jalanin aplikasi di container yang terisolasi. Lebih aman dari Docker karena gak butuh daemon dan bisa jalan tanpa root. Container bisa otomatis nyala bareng server lewat systemd.

### XFCE4
Desktop environment Linux yang ringan dan hemat resource. Cocok buat PC lama atau server yang butuh tampilan grafis tapi speknya terbatas.

### Superfile
File manager berbasis terminal yang tampilannya visual. Lebih praktis dari ngetik ls, cp, mv terus-terusan, terutama di server tanpa antarmuka grafis.

### iptables
Alat firewall Linux buat ngatur lalu lintas jaringan. Bisa blokir port yang gak dibutuhin dan buka port tertentu saja. Sering dipasang bareng fail2ban buat lawan brute force.

### CIS Benchmarks
Panduan keamanan standar internasional dari organisasi CIS. Isinya langkah-langkah konkret buat ngamanin sistem Linux, mulai dari firewall sampai manajemen user. Banyak dipakai pemerintah dan lembaga pendidikan.

### OpenSSH
Alat buat akses server dari jarak jauh secara terenkripsi. Bisa juga buat transfer file lewat scp/sftp. Login pake SSH key pair lebih disaranin daripada password biasa.

### Booster
Alat bikin initramfs yang lebih cepat dan ringan dari mkinitcpio. Penting kalau pakai enkripsi disk LUKS, karena dia yang minta passphrase saat booting sebelum sistem utama jalan.

### MPD, MPV, dan MPC
Tiga alat multimedia di Linux. MPD jalanin musik di background, MPC buat ngontrolnya lewat terminal, MPV buat muter video/audio/gambar. Kombinasi ketiganya cukup buat kebutuhan multimedia dari terminal saja.

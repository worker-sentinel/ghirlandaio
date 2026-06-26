# Resume Kelompok 7
## Nama: Resti Irma Agustin
## NIM: 12402051010025
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.
## Konteks
Resume ini ditulis berdasarkan tugas dari kelompok 7
## Pendahuluan
Menjelaskan LVM on LUKS vs LUKS on LVM, KeePassXC dan Secret Service, Podman, XFCE4, Superfile, iptables, CIS Benchmarks, OpenSSH, Booster, MPD, MPV, dan MPC.
## Pembahasan
### LVM on LUKS vs LUKS on LVM 
Dua cara berbeda buat gabungin enkripsi disk (LUKS) Linux Unified Key Setup sama manajemen volume (LVM) Logical Volume Manager. LVM on LUKS biasanya enkrip dulu baru dibagi dan cukup satu kunci buat semua. Kalo LUKS on LVM dibagi dulu baru tiap bagian dienkrip sendiri-sendiri jadi lebih aman tapi lebih ribet. Cocok buat sistem perpustakaan digital.
### KeePassXC dan Secret Service KeePassXC 
Aplikasi buat nyimpen password secara aman. Secret Service seperti GNOME Keyring/KDE Wallet adalah sistem Linux buat aplikasi ambil kredensial otomatis. Keduanya saling melengkapi, menjaga password dan sebagai authentication key untuk login ke aplikasi.
### Podman 
Alat buat jalanin aplikasi di container yang terisolasi dan bisa jalan tanpa root. Container bisa otomatis nyala bareng server lewat systemd. Podman berguna untuk menjalankan MySQL atau SLiMS yang sedang dipelajari anak ilmu perpustakaan.
### XFCE4 
Desktop environment Linux yang ringan dan hemat resource seperti RAM dan CPU. XFCE4 biasanya sering digunakan pada sistem operasional linux seperti Ubuntu, Debian dan Kali Linux.
### Superfile File 
manager berbasis terminal yang tampilannya berbasis TUI (Text User Interface). Lebih praktis dari ngetik ls, cp, mv terus-terusan, terutama di server tanpa antarmuka grafis.
### iptables 
Alat firewall Linux buat ngatur lalu lintas jaringan. Memblokir port yang gak dibutuhin dan buka port tertentu saja. Sering dipasang bareng fail2ban buat lawan brute force.
### CIS Benchmarks 
Panduan keamanan standar internasional dari organisasi CIS. Isinya langkah-langkah buat ngamanin sistem Linux, mulai dari firewall sampai manajemen user. CIS Benchmarks dijadikan acuan institusi pemerintah, lembaga pendidikan, dan perusahaan untuk memastikan infrastruktur mereka memenuhi standar keamanan yang telah teruji dan diakui.
### OpenSSH 
Alat buat akses server dari jarak jauh secara terenkripsi. Bisa juga buat transfer file lewat scp/sftp. Login pake SSH key pair lebih disaranin daripada password biasa.
### Booster 
Alat bikin initramfs (Initial RAM filesystem) yang lebih cepat dan ringan dari mkinitcpio. Membantu sistem meminta keamanan ulang dari pengguna sebelum akhirnya lanjut ke proses operasi utama boot sistem.
### MPD, MPV, dan MPC 
Tiga alat multimedia di Linux. MPD jalanin musik di background, MPC buat ngontrolnya lewat terminal, MPV buat muter video/audio/gambar. Kombinasi ketiganya cukup buat kebutuhan multimedia dari terminal saja.



# Resume Pertemuan ke7
## nama : Fairuz Pramathana Sani 
## Kelas : Ilmu Perpustakaan 4D
## NIM : 12402051050147
## Mata Kuliah : Perpustakaan dan Arsip Digital
## Dosen Pengampu : Al Muhdil Karim, S. IP., M.Hum

LVM on LUKS dan LUKS on LVM

LVM on LUKS dan LUKS on LVM adalah dua metode gabungan antara enkripsi disk dan pengatur partisi, pada LVM on LUKS hardisk digembok total dulu baru dibagi-bagi menjadi beberapa folder di dalamnya. Kelebihannya kita cuma butuh satu password untuk membuka semuanya sekaligus, sehingga cocok untuk sistem perpustakaan digital yang datanya diakses bersamaan. 
Sebaliknya pada LUKS on LVM hardisk dibagi-bagi dulu baru tiap folder digembok sendiri-sendiri secara mandiri, cara ini jauh lebih aman karena jika satu folder kebobolan, folder lainnya tetap aman terkunci, sehingga sangat pas untuk menyimpan arsip rahasia atau data sensitif.


Keepassxc dan secret service

KeePassXC adalah aplikasi brankas password lokal bersumber terbuka untuk menyimpan data login secara terenkripsi. Sementara Secret Service adalah sistem bawaan Linux yang bertugas sebagai jembatan aman agar aplikasi lain bisa mengambil data login tanpa mengekspos teks asli password-nya. Keduanya saling melengkapi untuk menjaga kata sandi, baik untuk membuka hardisk terenkripsi  maupun login ke server dan database.KeePassXC bisa langsung terintegrasi dengan Secret Service API, sehingga aplikasi lain di komputer bisa mengambil data login secara otomatis tanpa bikin repot mengetik manual setiap saat.

Podman

Podman adalah alat untuk menjalankan aplikasi di dalam wadah mandiri terisolasi. Berbeda dengan Docker Podman ini bersifat daemonless dan mendukung mode rootless (bisa jalan tanpa akses admin tertinggi), sehingga jauh lebih aman untuk server produksi. Dalam mengelola server, Podman sangat berguna untuk menjalankan aplikasi seperti MySQL atau SLiMS secara bersamaan tanpa takut port-nya tabrakan. Plus, Podman sudah terintegrasi dengan systemd, jadi aplikasi bisa diatur agar otomatis langsung menyala begitu server dihidupkan.


Xfce4

XFCE4 adalah tampilan desktop di Linux yang terkenal sangat enteng dan irit dalam penggunaan RAM maupun CPU. Jika dibandingkan dengan desktop lain yang berat seperti GNOME atau KDE, XFCE4 menawarkan tampilan yang fungsional dan lincah bahkan pada komputer dengan spesifikasi jadul atau terbatas ini menjadikannya pilihan paling pas untuk dipasang pada server yang kebetulan masih butuh tampilan visual, atau pada PC lama agar bisa bekerja optimal. XFCE4 sering ditemukan sebagai tampilan bawaan di distro seperti Debian, Xubuntu, dan Kali Linux.


Superfile

Superfile adalah aplikasi pengelola file modern berbasis teks  yang berjalan langsung di dalam terminal, server yang tidak punya tampilan desktop, mengelola file hanya mengandalkan perintah ketik biasa seperti ls, cp, atau mv bisa bikin pusing dan kurang efisien saat foldernya sudah menumpuk rumit. Superfile menyelesaikan masalah ini dengan menyediakan tampilan visual berbentuk panel-panel yang rapi, lengkap dengan fitur potong-kompas, hapus, ganti nama, hingga melihat isi file langsung di dalam terminal agar kerja admin bisa lebih cepat.


iptables

iptables adalah satpam internal Linux yang berfungsi mengatur keluar-masuknya lalu lintas jaringan di server lewat sistem kernel. Pakai alat ini, admin bisa bikin aturan detail mengenai port mana saja yang boleh diakses, dari IP mana, dan lewat jalur apa. Pintu atau port yang tidak penting bakal dikunci total secara otomatis untuk menghalau penyusup, sementara port penting seperti port 22 (SSH) atau port 80/443 (website) dibuka secara selektif. Jika disetting dengan benar, iptables menjadi benteng pertama yang ampuh menahan hacker, dan biasanya digabung dengan tools seperti fail2ban untuk perlindungan ekstra.


CIS (Center for Internet Security)

CIS adalah organisasi dunia yang merilis kumpulan standar dan panduan keamanan (CIS Benchmarks) untuk membantu admin mengamankan infrastruktur IT mereka. Panduan ini berisi rekomendasi konfigurasi keamanan yang sangat detail untuk berbagai sistem operasi (termasuk Linux), mulai dari urusan firewall, hak akses user, enkripsi data, hingga pencatatan aktivitas sistem. Aturan ini harus diikuti secara runtut agar sistem bisa mencapai tingkat kepatuhan (compliance) standar internasional, makanya panduan CIS ini sering dijadikan acuan wajib oleh pemerintah, kampus, dan perusahaan besar.


Openssh

Openssh adalah aplikasi gratisan yang memanfaatkan protokol SSH untuk mengakses dan mengontrol komputer atau server lain dari jarak jauh secara aman dan terenkripsi dari ujung ke ujung. Selain untuk remote komputer, OpenSSH juga bisa dipakai buat kirim file aman (lewat perintah scp/sftp) untuk aplikasi lain. Untuk urusan login, kita bisa pakai password biasa atau pasangan kunci digital (SSH key pair). pakai SSH key ini jauh lebih direkomendasikan karena selain lebih kebal hacker, kita juga tidak perlu mengetik password terus-menerus.


Booster

adalah alat pembuat initramfs yang dirancang agar proses booting Linux berjalan jauh lebih cepat dan ringan dibanding tools lama seperti mkinitcpio atau dracut. Initramfs sendiri bertugas menyiapkan driver minimal sebelum sistem file utama dimuat. Peran Booster ini krusial banget kalau kita pakai hardisk terenkripsi, karena dialah yang bertugas memunculkan layar ketik password gembok LUKS saat komputer baru dinyalakan. Berhubung cara setting-nya simpel dan bikin file boot-nya cepat kelar, Booster jadi pilihan yang sangat efisien untuk enkripsi penuh.

MPD, MPV, dan MPC
MPD, MPV, dan MPC adalah tiga serangkai aplikasi multimedia yang klop banget dipakai di terminal atau server Linux tanpa desktop. MPD bertindak sebagai mesin utama yang berjalan diam-diam di latar belakang untuk mengelola library dan memutar musik terus-menerus. Untuk menyuruh-nyuruh si MPD (seperti perintah play, pause, skip, atau ganti playlist), kita menggunakan MPC yang berbasis ketikan perintah singkat di terminal. Sementara MPV adalah pemutar media serbaguna yang sangat ringan dan sanggup melibas segala format video, audio, maupun gambar baik lewat terminal atau grafik sederhana.

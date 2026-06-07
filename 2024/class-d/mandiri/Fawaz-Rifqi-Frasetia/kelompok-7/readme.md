# Resume Materi Pertemuan 7

## Sistem Operasi & Keamanan Linux — Infrastruktur Server

---

## LVM on LUKS dan LUKS on LVM

Kombinasi antara LUKS (enkripsi disk) dan LVM (manajemen volume) dapat diterapkan dengan dua cara:

### LVM on LUKS

Pada metode ini, partisi fisik dienkripsi terlebih dahulu menggunakan LUKS, lalu LVM dibuat di atas partisi yang sudah terenkripsi.

**Kelebihan:**

* Hanya membutuhkan satu passphrase untuk membuka seluruh data.
* Semua logical volume langsung dapat diakses setelah partisi dibuka.
* Konfigurasi lebih sederhana.

**Cocok untuk:**

* Sistem yang seluruh datanya diakses bersama, seperti server perpustakaan digital.

### LUKS on LVM

Pada metode ini, LVM dibuat terlebih dahulu, kemudian setiap logical volume dienkripsi secara terpisah menggunakan LUKS.

**Kelebihan:**

* Setiap volume memiliki kunci enkripsi sendiri.
* Keamanan lebih granular dan terisolasi.
* Jika satu volume bocor, volume lain tetap aman.

**Cocok untuk:**

* Arsip atau penyimpanan data sensitif yang membutuhkan kontrol akses berbeda pada setiap volume.

---

## KeePassXC dan Secret Service

**KeePassXC** merupakan password manager open source yang menyimpan kredensial dalam database terenkripsi.

**Secret Service** (seperti GNOME Keyring atau KDE Wallet) menyediakan layanan penyimpanan kredensial yang aman untuk berbagai aplikasi Linux.

### Fungsi Utama

* Menyimpan password dan kunci enkripsi secara aman.
* Mengelola autentikasi untuk SSH, database, dan aplikasi lainnya.
* Mendukung integrasi otomatis antar aplikasi melalui API Secret Service.

Dengan kombinasi keduanya, pengelolaan password menjadi lebih aman dan praktis.

---

## Podman

Podman adalah container runtime yang digunakan untuk menjalankan aplikasi dalam lingkungan terisolasi.

### Karakteristik

* Tidak menggunakan daemon (*daemonless*).
* Mendukung mode *rootless* sehingga lebih aman.
* Cocok sebagai alternatif Docker.

### Manfaat

* Menjalankan banyak layanan tanpa konflik.
* Mendukung *port mapping* yang terpisah.
* Terintegrasi dengan systemd untuk menjalankan container otomatis saat booting.

---

## XFCE4

XFCE4 adalah desktop environment Linux yang ringan dan hemat sumber daya.

### Kelebihan

* Penggunaan RAM dan CPU rendah.
* Tetap responsif pada perangkat lama.
* Menyediakan antarmuka grafis yang lengkap namun sederhana.

### Umum Digunakan Pada

* Debian
* Xubuntu
* Kali Linux

---

## Superfile

Superfile merupakan file manager berbasis terminal (TUI) yang memudahkan pengelolaan file tanpa GUI.

### Fitur

* Navigasi direktori yang lebih visual.
* Copy, move, rename, dan delete file.
* Preview file langsung dari terminal.

### Manfaat

Membantu administrator server bekerja lebih cepat dibanding hanya menggunakan perintah dasar seperti `ls`, `cp`, dan `mv`.

---

## iptables

iptables adalah alat konfigurasi firewall Linux yang bekerja melalui framework Netfilter.

### Fungsi

* Mengatur lalu lintas jaringan masuk dan keluar.
* Membuka atau menutup port tertentu.
* Membatasi akses berdasarkan IP dan protokol.

### Contoh Penggunaan

* Membuka port SSH (22).
* Membuka port web (80 dan 443).
* Menutup port yang tidak diperlukan.

iptables sering dipadukan dengan Fail2ban untuk mencegah serangan brute force.

---

## CIS (Center for Internet Security)

CIS adalah organisasi yang menyediakan standar dan panduan keamanan untuk berbagai sistem IT.

### CIS Benchmarks

Berisi rekomendasi konfigurasi keamanan yang mencakup:

* Firewall
* Manajemen pengguna
* Hak akses
* Enkripsi data
* Logging dan auditing

### Manfaat

* Meningkatkan keamanan sistem.
* Membantu memenuhi standar keamanan industri.
* Banyak digunakan oleh pemerintah, kampus, dan perusahaan.

---

## OpenSSH

OpenSSH merupakan implementasi open source dari protokol SSH untuk akses jarak jauh yang aman.

### Kemampuan

* Remote login ke server.
* Transfer file melalui `scp` dan `sftp`.
* Port forwarding dan tunneling.

### Metode Autentikasi

1. Password
2. SSH Key Pair (lebih aman dan direkomendasikan)

Seluruh komunikasi dienkripsi sehingga terlindungi dari penyadapan.

---

## Booster

Booster adalah tool pembuat initramfs yang ringan dan cepat.

### Fungsi

* Menyiapkan lingkungan awal saat proses booting.
* Memuat driver dan utilitas penting sebelum root filesystem aktif.
* Mendukung pembukaan partisi LUKS saat startup.

### Kelebihan

* Konfigurasi sederhana.
* Proses pembuatan initramfs lebih cepat dibanding mkinitcpio atau dracut.

---

## MPD, MPV, dan MPC

Ketiga aplikasi ini sering digunakan untuk kebutuhan multimedia di Linux.

### MPD (Music Player Daemon)

* Berjalan sebagai layanan di background.
* Mengelola library dan pemutaran musik.

### MPC (Music Player Client)

* Klien berbasis command line untuk mengontrol MPD.
* Mendukung play, pause, next, playlist, dan lain-lain.

### MPV

* Pemutar multimedia serbaguna.
* Mendukung audio, video, dan gambar.
* Dapat dijalankan melalui terminal maupun GUI sederhana.

### Kesimpulan

Kombinasi MPD, MPC, dan MPV memungkinkan pengelolaan multimedia secara efisien, terutama pada sistem Linux minimalis atau server.

---

## Ringkasan Singkat

| Materi                     | Inti Pembahasan                              |
| -------------------------- | -------------------------------------------- |
| LVM on LUKS                | Satu enkripsi untuk seluruh volume           |
| LUKS on LVM                | Enkripsi terpisah untuk setiap volume        |
| KeePassXC & Secret Service | Manajemen password dan kredensial aman       |
| Podman                     | Container runtime yang aman dan ringan       |
| XFCE4                      | Desktop environment ringan                   |
| Superfile                  | File manager berbasis terminal               |
| iptables                   | Firewall bawaan Linux                        |
| CIS                        | Standar dan benchmark keamanan sistem        |
| OpenSSH                    | Akses server jarak jauh yang terenkripsi     |
| Booster                    | Pembuat initramfs yang cepat dan sederhana   |
| MPD, MPV, MPC              | Ekosistem multimedia Linux berbasis terminal |


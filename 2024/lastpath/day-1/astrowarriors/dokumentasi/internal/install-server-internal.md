# Dokumentasi Instalasi Server Internal

## 1. Menghubungkan ke Wi-Fi

```bash
iwctl
```

Menjalankan utilitas **iNet Wireless Daemon (iwd)** sebagai media konfigurasi koneksi Wi-Fi. Penggunaan jaringan yang terpercaya selama instalasi membantu menjaga keamanan sistem dan memastikan paket yang diunduh berasal dari sumber yang valid.---

```bash
device list
```

Menampilkan seluruh perangkat jaringan nirkabel yang berhasil dideteksi oleh sistem. Pemeriksaan ini bertujuan memastikan adaptor Wi-Fi siap digunakan sebelum dilakukan konfigurasi koneksi.---

```bash
station wlan0 get-networks
```

Menampilkan daftar jaringan Wi-Fi yang tersedia di sekitar perangkat. Memilih jaringan yang terpercaya akan membantu menjaga keamanan selama proses instalasi sistem.

---

```bash
station wlan0 scan
```

Melakukan pemindaian ulang terhadap jaringan Wi-Fi agar daftar jaringan yang ditampilkan itu kondisi terbaru.

---

```bash
station wlan0 connect "(nama wifi)"
exit
```

Menghubungkan perangkat ke jaringan Wi-Fi terus keluar dari utilitas **iwd** kalau koneksi udah berhasil dilakukan.

---

```bash
ping 8.8.8.8
```

Menguji koneksi internet untuk memastikan sistem dapat mengakses repositori resmi Arch Linux sehingga proses instalasi dapat berjalan dengan lancar.

---

# 2. Mengelola Partisi Disk

```bash
lsblk
```

Menampilkan seluruh perangkat penyimpanan beserta struktur partisinya. Pemeriksaan ini penting buat mastiin si administrator ini memilih media penyimpanan yang benar sebelum melakukan proses partisi.

---

```bash
cfdisk /dev/partisi
```

Contoh:

```bash
cfdisk /dev/sda
```

atau

```bash
cfdisk /dev/nvme0n1
```

Digunakan untuk membuat dan mengatur partisi pada media penyimpanan.

Minimal partisi:

```text
boot = 3 GB (EFI System)
root = 70 GB (Linux filesystem)
```

CIS merekomendasikan pemisahan partisi penting seperti **/boot**, **/home**, **/var**, **/var/log**, **/var/log/audit**, dan **/var/tmp**. Pemisahan ini bertujuan untuk membatasi dampak apabila salah satu partisi mengalami gangguan serta memungkinkan penerapan kebijakan keamanan yang berbeda pada setiap direktori.

---

# 3. Membuat LVM (Logical Volume Manager)

## Membuat Physical Volume

```bash
pvcreate /dev/nvme0n1p7
```

Menginisialisasi partisi menjadi **Physical Volume (PV)** sehingga dapat digunakan oleh Logical Volume Manager (LVM). Penggunaan LVM memudahkan pengelolaan kapasitas penyimpanan dan mendukung pembagian partisi sesuai rekomendasi CIS.

---

## Membuat Volume Group

```bash
vgcreate wc /dev/nvme0n1p7
```

Membuat **Volume Group (VG)** sebagai kumpulan ruang penyimpanan yang berasal dari Physical Volume. Dengan Volume Group, kapasitas penyimpanan menjadi lebih fleksibel untuk dikelola.

---

## Membuat Logical Volume

```bash
lvcreate -L 5G -n root wc
lvcreate -L 5G -n vars wc
lvcreate -L 1G -n vlog wc
lvcreate -L 1G -n vaud wc
lvcreate -L 1G -n home wc
lvcreate -L 1.5G -n vtmp wc
lvcreate -L 9G -n podman wc
lvcreate -l50%FREE -n ngapa wc
```

Membuat beberapa **Logical Volume** untuk direktori yang akan digunakan oleh sistem. Pemisahan direktori seperti **/home**, **/var**, **/var/log**, **/var/log/audit**, dan **/var/tmp** sesuai dengan rekomendasi CIS karena mempermudah penerapan pengamanan seperti **nodev**, **nosuid**, dan **noexec**, sehingga risiko penyalahgunaan sistem dapat dikurangi.

---

```bash
lsblk
```

Memastikan seluruh Logical Volume berhasil dibuat dan telah terdeteksi oleh sistem sebelum proses instalasi dilanjutkan.

---

# 4. Membuat Filesystem

```bash
mkfs.ext4 /dev/wc/root
mkfs.ext4 /dev/wc/vars
mkfs.ext4 /dev/wc/vlog
mkfs.ext4 /dev/wc/vaud
mkfs.ext4 /dev/wc/home
mkfs.ext4 /dev/wc/vtmp
mkfs.ext4 /dev/wc/podman
```

Membuat filesystem **EXT4** pada setiap Logical Volume agar dapat digunakan sebagai media penyimpanan sistem operasi. Dengan filesystem yang terpisah, setiap partisi dapat diberikan pengaturan keamanan sesuai fungsinya sehingga pengelolaan sistem menjadi lebih aman.

---

# 5. Konfigurasi LUKS

```bash
cryptsetup luksFormat /dev/wc/ngapa
```

Menginisialisasi enkripsi **LUKS** pada Logical Volume sehingga seluruh data yang tersimpan akan terlindungi. Penerapan enkripsi ini membantu menjaga kerahasiaan data dan mencegah akses langsung dari pihak yang tidak memiliki otorisasi.---

```bash
cryptsetup luksOpen /dev/wc/ngapa ngape
```

Membuka volume yang telah dienkripsi sehingga dapat digunakan oleh sistem setelah proses autentikasi berhasil dilakukan.

---

```bash
mkfs.ext4 /dev/mapper/ngape
```

Membuat filesystem pada volume yang telah didekripsi sehingga siap digunakan sebagai media penyimpanan yang tetap terlindungi oleh mekanisme enkripsi.

# 6. Mount Partisi

## Mount Root

```bash
mount /dev/wc/root /mnt
```

Melakukan *mount* partisi root ke direktori `/mnt` sebagai lokasi instalasi Arch Linux. Tahap ini diperlukan agar seluruh file sistem dipasang pada direktori tujuan sebelum proses instalasi dimulai.

---

## Mount Filesystem

```bash
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
```

Melakukan *mount* partisi EFI ke direktori `/boot`. Hak akses dibuat lebih ketat menggunakan `fmask=0077` dan `dmask=0077` sehingga hanya pengguna dengan hak administratif yang dapat mengakses maupun mengubah file boot. Pengaturan ini membantu menjaga keamanan proses boot.

---

```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/wc/vars /mnt/var
```

Memasang partisi **/var** dengan opsi `nodev` dan `nosuid` untuk mencegah pembuatan *device file* serta penggunaan file SUID yang tidak diperlukan. Pengaturan ini membantu mengurangi risiko eskalasi hak akses.

---

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vlog /mnt/var/log
```

Memasang partisi **/var/log** menggunakan opsi `nodev`, `nosuid`, dan `noexec`. Konfigurasi ini bertujuan agar file log tidak dapat dijalankan sebagai program sehingga keamanan data log tetap terjaga.

---

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vaud /mnt/var/log/audit
```

Memasang partisi **/var/log/audit** secara terpisah agar log audit tetap aman dan tidak mudah dimodifikasi. Penggunaan opsi keamanan juga membantu menjaga integritas data audit.

---

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/home /mnt/home
```

Memasang partisi **/home** secara terpisah untuk menyimpan data pengguna. Pemisahan ini memudahkan pengelolaan data pengguna sekaligus membatasi dampak apabila terjadi gangguan pada partisi sistem.

---

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vtmp /mnt/var/tmp
```

Memasang partisi **/var/tmp** menggunakan opsi keamanan `nodev`, `nosuid`, dan `noexec` agar file sementara tidak dapat digunakan untuk menjalankan program berbahaya.

---

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/podman /mnt/var/lib/containers
```

Memasang direktori penyimpanan container pada partisi tersendiri sehingga data container terpisah dari sistem utama dan lebih mudah dikelola.

---

### Verifikasi Mount

```bash
lsblk
```

Memastikan seluruh partisi telah berhasil dipasang pada lokasi yang sesuai sebelum proses instalasi sistem dilanjutkan.

---

# 7. Install Package

```bash
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 openssh firewalld podman pacman sudo wget git neovim iwd grep which pam_mount
```

Menginstal paket dasar Arch Linux beserta paket pendukung yang diperlukan. Penggunaan kernel **linux-hardened** memberikan perlindungan tambahan terhadap berbagai potensi eksploitasi, sedangkan instalasi paket yang hanya sesuai kebutuhan membantu mengurangi *attack surface* pada sistem.

---

# 8. Membuat File fstab

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

Membuat file **fstab** berdasarkan UUID setiap partisi agar proses *mount* dilakukan secara otomatis ketika sistem dinyalakan. Penggunaan UUID membuat konfigurasi lebih stabil karena tidak bergantung pada nama perangkat penyimpanan.

---

# 9. Menggunakan tmpfs pada `/tmp`

```bash
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```

Menambahkan konfigurasi **tmpfs** pada direktori `/tmp` sehingga file sementara disimpan di memori. Penggunaan opsi `nodev`, `nosuid`, dan `noexec` membantu mencegah penyalahgunaan direktori sementara sebagai media menjalankan program berbahaya.

---

# 10. Konfigurasi Sistem

Masuk ke lingkungan sistem yang baru diinstal.

```bash
arch-chroot /mnt
```

Masuk ke sistem hasil instalasi untuk melakukan konfigurasi lanjutan sebelum sistem digunakan.

---

## Mengatur Hostname

```bash
nvim /etc/hostname
```

Menentukan nama host yang akan digunakan sebagai identitas perangkat pada jaringan sehingga memudahkan proses administrasi.

---

## Mengatur Timezone

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

Mengatur zona waktu dan menyinkronkan waktu perangkat keras dengan waktu sistem. Waktu yang akurat penting untuk memastikan pencatatan log dan audit dapat dilakukan dengan benar.

---

## Mengatur Locale

```bash
nvim /etc/locale.gen
```

Mengaktifkan locale yang akan digunakan oleh sistem.

```bash
locale-gen
```

Membuat konfigurasi locale berdasarkan pengaturan yang telah dipilih.

```bash
locale > /etc/locale.conf
```

Menyimpan konfigurasi locale ke dalam file `locale.conf`.

```bash
nvim /etc/locale.conf
```

Ubah isi file menjadi:

```text
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

Konfigurasi locale yang konsisten membantu memastikan aplikasi dan layanan sistem dapat berjalan dengan baik.

# 11. Membuat User

## Membuat Direktori Home

```bash
mkdir /home/user
```

Membuat direktori **home** yang akan digunakan sebagai tempat penyimpanan data pengguna. Pemisahan direktori pengguna dari sistem membantu menjaga keamanan data serta memudahkan pengelolaannya.

---

## Menambahkan User

```bash
useradd -d /home/user auah
```

Membuat akun pengguna baru dengan direktori **home** yang telah ditentukan. Penggunaan akun pengguna biasa lebih disarankan daripada menggunakan akun **root** untuk aktivitas sehari-hari agar risiko perubahan sistem secara tidak sengaja dapat dikurangi.

---

## Mengatur Kepemilikan Direktori Home

```bash
chown -R auah:auah /home/user
```

Memberikan hak kepemilikan direktori **home** kepada pengguna yang bersangkutan sehingga hanya pengguna tersebut yang dapat mengelola data pribadinya.

---

## Mengatur Password Root

```bash
passwd
```

Membuat atau mengubah kata sandi akun **root**. Kata sandi yang kuat membantu melindungi akun administrator dari akses yang tidak sah.

---

## Mengatur Password User

```bash
passwd auah
```

Membuat kata sandi untuk akun pengguna agar proses autentikasi dapat dilakukan dengan aman.

---

## Memberikan Hak Akses Sudo

```bash
echo "auah ALL=(ALL:ALL) ALL" > /etc/sudoers.d/auah
```

Memberikan hak akses **sudo** kepada pengguna sehingga tugas administratif dapat dilakukan tanpa harus selalu menggunakan akun **root**. Pemberian hak akses administrator sebaiknya hanya diberikan kepada pengguna yang memang membutuhkannya.

---

# 12. Konfigurasi PAM Mount

Mengedit file konfigurasi PAM Mount.

```bash
nvim /etc/security/pam_mount.conf.xml
```

Membuka file konfigurasi **PAM Mount** untuk mengatur proses *mount* otomatis pada volume terenkripsi setelah pengguna berhasil melakukan login.

---

Menambahkan konfigurasi volume terenkripsi.

```xml
<volume
    user="auah"
    fstype="crypt"
    path="/dev/wc/ngapa"
    mountpoint="/home/user"
/>
```

Menghubungkan akun pengguna dengan volume terenkripsi sehingga data hanya dapat diakses setelah proses autentikasi berhasil dilakukan.

---

Mengedit konfigurasi PAM Login.

```bash
nvim /etc/pam.d/system-login
```

Menambahkan konfigurasi berikut.

```text
auth       required   pam_mount.so
session    optional   pam_mount.so
```

Mengaktifkan modul **pam_mount** pada proses autentikasi sehingga volume terenkripsi akan terbuka secara otomatis saat pengguna berhasil login.

---

Mengedit konfigurasi **mkinitcpio**.

```bash
nvim /etc/mkinitcpio.conf
```

Tambahkan `sd-encrypt` dan `lvm2` pada bagian **HOOKS**.

Penambahan modul tersebut memungkinkan sistem mengenali LVM dan volume terenkripsi sejak proses boot sehingga sistem dapat dijalankan dengan baik.

---

# 13. Menyalin Konfigurasi Jaringan

Keluar dari lingkungan **chroot**.

```bash
exit
```

Keluar dari sistem hasil instalasi sementara untuk menyalin konfigurasi jaringan.

---

Menyalin konfigurasi jaringan.

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

Menyalin konfigurasi jaringan ke sistem yang baru diinstal agar pengaturan jaringan tetap digunakan setelah proses instalasi selesai.

---

Masuk kembali ke lingkungan **chroot**.

```bash
arch-chroot /mnt
```

Masuk kembali ke sistem hasil instalasi untuk melanjutkan konfigurasi.

---

Mengedit preset kernel.

```bash
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

Ubah isi file menjadi:

```text
PRESETS=('default')
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
```

Menggunakan **linux-hardened** sebagai kernel utama karena memiliki perlindungan tambahan terhadap berbagai potensi eksploitasi dan meningkatkan keamanan sistem.

---

# 14. Instalasi Bootloader (systemd-boot)

Menginstal bootloader.

```bash
bootctl --path=/boot install
```

Menginstal **systemd-boot** pada partisi EFI agar sistem dapat melakukan proses boot.

---

Keluar dari lingkungan **chroot**.

```bash
exit
```

Keluar sementara dari sistem hasil instalasi.

---

Menginstal bootloader pada sistem.

```bash
bootctl --path=/mnt/boot install
```

Memastikan **systemd-boot** telah terpasang dengan benar pada sistem hasil instalasi sehingga sistem siap dijalankan.

---

# 15. Konfigurasi Kernel Command Line

Masuk kembali ke sistem.

```bash
arch-chroot /mnt
```

Masuk kembali ke sistem hasil instalasi untuk melakukan konfigurasi kernel.

---

Mengedit konfigurasi kernel.

```bash
nvim /etc/cmdline.conf
```

Isi file:

```text
root=/dev/wc/root rw
```

Menentukan lokasi partisi **root** yang akan digunakan saat proses boot.

---

Membangun ulang initramfs.

```bash
mkinitcpio -P
```

Menerapkan perubahan konfigurasi agar digunakan ketika sistem dijalankan.

---

Menghapus file konfigurasi lama.

```bash
rm -fr /etc/cmdline.conf
```

Menghapus konfigurasi yang sudah tidak digunakan untuk menghindari konflik dengan konfigurasi baru.

---

Membuat direktori konfigurasi.

```bash
mkdir /etc/cmdline.d
```

Menyiapkan direktori khusus untuk menyimpan konfigurasi kernel agar lebih mudah dikelola.

---

Membuat file konfigurasi baru.

```bash
nvim /etc/cmdline.d/01-boot.conf
```

Isi file:

```text
root=/dev/wc/root rw
```

Menyimpan konfigurasi kernel pada direktori yang telah disediakan.

---

Membangun ulang initramfs.

```bash
mkinitcpio -P
```

Memastikan seluruh perubahan konfigurasi telah diterapkan sebelum sistem digunakan.

---

# 16. Mengaktifkan Service

Mengaktifkan layanan Wi-Fi.

```bash
systemctl enable iwd
```

Mengaktifkan layanan **iwd** agar koneksi Wi-Fi dapat digunakan secara otomatis setelah sistem dinyalakan.

---

Mengaktifkan layanan jaringan.

```bash
systemctl enable systemd-networkd
```

Mengaktifkan layanan jaringan sehingga konfigurasi jaringan dijalankan secara otomatis saat proses boot.

---

Mengaktifkan DNS Resolver.

```bash
systemctl enable systemd-resolved
```

Mengaktifkan layanan DNS agar sistem dapat menerjemahkan nama domain menjadi alamat IP.

---

Mengaktifkan Firewall.

```bash
systemctl enable firewalld
```

Mengaktifkan **Firewalld** untuk mengontrol lalu lintas jaringan dan membatasi akses hanya pada layanan yang diperlukan sehingga keamanan sistem lebih terjaga.

---

Mengaktifkan SSH.

```bash
systemctl enable sshd
```

Mengaktifkan layanan **SSH** agar administrasi sistem dapat dilakukan dari jarak jauh melalui koneksi yang aman.

---

Mengaktifkan Podman.

```bash
systemctl enable --global podman
```

Mengaktifkan layanan **Podman** sehingga container dapat dijalankan secara otomatis sesuai kebutuhan.

---

# 17. Menyelesaikan Instalasi

Keluar dari lingkungan **chroot**.

```bash
exit
```

Keluar dari sistem hasil instalasi setelah seluruh konfigurasi selesai dilakukan.

---

Melepas seluruh partisi.

```bash
umount -R /mnt
```

Melepas seluruh partisi yang telah dipasang untuk memastikan tidak ada data yang masih tersimpan di memori sebelum sistem dimatikan atau dihidupkan ulang.

---

Melakukan restart sistem.

```bash
reboot
```

Memulai ulang sistem agar seluruh konfigurasi yang telah diterapkan dapat berjalan dan sistem siap digunakan.

# INSTALASI DESKTOP XFCE BESERTA FITUR-FITURNYA
Tugas ini disusun untuk memenuhi tugas mata kuliah Perpustakaan dan Arsip Digital
## Dosen Pengampu:
Al Muhdil Karim, S.IP, M.Hum.
## Disusun Oleh Kelompok 7:
1. Iskandar (12402051030027)
2. Muhammad Tri Andhika Yustisia (12402051030040)
3. Riski Hidayat (12402051050117)
# Kata Pengantar
Puji dan Syukur kami panjatkan ke hadirat Allah SWT yang Maha Esa atas segala rahmat dan karunia-Nya sehingga kami dapat menyelesaikan makalah ini dengan baik. makalah ini kami berikan judul “INSTALASI DESKTOP XFCE BESERTA FITUR-FITURNYA”.

Makalah ini disusun untuk memenuhi tugas kelompok pada mata kuliah Perpustakaan dan Arsip Digital. Tidak lupa juga kami ucapkan terima kasih kepada Bapak Al Muhdil Karim, S.IP, M.Hum. selaku dosen pengampu mata kuliah dan teman-teman yang telah ikut serta memberikan dukungan dan bantuan dalam proses penyusunan makalah ini.

Kami menyadari bahwa makalah ini masih jauh dari kata sempurna. Oleh karena itu, kritik dan saran yang membangun sangat kami harapkan demi penyempurnaan makalah ini di masa mendatang. Semoga makalah yang kami buat ini bermanfaat bagi para pembaca serta dapat menambah wawasan tentang Perpustakaan dan Arsip Digital.

# Bab I
## Pendahaluan
Arch Linux adalah distribusi GNU/Linux serbaguna x86-64 yang dikembangkan secara independen dan cukup fleksibel untuk memenuhi berbagai kebutuhan. Pengembangan berfokus pada kesederhanaan, minimalisme, dan keanggunan kode. Arch diinstal sebagai sistem dasar minimal, yang dikonfigurasi oleh pengguna, di mana lingkungan ideal mereka sendiri dirakit dengan hanya menginstal apa yang dibutuhkan atau diinginkan untuk tujuan unik mereka. Utilitas konfigurasi GUI tidak disediakan secara resmi, dan sebagian besar konfigurasi sistem dilakukan dari shell dengan mengedit file teks sederhana. Arch berupaya untuk selalu mengikuti perkembangan terbaru, dan biasanya menawarkan versi stabil terbaru dari sebagian besar perangkat lunak. Arch Linux menggunakan pengelola paket Pacman miliknya sendiri, yang menggabungkan paket biner sederhana dengan sistem pembuatan paket yang mudah digunakan. Hal ini memungkinkan pengguna untuk dengan mudah mengelola dan menyesuaikan paket mulai dari perangkat lunak resmi Arch hingga paket pribadi pengguna sendiri hingga paket dari sumber pihak ketiga. Sistem repositori juga memungkinkan pengguna untuk dengan mudah membangun dan memelihara skrip pembuatan, paket, dan repositori kustom mereka sendiri, mendorong pertumbuhan dan kontribusi komunitas.

Konfigurasi sistem pada tabel tersebut menunjukkan rancangan lingkungan kerja berbasis Arch Linux yang dirancang dengan fokus pada keamanan, efisiensi, fleksibilitas, dan performa sistem. Sistem ini menggunakan Booster sebagai initramfs generator untuk mempercepat proses booting, serta menerapkan metode enkripsi “LUKS on LVM” guna memberikan perlindungan data sekaligus fleksibilitas pengelolaan partisi. Antarmuka desktop menggunakan XFCE karena ringan dan stabil, sedangkan Superfile dipilih sebagai file manager berbasis terminal yang modern dan efisien. Layout disk mengikuti standar CIS (Center for Internet Security) yang menekankan pemisahan partisi penting demi meningkatkan keamanan sistem. Untuk kebutuhan containerisasi digunakan Podman dan Podman Desktop karena mendukung rootless container yang lebih aman dibanding pendekatan tradisional. Sistem keamanan jaringan diperkuat menggunakan iptables sebagai firewall untuk mengatur lalu lintas jaringan masuk dan keluar. Pada sisi multimedia digunakan MPD, MPC, dan MPV yang dikenal ringan dan optimal untuk lingkungan Linux minimalis. Pengelolaan password dilakukan menggunakan KeePassXC yang menyediakan penyimpanan password terenkripsi, sedangkan OpenSSH digunakan untuk akses jarak jauh secara aman melalui protokol SSH. Keseluruhan konfigurasi ini menunjukkan implementasi sistem Linux modern yang tidak hanya berorientasi pada penggunaan sehari-hari, tetapi juga memperhatikan aspek keamanan data, efisiensi resource, stabilitas sistem, dan kemudahan administrasi.

## Rumusan Masalah
- Bagaimana langkah instalasi booster
- Bagaimana langkah disk enkripsi luks on lvm
- Bagaimana langkah menginstall dekstop xfce
- Bagaimana langkah menginstall super file
- Bagaimana langkah disk layout CIS
- Bagaimana langkah instalasi podman, podman desktop
- Bagaimana langkah instalasi Firewall iptables
- Bagaimana langkah instalasi mpd,mpc,mpv
- Bagaimana langkah instalasi password keepass xc
- Bagaimana langkah instalasi key secret
- Bagaimana langkah acces openssh

## Tujuan Masalah
- Menjelaskan langkah instalasi booster
- Menjelaskan langkah mengdisk enkripsi luks on lvm
- Menjelaskan langkah menginstall desktop xfce
- MEnjelaskan langkah menginstall super file
- Menjelaskan langkah mengdisk layout CIS
- Menjelaskan langkah instalasi pdman, podman desktop
- Menjelaskan langkah instalasi Firewall iptables
- Menjelaskan langkah instalasi mpd, mpc, mpv
- Menjelaskan langkah instalasi password keepass xc
- Menjelaskan langkah instalasi key secret
- Menjelaskan langkah penggunaan acces openssh

# BAB II - PEMBAHASAN

## 1. Enkripsi Disk dengan LUKS

> **Referensi:** Arch Wiki - *Data-at-rest encryption* (https://wiki.archlinux.org/title/Data-at-rest_encryption); `cryptsetup` man page (https://man.archlinux.org/man/cryptsetup.8)

dm-crypt adalah fungsionalitas enkripsi device-mapper standar yang disediakan oleh kernel Linux. LUKS (Linux Unified Key Setup), yang digunakan secara default, adalah lapisan kenyamanan tambahan yang menyimpan semua informasi setup yang diperlukan untuk dm-crypt langsung pada disk.

```bash
cryptsetup luksFormat \
  --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --hash sha512 \
  --iter-time 5000 \
  /dev/nvme0n1p7
```

**Penjelasan parameter:**

| Parameter | Keterangan |
|---|---|
| `luksFormat` | Memformat partisi dengan enkripsi LUKS |
| `--type luks2` | Menggunakan format LUKS versi 2 yang lebih modern dan aman |
| `--cipher aes-xts-plain64` | Algoritma AES dengan mode XTS, standar enkripsi disk penuh |
| `--key-size 512` | Ukuran kunci 512-bit untuk keamanan tinggi |
| `--hash sha512` | Fungsi hash SHA-512 untuk derivasi kunci |
| `--iter-time 5000` | Waktu iterasi 5000 ms untuk memperlambat brute-force attack |
| `/dev/nvme0n1p7` | Partisi target yang akan dienkripsi |

> Setelah menjalankan perintah ini, sistem akan meminta pembuatan password untuk membuka enkripsi saat booting. Garis miring ke kiri (`\`) hanya digunakan untuk merapikan penulisan agar lebih mudah dibaca.

---

## 2. Membuka Enkripsi dan Membuat Physical Volume (LVM)

> **Referensi:** Arch Wiki - *LVM* (https://wiki.archlinux.org/title/LVM); Arch Wiki - *dm-crypt/Encrypting an entire system* (https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system)

Pola penggunaan LUKS on LVM memungkinkan enkripsi yang mencakup beberapa drive atau fleksibilitas tata letak partisi.

```bash
cryptsetup open /dev/nvme0n1p7 cryptlvm
pvcreate /dev/mapper/cryptlvm
vgcreate vg0 /dev/mapper/cryptlvm
pvs
vgs
```

**Penjelasan:**

- `cryptsetup open /dev/nvme0n1p7 cryptlvm` - Membuka partisi terenkripsi dan memetakannya ke `/dev/mapper/cryptlvm`. Password akan diminta di sini.
- `pvcreate /dev/mapper/cryptlvm` - Menginisialisasi perangkat sebagai Physical Volume (PV) untuk LVM.
- `vgcreate vg0 /dev/mapper/cryptlvm` - Membuat Volume Group (VG) bernama `vg0`.
- `pvs` / `vgs` - Menampilkan daftar physical volume / volume group untuk verifikasi.

---

## 3. Membuat Logical Volume (LV) - Disk Layout CIS

> **Referensi:** Arch Wiki - *Security* (https://wiki.archlinux.org/title/Security); CIS Benchmark for Linux - *Partitioning* (https://www.cisecurity.org/benchmark/distribution_independent_linux)

CIS (Center for Internet Security) merekomendasikan pemisahan partisi-partisi kritis sistem. Penerapan opsi mount seperti `noexec`, `nosuid`, dan `nodev` pada partisi terpisah seperti `/tmp`, `/var`, `/home` merupakan praktik penguatan keamanan sistem Linux yang standar.

```bash
lvcreate -L 2G vg0 -n lv_tmp
lvcreate -L 3G vg0 -n lv_var
lvcreate -L 2G vg0 -n lv_var_log
lvcreate -L 2G vg0 -n lv_var_log_audit
lvcreate -L 1G vg0 -n lv_var_tmp
lvcreate -L 5G vg0 -n lv_home
lvcreate -l 100%FREE vg0 -n lv_root
lvs
```

**Penjelasan:**

| Logical Volume | Ukuran | Fungsi |
|---|---|---|
| `lv_tmp` | 2G | Direktori `/tmp` (file sementara) |
| `lv_var` | 3G | Direktori `/var` (data variabel sistem) |
| `lv_var_log` | 2G | Log sistem `/var/log` |
| `lv_var_log_audit` | 2G | Log audit keamanan `/var/log/audit` |
| `lv_var_tmp` | 1G | File sementara `/var/tmp` |
| `lv_home` | 5G | Direktori home pengguna |
| `lv_root` | Sisa | Partisi root `/` (100% sisa ruang) |

- `lvcreate -l 100%FREE` - Menggunakan seluruh sisa ruang kosong untuk partisi root.
- `lvs` - Menampilkan semua logical volume untuk verifikasi.

---

## 4. Memformat Partisi (Filesystem)

> **Referensi:** Arch Wiki - *File systems* (https://wiki.archlinux.org/title/File_systems); `mkfs.ext4` man page (https://man.archlinux.org/man/mkfs.ext4.8)

```bash
mkfs.ext4 /dev/vg0/lv_root
mkfs.ext4 /dev/vg0/lv_tmp
mkfs.ext4 /dev/vg0/lv_var
mkfs.ext4 /dev/vg0/lv_var_log
mkfs.ext4 /dev/vg0/lv_var_log_audit
mkfs.ext4 /dev/vg0/lv_var_tmp
mkfs.ext4 /dev/vg0/lv_home
mkfs.fat -F 32 /dev/nvme0n1p6
```

**Penjelasan:**

- `mkfs.ext4` - Memformat logical volume dengan filesystem ext4, filesystem Linux modern yang andal dan stabil.
- `mkfs.fat -F 32 /dev/nvme0n1p6` - Memformat partisi EFI dengan filesystem FAT32, yang diperlukan untuk partisi boot EFI (ESP).

---

## 5. Mounting Partisi

> **Referensi:** Arch Wiki - *Installation guide* (https://wiki.archlinux.org/title/Installation_guide); `mount` man page (https://man.archlinux.org/man/mount.8)

```bash
mount /dev/vg0/lv_root /mnt
mkdir -p /mnt/{boot,tmp,var,home}
mkdir -p /mnt/var/{log,tmp}
mkdir -p /mnt/var/log/audit
mount /dev/nvme0n1p6 /mnt/boot
mount /dev/vg0/lv_tmp /mnt/tmp
mount /dev/vg0/lv_var /mnt/var
mount /dev/vg0/lv_var_log /mnt/var/log
mount /dev/vg0/lv_var_log_audit /mnt/var/log/audit
mount /dev/vg0/lv_var_tmp /mnt/var/tmp
mount /dev/vg0/lv_home /mnt/home
```

**Penjelasan:**

- `mount /dev/vg0/lv_root /mnt` - Memasang partisi root ke `/mnt` sebagai titik awal instalasi.
- `mkdir -p /mnt/{boot,tmp,var,home}` - Membuat direktori-direktori yang diperlukan sekaligus (`-p` untuk membuat parent directory jika belum ada).
- Setiap perintah `mount` berikutnya memasang logical volume yang sesuai ke direktori masing-masing.

> **Catatan:** Jika muncul error "mount point doesn't exist", buat direktorinya terlebih dahulu: `mkdir -p /mnt/var/log && mkdir -p /mnt/var/log/audit && mkdir -p /mnt/var/tmp`

---

## 6. Instalasi Sistem Dasar (pacstrap)

> **Referensi:** Arch Wiki - *Installation guide § Install essential packages* (https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages); Arch Wiki - *Pacman* (https://wiki.archlinux.org/title/Pacman)

```bash
pacstrap -K /mnt base base-devel linux linux-firmware lvm2 cryptsetup mkinitcpio networkmanager sudo vim amd-ucode
```

**Penjelasan paket:**

| Paket | Fungsi |
|---|---|
| `base` | Paket inti sistem Arch Linux |
| `base-devel` | Alat pengembangan dasar (gcc, make, dll) |
| `linux` | Kernel Linux |
| `linux-firmware` | Firmware untuk berbagai perangkat keras |
| `lvm2` | Tools manajemen LVM |
| `cryptsetup` | Tools enkripsi LUKS |
| `mkinitcpio` | Alat pembuatan initramfs |
| `networkmanager` | Manajer koneksi jaringan |
| `sudo` | Alat menjalankan perintah sebagai superuser |
| `vim` | Editor teks berbasis terminal |
| `amd-ucode` | Microcode update untuk prosesor AMD |

> **Catatan Penting:** Untuk prosesor Intel, ganti `amd-ucode` dengan `intel-ucode`.

---

## 7. Generate fstab

> **Referensi:** Arch Wiki - *Fstab* (https://wiki.archlinux.org/title/Fstab); `genfstab` man page (https://man.archlinux.org/man/genfstab.8)

```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

**Penjelasan:**

- `genfstab -U /mnt` - Membuat file `fstab` secara otomatis berdasarkan partisi yang terpasang. `-U` menggunakan UUID sebagai identifikasi partisi (lebih andal daripada nama perangkat).
- `>> /mnt/etc/fstab` - Menambahkan output ke file `/mnt/etc/fstab`.
- `cat /mnt/etc/fstab` - Menampilkan isi file fstab untuk verifikasi.

---

## 8. Konfigurasi Sistem (arch-chroot)

> **Referensi:** Arch Wiki - *Installation guide § Configure the system* (https://wiki.archlinux.org/title/Installation_guide#Configure_the_system); `arch-chroot` man page (https://man.archlinux.org/man/arch-chroot.8)

```bash
arch-chroot /mnt
```

Perintah ini masuk ke dalam sistem yang baru diinstal seolah-olah sistem tersebut sudah berjalan (chroot environment).

### 8.1 Zona Waktu

> **Referensi:** Arch Wiki - *System time* (https://wiki.archlinux.org/title/System_time); `hwclock` man page (https://man.archlinux.org/man/hwclock.8)

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

- `ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime` - Mengatur zona waktu ke Jakarta (WIB/UTC+7).
- `hwclock --systohc` - Menyinkronkan jam hardware dengan waktu sistem.

### 8.2 Locale (Bahasa)

> **Referensi:** Arch Wiki - *Locale* (https://wiki.archlinux.org/title/Locale); `locale-gen` man page (https://man.archlinux.org/man/locale-gen.8)

```bash
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

- `echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen` - Menambahkan locale bahasa Inggris UTF-8 ke daftar locale.
- `locale-gen` - Membuat file locale berdasarkan konfigurasi di `/etc/locale.gen`.
- `echo "LANG=en_US.UTF-8" > /etc/locale.conf` - Menetapkan locale default sistem.

### 8.3 Hostname dan Membuat User

> **Referensi:** Arch Wiki - *Users and groups* (https://wiki.archlinux.org/title/Users_and_groups); `useradd` man page (https://man.archlinux.org/man/useradd.8)

```bash
echo "dika" > /etc/hostname
passwd
useradd -m -G wheel -s /bin/bash dika
passwd dika
```

- `echo "dika" > /etc/hostname` - Mengatur nama host komputer.
- `passwd` - Mengatur password untuk user root.
- `useradd -m -G wheel -s /bin/bash dika` - Membuat user baru:
  - `-m` : Membuat direktori home untuk user
  - `-G wheel` : Menambahkan user ke grup `wheel` (diperlukan untuk sudo)
  - `-s /bin/bash` : Menggunakan bash sebagai shell default
- `passwd dika` - Mengatur password untuk user "dika".

### 8.4 Mengaktifkan sudo untuk Grup wheel

> **Referensi:** Arch Wiki - *Sudo* (https://wiki.archlinux.org/title/Sudo); `visudo` man page (https://man.archlinux.org/man/visudo.8)

```bash
EDITOR=vim visudo
```

Cari dan hapus tanda `#` pada baris berikut:

```
# %wheel ALL=(ALL:ALL) ALL
```

Menjadi:

```
%wheel ALL=(ALL:ALL) ALL
```

> **Tips vim:** gunakan `/` untuk mencari kata, `i` untuk masuk mode edit, `:wq` untuk menyimpan dan keluar, `:q!` untuk keluar tanpa menyimpan.

---

## 9. Konfigurasi mkinitcpio (Initramfs)

> **Referensi:** Arch Wiki - *Mkinitcpio* (https://wiki.archlinux.org/title/Mkinitcpio); Arch Wiki - *dm-crypt/System configuration* (https://wiki.archlinux.org/title/Dm-crypt/System_configuration)

```bash
vim /etc/mkinitcpio.conf
```

Ubah baris `HOOKS` dari:

```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block filesystems fsck)
```

Menjadi:

```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block encrypt lvm2 filesystems fsck)
```

**Penjelasan perubahan HOOKS:**

| Hook | Fungsi |
|---|---|
| `udev` | Menggantikan `systemd` agar kompatibel dengan hook `encrypt` dan `lvm2` |
| `keymap` | Dukungan keyboard layout di konsol |
| `consolefont` | Dukungan font di konsol |
| `encrypt` | Membuka enkripsi LUKS saat booting |
| `lvm2` | Mengaktifkan LVM saat booting |

Setelah menyimpan, generate ulang initramfs:

```bash
mkinitcpio -P
```

- `mkinitcpio -P` - Membuat ulang semua preset initramfs (image linux standar dan fallback).

---

## 10. Instalasi Bootloader (systemd-boot)

> **Referensi:** Arch Wiki - *Systemd-boot* (https://wiki.archlinux.org/title/Systemd-boot); `bootctl` man page (https://man.archlinux.org/man/bootctl.1)

### 10.1 Mendapatkan UUID dan Instalasi Bootloader

```bash
blkid -s UUID -o value /dev/nvme0n1p7
bootctl install
```

- `blkid -s UUID -o value /dev/nvme0n1p7` - Menampilkan UUID partisi terenkripsi. Catat UUID ini untuk langkah berikutnya.
- `bootctl install` - Menginstal systemd-boot sebagai bootloader ke partisi EFI.

### 10.2 Konfigurasi Boot Entry

```bash
vim /boot/loader/entries/arch.conf
```

Isi file konfigurasi (ganti UUID dengan nilai dari `blkid`):

```
title Arch Linux
linux /vmlinuz-linux
initrd /amd-ucode.img
initrd /initramfs-linux.img
options cryptdevice=UUID=90cc9214-41b7-46e2-903b-c1158809e36a:cryptlvm root=/dev/vg0/lv_root rw quiet
```

**Penjelasan:**

- `title` - Nama yang ditampilkan di menu boot
- `linux` - Path ke kernel Linux
- `initrd /amd-ucode.img` - Memuat microcode AMD (ganti `intel-ucode.img` untuk Intel)
- `initrd /initramfs-linux.img` - Memuat initramfs yang telah dibuat
- `cryptdevice=UUID=...:cryptlvm` - Memberi tahu kernel untuk membuka perangkat terenkripsi dengan UUID tersebut
- `root=/dev/vg0/lv_root` - Menentukan partisi root sistem
- `rw quiet` - Memasang root sebagai read-write, menyembunyikan pesan boot tidak penting

### 10.3 Konfigurasi Loader

```bash
vim /boot/loader/loader.conf
```

Isi:

```
default arch.conf
timeout 3
console-mode max
```

- `default arch.conf` - Entry boot default
- `timeout 3` - Menampilkan menu boot selama 3 detik
- `console-mode max` - Menggunakan resolusi konsol tertinggi

---

## 11. Finalisasi Instalasi

> **Referensi:** Arch Wiki - *NetworkManager* (https://wiki.archlinux.org/title/NetworkManager); Arch Wiki - *Systemd* (https://wiki.archlinux.org/title/Systemd)

```bash
systemctl enable NetworkManager
exit
umount -R /mnt
reboot
```

- `systemctl enable NetworkManager` - Mengaktifkan NetworkManager agar berjalan otomatis saat boot.
- `exit` - Keluar dari chroot environment.
- `umount -R /mnt` - Melepas semua partisi yang terpasang di `/mnt` secara rekursif.
- `reboot` - Me-restart komputer untuk boot ke sistem Arch Linux baru.

---

## 12. Konfigurasi Pasca-Instalasi

### 12.1 Koneksi Jaringan

> **Referensi:** Arch Wiki - *NetworkManager* (https://wiki.archlinux.org/title/NetworkManager)

```bash
nmtui
ping -c 3 8.8.8.8
```

- `nmtui` - Antarmuka TUI (Text User Interface) untuk mengelola koneksi jaringan.
- `ping -c 3 8.8.8.8` - Menguji koneksi internet dengan mengirim 3 paket ke DNS Google.

### 12.2 Instalasi Desktop Environment XFCE

> **Referensi:** Arch Wiki - *Xfce* (https://wiki.archlinux.org/title/Xfce); Arch Wiki - *LightDM* (https://wiki.archlinux.org/title/LightDM); Arch Wiki - *PipeWire* (https://wiki.archlinux.org/title/PipeWire)

Xfce adalah desktop environment yang ringan dan modular berbasis GTK. Xfce menerapkan filosofi UNIX tradisional tentang modularitas dan re-usability, menyertakan window manager, file manager, desktop, dan panel.

```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter xorg-server pipewire pipewire-pulse
sudo systemctl enable lightdm
sudo systemctl enable NetworkManager
```

**Penjelasan paket:**

| Paket | Fungsi |
|---|---|
| `xfce4` | Desktop environment XFCE yang ringan |
| `xfce4-goodies` | Plugin dan aplikasi tambahan untuk XFCE |
| `lightdm` | Display manager (login screen) |
| `lightdm-gtk-greeter` | Tampilan greeter untuk LightDM |
| `xorg-server` | Server grafis X11 |
| `pipewire` | Server audio modern |
| `pipewire-pulse` | Kompatibilitas PulseAudio untuk PipeWire |

---

## 13. Instalasi Aplikasi Tambahan

### 13.1 Instalasi yay (AUR Helper)

> **Referensi:** Arch Wiki - *AUR helpers* (https://wiki.archlinux.org/title/AUR_helpers); Arch Wiki - *Makepkg* (https://wiki.archlinux.org/title/Makepkg)

```bash
sudo pacman -S git go
cd /tmp
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

- `git clone https://aur.archlinux.org/yay.git` - Mengunduh source code yay dari AUR.
- `makepkg -si` - Membangun dan menginstal paket dari source (`-s` menginstal dependensi, `-i` langsung menginstal).

### 13.2 Instalasi Superfile (File Manager Terminal)

> **Referensi:** Arch Wiki - *List of applications/Utilities* (https://wiki.archlinux.org/title/List_of_applications/Utilities); Superfile official site (https://superfile.netlify.app)

Superfile adalah terminal file manager yang modern dan fancy, dikategorikan sebagai file manager berbasis terminal yang menawarkan pendekatan segar dan modern dalam manajemen file langsung dari terminal.

```bash
yay -S superfile-bin
```

### 13.3 Instalasi Podman dan Podman Desktop

> **Referensi:** Arch Wiki - *Podman* (https://wiki.archlinux.org/title/Podman); Podman official documentation (https://docs.podman.io)

Podman adalah alternatif dari Docker yang beroperasi tanpa daemon (daemonless) dan mendukung container rootless, sehingga container dapat dijalankan oleh pengguna biasa tanpa hak administrator.

```bash
yay -S podman-desktop
sudo pacman -S podman slirp4netns fuse-overlayfs
```

**Penjelasan:**

- `podman` - Alat manajemen container tanpa daemon (rootless).
- `slirp4netns` - Dependensi untuk networking Podman rootless.
- `fuse-overlayfs` - Dependensi untuk filesystem overlay Podman rootless.

#### Konfigurasi Podman Rootless

```bash
sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 riski
systemctl --user enable --now podman.socket
```

- `usermod --add-subuids` - Mendaftarkan rentang UID/GID untuk container rootless.
- `systemctl --user enable --now podman.socket` - Mengaktifkan socket Podman untuk user saat ini.

### 13.4 Instalasi KeePassXC (Password Manager)

> **Referensi:** Arch Wiki - *KeePass* (https://wiki.archlinux.org/title/KeePass); KeePassXC official site (https://keepassxc.org)

KeePassXC adalah manajer password open-source yang menyimpan credential secara terenkripsi dalam file database lokal (`.kdbx`). KeePassXC mengandung integrasi Freedesktop.org Secret Service yang memungkinkan aplikasi eksternal menggunakannya sebagai database terenkripsi.

```bash
sudo pacman -S keepassxc
```

Penggunaan KeePassXC untuk menyimpan password LUKS:

```
keepassxc
→ Create Database
→ New Entry (Ctrl+N)
  Title: LUKS encryption
  Username: riski
  Password: (isi password LUKS)
→ OK → Ctrl+S (save)
```

### 13.5 Instalasi OpenSSH

> **Referensi:** Arch Wiki - *OpenSSH* (https://wiki.archlinux.org/title/OpenSSH); OpenSSH official documentation (https://www.openssh.com/manual.html)

OpenSSH (OpenBSD Secure Shell) adalah sekumpulan program komputer yang menyediakan sesi komunikasi terenkripsi melalui jaringan komputer menggunakan protokol Secure Shell (SSH).

```bash
sudo pacman -S openssh
sudo systemctl enable --now sshd
```

---

## 14. Instalasi Firewall iptables

> **Referensi:** Arch Wiki - *Iptables* (https://wiki.archlinux.org/title/Iptables); `iptables` man page (https://man.archlinux.org/man/iptables.8)

iptables adalah utilitas command line untuk mengonfigurasi firewall kernel Linux yang diimplementasikan dalam proyek Netfilter. iptables digunakan untuk memeriksa, memodifikasi, meneruskan, mengarahkan ulang, dan/atau membuang paket IP.

```bash
sudo pacman -S iptables-nft
sudo nano /etc/iptables/iptables.rules
```

Isi file aturan firewall:

```
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]

-A INPUT -i lo -j ACCEPT
-A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p tcp --dport 2222 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp -s 127.0.0.1 --dport 6600 -j ACCEPT
-A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT

COMMIT
```

**Penjelasan aturan firewall:**

| Aturan | Fungsi |
|---|---|
| `:INPUT DROP` | Memblokir semua koneksi masuk secara default |
| `:FORWARD DROP` | Memblokir semua paket yang diteruskan |
| `:OUTPUT ACCEPT` | Mengizinkan semua koneksi keluar |
| `-i lo -j ACCEPT` | Mengizinkan traffic loopback (localhost) |
| `--ctstate ESTABLISHED,RELATED` | Mengizinkan koneksi yang sudah ada |
| `--dport 2222` | Mengizinkan koneksi SSH pada port 2222 |
| `--dport 80/443` | Mengizinkan koneksi HTTP dan HTTPS |
| `-s 127.0.0.1 --dport 6600` | Mengizinkan akses MPD hanya dari localhost |
| `-s 127.0.0.1 --dport 8080` | Mengizinkan akses web lokal hanya dari localhost |

---

## 15. Instalasi MPD, MPC, dan MPV

> **Referensi:** Arch Wiki - *Music Player Daemon* (https://wiki.archlinux.org/title/Music_Player_Daemon); Arch Wiki - *Mpv* (https://wiki.archlinux.org/title/Mpv); `mpd` man page (https://man.archlinux.org/man/mpd.1)

MPD (Music Player Daemon) adalah audio player yang memiliki arsitektur server-client. MPD memutar file audio, mengatur playlist, dan memelihara database musik dengan menggunakan sangat sedikit resource. MPC adalah client command-line resmi untuk mengontrol MPD. mpv adalah media player berbasis MPlayer yang mendukung berbagai format file video dan codec.

```bash
sudo pacman -S mpd mpc mpv
```

#### Konfigurasi MPD

```bash
mkdir -p ~/.config/mpd ~/.local/share/mpd ~/Music
sudo pacman -S nano
nano ~/.config/mpd/mpd.conf
```

Isi file konfigurasi MPD:

```
music_directory "~/Music"
db_file "~/.local/share/mpd/database"
log_file "~/.local/share/mpd/log"
pid_file "~/.local/share/mpd/pid"
audio_output {
    type "pipewire"
    name "PipeWire"
}
```

Simpan dengan `Ctrl+X`, lalu `Y`, lalu `Enter`.

```bash
systemctl --user enable --now mpd
```

---

## 16. Hardening Sistem (Kernel & Sysctl)

> **Referensi:** Arch Wiki - *Security* (https://wiki.archlinux.org/title/Security); CIS Benchmark for Linux (https://www.cisecurity.org/benchmark/distribution_independent_linux); `sysctl` man page (https://man.archlinux.org/man/sysctl.8)

```bash
sudo nano /etc/sysctl.d/99-cis.conf
```

Isi konfigurasi:

```
kernel.dmesg_restrict = 1
kernel.kptr_restrict = 2
kernel.yama.ptrace_scope = 1
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv6.conf.all.disable_ipv6 = 1
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
fs.suid_dumpable = 0
```

Terapkan konfigurasi:

```bash
sudo sysctl --system
```

**Penjelasan parameter:**

| Parameter | Fungsi |
|---|---|
| `kernel.dmesg_restrict = 1` | Membatasi akses log kernel hanya untuk root |
| `kernel.kptr_restrict = 2` | Menyembunyikan pointer alamat kernel dari semua user |
| `kernel.yama.ptrace_scope = 1` | Membatasi `ptrace` hanya untuk proses turunan langsung |
| `net.ipv4.ip_forward = 0` | Menonaktifkan IP forwarding (mesin ini bukan router) |
| `net.ipv4.conf.all.send_redirects = 0` | Menonaktifkan pengiriman ICMP redirect (mencegah MITM) |
| `net.ipv4.conf.all.accept_redirects = 0` | Menolak penerimaan ICMP redirect |
| `net.ipv4.conf.all.log_martians = 1` | Mencatat paket dengan alamat sumber tidak valid |
| `net.ipv6.conf.all.disable_ipv6 = 1` | Menonaktifkan IPv6 untuk mengurangi attack surface |
| `fs.protected_hardlinks = 1` | Mencegah hard link ke file yang bukan milik user |
| `fs.protected_symlinks = 1` | Mencegah eksploitasi symbolic link |
| `fs.suid_dumpable = 0` | Menonaktifkan core dump untuk proses setuid |

---

## 17. Konfigurasi SSH (Hardening)

> **Referensi:** Arch Wiki - *OpenSSH* (https://wiki.archlinux.org/title/OpenSSH); `sshd_config` man page (https://man.archlinux.org/man/sshd_config.5); CIS Benchmark - SSH Server Configuration

```bash
sudo nano /etc/ssh/sshd_config
```

Tambahkan konfigurasi berikut:

```
Port 2222
PermitRootLogin no
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
```

**Penjelasan:**

| Direktif | Fungsi |
|---|---|
| `Port 2222` | Mengganti port SSH dari 22 ke 2222 untuk mengurangi serangan otomatis |
| `PermitRootLogin no` | Melarang login langsung sebagai root via SSH |
| `PasswordAuthentication no` | Hanya kunci SSH yang diizinkan (lebih aman dari password) |
| `MaxAuthTries 3` | Maksimal 3 percobaan autentikasi per koneksi |
| `LoginGraceTime 30` | Koneksi yang tidak berhasil login dalam 30 detik akan diputus |

```bash
sudo systemctl restart sshd
```

#### Instalasi dan Konfigurasi Audit Daemon

> **Referensi:** Arch Wiki - *Audit framework* (https://wiki.archlinux.org/title/Audit_framework); `auditd` man page (https://man.archlinux.org/man/auditd.8)

```bash
sudo pacman -S audit
sudo systemctl enable --now auditd
```

- `audit` - Paket daemon audit Linux untuk mencatat aktivitas sistem keamanan.
- `systemctl enable --now auditd` - Mengaktifkan dan langsung menjalankan daemon audit.

---

## 18. Membuat SSH Key

> **Referensi:** Arch Wiki - *SSH keys* (https://wiki.archlinux.org/title/SSH_keys); `ssh-keygen` man page (https://man.archlinux.org/man/ssh-keygen.1)

```bash
ssh-keygen -t ed25519 -C "riski@archsec"
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cp ~/.ssh/id_ed25519.pub ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
cat ~/.ssh/authorized_keys
```

**Penjelasan:**

- `ssh-keygen -t ed25519` - Membuat pasangan kunci SSH menggunakan algoritma Ed25519 (lebih modern dan aman dari RSA). `-C` menambahkan komentar identifikasi.
- `chmod 700 ~/.ssh` - Mengatur izin direktori `.ssh` agar hanya dapat diakses pemilik.
- `chmod 600 ~/.ssh/authorized_keys` - SSH menolak file `authorized_keys` dengan izin terlalu longgar.

---

## 19. Menjalankan Omeka-S dengan Podman Compose

> **Referensi:** Podman Compose documentation (https://github.com/containers/podman-compose); Omeka-S official documentation (https://omeka.org/s/docs/user-manual)

### 19.1 Konfigurasi docker-compose.yml

```bash
cd ~/containers/omeka
nano docker-compose.yml
```

Isi file `docker-compose.yml`:

```yaml
version: "3.8"

services:
  db:
    image: docker.io/library/mariadb:10.11
    environment:
      MYSQL_ROOT_PASSWORD: riski123
      MYSQL_DATABASE: omeka
      MYSQL_USER: omeka
      MYSQL_PASSWORD: riski123
    volumes:
      - ./db-data:/var/lib/mysql:Z
    restart: unless-stopped

  omeka:
    image: docker.io/omeka/omeka-s:latest
    ports:
      - "8080:80"
    environment:
      OMEKA_DB_HOST: db
      OMEKA_DB_NAME: omeka
      OMEKA_DB_USER: omeka
      OMEKA_DB_PASSWORD: riski123
    volumes:
      - ./omeka-files:/var/www/html/files:Z
    depends_on:
      - db
    restart: unless-stopped
```

**Penjelasan:**

| Direktif | Fungsi |
|---|---|
| `mariadb:10.11` | Database MariaDB versi 10.11 |
| `omeka-s:latest` | Aplikasi Omeka-S versi terbaru |
| `ports: "8080:80"` | Memetakan port 8080 host ke port 80 container |
| `volumes` | Menyimpan data secara persisten di host |
| `:Z` | Label SELinux untuk akses volume di Podman |
| `depends_on` | Omeka hanya dijalankan setelah database siap |
| `restart: unless-stopped` | Container restart otomatis kecuali dihentikan manual |

### 19.2 Menjalankan Container

```bash
podman-compose down -v
rm -rf db-data omeka-files
mkdir -p db-data omeka-files
chmod -R 777 omeka-files
podman-compose up -d
podman ps
```

Buka browser dan akses: `http://localhost:8080`

---

## 20. Autostart Omeka-S dengan systemd User Service

> **Referensi:** Arch Wiki - *Systemd/User* (https://wiki.archlinux.org/title/Systemd/User); `systemd.service` man page (https://man.archlinux.org/man/systemd.service.5)

```bash
nano ~/.config/systemd/user/omeka.service
```

Isi file service:

```ini
[Unit]
Description=Omeka-S via Podman Compose
After=network.target

[Service]
WorkingDirectory=%h/containers/omeka
ExecStart=podman-compose up -d
ExecStop=podman-compose down
Type=oneshot
RemainAfterExit=yes

[Install]
WantedBy=default.target
```

**Penjelasan:**

| Direktif | Fungsi |
|---|---|
| `After=network.target` | Layanan dijalankan setelah jaringan tersedia |
| `WorkingDirectory=%h/containers/omeka` | `%h` adalah shortcut direktori home user |
| `Type=oneshot` | Service dianggap selesai setelah `ExecStart` dieksekusi |
| `RemainAfterExit=yes` | Service tetap dianggap aktif meski proses sudah selesai |
| `WantedBy=default.target` | Service diaktifkan pada target default user session |

```bash
systemctl --user enable --now omeka
loginctl enable-linger riski
```

- `loginctl enable-linger riski` - Mengizinkan service user milik `riski` tetap berjalan meski user belum login.

---

## 21. Pemecahan Masalah Boot

> **Referensi:** Arch Wiki - *Systemd-boot* (https://wiki.archlinux.org/title/Systemd-boot); Arch Wiki - *EFI system partition* (https://wiki.archlinux.org/title/EFI_system_partition)

Jika sistem gagal boot, lakukan langkah berikut:

```bash
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
arch-chroot /mnt
bootctl --esp-path=/boot install
bootctl list
exit
umount -R /mnt
reboot
```

Atau jika diperlukan instalasi ulang bootloader via EFI:

```bash
efibootmgr --create --disk /dev/nvme0n1 --part 6 \
  --label "Arch Linux" \
  --loader /EFI/systemd/systemd-bootx64.efi --unicode
efibootmgr --bootorder 0002,0001,0000
```

- `efibootmgr --create` - Membuat entri boot baru di UEFI firmware.
- `efibootmgr --bootorder` - Mengatur urutan prioritas boot.

---

## 22. Verifikasi Status Layanan dan Backup

> **Referensi:** `systemctl` man page (https://man.archlinux.org/man/systemctl.1); `cryptsetup` man page (https://man.archlinux.org/man/cryptsetup.8)

```bash
sudo systemctl status sshd
sudo systemctl status auditd
sudo systemctl status iptables
systemctl --user status mpd
podman ps
```

#### Backup LUKS Header

```bash
sudo cryptsetup luksHeaderBackup /dev/nvme0n1p7 --header-backup-file ~/luks-header-backup.img
```

`luksHeaderBackup` - Membuat backup header LUKS ke file. Header LUKS berisi informasi enkripsi yang sangat kritis; jika rusak, data di partisi tidak dapat dipulihkan. Simpan file backup ini di tempat yang aman (misalnya USB drive terpisah).


# BAB III - PENUTUP

## Kesimpulan

Instalasi Arch Linux dengan enkripsi LUKS, LVM, dan konfigurasi sistem lengkap ini menghasilkan sistem yang aman, modular, dan dapat dikustomisasi sepenuhnya. Dengan pemisahan partisi logis (`/`, `/home`, `/var`, `/tmp`, dan sebagainya), sistem menjadi lebih terlindungi dari potensi kerusakan data akibat disk penuh pada satu partisi.

Desktop environment XFCE yang ringan dan modular memberikan pengalaman pengguna yang fungsional sekaligus efisien dalam penggunaan resource. Konfigurasi hardening tambahan mencakup pembatasan akses kernel via sysctl, penguatan SSH dengan autentikasi berbasis kunci, firewall iptables, dan audit sistem - menjadikan instalasi ini tidak hanya fungsional tetapi juga mengikuti praktik keamanan terbaik (CIS Benchmark).

Seluruh aplikasi tambahan seperti Superfile, Podman, MPD/MPC/MPV, KeePassXC, dan Omeka-S melengkapi sistem sebagai platform yang tidak hanya berorientasi pada penggunaan sehari-hari, tetapi juga memperhatikan aspek keamanan data, efisiensi resource, stabilitas sistem, dan kemudahan administrasi.

---

## Daftar Pustaka

1. Arch Linux. (2024). *Installation guide*. https://wiki.archlinux.org/title/Installation_guide
2. Arch Linux. (2024). *Data-at-rest encryption*. https://wiki.archlinux.org/title/Data-at-rest_encryption
3. Arch Linux. (2024). *LVM*. https://wiki.archlinux.org/title/LVM
4. Arch Linux. (2024). *Dm-crypt/Encrypting an entire system*. https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system
5. Arch Linux. (2024). *Mkinitcpio*. https://wiki.archlinux.org/title/Mkinitcpio
6. Arch Linux. (2024). *Systemd-boot*. https://wiki.archlinux.org/title/Systemd-boot
7. Arch Linux. (2024). *Xfce*. https://wiki.archlinux.org/title/Xfce
8. Arch Linux. (2024). *Iptables*. https://wiki.archlinux.org/title/Iptables
9. Arch Linux. (2024). *Music Player Daemon*. https://wiki.archlinux.org/title/Music_Player_Daemon
10. Arch Linux. (2024). *Mpv*. https://wiki.archlinux.org/title/Mpv
11. Arch Linux. (2024). *OpenSSH*. https://wiki.archlinux.org/title/OpenSSH
12. Arch Linux. (2024). *Podman*. https://wiki.archlinux.org/title/Podman
13. Arch Linux. (2024). *KeePass*. https://wiki.archlinux.org/title/KeePass
14. Arch Linux. (2024). *Security*. https://wiki.archlinux.org/title/Security
15. Arch Linux. (2024). *SSH keys*. https://wiki.archlinux.org/title/SSH_keys
16. Arch Linux. (2024). *NetworkManager*. https://wiki.archlinux.org/title/NetworkManager
17. Arch Linux. (2024). *List of applications/Utilities*. https://wiki.archlinux.org/title/List_of_applications/Utilities
18. Arch Linux. (2024). *Audit framework*. https://wiki.archlinux.org/title/Audit_framework
19. Arch Linux. (2024). *Systemd/User*. https://wiki.archlinux.org/title/Systemd/User
20. Center for Internet Security. (2024). *CIS Benchmark for Distribution Independent Linux*. https://www.cisecurity.org/benchmark/distribution_independent_linux
21. Superfile. (2024). *Superfile official documentation*. https://superfile.netlify.app
22. Podman. (2024). *Podman official documentation*. https://docs.podman.io
23. KeePassXC. (2024). *KeePassXC official site*. https://keepassxc.org
24. Omeka. (2024). *Omeka-S user manual*. https://omeka.org/s/docs/user-manual

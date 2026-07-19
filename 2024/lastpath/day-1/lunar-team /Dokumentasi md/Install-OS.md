# Instalasi Arch Linux — Server LUKSonLVM

---

## 1. Bagi Partisi (LVM)

```bash
lsblk
pvcreate /dev/[nama partisi]
vgcreate [nama grup] /dev/[nama partisi]

lvcreate -L [size G|M] [nama grup] -n root
lvcreate -L [size G|M] [nama grup] -n vars
lvcreate -L [size G|M] [nama grup] -n vlog
lvcreate -L [size G|M] [nama grup] -n vaud
lvcreate -L [size G|M] [nama grup] -n vtmp
lvcreate -L [size G|M] [nama grup] -n home
lvcreate -l 50%FREE [nama grup] -n podman
lvcreate -l 50%FREE [nama grup] -n [nama home untuk internal]

lsblk
```

> LUKS pada LVM `home` ada **dua**: eksternal dan internal.

---

### Penjelasan setiap syntax:
- lsblk untuk menampilkan daftar block device (disk dan partisi) yang terpasang di sistem, dipakai untuk mengecek nama partisi sebelum dan sesudah diubah.
- pvcreate /dev/[nama partisi] untuk membuat sebuah partisi sebagai Physical Volume (PV), yaitu lapisan dasar dari LVM yang nantinya akan dikelompokkan lagi ke dalam Volume Group.
- vgcreate [nama grup] /dev/[nama partisi] membuat Volume Group (VG) dari PV tadi atau bisa dibilang seperti sebuah "wadah besar" penyimpanan yang nanti dipecah-pecah menjadi Logical Volume.
- lvcreate -L [size] [nama grup] -n [nama] membuat Logical Volume (LV) dengan ukuran tertentu bisa Mb bisa GB di dalam VG. Di sini dibuat beberapa LV terpisah untuk root, vars (/var), vlog (/var/log), vaud (/var/log/audit), vtmp (/var/tmp), dan home. Pemisahan ini adalah praktik hardening server: jika satu partisi penuh (misal log membludak), partisi lain seperti root tidak ikut penuh, dan masing-masing bisa diberi opsi mount keamanan berbeda.
- lvcreate -l50%FREE [nama grup] -n podman dan satu LV lagi untuk home internal memakai sisa ruang yang ada, dibagi rata 50:50 dari ruang yang tersisa setelah LV-LV sebelumnya dibuat (-l pakai persentase extent, beda dengan -L yang pakai ukuran absolut).
- Catatan di dokumen: LV home ada dua jenis — eksternal (untuk home biasa) dan internal (yang nanti dienkripsi dengan LUKS, untuk data sensitif/private).


## 2. Format Partisi

```bash
mkfs.ext4 /dev/[nama grup]/root
mkfs.ext4 /dev/[nama grup]/vars
mkfs.ext4 /dev/[nama grup]/vlog
mkfs.ext4 /dev/[nama grup]/vaud
mkfs.ext4 /dev/[nama grup]/vtmp
mkfs.ext4 /dev/[nama grup]/home
mkfs.ext4 /dev/[nama grup]/podman

# Kalau lupa nama partisi:
lsblk -o name,fstype

mkfs.vfat -F32 /dev/[nama partisi boot]
```
### Penjelasan sytax:
- mkfs.ext4 /dev/[nama grup]/[nama lv] memformat masing-masing LV dengan filesystem ext4, dilakukan untuk root, vars, vlog, vaud, vtmp, home, dan podman.
- lsblk -o name,fstype adalah versi lsblk dengan kolom kustom (nama device dan tipe filesystem), untuk verifikasi jika lupa nama partisi yang sudah diformat.
- mkfs.vfat -F32 /dev/[nama partisi boot] untuk memformat partisi boot dengan FAT32, karena UEFI memerlukan partisi EFI System Partition (ESP) berformat FAT32.

---

## 3. Mounting

```bash
mount /dev/[nama grup]/root /mnt

mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```

> `home` untuk **internal** tidak perlu dimounting di sini, karena nanti akan dimounting otomatis menggunakan aplikasi `pam_mount`.

### Setup LUKS untuk Home Internal

```bash
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
# Konfirmasi: YES
# Buat password — password ini HARUS SAMA dengan password user nanti
```
### Penjelasan Syntax:
- mount /dev/[nama grup]/root /mnt me-mount LV root ke /mnt, titik awal instalasi sistem baru 
- Perintah mount --mkdir -o [opsi] /dev/... /mnt/... me-mount tiap LV ke subdirektori yang sesuai sambil otomatis membuat foldernya (--mkdir). Opsi-opsi mount yang dipakai adalah fitur hardening filesystem
- rw itu untuk read and write
- uid=0,gid=0,fmask=0077,dmask=0077 (khusus partisi boot/FAT32) — memastikan hanya root yang punya akses penuh ke file dan folder di partisi boot, karena FAT32 tidak punya permission Unix sendiri.
- nodev — melarang interpretasi file device khusus di partisi tersebut (mencegah eksploitasi lewat device file palsu).
- nosuid — melarang bit SUID/SGID dieksekusi dari partisi tersebut (mencegah privilege escalation).
- noexec — melarang eksekusi binary dari partisi tersebut (dipakai di /var/log, /var/log/audit, /var/tmp karena partisi-partisi ini seharusnya hanya berisi data, bukan program yang dijalankan).
- relatime — mencatat waktu akses file secara efisien (tidak update setiap kali file dibaca, hanya kira-kira), mengurangi I/O dibanding atime penuh.
- LV podman di-mount ke /mnt/var/lib/containers, yaitu lokasi default penyimpanan image dan container podman, dipisah agar data container tidak mencampuri partisi root/var lainnya.
*Catatan: home internal sengaja tidak di-mount manual di sini karena nanti akan otomatis di-mount saat user login, lewat pam_mount (mount otomatis terenkripsi saat autentikasi berhasil).*
### Setup LUKS Home Internal
- cryptsetup luksFormat /dev/[nama grup]/[nama home internal] untuk mengenkripsi LV tersebut dengan LUKS (Linux Unified Key Setup), lapisan enkripsi disk-block sebelum filesystem dibuat. Konfirmasi "YES" diperlukan karena ini operasi destruktif. Password yang dibuat di sini sengaja disamakan dengan password user nanti, supaya proses login user otomatis bisa men-trigger pembukaan partisi terenkripsi tanpa perlu password kedua.


---

## 4. Install Package

```bash
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware \
  mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep \
  podman podman-compose asciinema
```
### Penjelasan syntax:
- pacstrap /mnt [daftar paket] menginstal sistem dasar Arch Linux beserta paket tambahan ke /mnt 
*Penjelasan tiap paket*
- intel-ucode — update microcode untuk prosesor Intel, berisi perbaikan bug/keamanan tingkat hardware dari Intel, dimuat saat boot oleh bootloader.
- base — paket meta yang berisi kumpulan utilitas dasar Arch Linux (seperti bash, coreutils, systemd, dll), fondasi minimal sistem.
- linux-hardened — varian kernel Linux dengan patch keamanan tambahan (hardening), lebih ketat dibanding kernel linux standar untuk mengurangi serangan.
- linux-hardened-headers — header kernel yang cocok dengan linux-hardened, dibutuhkan jika nanti ingin compile modul kernel pihak ketiga (misalnya driver DKMS).
- linux-firmware — kumpulan firmware biner untuk berbagai perangkat keras (WiFi, GPU, dll) yang dibutuhkan agar driver berfungsi.
- mkinitcpio — tool untuk membuat initramfs (initial ramdisk), yaitu lingkungan sementara saat boot sebelum root filesystem sesungguhnya dimuat.
- lvm2 — tools untuk Logical Volume Manager, dipakai jika partisi disk diatur memakai LVM (volume group, logical volume, dsb).
- sudo — mengizinkan user biasa menjalankan perintah dengan privilege root secara terkontrol.
- pacman — package manager Arch Linux itu sendiri, perlu eksplisit dimasukkan supaya tersedia di sistem baru.
- git — version control system, sering dibutuhkan untuk clone repository (misalnya AUR helper atau dotfiles).
- wget dan curl — tools untuk mengunduh file dari internet lewat command line, beda di gaya penggunaan/opsi tapi fungsinya mirip.
- neovim — text editor modern berbasis Vim, dipakai untuk mengedit file konfigurasi setelah chroot.
- iwd — iNet Wireless Daemon, daemon untuk mengelola koneksi WiFi (alternatif ringan dari NetworkManager/wpa_supplicant).
- firewalld — daemon untuk mengatur firewall secara dinamis, lebih user-friendly dibanding iptables manual.
- openssh — implementasi SSH untuk akses remote (server dan client), penting kalau mesin ini akan diakses dari jarak jauh.
- grep — tool pencarian teks/pattern, sebenarnya biasanya sudah ada di base, tapi dimasukkan eksplisit untuk memastikan tersedia atau tidaknyaa.
- podman — alternatif Docker untuk menjalankan container, tanpa daemon dan bisa rootless (lebih aman).
- podman-compose — tool untuk menjalankan multi-container setup memakai file compose (mirip docker-compose) dengan podman sebagai backend.
- asciinema — tool untuk merekam sesi terminal menjadi file yang bisa diputar ulang/dishare (berguna untuk dokumentasi atau demo).
---

## 5. After Install

```bash
genfstab -U /mnt > /mnt/etc/fstab
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab

cp /etc/systemd/network/* /mnt/etc/systemd/network

arch-chroot /mnt

echo [hostname] > /etc/hostname
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc

nvim /etc/locale.gen
# Cari baris EN_US, hapus tanda pagar (#) di kedua barisnya

locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf

pacman -S pam_mount

lsblk
```
### Penjelasan Syntax:
- genfstab -U /mnt > /mnt/etc/fstab menghasilkan file /etc/fstab secara otomatis berdasarkan partisi yang ter-mount di /mnt, menggunakan UUID (-U) sebagai identifier supaya tidak bergantung pada nama device yang bisa berubah-ubah.
- Baris echo "tmpfs /tmp tmpfs ..." >> /mnt/etc/fstab menambahkan entri agar /tmp di-mount sebagai tmpfs (disimpan di RAM, bukan disk) dengan opsi keamanan nosuid,nodev,noexec dan dibatasi ukuran 512MB — praktik umum hardening karena /tmp rawan dipakai untuk menyimpan file sementara yang berbahaya.
- cp /etc/systemd/network/* /mnt/etc/systemd/network menyalin konfigurasi jaringan dari live ISO ke sistem baru, supaya pengaturan network yang sudah berjalan di installer ikut terbawa.
- arch-chroot /mnt masuk ke environment sistem baru seolah-olah sedang boot dari situ (chroot = change root), semua perintah setelah ini dijalankan di dalam sistem target, bukan di live ISO lagi.

*Di dalam chroot:*
- echo [hostname] > /etc/hostname membuat nama host komputer.
- ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime membuat symlink zona waktu ke WIB (Jakarta).
- hwclock --systohc menyinkronkan jam hardware dengan jam sistem yang baru diset.
- nvim /etc/locale.gen membuka file locale untuk diedit manual, menghapus tanda # di baris en_US.UTF-8 agar locale itu di-generate.
- locale-gen menghasilkan locale yang sudah diaktifkan di file tadi.
- locale > /etc/locale.conf lalu diedit lagi dengan nvim untuk menetapkan locale default sistem.
- pacman -S pam_mount menginstal modul PAM untuk auto-mount partisi (dipakai nanti untuk home internal terenkripsi).
- lsblk cek ulang struktur partisi sebelum lanjut ke langkah selanjutnya.

---

## 6. Setup Home Internal

```bash
cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
# Masukkan password yang sudah dibuat sebelumnya

mkfs.ext4 /dev/mapper/[device name]

mkdir /home/[nama folder]
useradd -d /home/[nama folder] [nama user]

passwd [nama user]
# Masukkan password — HARUS SAMA dengan password saat cryptsetup luksFormat

echo "[nama user] ALL=(ALL:ALL) ALL" > /etc/sudoers.d/[nama user]

chown -R [nama user]:[nama user] /home/[nama folder]

nvim /etc/security/pam_mount.conf.xml

nvim /etc/pam.d/system-login

```
---
<img width="987" height="491" alt="WhatsApp Image 2026-06-24 at 1 10 31 AM" src="https://github.com/user-attachments/assets/59d020d6-68fb-4748-bd56-c905f36323a9" />

---

---
<img width="976" height="676" alt="WhatsApp Image 2026-06-24 at 1 10 31 AM (1)" src="https://github.com/user-attachments/assets/14b5a86a-4ecb-4510-b2da-85d71d724818" />

---


## 7. Mkinitcpio

```bash
mkdir /etc/mkinitcpio.d
nvim /etc/cmdline.d/boot.conf
nvim /etc/mkinitcpio.conf
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
### Penjelasan Syntax:
- mkdir /etc/mkinitcpio.d membuat direktori untuk menyimpan file preset mkinitcpio (jika belum ada).
- nvim /etc/cmdline.d/boot.conf mengedit parameter kernel command line tambahan
- nvim /etc/mkinitcpio.conf mengedit konfigurasi utama mkinitcpio: menentukan hooks (modul) apa saja yang dimasukkan ke initramfs, seperti hook lvm2 dan encrypt/sd-encrypt
- nvim /etc/mkinitcpio.d/linux-hardened.preset mengedit file preset khusus untuk kernel linux-hardened 

---

## 8. Install systemd-boot

```bash
bootctl --path=/boot install
mkinitcpio -P

systemctl enable systemd-networkd
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd

exit

umount -R

reebot
```
### Penjelasan Syntax:
- bootctl --path=/boot install untuk menginstal systemd-boot sebagai bootloader ke partisi boot (ESP)
- mkinitcpio -P untuk generate semua initramfs 
- systemctl enable [service] mengaktifkan beberapa service agar otomatis berjalan saat boot: systemd-networkd (manajemen jaringan), iwd (koneksi WiFi), firewalld (firewall), dan sshd (server SSH untuk akses remote).
- exit keluar dari chroot, kembali ke environment live ISO.
- Bagian "Khusus untuk Lenovo": bootctl --path=/mnt/boot install dijalankan dari luar chroot (karena di beberapa laptop Lenovo, instalasi bootloader dari dalam chroot kadang gagal mendeteksi NVRAM UEFI dengan benar, sehingga dilakukan ulang dari live ISO langsung menunjuk ke /mnt/boot).
- reboot me-restart sistem, lepas dari live ISO, dan boot ke sistem Arch Linux yang baru saja diinstal.


### Khusus untuk Lenovo

```bash
bootctl --path=/mnt/boot install
```

```bash
reboot
```

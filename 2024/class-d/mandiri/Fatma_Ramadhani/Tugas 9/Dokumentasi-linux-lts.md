# Dokumentasi Penginstalan Linux lts
## Nama: Fatma Ramadhani
## NIM: 12402051050157
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum
---
## 1. Instalasi Linux lts Blackbird Amanda
Cek wifi
```bash
iwctl
```
---
```bash
device list
```
---
```bash
station wlan0 connect "nama wifi"
```
```
exit
```
## 2. Verifikasi koneksi internet
```bash
ping detik.com
```
## 3. Membuat Filesystem Ext4
```bash
lsblk
```
```bash
mkfs.ext4 -b 4096 /dev/(nama grup)/root
mkfs.ext4 -b 4096 /dev/(nama grup)/vars
mkfs.ext4 -b 4096 /dev/(nama grup)/vtmp
mkfs.ext4 -b 4096 /dev/(nama grup)/vlog
mkfs.ext4 -b 4096 /dev/(nama grup)/vaud
mkfs.ext4 -b 4096 /dev/(nama grup)/home
mkfs.ext4 -b 4096 /dev/(nama grup)/dock
mkfs.vfat -F32 -n BOOT /dev/(nama boot efi system)
```
---
```bash
lsblk
```
## 4. Mounting
partisi root
```bash
mount /dev/proc/root /mnt
```
partisi vars
```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/vars /mnt/var
```
partisi vtmp
```bash
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/tmp
```
partisi vlog
```bash
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/vlog
```
partisi vaud
```bash
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
partisi home
```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/home /mnt/home
```
partisi dock
```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/dock /mnt/var/lib/docker
```
## 5. Instalasi sistem dasar
Kita menggunakan pacstrap.
```bash
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio neovim lvm2 base sudo curl wget docker which firewalld iwd pacman grep
```
## 6. Masuk ke Chroot
```bash
arch-chroot /mnt
```
## 7. Set Localtime
Localtime
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

Locale
Uncomenting `en_US`
```
nvim /etc/locale.gen
```
Generate bahasa yang di uncomenting.
```
locale-gen
```
```
locale > /etc/locale.conf
```

Konfigurasi locale dengan mengisi `LANG=C` dan `LANG=ALL` dengan `en_US.UTF-8`
```
nvim /etc/locale.conf
```
## 8. Instalasi Bootloader Systemd-Boot
```
bootctl --path=/boot install
```
## 9. Generate Initramfs
```
mkinitcpio -P
```
## 10. Mengaktifkan layanan wireless
```
systemctl enable iwd
```
## 11. Aktifkan Network Manager, DNS Resolver, dan Docker
```
systemctl enable systemd-networkd
```
---
```
systemctl enable systemd-resolved
```
---
```
systemctl enable docker
```
## 12. Instalasi Cockpit
```
pacman -S cockpit
```
## 13. Aktifkan firewall
```
systemctl enable firewalld
```
## 14. Keluar dari Chroot
```
exit
```
## 15. Umount seluruh partisi
```
umount -R /mnt
```
## 16. Reboot
```
reboot
```

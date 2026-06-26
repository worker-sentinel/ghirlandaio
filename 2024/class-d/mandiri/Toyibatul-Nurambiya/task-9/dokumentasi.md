# Dokumentasi Penugasan Kelompok 9 
## Nama: Toyibatul Nurambiya
## NIM: 12402051030096
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M. Hum

### 1. Konfigurasi Jaringan
```iwctl```
```device list```
```station wlan0 scan```
```station wlan0 get-networks```
```station wlan0 connect nama_wifi```
### 2. Pengujian Koneksi Jaringan
```ping 1.1.1.1```
### 3. Melakukan Rekaman dengan Asciinema
```asciinema rec coba1.cast```
### 4. Melihat Partisi Disk
```lsblk```
### 5. Membagi Partisi EFI System dan Linux Filesystem
```cfdisk /dev/sda```
### 6. Mengenkripsi Partisi dengan LUKS
```cryptsetup luksFormat --sector-size=4096 /dev/sda6```
### 7. Membuka Partisi Enkripsi
```cryptsetup open /dev/sda6 miya```
### 8. Membuat LVM
```pvcreate /dev/mapper/miya```
### 9. Membuat Volume Group
```vgcreate proc /dev/mapper/syifa```
### 10. Membuat LV
```lvcreate -L 10G proc -n root```
```lvcreate -L 10G proc -n vars```
```lvcreate -L 1G proc -n vlog```
```lvcreate -L 1G proc -n vaud```
```lvcreate -L 1G proc -n vtmp```
```lvcreate -L 10G proc -n home```
```lvcreate -l 100%FREE proc -n dock```
### 11. Format Filesystem
```mkfs.ext4 /dev/proc/root```
```mkfs.ext4 /dev/proc/vars```
```mkfs.ext4 /dev/proc/vlog```
```mkfs.ext4 /dev/proc/vaud```
```mkfs.ext4 /dev/proc/vtmp```
```mkfs.ext4 /dev/proc/home```
```mkfs.ext4 /dev/proc/dock```
### 12. Mount Root
```mount --mkdir /dev/proc/root /mnt```
### 13. Mount Partisi lain
```mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/tmp```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/dock /mnt/var/lib/docker```
### 14. Mount EFI Patition
```mount --mkdir /dev/sda5 /mnt/boot```
### 15. Instal Paket Dasar Arch
```pacstrap /mnt base linux-lts linux-firmware mkinitcpio lvm2 sudo curl neovim intel-ucode```
### 16. Membuat fstab
```genfstab -U /mnt > /mnt/etc/fstab```
### 17. Menambah tmpfs
```echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab```
### 18. Masuk ke sistem baru
```arch-chroot /mnt```
### 19. Mengatur Hostname
```echo "mia" > /etc/hostname```
### 20. Mengatur Timezone
```ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```
### 21. Sinkronisasi Jam
```hwclock --systohc```
### 22. Mengatur Locale
```nvim /etc/locale.gen```
```en_US.UTF-8 UTF-8```
```locale-gen```
### 23. Membuat User
```useradd -m miya```
### 24. Membuat Password
```passwd miya```
### 25. Membuat Hak Sudo
```echo "syifa ALL=(ALL:ALL) ALL" > /etc/sudoers.d/syifa```
### 26. Konfigurasi Kernel Command Line
```mkdir /etc/cmdline.d```
```touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}```
```echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda5)=miya root=/dev/system/root" > /etc/cmdline.d/01-boot.conf```
```cat  /etc/cmdline.d/01-boot.conf```
### 27. Konfigurasi mkinitcpio
```nvim /etc/mkinitcpio.conf```
### 28. Install Bootloader
```bootctl --path=/boot install```
### 29. Generate Initramfs
```mkinitcpio -P```
### 30. Mengaktifkan Network
```systemctl enable systemd-networkd```
```systemctl enable systemd-resolved```
```systemctl enable iwd```
```systemctl enable firewalld```
### 31. Keluar dan Umount
```exit```
```umount -R /mnt```

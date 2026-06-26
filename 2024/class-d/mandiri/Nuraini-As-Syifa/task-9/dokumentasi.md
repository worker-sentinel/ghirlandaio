# Dokumentasi penginstallan penugasan kelompok 9
## Nama: Nuraini As Syifa
## NIM: 12402051030088
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum.
### 1. Konfigurasi Koneksi Jaringan:
```iwctl```
```device list```
```station wlan0 scan```
```station wlan0 get-networks```
```station wlan0 connect nama_wifi```
### 2. Pengujian Koneksi Jaringan:
```ping 1.1.1.1```
### 3. Melakukan rekaman dengan asciinema
```asciinema rec coba1.cast```
### 4. Melihat Partisi
```lsblk```
### 5. Membagi Partisi EFI System dan Linux Filesystem
```cfdisk /dev/nvme0n1```
### 6. Mengenkripsi Partisi dengan LUKS
```cryptsetup luksFormat --sector-size=4096 /dev/nvme0n1p7```
### 7. Membuka Partisi Enkripsi
```cryptsetup open /dev/nvme0n1p7 syifa```
### 8. Membuat LVM
```pvcreate /dev/mapper/syifa```
### 9. Membuat volume group
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
```mount /dev/proc/root /mnt```
### 13.Mount Partisi Lain
```mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var ```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home```
```mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/dock /mnt/var/lib/docker```
### 14. Mount EFI Partition
```mount --mkdir /dev/nvme0n1p6 /mnt/boot```
### 15. Install Paket Dasar Arch
```pacstrap /mnt base linux-lts linux-firmware mkinitcpio lvm2 sudo curl neovim intel-ucode```
### 16. Membuat fstab
```genfstab -U /mnt > /mnt/etc/fstab```
### 17. Menambah tmpfs
```gecho "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab```
### 18. Masuk ke Sistem Baru
```arch-chroot /mnt```
### 19. Mengatur Hostname
```echo "assyifa" > /etc/hostname```
### 20. Mengatur Timezone
```ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime```
### 21. Sinkronisasi Jam
```hwclock --systohc```
### 22. Mengatur locale
```nvim /etc/locale.gen```
```en_US.UTF-8 UTF-8```
```locale.gen```
### 23. Membuat User
```useradd -m syifa```
### 24. Memberi Password
```passwd syifa```
### 25. Memberikan Hak Sudo
```echo "syifa ALL=(ALL:ALL) ALL" > /etc/sudoers.d/syifa```
### 26. Konfigurasi Kernel Command Line
```mkdir /etc/cmdline.d```
```touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}```
```echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6)=asyifa root=/dev/proc/root" > /etc/cmdline.d/01-boot.conf```
```cat  /etc/cmdline.d/01-boot.conf```
### 27. Konfigurasi mkinitcpio
```nvim /etc/mkinitcpio.conf```
### 28. Install Boatloader
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

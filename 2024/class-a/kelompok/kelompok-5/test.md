# TUTORIAL INSTALASI BASE ARCH LINUX
## Persiapan sebelum instalasi
1. Download ISO arch linux pada link berikut ini: https://archlinux.org/download/ , cari mirror terdekat yaitu Indonesia lalu pilih yang berukuran sekitar 1G
2. Gunakan aplikasi seperti rufus atau balena etcher untuk melakukan flash USB, ISO arch linux akan ditulis pada flashdisk agar jadi live usb. Pastikan USB yang di flash memimiliki kapasitas setidaknya 8GB
3. Lakukan disk partisi melalui Disk management, dapat diakses melalui mengklik kanan mouse pada windows atau menggunakan aplikasi mini partition wizard tool yang lebih mudah


## Boot ke Live Environment
1. Colokan flashdisk yang sudah di flash burnt. restart laptop lalu masuk ke mode BIOS untuk laptop saya menekan f2
2. matikan Secure Boot, lalu cari urutan boot
3. Pindahkan flashdisk yang sudah di flash burnt menjadi urutan pertama
4. tekan f10 untuk save dan keluar
5. anda sudah masuk ke live environment

## konek internet
iwctl
```
iwctl
```
```
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect NamaWifi
```
alternatif jika phy0 belum dinyalakan
Keluar dulu dari iwctl:
```
exit
```
```
rfkill list
```
Kalau ada tulisan Soft blocked: yes atau Hard blocked: yes
```
rfkill unblock all
rfkill unblock wifi
```
```
ip link set wlan0 up
```
Update sistem waktu
```
timedatectl
```

## Partisi Disk
Di live environment anda dapat melihat disk yang dibagi menjadi *block device* seperti /dev/sda, /dev/nvme0n1. Untuk mengidentifikasi device ini gunakan lsblk atau fdisk
```
fdisk -l
```

Mirror
```
nano /etc/pacman.d/mirrorlist
```
packages penting
```
pacstrap -K /mnt base linux linux-firmware amd-ucode networkmanager nano sudo grub efibootmgr os-prober dosfstools mtools ntfs-3g man-db man-pages texinfo sof-firmware
```
fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```
chroot
```
arch-chroot /mnt
```
set time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

localization
```
pacman -S nano
```
masuk ke folder
```
nano /etc/locale.gen
```
dibuka komentarnya 
```
en_US.UTF-8 UTF-8
```
jalankan
```
locale -gen
```

ketik
```
nano /etc/locale.gen
```
ketik
```
LANG=en_US.UTF-8
```
jalankan
```
locale -gen
```
hostname
```
nano /etc/hostname
```

ketik nama hostname

network management
pastikan network interface telah dilist dan dienable
```
ip a
ping archlinux.com
```
instal networkmanager
```
pacman -S networkmanager
```
start networkmanager
```
systemctl enable NetworkManager.service
```

initframs
```
mkinitcpio -P
```
root password
```
passwd
```
boot loader
install efibootmgr
```
pacman -S grub efibootmgr
```
install grub package
```
grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
```
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
Use the ```grub-mkconfig tool``` to generate ```/boot/grub/grub.cfg```:
```
grub-mkconfig -o /boot/grub/grub.cfg
```
reboor
```
umount -R /mnt
```

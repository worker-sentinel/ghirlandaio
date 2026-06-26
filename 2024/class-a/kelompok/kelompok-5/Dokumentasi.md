# TUTORIAL INSTALASI BASE ARCH LINUX
## Persiapan sebelum instalasi
1. Download ISO arch linux pada link berikut ini: https://archlinux.org/download/ , cari mirror terdekat yaitu Indonesia lalu pilih yang berukuran sekitar 1G
2. Gunakan aplikasi seperti rufus atau balena etcher untuk melakukan flash USB, ISO arch linux akan ditulis pada flashdisk agar jadi live usb. Pastikan USB yang di flash memimiliki kapasitas setidaknya 8GB

<img width="997" height="637" alt="image" src="https://github.com/user-attachments/assets/3447d3f0-3482-457e-b0c4-a3306bfcc62c" />
di balena etcher masukan file ISO Arch linux, pilih flashdisk lalu tinggal tekan flash

3. Lakukan disk partisi melalui Disk management, dapat diakses melalui mengklik kanan mouse pada windows atau menggunakan aplikasi mini partition wizard tool yang lebih mudah
<img width="1272" height="890" alt="image" src="https://github.com/user-attachments/assets/8279b91c-6a4a-4c74-92f4-8b8919f2d38a" />
* pilih disk yang mau diambil disknya
* misalnya saya disk D, klik kanan pada disk d lalu split atur berapa giga yang kalian inginkan untuk arch linux lalu klik apply lalu restart

## Boot ke Live Environment
1. Colokan flashdisk yang sudah di flash burnt. restart laptop lalu masuk ke mode BIOS untuk laptop saya menekan f2
2. matikan Secure Boot, lalu cari urutan boot
3. Pindahkan flashdisk yang sudah di flash burnt menjadi urutan pertama
4. tekan f10 untuk save dan keluar
5. anda sudah masuk ke live environment

## konek internet
jika menggunakan kabel lan tinggal colok


jika menggunakan wifi:
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
### cari disk mana yang mau dipartisi

<img width="1920" height="1080" alt="Desain tanpa judul (2)" src="https://github.com/user-attachments/assets/f5f13826-fd86-4c1d-98d2-b6cf1a5f208e" />

### masuk ke disk yang mau dipartisi
disini disk yang mau saya partisi adalah ```nvme0n1```
untuk format disk ketik 
```
fdisk /dev/nvme0n1
```


<img width="566" height="757" alt="image" src="https://github.com/user-attachments/assets/45b27646-d826-488b-b53a-48960bfde1ee" />


1. ubah mode partisi dari MBR menggunakan GPT menggunakan ```g```
2. cek berapa free disk yang sudah disiapkan untuk arch linux dengan tombol ```F``` ***kapital ya***

### sesuaikan dengan layout ini
<img width="937" height="271" alt="image" src="https://github.com/user-attachments/assets/0ae7bdc3-e491-449b-976c-05096373697e" />

### Device pertama adalah boot


1. ketik ```n``` untuk membuat partisi baru
2. gunakan default number saja klik enter, misal ```nvme0n1p6```
3. gunakan default yang kedua enter saja
4. lalu tentukan berapa memory untuk boot sesuai panduan di arch wiki adalah 1G jadi ketik ```+1G```


### ubah tipe disknya

<img width="578" height="798" alt="image" src="https://github.com/user-attachments/assets/cd5a5bce-a0db-4d29-af3b-8f0165feb458" />

1. lihat list tipe disk ```l```
2. disini kita bisa lihat EFI System adalah nomor 1, tekan ```q``` untuk keluar
3. tekan ```t``` lalu ketik ```6``` sesuai disk yang ingin kita ganti tipenya
4. lalu masukan nomor tipe disk tujuan, ketik ```1``` untuk merubah linux filesystem (default) ke EFI System

### lalu buat partisi baru dan lakukan hal yang sama untuk device root dan linux swap

misalnya nama devicenya adalah
```fdisk /dev/nvme0n1p7``` untuk linux swap adalah 4G
```fdisk /dev/nvme0n1p8``` untuk root adalah sisanya misalnya 95G

### lalu format disk

### untuk boot
```
mkfs.fat -F 32 /dev/nvme0n1p6
```

### untuk swap
```
mkswap /dev/nvme0n1p7
swapon /dev/nvme0n1p7
```

### untuk root
```
# mkfs.ext4 /dev/nvme0n1p8
```

https://wiki.archlinux.org/title/Partitioning#Example_layouts

## Mount partisi
mount root ke ```/mnt```
```
mount /dev/nvme0n1p8 /mnt
```
### untuk UEFI System mount ke ufi system 
```
mount --mkdir /dev/nvme0n1p6 /mnt/boot
```

## Mirror
jika diperlukan semisalnya downloadnya masih lambat
```
nano /etc/pacman.d/mirrorlist
```
### pilih yang kaliah rasa lebih cocok dan pindahkan paling atas

## Install packages penting

paket untuk basic linux kernel dan perangkat keras secara umum
```
pacstrap -K /mnt base linux linux-firmware
```
paket yang sudah dikustomisasi untuk laptop saya
```
pacstrap -K /mnt base linux linux-firmware amd-ucode networkmanager nano sudo grub efibootmgr os-prober
```
base: Sistem dasar arch linux agar bisa jalan
linux: Kernel linux agar bisa boot
linux firmware: Driver agar hardware laptop bisa berfungsi (binary blob)
amd-ucode: karena AMD processor
networkmanager: untuk menyambung ke wifi
nano: untuk console text editor
sudo: agar admin bisa memberikan authoritas ke user lain, dan user bisa memberikan command sebagai root
grub: bootloader softwar yang dimulai oleh firmware
efibootmgr: igunakan oleh skrip instalasi GRUB untuk menulis entri boot ke dalam NVRAM
os-prober: untuk deteksi os lain secara otomatis
di arch chroot
```
nano /etc/default/grub
```
uncomment
```
GRUB_DISABLE_OS_PROBER=false
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```
### fstab
agar Agar nanti setelah Arch Linux di-boot, sistem tahu format partisi disk yang disk ada dimana
dibuatlah file
```
/etc/fstab
```
```fstab``` adalah daftar partisi yang harus otomatis dimount saat Linux menyala
```
genfstab -U /mnt >> /mnt/etc/fstab
```

### chroot
### untuk ke system environmt 
```
arch-chroot /mnt
```
set time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

### localization
karena sudah install nano


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
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```
hostname
```
nano /etc/hostname
```

### ketik nama hostname

### network management
pastikan network interface telah dilist dan dienable
```
ip a
ping archlinux.com
```
### instal networkmanager
```
pacman -S networkmanager
```
### start networkmanager
```
systemctl enable NetworkManager.service
```

### initframs
```
mkinitcpio -P
```
root password
```
passwd
```
### boot loader
install efibootmgr
```
pacman -S grub efibootmgr
```
### install grub package
contoh
```
grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
```
sesuaikan dengan folder boot
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
Use the ```grub-mkconfig tool``` to generate ```/boot/grub/grub.cfg```:
```
grub-mkconfig -o /boot/grub/grub.cfg
```
Reboot
```
exit
```
```
umount -R /mnt
```
```
reboot
```

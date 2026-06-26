# Menginstall OS Server

## Mengaktifkan rekaman asciinema

asciinema record serverandromeda.cast


## Menghubungkan ke jaringan wifi

iwctl


device list



station wlan0 get-networks

menampilkan hasil scan berupa daftar SSID yang ditemukan


station wlan0 scan

meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar

station wlan0 connect (nama wifi)


station wlan0 connect "wifi sekolah"

jika nama wifi lebih dari 1 kata. gunakan tanda "..."

exit

## Memeriksa partisi

lsblk


## Membuat dan mengatur partisi

cfdiks /dev/(partisi [sda/nvme0n1p-])


## Jumlah partisi

boot = 3g [efi system] sda7
root = 48.8g [linux filesystem] sda8


## Cek kembali partisi

lsblk

## Format luks
dengan menggunakan root 

cryptsetup luksFormat /dev/sda7

masukkan kata sandi

cryptsetup luksOpen /dev/sda7 tama


3# Setup lvm

pvcreate /dev/mapper/tama


vgcreate andromeda /dev/mapper/tama


## Membuat logical volume

lvcreate -L 14G andromeda -n root
lvcreate -L 10G andromeda -n vars
lvcreate -L 3G andromeda -n vlog
lvcreate -L 4G andromeda -n vtmp
lvcreate -L 4G andromeda -n vaud
lvcreate -L 5G andromeda -n home
lvcreate -L 8G andromeda -n podman


## Format

mkfs.vfat -F32 -n BOOT /dev/sda8
mkfs.ext4 /dev/andromeda/root
mkfs.ext4 /dev/andromeda/vars
mkfs.ext4 /dev/andromeda/vlog
mkfs.ext4 /dev/andromeda/vtmp
mkfs.ext4 /dev/andromeda/vaud
mkfs.ext4 /dev/andromeda/home
mkfs.ext4 /dev/andromeda/podman


## Periksa partisi Kembali untuk memastikan

lsblk

## Mount

mount /dev/proc/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda8 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/andromeda/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/andromeda/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/andromeda/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/andromeda/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/andromeda/podman /mnt/var/lib/containers

## Periksa partisi Kembali

lsblk

## Instal package

pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman

## Generate fstab

genfstab -U /mnt > /mnt/etc/fstab


## Copy konfigurasi network dari live environment ke sistem baru

cp /etc/systemd/network/* /mnt/etc/systemd/network

## Tambahkan

echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 


## Periksa Kembali

cat /mnt/etc/fstab


## Masuk ke system

arch-chroot /mnt

## Sinkronisasi Waktu

ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime


hwclock --systohc


## Atur Bahasa dan lokasi

nvim /etc/locale.gen

Ketik tanda "/" agar pencarian lebih cepat
/en_US

Tekan enter lalu tekan "i"

Lalu hapus tanda "#" pada
en_US.UTF-8 UTF-8
en_US ISO-8859-1

Tekan esc setelah itu :wq

## Menghasilkan dan mengatur konfigurasi locale sistem agar menggunakan bahasa dan format regional en_US.UTF-8

locale-gen


locale > /etc/locale.conf


nvim /etc/locale.conf


isi lang=C menjadi lang=en_US.UTF-8

isi ALL=en_US.UTF-8


## Membuat user

useradd -m server


passwd server


echo "server ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/server

## Mengatur parameter

mkdir /etc/cmdline.d


touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}


echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda7=tama) root=/dev/andromeda/root" > /etc/cmdline.d/01-boot.conf

## Mengatur mkinitcpio

nvim /etc/mkinitcpio.conf

<img width="681" height="67" alt="WhatsApp Image 2026-06-19 at 01 03 33" src="https://github.com/user-attachments/assets/3223c663-e7c5-49fe-a389-aa6ec040bce8" />


Setelah kata 'block' ketik 'sd-encrypt lvm2'

## Mengubah konfigurasi initramfs kernel Linux LTS

nvim /etc/mkinitcpio.d/linux-lts.preset


## Mengubah seperti yang ada di foto

<img width="517" height="289" alt="WhatsApp Image 2026-06-19 at 01 03 33 (1)" src="https://github.com/user-attachments/assets/a1cce8b5-d65e-4084-9dc6-d4a322753c7e" />


## Install bootctl

bootctl --path=/mnt/boot install 

Lanjut

mkinitcpio -P


## Aktifkan systemctl enable

systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 

## Selesai instalasi os

exit
umount -R /mnt

## Menghentikan rekaman
ctrl+d

asciinema upload serverandromeda.cast

## Selesai

reboot

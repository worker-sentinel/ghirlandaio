# Installasi OS server

## Connect Wifi
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect (nama jaringan)
```
```
exit
```
```
ping 8.8.8.8
```

## Setup Luks
```
cryptsetup luksFormat /dev/p.root
```
```
cryptsetup luksOpen /dev/p.root ping (nama device - boleh apa aja)
```
```
pvcreate /dev/mapper/ping
```
```
vgcreate lit /dev/mapper/ping
```

## Setup LVM
```
lvcreate -L 8G lit -n root 
lvcreate -L 5G lit -n vars 
lvcreate -L 1G lit -n vlog
lvcreate -L 1G lit -n vaud
lvcreate -L 1.5G lit -n vtmp
lvcreate -l50% lit -n home
lvcreate -l100%FREE lit -n podman 

lsblk
```

## Formating LV
```
mkfs.ext4 /dev/lit/root
mkfs.vfat -F32 /dev/sda5
mkfs.ext4 /dev/lit/vars
mkfs.ext4 /dev/lit/vtmp
mkfs.ext4 /dev/lit/vlog
mkfs.ext4 /dev/lit/vaud
mkfs.ext4 /dev/lit/home
mkfs.ext4 /dev/lit/podman

lsblk
```

## Mounting
```
mount /dev/lit/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/lit/vars /mnt/var         
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/lit/vtmp /mnt/var/tmp     
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/lit/vlog /mnt/var/log  
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/lit/vaud /mnt/var/log/audit      
mount --mkdir -o rw,nodev,nosuid,relatime /dev/lit/podman /mnt/var/lib/container
mount --mkdir -o rw,nodev,nosuid,relatime /dev/lit/home /mnt/

lsblk
```

## Install Packages
```
pacstrap /mnt base intel-ucode base  linux-lts linux-lts-headers linux-firmware  mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman 
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
# Format tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
# Chroot
```
arch-chroot /mnt
```
# Membuat Hostname
```
echo "server" > /etc/hostname
```
# Mengatur Waktu Dan Bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
>Hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
```
locale-gen
```
locale > /etc/locale.conf
```
nvim /etc/locale.conf
```
>Pada LANG=en_US.UTF-8 ubah menjadi LANG=en_US.UTF-8 kemudian dibagian LC_ALL= tambahkan juga en_US.UTF-8
```
```
## Menambahkan User
```
useradd -d /home/user milky
```
## Mengatur Password User
```
passwd milky
```
## Konfigurasi Sudo
```
echo 'milky ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/milky
```
sudo su
```
```
# Kernel Parameter
```
mkdir /etc/cmdline.d
```
touch /etc/cmdline.d/{01-boot.conf,02-nisc.conf}
```
lsblk
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/ping)=proc root=/dev/lit/root" > /etc/cmdline.d/01-boot.conf
```
echo "rw" > /etc/cmdline.d/02-nisc.conf
```
# mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
>Pada hooks paling bawah, tambahkan lvm2 sd-encrypt setelah sd-vconsole
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>Hapus semua pagar pada baris ALL, kemudian ubah baris pertama ALL jadi "/etc/mkinitcpio.d/default.conf", untuk dua baris setelahnya tambahkan 'kernel' setelah 'boot'.
```
>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus 'EFI' dan ubah 'Linux' jadi 'linux'
```
mkinitcpio -P
```
systemctl enable NetworkManager
```
systemctl enable systemd-networkd.socket
```
systemctl enable systemd-resolved
```
# Selesai
```
exit
```
```
reboot

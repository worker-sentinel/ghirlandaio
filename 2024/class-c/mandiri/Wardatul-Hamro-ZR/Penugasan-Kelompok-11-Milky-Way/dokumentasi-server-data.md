# Dokumentasi instalasi os server app
```
## connect wifi
iwctl
```
station wlan0 get-networks
```
station wlan0 connect “nama wifi”
```
Masukan pw

exit
```
ping 8.8.8.8
```
lsblk
```

## set up luks
```
cryptsetup luksFormat /dev/sda6
```
cryptsetup luksOpen /dev/sda6/way
```
pvcreate /dev/mapper/way
```
lsblk
```
vgcreate lit /dev/mapper/way
```
lsblk
```
lvcreate -L 10G lit -n root
```
lvcreate -L 10G lit -n vars
```
lvcreate -L 1G lit -n vlog
```
lvcreate -L 1G lit -n vaud
```
lvcreate -L 1.5G lit -n vtmp
```
lvcreate -l50%FREE lit -n home
```
lvcreate -l00%FREE lit -n podman
```
lsblk
```

## Formating LV
```
mkffs.vfat -F32 -n BOOT /dev/nvme0n1p5
```
mkfs.ext4 /dev/server/root
```
mkfs.ext4 /dev/server/vars
```
mkfs.ext4 /dev/server/vlog
```
mkfs.ext4 /dev/server/vtmp
```
mkfs.ext4 /dev/server/vaud
```
mkfs.ext4 /dev/server/home
```
mkfs.ext4 /dev/server/podman
```
lsblk
```

## Mounting
```
mount /dev/server/root /mnt
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p5 /mnt/boot
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/server/vars /mnt/var
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vlog /mnt/var/log
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vaud /mnt/var/log/audit
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vtmp /mnt/var/tmp
```
mount –mkdir -o rw,nodev,nosuid ,relatime /dev/server/home /mnt/home
```
mount –mkdir -o rw,nodev,nosuid ,relatime /dev/server/podman /mnt/var/lib/containers
```
lsblk
```

## Install Packages
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
## Fstab

genfstab -U /mnt > /mnt/etc/fstab
```
## Format tmpfs ke tmp

echo “tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
``

## Chroot
```
arch-chroot /mnt
```

## Membuat Hostname
```
echo server > /etc/hostname
```

## Mengatur Waktu Dan Bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
hwclock –systohc
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
## Menambahkan user
```
useradd -m milky
``
passwd milky
```
Masukkan pw


## Konfigurasi sudo
```
echo “milky ALL=(ALL:ALL) ALL” > /etc/sudoers.d/milky
```
su milky
```
sudo su
```
Masukkan pw


## mkinitcpio
```
mkdir /etc/cmdline.d
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
lsblk
```
echo “rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6=lit root=/dev/server/root)” > /etc/cmdline.d/01-boot.conf
```
echo rw > /etc/cmdline.d/02-misc.conf
```
nvim /etc/mkinitcpio.conf
```
>Pada hooks paling bawah, tambahkan sd-encrypt
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt  lvm2 block filesystems fsck)  
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>Hapus semua pagar pada baris ALL

>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus ‘efi’ dan ubah jadi ‘boot’

bootctl –path=/boot install
```
mkinitcpio -P 
```
systemctl enable systemd-networkd
```
systemctl enable systemd-resolved
``	
systemctl enable iwd
```
systemctl enable sshd
```

## Selesai
```
exit
```
umount -R /mnt
```
reboot
```

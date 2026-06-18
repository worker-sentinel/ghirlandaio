# Arch Linux Install
## 1. Connect wifi

iwctl

station wlan0 get-networks

station wlan0 connect <namawifi>

exit

## 2.Setup LUKS

cryptsetup luksFormat /dev/sda7

cryptsetup luksOpen /dev/sda7 lilis

## 3.setup LVM

pvcreate /dev/mapper/lilis

vgcreate level /dev/mapper/lilis

lvcreate -L 10G proc -n root

lvcreate -L 10G proc -n vars

lvcreate -L 1G proc -n vlog

lvcreate -L 1G proc -n vaud

lvcreate -L 1G proc -n vtmp

lvcreate -L 10G proc -n home

lvcreate -l100%FREE proc -n dock

## 4.filesystem format

mkfs.vfat -F32 -S 4096 -n BOOT /dev/sda5

mkfs.ext4 /dev/proc/root

mkfs.ext4 /dev/proc/vars

mkfs.ext4 /dev/proc/vlog

mkfs.ext4 /dev/proc/vaud

mkfs.ext4 /dev/proc/vtmp

mkfs.ext4 /dev/proc/home

mkfs.ext4 /dev/proc/dock

## 5.bagian mount

mount /dev/level/root /mnt

mount --mkdir -o rw,nodev,nosuid,relatime           /dev/proc/vars /mnt/var

mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/proc/vlog /mnt/var/log

mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/proc/vaud /mnt/var/log/audit

mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/proc/vtmp /mnt/var/tmp

mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/proc/home /mnt/home

mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/proc/dock /mnt/var/lib/docker

mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot

## 6.bagian pacstrap

pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware \
  mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep docker

## 7.masukkan chroot

arch-chroot /mnt

## 8.bagian locale dan timezone

locale-gen

echo "LANG=en_US.UTF-8" > /etc/locale.conf

ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

hwclock --systohc

## 9.buat nama hostname

echo "lilis" > /etc/hostname

## 10.bagian mkinitcpio

HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)

default_uki="/boot/EFI/Linux/arch-linux-lts.efi"

mkinitcpio -P

## 11.bagian enable

systemctl enable systemd-networkd

systemctl enable systemd-resolved

systemctl enable iwd

systemctl enable firewalld

exit

bootctl --path=/mnt/boot install

umount -R /mnt

reboot

# install apache

sudo pacman -S apache

nvim /etc/httpd/conf/httpd.conf

nvim /etc/httpd/conf/extra/httpd-vhosts.conf

systemctl enable httpd

systemctl start httpd

# install docker swarm

pacman -S cockpit

systemctl enable cockpit

systemctl start cockpit

firewall-cmd --zone=public --add-port=7946/tcp --permanent

firewall-cmd --zone=public --add-port=7946/udp --permanent

firewall-cmd --zone=public --add-port=4789/udp --permanent

firewall-cmd --zone=public --add-service=cockpit --permanent

firewall-cmd --reload

ip a


Format luks
```
Cryptsetup luksFormat/dev/partisi
```
## Format Partisi
```
mkfs.ext4 /dev/[nama grup]/partisi
```
##  Format Partisi vfat
```
mkfs.vfat -F32 /dev/[nama partisi boot]
```
## Mounting
```
mount /dev/[nama grup]/root /mnt
```
```
mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
```
```
## luksFormat
```
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
```
> LuksFormat digunakan untuk memformat nama grup yang menjadi partisi, dan yang menjadi partisi kita menjadi home internal

## Install Package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```
## After Install
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo “tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```
arch-chroot /mnt
```
```
echo [hostname] > /etc/hostname
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock —systohc
```
```
nvim /etc/locale.gen (cari EN_US lalu hapus hashtag dua-duanya)
```
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
pacman -S pam_mount
```
```
lsblk
```
## SETUP HOME INTERNAL
```
cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
```

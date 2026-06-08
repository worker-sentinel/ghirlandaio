# KONEK WIFI

```
iwctl
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect "nama_jaringan"
```
```
exit
```
## tes jaringan

```
ping google.com
```

#  PARTISI

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```
### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 3G [EFI system]
system = 50G [linux filesystem]
```
`
#   SETUP LUKS

```
cryptsetup luksFormat --sector-size 4096 /dev/partisi
```
```
cryptsetup luksOpen /dev/partisi [creamy]
```
```
pvcreate /dev/mapper/[creamy]
```
```
vgcreate [pudding] /dev/mapper/[creamy]
```

# SETUP LVM

### root
```
lvcreate -L 15G pudding -n root
```
```
mkfs.ext4 /dev/pudding/root
```
```
mount /dev/pudding/root /mnt
```
### boot
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
```
### vars

```
lvcreate -L 15G pudding -n vars
```
```
mkfs.ext4 /dev/pudding/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/vars /mnt/var
```
### vtemp
```
lvcreate -L 1G pudding -n vtemp
```
```
mkfs.ext4 /dev/pudding/vtemp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vtemp /mnt/var/tmp
```
### vlog
```
lvcreate  -L 1G pudding -n vlog
```
```
mkfs.ext4 /dev/pudding/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vlog /mnt/var/log
```
### vaud
```
lvcreate -L 1G pudding -n vaud
```
```
mkfs.ext4 /dev/pudding/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vaud /mnt/var/log/audit
```
### dock
```
lvcreate -L 10G pudding -n dock
```
```
mkfs.ext4 /dev/pudding/dock
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/dock /mnt/var/lib/docker
```


### home
```
lvcreate -l70%FREE pudding -n home
```
```
mkfs.ext4 /dev/pudding/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/home /mnt/home
```

# INSTALL PACKAGE
```
lspci
```
> untuk melihat jenis hardware

```
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema firefox linux-firmware-realtek firewalld
```
# regist partisi
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
# chroot
```
arch-chroot /mnt
```
## buat hostname
```
echo "hostnameanda" > /etc/hostname
```
## time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
> cari en_US lalu uncommenting keduanya
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> di line paling atas ganti 'C' menjadi en_US
> di line paling bawah tambahkan en_US.UTF-8
## user
```
useradd -m 'nama_user'
```
```
passwd 'nama_user'
```
```
echo "nama_user ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/nama_user
```
## kernel parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi_luks])=creamy root=/dev/pudding/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
## mkinitcpio
```
cd /boot
```
```
mkdir kernel efi
```
```
cd efi
```
```
mkdir linux
```
```
cd ..
```
```
mv vmlinuz-* intel-* kernel
```
```
rm -fr initramfs-*
```
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> tambahkan lvm2 dan sd-encrypt setelah sd-vcondole di hooks paling bawah

```
nvim /etc/mkinitcpio.d/linuxx-lts-preset
```
> sesuaikan

```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
# finish installation
```
exit
```
```
umount -R /mnt
```
```
reboot
```



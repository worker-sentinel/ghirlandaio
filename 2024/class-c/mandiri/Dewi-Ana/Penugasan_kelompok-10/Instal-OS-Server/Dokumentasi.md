# install os server 
iwctl
```
station wlan0 get-networks
```
station wlan0 connect (nama_wifi)
```
exit
```
## Cek jaringan
```
ping 8.8.8.8
```
## Start asciinema
```
asciinema rec (nama_file).cast
```
## Cek partisi
```
lsblk
```
## Buat partisi
```
cfdisk /dev/partisi (sda/nvme)
```
EFI System= 2G
```
Linux Filesystem=46.8G
```
## format luks 
```
cryptsetup luksFormat /dev/nvme0n1p7
cryptsetup luksOpen /dev/nvme0n1p7 (lenovo)
```
## membuat physical volume
```
pvcreate /dev/mapper/(lenovo)
```
## Membuat volume Group
```
vgcreate (kelompok2) /dev/mapper/(lenovo)
```
## Membuat logical volume
```
lvcreate -L 8G (kelompok2) -n root
lvcreate -L 8G (kelompok2) -n vars
lvcreate -L 1G (kelompok2) -n vlog
lvcreate -L 1G (kelompok2) -n vtmp
lvcreate -L 1G (kelompok2) -n vaud
lvcreate -L 5G (kelompok2) -n home
lvcreate -L 5G (kelompok2) -n podman
```
## Memformat lvm
```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
mkfs.ext4 /dev/kelompok2/root
mkfs.ext4 /dev/kelompok2/vars
mkfs.ext4 /dev/kelompok2/vlog
mkfs.ext4 /dev/kelompok2/vtmp
mkfs.ext4 /dev/kelompok2/vaud
mkfs.ext4 /dev/kelompok2/home
mkfs.ext4 /dev/kelompok2/podman
```
## Cek Partisi
```
lsblk
```
## Mounting partisi
```
mount /dev/kelompok2/root /mnt
mount --mkdir -o uid=0,gid=0,fmaks=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/kelompok2/vars /mnt/vars
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/kelompok2/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/kelompok2/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/kelompok2/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/kelompok2/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/kelompok2/podman /mnt/var/lib/containers
```
## cek partisi 
```
lsblk
```
## install package 
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```
## FSTAB
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Konfigurasi jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Entri tmpfs
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
cat /mnt/etc/fstab
```
## Masuk ke sistem Arch linux
```
arch-chroot /mnt
```
##  Mengatur waktu dan bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
nvim /etc/locale.gen
```
>search en_US, lalu hapus hastag
``` 
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
>bagian LANG=C.UTF-8 diubah menjadi LANG=en_US.UTF-8
>bagian LC_ALL= ditambahkan en_US.UTF-8
```
## Membuat user
```
useradd -m sagitarius
passwd
```
>masukkan password, retype password
## sudo untuk user
```
echo "sagitarius ALL=(ALL:ALL) ALL" > /etc/sudoers.d/sagitarius
```
## Kernel
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root]=lenovoroot=/dev/kelompok2/root" > /etc/cmdline.d/01-boot.conf
echo "rw" > /etc/cmdline.d/02-misc.conf
```
## mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
>search HOOKS paling bawah, tambahkan sd-encrypt lvm2 setelah kata "sd-vconsole block"
```
nvim /etc/mkinitcpio.d/linux-lts.preset
>hapus semua hastag pada baris ALL
>tambahkan hastag pada default_image
>bagian default_uki hapus hastag dan kata "efi" pertama diganti boot
```
## install boot
```
bootctl --path=/mnt/boot install
mkinitcpio -P
```
## mengaktifkan systemd-networkd
```
systemctl enable systemd-networkd
```
## mengaktifkan DNS (domain name system)
```
systemctl enable systemd-resolved
```
## mengaktifkan wifi
```
systemctl enable iwd 
```
## keluar system
```
exit
```
## install boot (khusus lenovo)
```
bootctl --path=/mnt/boot install
```
## melepas mount file system
```
umount -R /mnt
```
## Reboot
```
reboot
```

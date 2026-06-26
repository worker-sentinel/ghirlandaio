# INSTALASI OS ADMIN 
## connect wifi
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect (nama_wifi)
```
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
```
EFI System= 4G
Linux Filesystem=48.7G
```
## format luks 
```
cryptsetup luksFormat /dev/nvme0n1p6
cryptsetup luksOpen /dev/nvme0n1p6 (sagitarius)
```
## membuat physical volume
```
pvcreate /dev/mapper/(sagitarius)
```
## Membuat volume Group
```
vgcreate (sagi) /dev/mapper/(sagitarius)
```
## Membuat logical volume
```
lvcreate -L 10G (sagi) -n root
lvcreate -L 10G (sagi) -n vars
lvcreate -L 1G (sagi) -n vlog
lvcreate -L 1G (sagi) -n vtmp
lvcreate -L 1G (sagi) -n vaud
lvcreate -L 5G (sagi) -n home
lvcreate -L 5G (sagi) -n podman
```
## Memformat lvm
```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
mkfs.ext4 /dev/sagi/root
mkfs.ext4 /dev/sagi/vars
mkfs.ext4 /dev/sagi/vlog
mkfs.ext4 /dev/sagi/vtmp
mkfs.ext4 /dev/sagi/vaud
mkfs.ext4 /dev/sagi/home
mkfs.ext4 /dev/sagi/podman
```
## Cek Partisi
```
lsblk
```
## Mounting partisi
```
mount /dev/sagi/root /mnt
mount --mkdir -o uid=0,gid=0,fmaks=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/sagi/vars /mnt/vars
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sagi/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/sagi/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/sagi/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/sagi/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/sagi/podman /mnt/var/lib/containers
```
## cek partisi 
```
lsblk
```
## install package 
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
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
useradd -m sagitariusadmin
passwd
```
>masukkan password, retype password
## sudo untuk user
```
echo "sagitariusadmin ALL=(ALL:ALL) ALL" > /etc/sudoers.d/sagitariusadmin
```
## Kernel
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root]=sagi root=/dev/sagi/root" > /etc/cmdline.d/01-boot.conf
echo "rw" > /etc/cmdline.d/02-misc.conf
cat /etc/cmdline.d/01-boot.conf,02-misc.conf
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
umount -R /mnt
```
## mengakhiri asciinema
```
ctrl+d
asciinema upload "nama_file".cast
```
## Reboot
```
reboot
```

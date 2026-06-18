# Menginstall OS Server

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```
cek driver wifi setiap laptop
```
station wlan0 get-network
```
melihat jaringan yang tersedia
```
station wlan0 scaan
```
memindaai jaringan yang ada
```
station wlan0 connect "(nama wifi)"
exit
```
## Memeriksa partisi
```
lsblk
```
membagi partisi
```
cfdiks /dev/partisi [sda/nvme0n1p1]
```
minimal partisi
```
boot = 3G [EFI system}
root = 95G [Linux filesystem]
```
```
lsblk lagi
```
## Format LUKS
```
cryptsetup luksFormat /dev/[partisi root]
```
```
masukkan kata sandi
```
```
cryptsetup luksOpen /dev/[partisi root] [nama device]
```
## Setup LVM 
```
pvcreate /dev/mapper/[nama device]
```
```
vgcreate [nama grup] /dev/mapper/[nama device]
```
## Membuat Logical Volume
```
lvcreate -L 8GB [nama grup] -n root
``````
```
lvcreate -L 8GB [nama grup] -n vars
```
```
lvcreate -L 1GB [nama grup] -n vlog
```
```
lvcreate -L 1GB [nama grup] -n vtmp
```
```
lvcreate -L 1GB [nama grup] -n vaud
```
```
lvcreate -L 5GB [nama grup] -n home
```
```
lvcreate -L 5GB [nama grup] -n podman
```
## Formating
```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
```
```
mkfs.ext4 /dev/[nama grup]/root/
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mkfs.ext4 /dev[nama grup]/home
```
```
mkfs.ext4 /dev[nama grup]/podman
```
## Periksa
```
lsblk
```
## Mounting
```
mount /dev/proc/root /mnt
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nosuid,relatime /dev/nama grup]/home /mnt/home
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```
## Periksa 
```
lsblk
```
## Install Package
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Mensetting jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Formating Tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
## Memeriksa kembali
```
cat /mnt/etc/fstab
```
## Masuk ke Dalam Sistem
```
arch-chroot /mnt
```
## Mensiknronisasi Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
## Mengatur Bahasa dan Lokasi
```
nvim /etc/locale.gen
```
agar pencarian lebih cepat, bisa mengklik tanda "/"
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
isi lang=C menjadi lang=en_US.UTF-8 dan 
isi ALL=en_US.UTF-8
```
## Membuat User
```
useradd -m kelompok10
```
```
passwd kelompok10
```
```
echo "kelompok10ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/kelompok10
```
## Mengatur Parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi root)=nama device root=/dev/nama grup/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
## Mengatur Mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
```
tambahkan kalimat sesudah "block"
This will create a systend based initramfs which loads an encrypted root filesysten.
HOOKS=(base systend autodetect microcode modconf kas keyboard sd-vconsole block sd-encrypt filesystens fsck

NOTE: If you have /usr on a separate partition, you MUST include the usr and fsck hooks.
HOOKS (base systend autodetect migrocode modconf kms keyboard sd-vconsole block [sd-encrypt lvm2] filesystems fsck)

COMPRESSION
is used for Linux 5.9 and gzip compression is used for Linux < 5.9.
Use 'cat' to create an uncompressed image.
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
**ikutin yang ada di sini
```
ALL_config="/etc/nkinitcpio.d/default.conf"
ALL kuer="/boot/kernel/vmlinuz-linux-lts"
ALL kerneldest="/boot/kernel/unlinuz-linux-lts"

PRESETS-('default')
#PRESETS-('default' 'fallback')
#default_config="/etc/nkinitcpio.conf"

#default_inage="/boot/initranfs-linux-lts.ing"
default uki="/boot/efi/linux/arch-linux-lts.efi"
#default_options="-splash /usr/share/systend/bootct1/splash-arch.

sfallback_config="/etc/nkinitcpio.conf"
#fallback inage="/boot/initranfs-linux-its-fallback.ing"
#fallback_uki="/efi/EFI/Linux/arch-linux-Its-fallback.efi"
#fallback_options="-S autodetect"
```
## Menginstall Bootctl
```
bootctl --path=/mnt/boot install
```
untuk yang bukan lenovo bisa lanjut ke tahap betikutnya, tetapi khusus device lenovo disarankan mengikuti ini:
```
exit
```
```
bootctl --path=/mnt/boot install
```
kemudian masuk kembali ke sistem
```
arch-chroot /mnt
```
```
mkinitcpio -P
```
## Aktifkan sistem berikut:
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd
```
## BOOTING
```
exit
```
```
umount -R /mnt
```
mematikan asciinema
```
ctrl+d
```
```
asciinema upload [nama file],cast
```
foto link asciinema
```
reboot




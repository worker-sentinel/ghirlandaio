## Menginstall os untuk Server APK

## Sambungkan internet pada laptop
```
iwctl
station wlan0 get-networks
station wlan0 connect "Nama jaringan"
masukkan password wifi
```
## Format luks 
```
cryptsetup luksFormat /dev/(Partisi root)
```
## Open Luks
```
cryptsetup luksOpen /dev/(partisi root)/(nama device)
```
## Membuat Physical volume
```
pvcreate /dev/mapper/(nama device)
```
## Membuat Volume Group
```
vgcreate (nama group) /dev/mapper/(nama device)
```
## Membuat logical volume
```
lvcreate -L 10G (nama group) -n root
lvcreate -L 5G (nama group) -n vars
lvcreate -L 2G (nama group) -n vlog
lvcreate -L 1G (nama group) -n vaud
lvcreate -L 2G (nama group) -n vtmp
lvcreate -l50%FREE (nama group) -n home
lvcreate -l100%FREE (nama group) -n podman
```
Cek hasil partisi
```
lsblk
```
## Format partisi yang sudah dibuat
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
mkfs.ext4 /dev/(nama group)/root
mkfs.ext4 /dev/(nama group)/vars
mkfs.ext4 /dev/(nama group)/vlog
mkfs.ext4 /dev/(nama group)/vaud
mkfs.ext4 /dev/(nama group)/vtmp
mkfs.ext4 /dev/(nama group)/home
mkfs.ext4 /dev/(nama group)/podman
```
## Mount partisi
```
mount /dev/(nama group) /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi boot) /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama group)/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama group)/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama group)/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama group)/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama group)/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama group)/podman /mnt/var/lib/containers
```
Cek hasil Mount
```
lsblk
```
## Install Pacstrap
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
## Generate file fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Tambah entry manual ke fstab
```
echo 'tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
## Konfigurasi network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Masuk ke arch-chroot
```
arch-chroot /mnt
```
## Set hostname sistem
```
echo server > /etc/hostname
```
## Set zona waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
## Sinkron hardware clock
```
hwclock --systohc
```
## Buka file locale.gen
```
nvim /etc/locale.gen
Tekan huruf "i" dan cari nama #en_US
Hapus tanda "#" pada #en_US.UTF-8 UTF-8 dan #en_US ISO-8859-1
Setelah dihapus tekan tombol ESC dan tekan (:) lalu ketik wq
```
## Setup locale
```
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
```
## Tambahkan beberapa hal
```
Tekan huruf "i" lalu ubah LANG=C menjadi "LANG=en_US"
Lalu tambahkan "en_US.UTF-8" pada baris LC.ALL=
Tekan tombol ESC dan tekan (:) lalu ketik wq
```
## Menambahkan user
```
useradd -m (nama user)
passwd (nama user)
```
## Memberikan hak akses kepada user
```
echo "(nama user) ALL=(ALL:ALL) ALL" > /etc/sudoers.d/(nama user)
cek dengan su (nama user) lalu sudo su dan masukkan password yg sudah dibuat tdi
```
## Mengatur Parameter
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d{01-boot.conf,02-misc.conf}
echo "rd.luks.name=$(blkid -s UUID -o value /dev/(partisi root)=(nama device) root=/dev/(nama group/root" > /etc/cmdline.d/01-boot.conf
```
## Mengatur mkinitcpio
```
nvim /etc/mkinitcpio.conf
Tekan huruf "i" lalu samakan pada baris HOOKS yang paling bawah seperti ini
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)
Setelah itu tekan ESC dan tekan ketik (:wq)
Setelah itu ketik nvim /etc/mkinitcpio.d/linux-lts.preset lalu tekan huruf "i"
Hapus semua tanda "#" pada ketiga baris ALL, lalu tambahkan tanda "#" baris "default_images
Pada baris "#default_uki" ubah kata "efi" pertama menjadi "boot"
Tekan tombol ESC dan ketik ":wq"
```
## Install boot
```
bootctl --path=/boot install
```
## Menginstall mkinitcpio
```
mkinitcpio -P
```
## Enable systemctl
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable sshd
exit sampai kembali ke root@blackbird
umount -R /mnt
reboot
```
## Selesai

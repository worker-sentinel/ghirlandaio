#   Install OS Hardened Luks-On-Lvm

## Cek Partisi
```
lsblk
```
> Digunakan untuk menampilkan daftar disk dan partisi yang tersedia sebelum proses konfigurasi dimulai.

## Membuat Physical Volume (PV)
```
pvcreate /dev/[nama partisi]
```
> Menginisialisasi partisi agar dapat digunakan sebagai Physical Volume dalam LVM.

## Membuat Volume Group (VG)
```
vgcreate [nama grup] /dev/[nama partisi]
```
> Menggabungkan Physical Volume ke dalam sebuah Volume Group sebagai ruang penyimpanan utama untuk Logical Volume.

## Membuat Logical Volume (LV)
untuk root
```
lvcreate -L [size G | M] [nama grup] -n root
```
untuk vars
```
lvcreate -L [size G | M] [nama grup] -n vars
```
untuk var log
```
lvcreate -L [size G | M] [nama grup] -n vlog
```
untuk var log audit
```
lvcreate -L [size G | M] [nama grup] -n vaud
```
untuk var tmp
```
lvcreate -L [size G | M] [nama grup] -n vtmp
```
untuk home
```
lvcreate -L [size G | M] [nama grup] -n home
```
untuk podman
```
lvcreate -l50%FREE [nama grup] -n podman
```
untuk home internal
```
lvcreate -l00%FREE [nama grup] -n [nama home untuk internal]
```
> Luks on lvm home nya ada (eksternal dan internal)

## Format Partisi
```
mkfs.ext4 /dev/[nama grup]/root
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mkfs.ext4 /dev/[nama grup]/home
```
```
mkfs.ext4 /dev/[nama grup]/podman
```
> Memformat setiap Logical Volume menggunakan filesystem ext4 agar siap digunakan sebagai media penyimpanan.

##  Format Partisi vfat
```
mkfs.vfat -F32 /dev/[nama partisi boot]
```
> Memformat partisi boot menggunakan filesystem FAT32 agar kompatibel dengan UEFI.

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
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```
> Melakukan mounting seluruh partisi dengan opsi keamanan yang sesuai sebelum instalasi sistem.
>
> Home untuk internal tidak perlu dimounting, karena nanti mounting otomatis pake aplikasi
pam_mount.

## luksFormat
```
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
```
> Melakukan pembuatan password, password nya harus sama kayak password user
>
> Mengenkripsi partisi home internal menggunakan LUKS untuk meningkatkan keamanan data pengguna.

## Install Package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```
> Menginstal Arch Linux Hardened beserta paket-paket yang dibutuhkan selama proses deployment server.

## After Install
```
genfstab -U /mnt > /mnt/etc/fstab
```
> Membuat konfigurasi fstab agar seluruh partisi dapat dimount secara otomatis saat sistem dijalankan.

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
> masukin password yg tadi udah dibuat
```
mkfs.ext4 /dev/mapper/[device name]
```
```
mkdir /home/[nama folder]
```
```
useradd -d /home/[nama folder] [nama user]
```
```
passwd [nama user]
```
> masukin password. password harus sama kayak pas cryptsetup luksFormat
```
echo “[nama user] ALL = (ALL:ALL)” > /etc/sudoers.d/[nama user]
```
```
chown -R [nama user]:[nama user] /home/[nama folder]
```
```
nvim /etc/security/pam_mount.conf.xml
```
```
nvim /etc/pam.d/system-login
```
## MKINITCPIO
```
mkdir /etc/mkinitcpio.d
```
```
nvim /etc/cmdline.d/boot.conf
```
```
nvim /etc/mkinitcpio.conf
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
## INSTALL SYSTEMD BOOT
```
bootctl --path=/boot install
```
> Menginstal bootloader systemd-boot agar sistem dapat melakukan proses boot menggunakan Linux Hardened.

```
mkinitcpio -P
```
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-networkd
```
```
systemctl enable iwd
```
```
systemctl enable firewalld
```
```
systemctl enable sshd
```
```
exit
```
```
reboot
```

# partition
```
cryptsetup luksFormat /dev/partition_name
```
```
cryptsetup luksOpen /dev/partition_name device_name
```
```
pvcreate /dev/mapper/device_name
```
```
vgcreate group_name /dev/mapper/device_name
```

## root
```
lvcreate -L size (G | M) group_name -n root
```
```
mkfs.ext4 -b /dev/group_name/root
```
```
mount /dev/group_name/root /mnt
```

Menggunakan ext4 karena stabil dan versi terbaru

## boot
```
mkfs.vfat -F32 -n BOOT /dev/partition
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/paritition /mnt/boot
```


## var
```
lvcreate -L size (G | M) group_name -n volume_name (example:vars)
```
```
mkfs.ext4 -b /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/volume_name /mnt/var
```
rw ( Read-Write ) = Mengizinkan sistem untuk membaca dan menulis data pada partisi
nodev ( No Devices ) = Mencegah pengguna jahat membuat perangkat palsu untuk mengakses hardware secara langsung. 
nosuid ( No Set-User-ID ) = Mengabaikan bit Set-User-ID (SUID) pada file. Jika sebuah program memiliki hak akses root, opsi ini mencegah program tersebut dijalankan dengan hak akses root oleh pengguna biasa, sehingga membatasi potensi eksploitasi keamanan.
relatime ( Relative Access Time ) = Mencatat waktu akses file, jika waktu modifikasi atau perubahan sebelumnya lebih lama dari waktu saat ini. Ini sangat mengoptimalkan kinerja drive (terutama SSD)

## vtmp
```
lvcreate -L size (G | M) group_name -n volume_name (example:vtmp)
```
```
mkfs.ext4 -b /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/tmp
```

rw ( Read-Write ) = Mengizinkan sistem untuk membaca dan menulis data pada partisi
nodev ( No Devices ) = Mencegah pengguna jahat membuat perangkat palsu untuk mengakses hardware secara langsung. 
nosuid ( No Set-User-ID ) = Mengabaikan bit Set-User-ID (SUID) pada file. Jika sebuah program memiliki hak akses root, opsi ini mencegah program tersebut dijalankan dengan hak akses root oleh pengguna biasa, sehingga membatasi potensi eksploitasi keamanan.
relatime ( Relative Access Time ) = Mencatat waktu akses file, jika waktu modifikasi atau perubahan sebelumnya lebih lama dari waktu saat ini. Ini sangat mengoptimalkan kinerja drive (terutama SSD)
noexec ( No Execute ) = Melarang sistem menjalankan file biner (aplikasi) secara langsung dari partisi tersebut. Ini adalah fitur keamanan vital untuk memblokir malware atau script berbahaya agar tidak bisa dieksekusi.

## vlog
```
lvcreate -L size (G | M) group_name -n volume_name (example:vlog)
```
```
mkfs.ext4 -b /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/log
```

## vaud
```
lvcreate -L size (G | M) group_name -n volume_name (example:vaud)
```
```
mkfs.ext4 -b /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/log/audit
```

## home
```
lvcreate -l50%FREE group_name -n home
```
```
mkfs.ext4 -b /dev/group_name/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/home /mnt/home
```


# packages
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base base-devel neovim firewalld openssh
```
- intel-ucode = untuk prosesor intel ( pakai amd-ucode jika prosesor kalian amd )
- linux-lts = sebagai kernel linux ( gunakan kernel yang sesuai kebutuhan )
- linux-lts-headers = package yang berisi header atau ( sumber inti kernel tersebut ) dibutuhkan untuk menyusun atau mengompilasi driver perangkat keras
- linux-firmware = paket perangkat lunak dalam sistem operasi Linux yang berisi kumpulan blob biner firmware. Paket ini berfungsi sebagai jembatan penting agar driver kernel dapat berkomunikasi dan mengontrol perangkat keras, seperti kartu Wi-Fi, Bluetooth, GPU, dan webcam.
- mkinitcpio = pembuat initramfs (Initial RAM filesystem) modular yang secara khusus digunakan dalam sistem operasi berbasis Arch Linux
- lvm2 = Logical Volume Manager versi 2 perangkat lunak manajemen disk tingkat lanjut pada sistem Linux. Alat ini memisahkan sistem operasi dari batasan perangkat keras fisik, memungkinkan Anda untuk mengatur, menggabungkan, dan mengubah ukuran kapasitas ruang penyimpanan dengan mudah.
- base = packages basic untuk installasi archlinux
- base-devel = basic tools untuk packages archlinux


# fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
# tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

# chroot
```
arch-chroot /mnt
```

## hostname
```
echo [nama komputer] > /etc/hostname
```

## localtime
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## locale

```
nvim /etc/locale.gen
```
uncommentin both `en_US.UTF-8`
```
en_US.UTF-8
en_US.ISO-8859-1
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
add value like below
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```


## user admin
```
useradd -m [user_name]
```
```
passwd [user_name]
```
```
echo "[user_name] ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
```
## user operator
```
useradd -m [user_name]
```
```
passwd [user_name]
```

## cmdline
```
mkdir -p /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partition_name)=device_name root=/dev/group_volume/volume_name" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## boot
```
rm -fr /boot/initramfs-linux-*
```

## initramfs
```
nvim /etc/mkinitcpio.conf
```
add `sd-encrypt` and `lvm` after `sd-vconsole`
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
the same as the configuration below
```
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"
ALL_kerneldest="/boot/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"

```
```
touch /etc/vconsole.conf
```
```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
## desktop
```
pacman -S xfce4 sddm networkmanager
```
## service
```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
```

```
systemctl enable NetworkManager
```

```
systemctl enable firewalld
```

```
systemctl enable sddm
```

## booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```

## after booting
### setup firewalld
1. check zone. example in below
```
sudo firewall-cmd --list-all-zone
```
2. allow port. example in below

```
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```
3. allow service. example in below
```
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```
4. remove service. example in below
```
sudo firewall-cmd --zone=internal --remove-service=ssh --permanent
```
>[Note]
> hapus semua service dan port selain di zone public
```
sudo firewall-cmd --reload
```

### setup hardening kernel
#### example for disale module kernel
for check wireless device
```
lspci -knnd ::0280
```
value
```
02:00.0 Network controller [0280]: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak] [8086:24fb] (rev 10)
	Subsystem: Intel Corporation Device [8086:2110]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```
> iwlwifi is a module
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
value
```
install usb-storage /bin/false
blacklist usb-storage
install iwlwifi /bin/false
blacklist iwlwifi
```
```
sudo modprobe -r usb-storage
```
```
sudo mkinitcpio -P
```

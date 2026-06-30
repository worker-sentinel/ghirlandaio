# INSTALASI OS INTERNAL

## Masuk ke Jaringan
```
iwctl
device list
station wlan0 get-networks
station wlan0 connect (gunakan internet yang diinginkan)
exit
ping 8.8.8.8
```

## Rekam Asciinema
```
asciinema rec andromeda.cast
```

## Cek Partisi
```
lsblk
```
## Membuat Phsycal Volume
```
pvcreate /dev/[nama partisiroot]
```
## Membuat Volume Group
```
vgcreate andromeda /dev/[nama partisi root]
```
## membuat Logical Volume:
```
lvcreate -L 15G andromeda -n root
```

```
lvcreate -L 12G andromeda -n vars
```

```
lvcreate -L 2G andromeda -n vlog
```

```
lvcreate -L 1G andromeda -n vaud
```

```
lvcreate -L 1G andromeda -n vtmp
```

```
lvcreate -L 10G andromeda -n home
```

```
lvcreate -l50%FREE andromeda -n podman
```
untuk home internal
```
lvcreate -l50%FREE andromeda -n andromeda
```
> di luks on lvm, home nya ada 2 (eksternal dan internal)

## Cek kembali partisinya
```
lsblk
```

## Format Partisi
```
mkfs.ext4 /dev/andromeda/root
```
```
mkfs.ext4 /dev/andromeda/vars
```
```
mkfs.ext4 /dev/andromeda/vlog
```
```
mkfs.ext4 /dev/andromeda/vaud
```
```
mkfs.ext4 /dev/andromeda/vtmp
```
```
mkfs.ext4 /dev/andromeda/home
```
```
mkfs.ext4 /dev/andromeda/podman
```


##  Format Partisi vfat
```
mkfs.vfat -F32 /dev/[partisi boot]
```

## Mounting
```
mount /dev/andromeda/root /mnt
```
```
mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/andromeda/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/andromeda/podman /mnt/var/lib/containers
```
> home internal tidak perlu dimounting, karena nanti akan ter-mounting otomatis menggunakan aplikasi
pam_mount

## luksFormat
```
cryptsetup luksFormat /dev/andromeda/andromeda
```

## Install Package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```

## Men-generate setelah Install package
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo “tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```
arch-chroot /mnt
```
## Membuat Hostname
```
echo andromeda > /etc/hostname
```
## Sinkronisasi Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock —systohc
```
## Mengatur Bahasa dan Lokasi
```
nvim /etc/locale.gen 
```
> cari /en_US lalu hapus kedua # di depannya
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
## Install pam_mount
```
pacman -S pam_mount
```

## Cek kembali partisi
```
lsblk
```
## SETUP HOME INTERNAL
```
cryptsetup luksOpen /dev/andromeda/andromeda andromeda
```
```
mkfs.ext4 /dev/mapper/andromeda
```
```
mkdir /home/andromeda
```
## Membuat nama untuk user
```
useradd -d /home/andromeda
```
```
passwd andromeda
```
```
echo “andromeda ALL=(ALL:ALL)” > /etc/sudoers.d/andromeda
```
```
chown -R andromeda:andromeda /home/andromeda
```
```
nvim /etc/security/pam_mount.conf.xml
```
```
nvim /etc/pam.d/system-login
```

## Mkinitcpio
```
mkdir /etc/mkinitcpio.d
```
```
nvim /etc/mkinitcpio.conf
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
> samakan dengan yang di bawah ini
```
samakan dengan yang di bawah ini

# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/efi/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```
## Instalasi System
```
bootctl --path=/boot install
```
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
umount -R /mnt
```
```
asciinema upload andromeda.cast
```
```
reboot
```

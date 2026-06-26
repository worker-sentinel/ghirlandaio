### Installasi OS Hardened Arsip

### Koneksi WIFI
```
iwctl
```
```
device list
```
```
station wlan0 get-networks
```
```
station wlan0 connect (nama wifi) 
```
```
exit
```

### Mengetes Jaringan
```
ping google.com
```

### Buat partisi
```
cfdisk /dev/
```

### Membagi partisi sesuai penyimpanan yang ada

```
boot = 3G EFI System
root = sisanya G Linux filesystem 
```

### Membagi partisi
```
pvcreate /dev/[nama_partisi]
```
```
vgcreate kal /dev/[nama_partisi]
```
```
lvcreate -L size G kal -n root
```
```
lvcreate -L size G kal -n vars
```
```
lvcreate -L size G kal -n vlog
```
```
lvcreate -L size G kal -n vaud
```
```
lvcreate -L size G kal -n vtmp
```
```
lvcreate -L size G kal -n home
```
```
lvcreate -l100%FREE kal -n podman
```

### Format partisi

```
mkfs.ext4 /dev/kal/root
```
```
mkfs.ext4 /dev/kal/vars
```
```
mkfs.ext4 /dev/kal/vlog
```
```
mkfs.ext4 /dev/kal/vaud
```
```
mkfs.ext4 /dev/kal/vtmp
```
```
mkfs.ext4 /dev/kal/home
```
```
mkfs.ext4 /dev/kal/podman
```
```
mkfs.vfat -F32 /dev/kal
```

### Mounting

```
mount /dev/kal/root /mnt
```
```
mount —mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/PartisiBoot /mnt/boot
```
```
mount —mkdir -o rw,nodev,nosuid,relatime /dev/kal/vars /mnt/var
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/kal/vlog /mnt/var/log
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/kal/vaud /mnt/var/audit
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/kal/vtmp /mnt/var/tmp
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/kal/home /mnt/home
```
```
mount —mkdir -o rw,nodev,nosuid,relatime /dev/kal/podman /mnt/var/lib/containers
```

```
cryptsetup luksFormat /dev/kal/container(nama home internal)
```

### Install package

```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```

### Setelah install
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo “tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

### Masuk arch chroot
```
arch-chroot /mnt
```

### Buat hostname & sinkron waktu

```
echo [hostname] > /etc/hostname
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen 
```

*cari en_US lalu hapus #*
*esc kemudian ketik :wq*

```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```

*Paling atas diubah menjadi en_US dan pada bagian paling bawah tambahkan en_US.UTF-8*

### Membuka Enkripsi LUKS 
```
cryptsetup luksOpen /dev/kal/containers [device name]
```

### Membuat system
```
mkfs.ext4 /dev/mapper/[device name]
```
```
mkdir /home/[nama folder]
```

### Buat user
```
useradd -d /home/[nama folder] [nama user]
passwd [nama user]
```
```
echo “[nama user] ALL‎ = (ALL:ALL)” > /etc/sudoers.d/[nama user]
```

### Mengatur kepemilikan home
```
chown -R [nama user]:[nama user] /home/[nama folder]
```

### Konfigurasi MKINITCPIO
```
mkdir /etc/mkinitcpio.d
```
```
nvim /etc/cmdline.d/boot.conf
```
**setelah masuk ke tampilan itu isi nya kosong, lalu pencet insert dan ketik root=/dev/namagrup/root rw**

```
nvim /etc/mkinitcpio.conf
```
**cari bagian HOOKS yang tidak memiliki tagar dan posisinya di atas kata “COMPRESSION” di file itu, dibaris “HOOKS” setelah kata sd-vconsole spasi lalu ketik lvm2**

**esc kemudian :wq**

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

### mkinitcpio preset file for the 'linux-hardened' package
```
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/EFI/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```

### Instalasi

```
bootctl —path=/boot install
```

## generate initramfs

```
mkinitcpio -P
```

## Mengaktifkan layanan jaringan, Wi-Fi, firewall dan SSH

```
systemctl enable systemd-networkd
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
```
```
exit
```
```
reboot
```
## Selesai


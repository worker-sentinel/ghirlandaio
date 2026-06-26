# Menginstall OS Server

## Mengaktifkan rekaman asciinema
```
asciinema record server2andromeda.cast
```

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```

```
station wlan0 get-networks
```
menampilkan hasil scan berupa daftar SSID yang ditemukan

```
station wlan0 scan
```
meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar
```
station wlan0 connect (nama wifi)
```
```
station wlan0 connect "wifi sekolah"
```
jika nama wifi lebih dari 1 kata. gunakan tanda "..."
```
exit
```
## Memeriksa partisi
```
lsblk
```

## Membuat dan mengatur partisi
```
cfdiks /dev/nvme0n1p
```

## Jumlah partisi
```
boot = 3g [efi system] sda7
root = 48.8g [linux filesystem] sda8
```

## Cek kembali partisi
```
lsblk
```
## Format luks
dengan menggunakan root 
```
cryptsetup luksFormat /dev/nvme0n1p8
```
masukkan kata sandi
```
cryptsetup luksOpen /dev/nvme0n1p8 andromeda
```

# Setup lvm
```
pvcreate /dev/mapper/andromeda
```
```
vgcreate andromeda /dev/mapper/andromeda
```

## Membuat logical volume
```
lvcreate -L 15G andromeda -n root
lvcreate -L 14G andromeda -n vars
lvcreate -L 2G andromeda -n vlog
lvcreate -L 2G andromeda -n vtmp
lvcreate -L 2G andromeda -n vaud
lvcreate -L 14G andromeda -n home
lvcreate -l100%FREE andromeda -n podman
```

## Format
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p7
mkfs.ext4 /dev/andromeda/root
mkfs.ext4 /dev/andromeda/vars
mkfs.ext4 /dev/andromeda/vlog
mkfs.ext4 /dev/andromeda/vtmp
mkfs.ext4 /dev/andromeda/vaud
mkfs.ext4 /dev/andromeda/home
mkfs.ext4 /dev/andromeda/podman
```

## Periksa partisi Kembali untuk memastikan
```
lsblk
```
## Mount
```
mount /dev/andromeda/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/podman /mnt/podman
```
## Periksa partisi Kembali
```
lsblk
```
## Instal package
```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```
## file system table (fstab)
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
cat /mnt/etc/fstab
```
## masuk ke dalam chroot
```
arch-chroot /mnt
```
## membuat hostname
```
nvim /etc/hostname
```
```
tekan huruf "i" lalu masukkan hostname (sesuai keinginan) lalu klik 'esc' dan ketik ':wq'
```
```
cat /etc/hostname
```
## Sinkronisasi Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## Atur Bahasa dan lokasi
```
nvim /etc/locale.gen
```
Ketik tanda "/" agar pencarian lebih cepat
/en_US

Tekan enter lalu tekan "i"

Lalu hapus tanda "#" pada
en_US.UTF-8 UTF-8
en_US ISO-8859-1

Tekan esc setelah itu :wq

## Menghasilkan dan mengatur konfigurasi locale sistem agar menggunakan bahasa dan format regional en_US.UTF-8
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
isi lang=C menjadi lang=en_US.UTF-8

isi ALL=en_US.UTF-8
```

## Membuat user
```
mkdir -p /home/user
```
```
useradd -d /home/user server
```
```
passwd
```
```
chown -R server:server /home/user
```
```
passwd server
```
```
echo 'server ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/server
```
```
cat /etc/sudoers.d/server
```

## konfigurasi bootloader
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
nvim /etc/cmdline.d/01-boot.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p8)=server root=/dev/andromeda/root" > /etc/cmdline.d/01-boot.conf
```
```
nvim /etc/cmdline.d/06-misc.conf
```
## konfigurasi mkinitcpio
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> cari hooks tanpa tanda # lalu samakan dengan HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)

## menyiapkan layout direktori UKI
### berpindah ke direktori boot
```
cd /boot
```
### pengecekan file booting
```
ls
```
```
mkdir efi kernel
``` 
```
cd efi/
```
```
mkdir linux
```
```
cd ..
```
```
ls
```
```
mv vmlinuz-linux-hardened intel-ucode.img kernel/
```
```
ls
```
```
rm -fr initramfs-linux-hardened.img
```
```
ls
```
```
ls efi/
```
```
ls kernel/
```
## Mengubah konfigurasi initramfs kernel Linux LTS
```
nvim /etc/mkinitcpio.d/linux-hrdened.preset
```

samakan dengan ini
```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.d/default.conf"
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
```
mkinitcpio -P
```

## Install bootctl
```
bootctl --path=/mnt/boot install 
```
```
lablk -o name,uuid
```
```
cat /etc/cmdline.d/01-boot.conf
```
```
ls efi/linux
```
mengecek systemd boot
```
bootctl status
```
## Selesai instalasi os
```
exit
umount -R /mnt
```
## Menghentikan rekaman
ctrl+d

asciinema upload serverandromeda.cast

## Selesai
```
reboot
```

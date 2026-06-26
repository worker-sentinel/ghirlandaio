# Instalasi OS Server


## Rekam

```
asciinema record namafile.cast
```

## Connect jaringan wifi

```
iwctl
```

Menampilkan list device

```
device list
```


Pastikan device nya on, jika belum on, gunakan command:

```
rfkill unblock all
```

Scan wifi

```
station wlan0 scan
```

Menampilkan wifi yang tersedia

```
station wlan0 get-network
```
Sambungkan wifi

```
station wlan0 connect “nama wifi”
```

```
exit
```

Tes jaringan wifi

```
ping 8.8.8.8
```

## Cryptsetup dan format luks

```
cryptsetup luksFormat /dev/partisi sistem
```

```
cryptsetup luksOpen /dev/partisi sistem proc
```

```
pvcreate /dev/mapper/proc
```

```
vgcreate [nama volume] /dev/mapper/proc
```

## Menampilkan device

```
lsblk
```


## Create logical volume

```
lvcreate -L 8G server -n root
```
```
lvcreate -L 5G server -n vars
```
```
lvcreate -L 1G server -n vlog
```
```
lvcreate -L 1G server -n vaud
```
```
lvcreate -L 1.5G server -n vtmp
```
```
lvcreate -l50%FREE server -n home
```
```
lvcreate -l100%FREE server -n podman
```

Setelah itu cek kembali dengan

```
lsblk
```

## Format partisi

```
mkfs.ext4 /dev/server/root
```
```
mkfs.vfat -F32 /dev/partisi boot
```
```
mkfs.ext4 /dev/server/vars
```
```
mkfs.ext4 /dev/server/vlog
```
```
mkfs.ext4 /dev/server/vaud
```
```
mkfs.ext4 /dev/server/vtmp
```
```
mkfs.ext4 /dev/server/home
```
```
mkfs.ext4 /dev/server/podman
```

## Mounting partisi

```
mount /dev/server/root /mnt
```

```
lsblk
```

```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi boot /mnt/boot
```

```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/server/vars /mnt/var
```

```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vlog /mnt/var/log
```

```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vaud /mnt/var/log/audit
```

```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server/vtmp /mnt/var/tmp
```

```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/server/home /mnt/home
```

```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/server/podman /mnt/var/lib/containers
```

Cek kembali dengan:

```
lsblk
```

## Menginstall sistem OS

```
pacstrap /mnt base amd-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl which grep iwd podman
```

## Membuat fstab
```
genfstab -U /mnt/etc/fstab
```

## Menambah tmpfs

```
echo “tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```

```
cat /mnt/etc/fstab
```

# Mengkonfigurasi network

```
cp /etc/systemd/network/*  /mnt/etc/systemd/network
```

## Masuk root

```
arch-chroot /mnt
```

```
echo server > /etc/hostname
```

## Mengatur timezone

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

```
hwclock –systohc
```

## Mengatur locale

```
nvim /etc/locale.gen
```

hapus hastag pada:

```
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1
```

agar jadi:

```
en_US.UTF-8 UTF-8
en_US ISO-8859-1
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
Edit pada bagian:

```
LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)
```

## Membuat useradd

```
useradd -m server
```

```
passwd server
```

```
echo “server ALL=(ALL:ALL) ALL” > /etc/sudoers.d/server
```

```
su server
```

```
sudo su
```

Kemudian masukkan password

## Konfigurasi cmdline

```
mkdir /etc/cmdline.d
```

```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

Cek kembali:

```
lsblk
```

```
cat /etc/cmdline.d/01-boot.conf
```

```
echo “rd.luks.name=$(blkid -s UUID -o value /dev/partisi boot)=proc root=/dev/server/root” > /etc/cmdline.d/01-boot.conf
```

```
echo rw > /etc/cmdline.d/02-misc.conf
```

```
nvim /etc/mkinitcpio.conf
```

Edit bagian HOOKS terakhir (yang tidak ada # di depannya), tambahin (sd-encrypt lvm2)

Edit bagian

```
nvim /etc/mkinitcpio.d/linux-hardened.conf.preset
```

```
= mkinitcpio preset file for the 'linux-hardened' package
ALL_config="/etc/mkinitepio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/vmlinuz-linux-hardened"


PRESETS=( 'default')
#PRESETS=('default' ‘fallback')

#default_config="/etc/mkinitcpio.conf”
#default_image="/boot/initramfs-linux-hardened.img”
 default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
#default_options="--splash /us/share/systend/bootctl/splash-arch.bmp”
#fallback_config=”/etc/mkinitcpio.conf”
#fallback_image="/boot/initramfs-linux-hardened-fallback.img “
#fallback_uki=”/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect”
```

## Menginstall bootctl

```
bootctl –path=/boot install
```

```
mkinitcpio -P 
```

## Mengaktifkan service 

```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
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
reboot
```

# Dokumentasi OS Server
## Menghubungkan jaringan

```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-network
```
```
station wlan0 connect “nama wifi”
```
```
exit
```
```
ping 8.8.8.8
```

## aktifkan asciinema

## cryptsetup dan format luks

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
vgcreate [nama volume ] /dev/mapper/proc
```

## cek partisi

```
lsblk
```

## Buat logical volume

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
```
lsblk
```

## Memformat partisi

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

## Mount

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
```
lsblk
```

## Pacstrap

```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```

## Membuat fstab
```
genfstab -U /mnt/etc/fstab
```

## Menambah tmpfs

```
echo ‘“tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
```
cat /mnt/etc/fstab
```

# Konfigurasi network

```
cp /etc/systemd/network/*  /mnt/etc/systemd/network
```

## Masuk sistem baru

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

hapus hastag 
    # en_US.UTF-8 UTF-8
		#en_US ISO-8859-1

```    
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```

LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)

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

enter password

## Konfigurasi cmdline

```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
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

## bagian HOOKS akhir tambahin (sd-encrypt lvm2)

```
nvim /etc/mkinitcpio.d/linux-lts.conf.preset
```

= mkinitcpio preset file for the 'linux-lts' package
ALL_config="/etc/mkinitepio.conf"
ALL_kver="/boot/vmlinuz-linux-Its"
ALL_kerneldest="/boot/vmlinuz-linux-lts"


PRESETS=( 'default')
#PRESETS=('default' ‘fallback')

#default_config="/etc/mkinitcpio.conf”
#default_image="/boot/initramfs-linux-lts.img”
 default_uki="/boot/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /us/share/systend/bootctl/splash-arch.bmp”
#fallback_config=”/etc/mkinitcpio.conf”
#fallback_image="/boot/initramfs-linux-lts-fallback.img “
#fallback_uki=”/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect”

## Menginstall bootctl

```
bootctl –path=/boot install
```
```
mkinitcpio -P 
```
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

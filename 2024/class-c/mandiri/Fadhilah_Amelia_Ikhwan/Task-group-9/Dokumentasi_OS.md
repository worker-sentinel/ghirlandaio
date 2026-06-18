# Konek Wifi

```
iwctl
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect "nama_jaringan"
```
```
exit
```
## tes jaringan

```
ping 8.8.8.8
```

# Partisi

## Membagi Partisi
```
cfdisk /dev/[partisi]
```
### Minimal Partisi
#### **Menyesuaikan dengan penyimpanan yang ada**
```
boot = 4G [EFI System]
System = 44.8G [linux filesystem]
```

# Setup LUKS

```
cryptsetup luksFormat --sector-size 4096 /dev/partisi_sysytem
```
```
crypsetup luksOpen /de/partisi_system [ayam]
```

```
pvecreate /dev/mapper/[ayam]
```
```
vgcreate [pitik] /dev/maapper[ayam]
```

# SETUP LVM

### root
```
lvcreate -L 15G pitik -n root
```
```
mkfs.ext4 /dev/pitik/root
```
```
mount /dev/pitik/root /mnt
```
### boot
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
```
### vars

```
lvcreate -L 10G pitik -n vars
```
```
mkfs.ext4 /dev/pitik/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pitik/vars /mnt/var
```
### vtmp

```
lvcreate -L 1.5G pitik -n vtmp
```
```
mkfs.ext4 /dev/pitik/vtmp
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pitik/vtmp /mnt/var/tmp
```

### vlog
```
lvcreate  -L 1.5G pitik -n vlog
```
```
mkfs.ext4 /dev/pitik/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pitik/vlog /mnt/var/log
```
### vaud
```
lvcreate -L 1G pitik -n vaud
```
```
mkfs.ext4 /dev/pitik/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pitik/vaud /mnt/var/log/audit
```
### dock
```
lvcreate -L 8G pitik -n dock
```
```
mkfs.ext4 /dev/pitik/dock
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pitik/dock /mnt/var/lib/docker
```


### home
```
lvcreate -L 5G pitik -n home
```
```
mkfs.ext4 /dev/pitik/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/home /mnt/home
```

# INSTALL PACKAGE
```
lspci
```
> untuk melihat jenis hardware

```
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema linux-firmware-realtek firewalld
```
# regist partisi
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
# chroot
```
arch-chroot /mnt
```
## buat hostname
```
echo "amel" > /etc/hostname
```
## time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
> cari en_US lalu uncommenting keduanya
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> di line paling atas ganti 'C' menjadi en_US
> di line paling bawah tambahkan en_US.UTF-8
## user
```
useradd -m 'nama_user'
```
```
passwd 'nama_user'
```
```
echo "nama_user ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/nama_user
```
## kernel parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi_luks])=creamy root=/dev/pudding/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
## mkinitcpio
```
cd /boot
```
```
mkdir kernel efi
```
```
cd efi
```
```
mkdir linux
```
```
cd ..
```
```
mv vmlinuz-linux-lts intel-ucode.img kernel
```
```
rm -fr initramfs-linux-hardened.img
```
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> tambahkan lvm2 dan sd-encrypt setelah sd-vcondole di hooks paling bawah

```
nvim /etc/mkinitcpio.d/linuxx-lts-preset
```
samakan dengan ini
```
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.d/default.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/boot/efi/linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"
```
```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
```
systemctl enable iwd
```
```
systemctl enable systemd-networkd.socket
```
```
systemctl enable systemd-resolved
```
# finish installation
```
exit
```
```
umount -R /mnt
```
```
reboot
```
---

# after installation

## network
```
nvim /etc/iwd/main.conf
```
> tambahkan
```
[General]
EnableNetworkConfiguration=true
```

## disable module

```
nvim /etc/modprobe.d/hardening.conf
```

> isi

```
install    cramfs           /bin/false


install    freexfs          /bin/false


install    hfs              /bin/false


install    hfsplus          /bin/false


install    jffs2            /bin/false


install    udf              /bin/false


install    fire-wire-core   /bin/false


install    usb_storage      /bin/false

```
selanjutnya ketik : 
```
mkinitcpio -P
```
cek list module :
```
lsmod
```
```
lsmod | grep namamodule
```

## setup firewalld

```
systemctl enable --now firewalld
```
```
sudo firewall-cmd --zone=public --add-service=http --permanent 
```
```
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=7946/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=4789/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=8000/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=5432/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
```
```
sudo firewall-cmd --reload
```
Selanjutnya, untuk melihat list-list port dan sistem yang sudah di enable ketik:
```
sudo firewall-cmd --list-ports
```

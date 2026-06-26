# KONEK WIFI

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
ping google.com
```

#  PARTISI

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```
### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 3G [EFI system]
system = 50G [linux filesystem]
```
## setup lvm 

### root
```
lvcreate -L 15G pudding -n root
```
```
mkfs.ext4 /dev/pudding/root
```
```
mount /dev/pudding/root /mnt
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
lvcreate -L 15G pudding -n vars
```
```
mkfs.ext4 /dev/pudding/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/vars /mnt/var
```

### vtemp
```
lvcreate -L 1G pudding -n vtemp
```
```
mkfs.ext4 /dev/pudding/vtemp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vtemp /mnt/var/tmp
```

### vlog
```
lvcreate  -L 1G pudding -n vlog
```
```
mkfs.ext4 /dev/pudding/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vlog /mnt/var/log
```

### vaud
```
lvcreate -L 1G pudding -n vaud
```
```
mkfs.ext4 /dev/pudding/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/pudding/vaud /mnt/var/log/audit
```

### podman
```
lvcreate -L 10G pudding -n podman
```
```
mkfs.ext4 /dev/pudding/dock
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/podman /mnt/var/lib/containers
```

### home
```
lvcreate -l70%FREE pudding -n home
```
```
mkfs.ext4 /dev/pudding/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/pudding/home /mnt/home
```

## setup luks
```
cryptsetup luksFormat /dev/pudding/home
```

## install package
```
pacstrap /mnt intel-ucode base pacman sudo linux-hardened linux-hardened-headers lvm2 mkinitcpio  docker neovim git iwd asciinema linux-firmware firewalld podman podman compose nginx
```

## regist partisi
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
## chroot
```
arch-chroot /mnt
```

## buat hostname
```
echo "hostnameanda" > /etc/hostname
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


## LUKS OPEN

```
cryptsetup luksOpen /dev/[pdding)/[home] [kudalepas]
```
```
mkfs.ext4 /dev/mapper/[kudalepas]
```
## HOME USER DIRECTORY

```
mkdir /home/user
```

```
useradd -d /home/user (nama user)
```

```
chown -R (nama user):(nama user) /home/user
```

```
passwd (nama user)
```

```
echo "(nama user) ALL=(ALL:ALL)" > /etc/sudoers.d/(nama user)
```

## config volume
```
nvim /etc/security/pam_mount.conf.xml
```
> samakan
```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE pam_mount SYSTEM "pam_mount.conf.xml.dtd">
<!--
	See pam_mount.conf(5) for a description.
-->

<pam_mount>

		<!-- debug should come before everything else,
		since this file is still processed in a single pass
		from top-to-bottom -->

<debug enable="0" />

		<!-- Volume definitions -->


		<!-- pam_mount parameters: General tunables -->

<!--
<luserconf name=".pam_mount.conf.xml" />
-->

<!-- Note that commenting out mntoptions will give you the defaults.
     You will need to explicitly initialize it with the empty string
     to reset the defaults to nothing. -->
<mntoptions allow="nosuid,nodev,loop,encryption,fsck,nonempty,allow_root,allow_other" />
<!--
<mntoptions deny="suid,dev" />
<mntoptions allow="*" />
<mntoptions deny="*" />
-->
<mntoptions require="nosuid,nodev" />

<!-- requires ofl from hxtools to be present -->
<logout wait="0" hup="no" term="no" kill="no" />

<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/[pudding]/[name]" 
    mountpoint="/home/user]" 
/>
		<!-- pam_mount parameters: Volume-related -->

<mkmountpoint enable="1" remove="true" />


</pam_mount>
```

## system_login
```
nvim /etc/pam.d/system-login
```
> samakan

```
#%PAM-1.0

auth       required   pam_shells.so
auth       requisite  pam_nologin.so
auth       include    system-auth
auth       required   pam_mount.so

account    required   pam_access.so
account    required   pam_nologin.so
account    include    system-auth

password   include    system-auth

session    optional   pam_loginuid.so
session    optional   pam_keyinit.so       force revoke
session    include    system-auth
session    optional   pam_lastlog2.so      silent
session    optional   pam_motd.so
session    optional   pam_mail.so          dir=/var/spool/mail standard quiet
session    optional   pam_umask.so
session    optional  pam_mount.so
-session   optional   pam_systemd.so
session    required   pam_env.so
```

## SYSTEMD

```
mkdir /etc/cmdline.d
```
```
nvim /etc/cmdline.d/boot.conf
```
> isi

```
root=/dev/[nama_vg]/[nama_lv] rw
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> tambahkan lvm2 di hooks paling bawah setelah sd-vconsole


## mkinitcpio
```
nvim /etc/mkinitcpio.d/linuxx-hardened-preset
```
> samakan
```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
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

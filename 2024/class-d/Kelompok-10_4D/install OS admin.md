# Instalasi OS admin

## Kelompok 10
1. Kenari Namora Simanjuntak
2. Luthfah Nurussalamah
3. Toyibatul Nurambiya

### Merekam Asciinema

```
asciinema rec nama_file.cast
```

### Connect Wifi

```
iwctl
```

```
device list
```

**Cek driver wifi setiap laptop**

```
station (driver wifi) get-networks
```

**Melihat jaringan yang tersedia**

```
station (driver wifi) scan
```

**Memindai jaringan yang ada**

```
station {device wifi} connect "{nama wifi}"
```

```
exit
```

**Menghubungkan ke jaringan yang sudah ditentukan**

## Periksa Jaringan 

```
ping 1.1.1.1
```

## Check Partisi
### jika ingin melihat partisi beserta type nya

```
lsblk -o name,fstype,size
```

```
lsblk
```

## Bagi Partisi

```
cfdisk /dev/[partisi]
``` 

**untuk membentuk layout yg mau di install**

## Minimal Partisi
### menyesuaikan dengan penyimpanan yang ada
```
boot = 1G [EFI system]
root = 49G [linux filesystem/]
```
```
lsblk
```

## Setup LVM

```
pvcreate /dev/[partisi root]
```

```
vgcreate proc /dev/[partisi root]
```

## Membuat Logical Volume 

```
lvcreate -L 10G proc -n root
```

```
lvcreate -L 10G proc -n vars
```

```
lvcreate -L 1G proc -n vtmp
```

```
lvcreate -L 1G proc -n vlog
```

```
lvcreate -L 1G proc -n vaud
```

```
lvcreate -L 1G proc -n home
```

## Formating

```
mkfs.ext4 /dev/proc/root
```

```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
```

```
mkfs.ext4 /dev/proc/vars
```

```
mkfs.ext4 /dev/proc/vtmp
```

```
mkfs.ext4 /dev/proc/vlog
```

```
mkfs.ext4 /dev/proc/vaud
```

```
mkfs.ext4 /dev/proc/home
```

## Mounting

```
mount /dev/proc/root /mnt
```

```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

```
mount --mkdir -o rw,nosuid,relatime /dev/proc/home /mnt/home
```

## Setup LUKS

```
lvcreate -l50%FREE proc -n [nama]
```

```
cryptsetup luksFormat /dev/proc/[nama]
```

## Install Package 

### intel

```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

### amd

```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

## Fstab

```
genfstab -U /mnt > /mnt/etc/fstab
```

## Formating tmpfs ke tmp

```
echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab
```

## chroot

```
arch-chroot /mnt
```

```
echo [nama] > /etc/hostname
```

## Localtime

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

```
hwclock --systohc
```

## Locale

```
nvim /etc/locale.gen
```
**di pencarian nvim menggunakan `/`**

```
uncommenting kedua en_US
```

### Generate bahasa

```
locale-gen
```

```
locale > /etc/locale.conf
```

### config locale

```
nvim /etc/locale.conf
```

### config file locale 

```
isi lang=C menjadi lang=en_US.UTF-8 dan isi ALL=en_US.UTF-8
```

## pam_mount

```
cryptsetup luksOpen /dev/proc/miya [nama device]
```

```
mkfs.ext4 /dev/mapper [nama device]
```

## Membuat user

```
mkdir /home/user
```

```
useradd -d /home/user mia
```

```
passwd mia
```

```
chown -R [username]:[username] /home/user
```

```
passwd
```

```
echo "mia ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/none
```

## Config volume

```
nvim /etc/security/pam_mount.conf.xml
```
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
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[name]" 
    mountpoint="/home/user]" 
/>
		<!-- pam_mount parameters: Volume-related -->

<mkmountpoint enable="1" remove="true" />


</pam_mount>
```
```
<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[user name]" 
    mountpoint="/home/[user name]" 
/>
```

## Mengupdate konfigurasi pam_mount

```
nvim /etc/pam.d/system-login
```

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

## Booster

```
nvim /etc/booster.yaml
```

```
network:
  dhcp: on
universal: false
modules: -*,ext4 
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```

## Prepare Boot

```
cd /boot
``` 

untuk cek kernel 

```
ls /usr/lib/modules
```

```
booster build --kernel-version <version> /boot/booster-linux-lts-new.img
```

```
rm -fr booster-linux-lts.img
```

## systemd-boot

```
bootctl --path=/boot install
``` 

```
nvim /boot/loader/entries/booster.conf
``` 

```
title    arch with booster
linux    /vmlinuz-linux-lts
initrd   /intel-ucode.img
initrd   /booster-linux-lts-new.img
options  root=/dev/proc/root rw
```

```
nvim /boot/loader/loader.conf
``` 

add value
```
default  booster.conf
``` 
lalu
```
esc
```
```
:wq
``` 

```
bootctl --graceful update
``` 

## Booting

```
exit
``` 

```
umount -R /mnt
```

#### Hentikan asciinema

```
CTRL+D
```

```
asciinema upload nama_file.cast
```

```
reboot
```



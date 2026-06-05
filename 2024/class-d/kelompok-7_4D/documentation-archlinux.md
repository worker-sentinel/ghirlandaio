# CONNECT WIFI

```bash
iwctl
```
---
```bash
device list
```
#### Catatan : Untuk cek driver wifi setiap laptop
---
```bash
station (driver wifi) get-network
```
#### Catatan : untuk melihat jaringan yang tersedia
---
```bash
station (driver wifi) scan
```
#### Catatan : Untuk memindai jaringan yang ada
---
```bash
station {device wifi} connect "{nama wifi}"
```
```
exit
```
#### Catatan : Untuk menghubungkan ke jaringan yang sudah ditentukan

# Memeriksa jaringan 

```bash
ping 1.1.1.1
```
# jika sudah masuk kedalam install arch linux nya langsung ikutin langkah dibawah


****

# CHECKING PARTISI
## jika ingin melihat partisi beserta type nya
```
lsblk -o name,fstype,size 
```
## Jika ingin melihat partisinya saja
```
lsblk
```

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```

### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 1G [EFI system]
root = 49G [linux filesystem/]
```

#### kalo salah satu partisi PENTING ke hapus, langsung QUIT aja jangan di WRITE

```
lsblk (lagi)
```
****
# partition luks on lvm with disk layout CIS
> adalah partisi yang di luks dan didalamnya terdapat lvm
## setup lvm
```
pvcreate /dev/[partisi root]
```
```
vgcreate proc /dev/[partisi root]
```

### create logical volume
```
lvcreate -L size (G | M) proc -n root
```
```
lvcreate -L size (G | M) proc -n vars
```
```
lvcreate -L size (G | M) proc -n vtmp
```
```
lvcreate -L size (G | M) proc -n vlog
```
```
lvcreate -L size (G | M) proc -n vaud
```
```
lvcreate -L size (G | M) proc -n home
```

## formating 
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
mount --mkdir -o uid=0,gid=0,dmask=0077,fmask=0077 /dev/paritition /mnt/boot
```

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
```

## setup luks
```
lvcreate -l50%FREE  proc -n [name]
```
```
cryptsetup luksFormat /dev/proc/[name]
```

# packages
## intel
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

## amd
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```


## fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## formating tmpfs ke tmp
```
echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab
```

## chroot
```
arch-chroot /mnt
```

## jika 1 kata tidak perlu pake `""` kalo lebih menggunakan petik `""`
```
echo [nama komputer] > /etc/hostname
```

## LOCALTIME
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
****
## LOCALE

```
nvim /etc/locale.gen
```

## lalu pencarian di nvim menggunakan `/`

```
lalu uncommenting kedua en_US
```

### generate bahasa yg di uncommenting 
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
isi lang=C menjadi lang=en_US.UTF-8
dan isi ALL=en_US.UTF-8
```
****

# pam_mount
## buka luks nya
```
cryptsetup luksOpen /dev/proc/ikhsan [nama device]
```
```
mkfs.ext4 /dev/mapper/[nama device]
```
## USERADD
```
mkdir /home/user
```
```
useradd -d  /home/user [user name]
passwd [user name]
```
```
chown -R [user name]:[user name] /home/user
```
```
passwd
```
> password must same like luks for this partition

```
echo '[user name] ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
```

## config volume
```
nvim /etc/security/pam_mount.conf.xml
```
> sama kan dengan code yang dibawah
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
    path="/dev/proc/[name]" 
    mountpoint="/home/user]" 
/>
		<!-- pam_mount parameters: Volume-related -->

<mkmountpoint enable="1" remove="true" />


</pam_mount>
```

> yang harus diperhatikan pada seluruh config
```
<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[user name]" 
    mountpoint="/home/[user name]" 
/>
```

## update konfigurasi pam_mount
```
nvim /etc/pam.d/system-login
```

> sama kan dengan code yang dibawah
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
> untuk memberitahu kepada sistem untuk menggunakan `pam_mount` saat login prosess

## BOOSTER
```
nvim /etc/booster.yaml
```

add value
```
network:
  dhcp: on
universal: false
modules: -*,ext4
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```

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
### Jika gagal muncul di boot option kalian keluar terlebih dahulu dari arch-chroot
```
exit
```
```
bootctl --path=/mnt/boot install
```
---
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

```
bootctl --graceful update 
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

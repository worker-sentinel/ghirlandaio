

## Menghubungkan ke internet

```bash
iwctl
```

lalu ketik

```bash
station wlan0 connect "namawifi"
```

lalu cek

```bash
ping 8.8.8.8
```

jika muncul reply dari `8.8.8.8` berarti internet sudah terhubung.

## membagi partisi


setelah itu
## Membuat Physical Volume

Partisi yang paling besar digunakan sebagai Physical Volume.

```bash
pvcreate /dev/sdx
```


## Membuat Volume Group

Volume Group Bernama `system`

```bash
vgcreate system /dev/sdax
```

## Membuat Logical Volume

### Root
Fungsi: menyimpan file konfigurasi dan package Linux.

```bash
lvcreate -L <SIZE_G|M> system -n root
```

### Var
Fungsi: menyimpan data dinamis yang bisa dibuat, dimodifikasi, atau dihapus.

```bash
lvcreate -L <SIZE_G|M> system -n vars
```

### Var Log
Fungsi: menyimpan log jurnal dan aktivitas aplikasi Linux.

```bash
lvcreate -L <SIZE_G|M> system -n vlog
```

Contoh:
```bash
lvcreate -L 1G system -n vlog
```

### Var Audit
Fungsi: mencatat log yang bersifat kritikal dan aktivitas kernel.

```bash
lvcreate -L <SIZE_G|M> system -n vaud
```

Contoh:
```bash
lvcreate -L 512M system -n vaud
```

### Var Tmp
Fungsi: menyimpan data sementara yang tetap ada setelah reboot.

```bash
lvcreate -L <SIZE_G|M> system -n vtmp
```

Contoh:
```bash
lvcreate -L 4G system -n vtmp
```

### Home Public
Fungsi: menyimpan data user seperti PDF, gambar, foto, dan dokumen umum.

```bash
lvcreate -L <SIZE_G|M> system -n home
```

### Home Pribadi
Menggunakan sisa ruang kosong.

```bash
lvcreate -l 50%FREE system -n crab
```

# Format Partisi

## EFI System Partition

Partisi EFI menggunakan filesystem FAT32.

```bash
mkfs.vfat -F32 /dev/sda5
```

Keterangan:
- `mkfs` = membuat filesystem
- `vfat` = FAT32
- digunakan oleh UEFI


## Format Logical Volume

### Root

```bash
mkfs.ext4 /dev/system/root
```

### Var

```bash
mkfs.ext4 /dev/system/vars
```

### Var Log

```bash
mkfs.ext4 /dev/system/vlog
```

### Var Audit

```bash
mkfs.ext4 /dev/system/vaud
```

### Var Tmp

```bash
mkfs.ext4 /dev/system/vtmp
```

### Home Public

```bash
mkfs.ext4 /dev/system/home
```

## Enkripsi Home Pribadi

```bash
cryptsetup luksFormat /dev/system/crab
```

Jawab:

```text
YES
```

Masukkan password dan verifikasi 2x.

# Mounting Filesystem

## Root

```bash
mount /dev/system/root /mnt
```


## EFI

```bash
mount --mkdir \
-o uid=0,gid=0,fmask=0077,dmask=0077 \
/dev/sda5 /mnt/boot
```


## Var Log

```bash
mount --mkdir \
-o rw,nodev,noexec,nosuid \
/dev/system/vlog /mnt/var/log
```


## Var Audit

```bash
mount --mkdir \
-o rw,nodev,noexec,nosuid \
/dev/system/vaud /mnt/var/log/audit
```


## Var Tmp

```bash
mount --mkdir \
-o rw,nodev,noexec,nosuid \
/dev/system/vtmp /mnt/var/tmp
```


## Home Public

```bash
mount --mkdir \
-o rw,nodev,noexec,nosuid \
/dev/system/home /mnt/home
```

Partisi `crab` tidak di-mount karena akan dibuka via `pam_mount`.

# Instalasi Package

```bash
pacstrap /mnt base base-devel 
intel-ucode linux-lts linux-firmware linux-lts-headers lvm2 neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

# Generate FSTAB

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

Tambahkan tmpfs:

```bash
echo "tmpfs /tmp tmpfs defaults,nodev,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```

# Masuk Chroot

```bash
arch-chroot /mnt
```

# Hostname

```bash
nvim /etc/hostname
```

# Timezone

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

# Locale

```bash
nvim /etc/locale.gen
```

Aktifkan:
```text
en_US.UTF-8 UTF-8
```

```bash
locale-gen
```

```bash
locale > /etc/locale.conf
```

# Setup Home Terenkripsi

```bash
cryptsetup luksOpen /dev/system/crab capit
mkfs.ext4 /dev/mapper/capit
```

```bash
mkdir /home/user
useradd -d /home/user mutia
chown -R mutia:mutia /home/user
passwd ******
```

```bash
echo "mutia ALL=(ALL:ALL) ALL" > /etc/sudoers.d/mutia
```

# PAM Mount

```
nvim /etc/security/pam_mount.conf.xml
```
```
<logout wait="0" hup="no" term="no" kill="no" />
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[name]" 
    mountpoint="/home/user]" 
/>
```
```
nvim /etc/pam.d/system-login
```
```
nvim /etc/pam.d/system-login
```
```
> sama kan dengan code yang dibawah
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

# Booster

```bash
nvim /etc/booster.yaml
ls /usr/lib/modules
booster build --kernel-version 6.18.32-2-lts /boot/booster-linux-lts-new.img
rm -fr booster-linux-lts.img
```

# systemd-boot

```bash
bootctl --path=/boot install
bootctl --graceful
```
# Boot Entry

```bash
nvim /boot/loader/entries/booster.conf
```

```text
title archlinux with booster
linux /vmlinuz-linux-lts
initrd /intel-ucode.img
initrd /booster-linux-lts-new.img
options root=/dev/system/root rw
```

# Loader Config

```bash
nvim /boot/loader/loader.conf
```

```text
default booster.conf
```

# Finish

```bash
exit
umount -R /mnt
reboot
```
```

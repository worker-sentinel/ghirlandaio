# Dokumentasi Instalasi OS Server App

## 1. Menghubungkan ke Wi-Fi
```
iwctl
```
```
device list
```
Cek driver wifi setiap laptop
```
station wlan0 get-network
```
Melihat jaringan yang tersedia
```
station wlan0 scan
```
Memindai jaringan yang ada
```
station wlan0 connect "(nama wifi)"
exit
```
Memastikan koneksi internet aktif
```bash
ping 8.8.8.8
```

---

## 2. Mengelola Partisi Disk

```
lsblk
```
Membagi partisi
```
cfdisk /dev/partisi [sda/nvme0n1p1]
```
Minimal partisi
```
boot = 3G [EFI system}
root = 70G [Linux filesystem]
```

## 3. Membuat LVM (Logical Volume Manager)

### Membuat Physical Volume

```bash
pvcreate /dev/nvme0n1p7
```

### Membuat Volume Group

```bash
vgcreate wc /dev/nvme0n1p7
```

### Membuat Logical Volume

```bash
lvcreate -L 5G -n root wc 
lvcreate -L 5G -n vars wc 
lvcreate -L 1G -n vlog wc 
lvcreate -L 1G -n vaud wc 
lvcreate -L 1G -n home wc 
lvcreate -L 1.5G -n vtmp wc 
lvcreate -L 9G -n podman wc 
lvcreate -l50%FREE -n ngapa wc
```
### Verifikasi Logical Volume
```bash
lsblk
```

## 4. Membuat Filesystem

Memformat logical volume yang telah dibuat:

```bash
mkfs.ext4 /dev/wc/root 
mkfs.ext4 /dev/wc/vars 
mkfs.ext4 /dev/wc/vlog 
mkfs.ext4 /dev/wc/vaud 
mkfs.ext4 /dev/wc/home 
mkfs.ext4 /dev/wc/vtmp 
mkfs.ext4 /dev/wc/podman     
```

## 5. Konfigurasi LUKS
```bash
cryptsetup luksFormat /dev/wc/ngapa
```
```bash
cryptsetup luksOpen /dev/wc/ngapa ngape   
```
```bash
mkfs.ext4 /dev/mapper/ngape
```
## 6. Mount Partisi

### Mount Root
```bash
mount /dev/wc/root /mnt
```
### Mount Filesystem
```bash
mount /dev/wc/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot   
mount --mkdir -o rw,nodev,nosuid,relatime /dev/wc/vars /mnt/var
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vlog /mnt/var/log
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/home /mnt/home
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/podman /mnt/var/lib/containers
```
       
### Verifikasi Mount

```bash
lsblk
```

## 7. Install Package 

```bash
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 openssh firewalld podman pacman sudo wget git neovim iwd grep which pam_mount     
```
## 8. Genfstab
```bash
genfstab -U /mnt > /mnt/etc/fstab
```
## 9. Mounting RAM
```bash
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
## 10. Konfigurasi Sistem

Masuk ke sistem yang baru diinstal:

```bash
arch-chroot /mnt
```

### Mengatur Hostname

```bash
nvim /etc/hostname
```

### Mengatur Timezone

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

### Mengatur Locale

```bash
nvim /etc/locale.gen
locale-gen
```
hapus # en_US<br>
Locale
```bash
locale-gen
```
```bash
locale > /etc/locale.conf
```
```bash
nvim /etc/locale.conf
```
```
isi lang=C menjadi lang=en_US.UTF-8 dan 
isi ALL=en_US.UTF-8
```
## 11. Membuat User

### Membuat Direktori Home

```bash
mkdir /home/user
```

### Menambahkan User

```bash
useradd -d /home/user auah
```

### Mengatur Kepemilikan Direktori Home

```bash
chown -R auah:auah /home/user
```

### Mengatur Password Root

```bash
passwd
```

### Mengatur Password User

```bash
passwd auah
```

### Memberikan Hak Akses Sudo

```bash
echo "auah ALL=(ALL:ALL) ALL" > /etc/sudoers.d/auah
```
---
## 12. Konfigurasi PAM Mount

Mengedit file konfigurasi PAM Mount:

```bash
nvim /etc/security/pam_mount.conf.xml
```

Menambahkan konfigurasi volume terenkripsi:

```xml
<volume
    user="auah"
    fstype="crypt"
    path="/dev/wc/ngapa"
    mountpoint="/home/user"
/>
```
### Mengedit PAM Login

```bash
nvim /etc/pam.d/system-login
```

Menambahkan modul PAM Mount:

```text
auth       required   pam_mount.so
session    optional   pam_mount.so
```
```
nvim /etc/mkinitcpio.conf
```
>pada hooks paling bawah, tambahkan sd-encrypt lvm2 di samping sd-vconsole

## 13. Menyalin Konfigurasi Jaringan

Keluar dari lingkungan chroot:

```bash
exit
```

Menyalin konfigurasi jaringan ke sistem yang baru diinstal:

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
Masuk kembali ke chroot:

```bash
arch-chroot /mnt
```
Mengedit preset kernel hardened:

```bash
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
Mengubah konfigurasi preset menjadi:

```text
PRESETS=('default')
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
```
### 14. Instalasi Bootloader (systemd-boot)

Menginstal systemd-boot:

```bash
bootctl --path=/boot install
```

Keluar dari chroot:

```bash
exit
```

Menginstal systemd-boot pada sistem yang telah dipasang:

```bash
bootctl --path=/mnt/boot install
```
## 15. Konfigurasi Kernel Command Line

Masuk kembali ke sistem:

```bash
arch-chroot /mnt
```

Mengedit konfigurasi kernel command line:

```bash
nvim /etc/cmdline.conf
```
>Isi file:
root=/dev/wc/root rw
```bash
mkinitcpio -P
```
Menghapus file cmdline lama:

```bash
rm -fr /etc/cmdline.conf
```

Membuat direktori cmdline:

```bash
mkdir /etc/cmdline.d
```

Membuat file konfigurasi boot:

```bash
nvim /etc/cmdline.d/01-boot.conf
```
>Isi file:
>root=/dev/wc/root rw


Jalankan lagi
```bash
mkinitcpio -P
```
## 16. Mengaktifkan Service

Mengaktifkan layanan Wi-Fi:

```bash
systemctl enable iwd
```

Mengaktifkan layanan jaringan:

```bash
systemctl enable systemd-networkd
```

Mengaktifkan DNS resolver:

```bash
systemctl enable systemd-resolved
```

Mengaktifkan firewall:

```bash
systemctl enable firewalld
```

Mengaktifkan SSH daemon:

```bash
systemctl enable sshd
```

Mengaktifkan Podman

```bash
systemctl enable --global podman
```
## 17. Menyelesaikan Instalasi

Keluar dari lingkungan chroot:

```bash
exit
```

Melepas seluruh mount point:

```bash
umount -R /mnt
```

Reboot sistem:

```bash
reboot
```

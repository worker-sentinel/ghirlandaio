# Dokumentasi Instalasi OS Blackbird

---
## CONNECT WIFI
Sambungkan koneksi wifi
```
iwctl
```

cek wifi
```
device list
```

Menyambungkan wifi
```
station wlan0 connect (nama wifi)
```

```
exit
```

Periksa koneksi apa sudah tersambung
```
ping 8.8.8.8
```

---
## MEMBUAT PARTISI
Pembagian Partisi
```
cfdisk /dev/(partisi)
```

Minimal partisi disesuaikan dengan penyimpanan
```
boot = 4G (EFI system)
root = 44.8G (Linux filesystem)
```

Melihat Partisi yang telah dibuat
```
lsblk
```

---
## Enkripsi dengan LUKS
Untuk menyiapkan partisi yang akan dienskripsi.
```
cryptsetup luksFormat /dev/(partisi root)
```

Membuka dan membuat perangkat pemetaan perangkat logis, yang dipasang ke partisi terenkripsi LUKS.
```
cryptsetup luksOpen /dev/(partisi root) (nama pemetaan)
```

---
## Konfigurasi LVM
**Menyiapkan Physical volume untuk membuat Volume Group**
```
pvcreate /dev/mapper/(nama pemetaan)
```
```
vgcreate (nama grup) /dev/mapper/(nama pemetaan)
```
```
lsblk
```

**Membuat Logical Volume root**

Pada langkah ini, sudah termasuk format dan mounting
```
lvcreate -L (G|M) (nama grup) -n root
```
```
mkfs.ext4 /dev/(nama grup)/root
```
Format partisi boot
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
```
Mounting partisi grup root
```
mount /dev/(nama grup)/root /mnt
```
Mounting partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi boot) /mnt/boot
```

**Membuat logical volume**

```
lvcreate -L (G|M) (nama grup) -n var
```
```
lvcreate -L (G|M) (nama grup) -n vlog
```
```
lvcreate -L (G|M) (nama grup) -n vaud
```
```
lvcreate -L (G|M) (nama grup) -n vtmp
```
```
lvcreate -L (G|M) (nama grup) -n home
```
```
lvcreate -L (G|M) (nama grup) -n dock
```

---
## Format Partisi

Format pasrtisi grup logical volume
```
mkfs.ext4 /dev/(nama grup)/var
```
```
mkfs.ext4 /dev/(nama grup)/vlog
```
```
mkfs.ext4 /dev/(nama grup)/vaud
```
```
mkfs.ext4 /dev/(nama grup)/home
```
```
mkfs.ext4 /dev/(nama grup)/dock
```

---
## Mounting Partisi
Mounting partisi grup logical volume
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/var /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/log/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/dock /mnt/var/lib/docker
```
```
lsblk
```

---
## Install Packages
> Instalasi package disesuaikan dengan prosesor laptop\

**AMD**
```
pacstrap /mnt linux-hardened linux-hardened-headers lvm2 mkinitcpio linux-firmware-amdgpu linux-firmware-realtek linux-firmware-atheros git neovim less amd-ucode firewalld pacman sudo iwd bash iputils iproute2
```
**Intel**
```
pacstrap /mnt linux-hardened linux-hardened-headers lvm2 mkinitcpio linux-firmware-intel linux-firmware-realtek linux-firmware-atheros git neovim less intel-ucode firewalld pacman sudo iwd bash iputils iproute2
```

---
## File System Table (fstab)

```
genfstab -U /mnt > /mnt/etc/fstab
```
```
cat /mnt/etc/fstab
```

---
## Membuat hostname, set localtime dan locale
**Masuk ke dalam chroot**

```
arch-chroot /mnt
```
```
nvim /etc/hostname
```
> masukkan hostname (bebas apa saja), lalu klik esc dan ketik :wq
```
cat /etc/hostname
```

**Set Localtime**
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
> hapus tanda pagar di en_US, setelah itu esc :wq lagi

Generate bahaasa
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> Ubah LANG=C menjadi LANG=en_US dan LC_ALL= menjadi LC_ALL=en_US.UTF-8

---
## Home User Directory
```
mkdir -p /home/user
```
```
useradd -d /home/user (nama user)
```
```
passwd
```
```
chown -R (nama user):(nama user) /home/user
```
```
passwd (nama user)
```
```
echo '(nama user) ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/(nama user)
```
```
cat /etc/sudoers.d/(nama user)
```

---
## Konfigurasi Bootloader
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
nvim /etc/cmdline.d/01-boot.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/(partisi root))=(nama user) root=/dev/(nama grup)/root" > /etc/cmdline.d/01-boot.conf
```
```
nvim /etc/cmdline.d/06-misc.conf
```
> ketik rw

Konfigurasi mkinitcpio
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> cari hingga HOOKS yang tidak ada lambang pagar
> samakan seperti ini, HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)

**Menyiapkan layout direktori UKI**

Berpindah ke direktori Boot
```
cd /boot
```
Pengecekan file Booting
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
mv vmlinuz-linux-hardened amd-ucode.img kernel/
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
**Mengkonfigurasi Preset Mkinitcpio untuk UKI**
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
Samakan dengann ini
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

---
## Systemd boot
```
bootctl --path=/boot install
```
```
lsblk -o name,uuid
```
```
cat /etc/cmdline.d/01-boot.conf
```
```
ls efi/linux
```
untuk mengecek apakah systemd boot telah terpasang
```
bootctl status
```

---
## Booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```

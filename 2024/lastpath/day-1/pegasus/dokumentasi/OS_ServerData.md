
## MEMBUAT PARTISI
Pembagian Partisi
```
cfdisk /dev/(partisi)
```

disesuaikan dengan penyimpanan
```
boot = 3G (EFI system)
root = 55.6G (Linux filesystem)
```

## Melihat Partisi yang sudah dibuat
```
lsblk
```

---
## Enkripsi dengan LUKS
Untuk menyiapkan partisi yang akan dienskripsi.
```
cryptsetup luksFormat /dev/sda6
```
```
cryptsetup luksOpen /dev/sda6 data_pegasus
```

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

Sambung wifi
```
station wlan0 connect nama wifi
```

```
exit
```

## Konfigurasi LVM
**Menyiapkan Physical volume untuk membuat Volume Group**
```
pvcreate /dev/mapper/data_pegasus
```
```
vgcreate dhea /dev/mapper/data_pegasus
```
```
lsblk
```

**Membuat Logical Volume root**

Pada langkah ini, sudah termasuk format dan mounting
```
lvcreate -L 10G dhea -n root
```
```
mkfs.ext4 /dev/dhea/root
```
Format partisi boot
```
mkfs.vfat -F32 -n BOOT /dev/sda5
```
```
mount /dev/dhea/root /mnt
```
Mounting partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot
```

**Membuat logical volume**

```
lvcreate -L 7G dhea -n var
```
```
lvcreate -L 1G dhea -n vlog
```
```
lvcreate -L 1G dhea -n vaud
```
```
lvcreate -L 1G dhea -n vtmp
```
```
lvcreate -L 8G dhea -n home
```
```
lvcreate -L 12G dhea -n podman
```

## Format Partisi

Format pasrtisi grup logical volume
```
mkfs.ext4 /dev/dhea/var
```
```
mkfs.ext4 /dev/dhea/vlog
```
```
mkfs.ext4 /dev/dhea/vaud
```
```
mkfs.ext4 /dev/dhea/home
```
```
mkfs.ext4 /dev/dhea/podman
```

## Mounting Partisi
Mounting partisi grup logical volume
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/var /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/vtmp /mnt/var/log/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/dhea/podman /mnt/var/lib/containers
```
```
lsblk
```

## Install Package
```
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware lvm2 mkinitcpio sudo curl neovim iwd firewalld pacman podman
```
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size-1G 0 0" >> /mnt/etc/fstab
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
masukkan hostname, lalu klik esc dan ketik :wq
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

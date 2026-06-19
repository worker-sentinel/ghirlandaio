# Install Arch Linux LTS + LUKS + LVM (Amanda Blackbird)

## 1. Rekam Instalasi (Opsional)

```bash
asciinema rec nama.cast
```

---

## 2. Koneksi Wi-Fi

```bash
iwctl
```

```bash
station wlan0 get-networks
```

```bash
station wlan0 connect nama_wifi
```

Masukkan password Wi-Fi lalu:

```bash
exit
```

Cek koneksi:

```bash
ping 1.1.1.1
```

---

## 3. Partisi Disk

```bash
cfdisk /dev/sda
```
buat partisi

Cek:

```bash
lsblk
```

---

## 4. Enkripsi LUKS

```bash
cryptsetup luksFormat /dev/p.root
```

Ketik:

```text
YES
```

Buka LUKS:

```bash
cryptsetup luksOpen /dev/p.root name
```

Masukkan password.

---

## 5. LVM

### Physical Volume

```bash
pvcreate /dev/mapper/name
```

### Volume Group

```bash
vgcreate nama /dev/mapper/nama
```

### Logical Volume

```bash
lvcreate -L MG nama -n root
```

```bash
lvcreate -L MG nama -n vars
```

```bash
lvcreate -L MG nama -n vlog
```

```bash
lvcreate -L MG nama -n vaud
```

```bash
lvcreate -L MG nama -n vtmp
```

```bash
lvcreate -L MG nama -n home
```

```bash
lvcreate -L MG nama -n dock
```

Atau:



Cek:

```bash
lsblk
```

---

## 6. Format Filesystem

```bash
mkfs.ext4 /dev/nama/root
```

```bash
mkfs.vfat -F32 -S4096 -n BOOT /dev/p.boot
```

```bash
mkfs.ext4 /dev/nama/vars
```

```bash
mkfs.ext4 /dev/nama/vlog
```

```bash
mkfs.ext4 /dev/nama/vaud
```

```bash
mkfs.ext4 /dev/nama/vtmp
```

```bash
mkfs.ext4 /dev/nama/home
```


Cek:

```bash
lsblk
```

---

## 7. Mount

```bash
mount /dev/nama/root /mnt
```

```bash
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/p.boot /mnt/boot
```

```bash
mount --mkdir -o rw,nodev,nosuid,relatime /dev/nama/vars /mnt/var
```

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama/vlog /mnt/var/log
```

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama/vaud /mnt/var/log/audit
```

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama/vtmp /mnt/var/tmp
```

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama/home /mnt/home
```


Cek:

```bash
lsblk
```

---

## 8. Install Base System

```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 neovim sudo curl iwd firewalld pacman which grep docker
```

Generate fstab:

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

Salin konfigurasi network:

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

Tambahkan tmpfs:

```bash
echo "tmpfs /tmp tmpfs defaults,nodev,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```

Masuk chroot:

```bash
arch-chroot /mnt
```

---

## 9. Hostname dan Timezone

```bash
echo nama > /etc/hostname
```

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

```bash
hwclock --systohc
```

---

## 10. Locale

Edit:

```bash
nvim /etc/locale.gen
```

Hilangkan `#` pada:

```text
en_US.UTF-8 UTF-8
```

Generate:

```bash
locale-gen
```

Buat locale.conf:

```bash
locale > /etc/locale.conf
```

Edit:

```bash
nvim /etc/locale.conf
```

Isi:

```text
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

---

## 11. User

```bash
useradd -m user
```

```bash
passwd user
```

Tambahkan sudo:

```bash
echo "User ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
```

---

## 12. Kernel Command Line

```bash
mkdir /etc/cmdline.d
```

```bash
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

```bash
ls /etc/cmdline.d/
```

Cek partisi:

```bash
lsblk
```

Isi:

```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda luks)=nama device luks root=/dev/nama/root" > /etc/cmdline.d/01-boot.conf
```

```bash
echo "rw" > /etc/cmdline.d/02-misc.conf
```

---

## 13. MKINITCPIO

Hapus initramfs lama:

```bash
rm -fr /boot/initramfs-linux-lts.img
```

Edit:

```bash
nvim /etc/mkinitcpi.conf
```



Ubah HOOKS menjadi:

```text
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```

---

## 14. Preset Linux LTS

Edit:

```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```

Ubah menjadi:

```text
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"
ALL_kerneldest="/boot/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"
```

---

## 15. Console dan Bootloader

```bash
touch /etc/vconsole.conf
```

```bash
bootctl --path=/boot install
```

Generate initramfs:

```bash
mkinitcpio -P
```

Cek isi boot:

```bash
cd /boot
ls
```

---

## 16. Enable Services

```bash
systemctl enable iwd
```

```bash
systemctl enable systemd-networkd
```

```bash
systemctl enable systemd-resolved
```

```bash
systemctl enable firewalld
```

Keluar dari chroot:

```bash
exit
```

Install bootloader ke target:

```bash
bootctl --path=/mnt/boot install
```

---

## 17. Finish

Unmount:

```bash
umount -R /mnt
```

Keluar:

```bash
exit
```

Upload rekaman:

```bash
asciinema upload nama.cast
```

Reboot:

```bash
reboot
```

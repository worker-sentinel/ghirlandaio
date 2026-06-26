# Install OS APP (Hardened)


## 1. Record

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
station wlan0 connect nama wifi
```

Masukkan password Wi-Fi lalu:

```bash
exit
```

Cek koneksi:

```bash
ping 8.8.8.8
```

---

## 3. Partisi Disk

```bash
cfdisk /dev/nvme0n1
```

buat partisi boot dan root linux filesystem, contoh

```
3G EFI System
27G Linux filsystem
```

Cek:

```bash
lsblk
```

---

## 4. Enkripsi LUKS

```bash
cryptsetup luksFormat –sector-size=4096 /dev/p.root
```

(contoh p.root nya = nvme0n1p6 atau sda4, tergantung laptop masing”)

Ketik:

```text
YES
```


Buka LUKS:

```bash
cryptsetup luksOpen /dev/p.root name
```

Masukkan password

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
lvcreate -L MG nama -n podman
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
mkfs.ext4 /dev/name/vars
```

```bash
mkfs.ext4 /dev/name/vlog
```

```bash
mkfs.ext4 /dev/name/vaud
```

```bash
mkfs.ext4 /dev/name/vtmp
```

```bash
mkfs.ext4 /dev/name/home
```
```bash
mkfs.ext4 /dev/name/podman


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

```bash
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/nama/podman /mnt/var/lib/containers
```


Cek:

```bash
lsblk
```

---

## 8. Install Base System

```bash
pacstrap /mnt intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman
```

Generate fstab:

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

```bash
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

Masuk chroot:

```bash
arch-chroot /mnt
```

---

## 9. Hostname dan Timezone

```bash
nvim /etc/hostname
```

di pengeditan nvim pencet i untuk insert, kemudian masukkan hostname dan esc
```
:wq
```

Setelah itu dia akan balik ke root, untuk mengecek hostname ketik:
```bash
cat /etc/hostname
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
en_US ISO-8859-1
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
echo "user ALL=(ALL:ALL) ALL" > /etc/sudoers.d/user
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

```bash
rm -fr /boot/initramfs-linux-*
```

untuk mengecek buka dengan 

```bash
ls /boot/
```

```bash
mkdir -p /boot/efi /boot/efi/linux /boot/efi/systemd /boot/efi/boot /boot/kernel 
```

untuk mengecek lagi buka dengan 


```bash
ls /boot/
```


```bash
mv /boot/intel-ucode.img /boot/vmlinuz-linux-* /boot/kernel/
```

---

## 13. MKINITCPIO


```bash
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```

Edit:

```bash
nvim /etc/mkinitcpio.d/default.conf
```


Ubah HOOKS menjadi:

```text
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```

---

## 14. Preset Linux Hardened

Edit:

```bash
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

Ubah menjadi:

```text
# mkinitcpio preset file for the 'linux-hardened’ package

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

---

## 16. Enable Services

```bash
systemctl enable systemd-networkd
```

```bash
systemctl enable systemd-resolved
```

```bash
systemctl enable iwd
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

## 17. Umount

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


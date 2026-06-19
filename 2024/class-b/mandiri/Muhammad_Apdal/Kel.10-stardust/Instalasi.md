# Dokumentasi Server

## Memeriksa Partisi

```bash
lsblk
```

## Format LUKS

```bash
cryptsetup luksFormat /dev/nvme0n1p7
```

masukkan password

```bash
cryptsetup luksOpen /dev/nvme0n1p7(partisi root) stardust (nama device)
```

## Setup LVM

```bash
pvcreate /dev/mapper/stardust (nama device)
```

```bash
vgcreate system /dev/mapper/(nama device)
```

## Membuat Logical Volume

```bash
lvcreate -L 10G system -n root
lvcreate -L 10G system -n vars
lvcreate -L 1G system -n vlog
lvcreate -L 1G system -n vaud
lvcreate -L 1G system -n vtmp
lvcreate -L 10G system -n home
lvcreate -L 5G system -n podman
```

## Memformat Partisi

```bash
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p6
mkfs.ext4 /dev/system/root
mkfs.ext4 /dev/system/vars
mkfs.ext4 /dev/system/vlog
mkfs.ext4 /dev/system/vaud
mkfs.ext4 /dev/system/vtmp
mkfs.ext4 /dev/system/home
mkfs.ext4 /dev/system/podman
```

## Cek Partisi

```bash
lsblk
```

## Mounting Partisi

```bash
mount /dev/system/root /mnt

mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/vars /mnt/var

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/var/log

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/var/tmp

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/home /mnt/home

mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/podman /mnt/var/lib/containers
```

## Install Package Arch Linux

```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

## Fstab

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

## Mengatur Koneksi Internet

```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Format tmpfs ke tmp

```bash
echo “tmpfs /tmp tmpfs defaults,rw, nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```


## Masuk ke Dalam Sistem

```bash
arch-chroot /mnt
```

## Membuat Hostname

```bash
echo stardust > etc/hostname
```

## Mengatur Waktu dan Bahasa

```bash
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

```bash
hwclock --systohc
```

## Mengatur Bahasa

```bash
nvim /etc/locale.gen
```

pagarnya apus pada bagian:

```text
en_US.UTF-8 UTF-8
en_US ISO-8859-1
```

```bash
locale-gen
```

```bash
locale > /etc/locale.conf
```

```bash
nvim /etc/locale.conf
```

LANG=C.UTF-8 diganti jadi LANG=en_US.UTF-8 lalu dibagian bawahnya tambahkan LC_ALL=en_US.UTF-8

## Membuat User

```bash
useradd -m stardust (nama grup)
```

```bash
passwd stardust
```

membuat password

```bash
echo "stardust ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/stardust
```

## Mengatur Parameter

```bash
mkdir /etc/cmdline.d
```

```bash
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```

```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=stardust root=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```

```bash
echo "rw" > /etc/cmdline.d/06-misc.conf
```

## Pindah ke Folder Boot

```bash
cd /boot
```

```bash
mkdir kernel efi
```

```bash
cd efi
```

```bash
mkdir linux
```

```bash
cd ..
```

```bash
mv vmlinuz-* intel-* kernel
```

```bash
ls
```

```bash
rm -fr initramfs-linux-lts.img
```

```bash
ls
```

```bash
ls efi/
```

```bash
ls kernel/
```

## Mengatur mkinitcpio

```bash
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```

```bash
nvim /etc/mkinitcpio.d/default.conf
```

(dibagian HOOKS tambahkan sd-vconsole lvm2 sd-encrypt setelah kms keyboard)

mengedit file konfigurasi

```bash
nvim /etc/mkinitcpio.d/linux-lts-preset
```

(EFI ganti boot)

## Menginstall Bootloader

```bash
bootctl --path=/boot install
```

namun karena error yang disebabkan bootnya belum termounting maka disolving terlebih dahulu dengan memounting bootnya lagi yaitu:

```bash
mount /dev/nvme0n1p6 /mnt/boot
```

```bash
arch-chroot /mnt
```

```bash
bootctl --path=/boot install
```

karena eror lagi maka solving dengan:

```bash
ls -l /etc/mkinitcpio.d/default.conf
```

```bash
ls -d /lib/modules/*lts*
```

```bash
ls -la /boot/efi/linux/
```

```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```

lalu pada config bagian:

```text
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
```

isinya diganti menjadi:

```text
6.8.35-l-lts
```

lalu config bagian ini:

```text
ALL_kerneldest="6.8.35-l-lts"
```

isinya diganti menjadi:

```text
/boot/kernel/vmlinuz-linux-lts
```

```bash
mkinitcpio -P
```

## Aktifkan Sistem

```bash
systemctl enable systemd-resolved
systemctl enable systemd-networkd
systemctl enable firewalld
```

setelah itu exit

```bash
exit
```

## Upload Asciinema

```bash
umount -R /mnt
```

lalu reboot

```bash
reboot
```















































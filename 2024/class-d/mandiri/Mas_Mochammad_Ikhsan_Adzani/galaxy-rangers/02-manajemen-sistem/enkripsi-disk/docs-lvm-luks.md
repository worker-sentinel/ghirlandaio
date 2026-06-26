# luks setup
```
cryptsetup luksFormat /dev/[partisi root] 
```

```
cryptsetup luksOpen /dev/[partisi root]  [nama device]
```

# pv & vg setup
```
pvcreaate /dev/mapper/[nama device]
```

```
vgcreate [nama grup] /dev/mapper/[nama device]
```

## logical volume setup
### Partisi Sistem (Root)
```
lvcreate -L [ukuran]G [nama grup] -n root
```
```
mkfs.ext4 /dev/[nama grup/root
```

```
mount /dev/[nama grup]/root /mnt
```

### Partisi Boot (EFI/FAT32)
```
mkfs.vfat -F32 /dev/[partisi boot]
```

```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot /mnt/boot
```

### Partisi Data Variabel (/var)
```
lvcreate -L [ukuran]G [nama grup] -n vars
```
```
mkfs.ext4 /dev/[nama grup/vars
```
```
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
### Partisi Temporary (/var/tmp)
```
lvcreate -L [ukuran]G [nama grup] -n vtmp
```
```
mkfs.ext4 /dev/[nama grup/vtmp
```
```
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```

### Partisi Log Sistem (/var/log)
```
lvcreate -L <ukuran> <nama_grup> -n vlog
```

```
mkfs.ext4 -b 4096 /dev/<nama_grup>/vlog
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/<nama_grup>/vlog /mnt/var/log
```

### Partisi Audit (/var/log/audit)
```
lvcreate -L <ukuran> <nama_grup> -n vaud
```
```
mkfs.ext4 -b 4096 /dev/<nama_grup>/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/<nama_grup>/vaud /mnt/var/log/audit
```
### Partisi Pengguna (/home)
```
lvcreate -l50%FREE <nama_grup> -n home
```
```
mkfs.ext4 -b 4096 /dev/<nama_grup>/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/<nama_grup>/home /mnt/home
```

### Partisi podman (/var/lib/containers)
```
lvcreate -l50%FREE <nama_grup> -n podi
```
```
mkfs.ext4 -b 4096 /dev/group_name/podi
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/podi /mnt/var/lib/containers
```

### Instalasi Paket ke dalam Partisi
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman openssh
```
### Buat Fstab & Tambahkan Tmpfs (RAM Disk)
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

## Masuk ke Lingkungan Sistem Baru
```
arch-chroot /mnt
```
### Identitas Komputer & Waktu
```
echo <nama_komputer> > /etc/hostname
```
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
### Konfigurasi Bahasa (Locale)
```
nvim /etc/locale.gen
```
>  lalu hapus tanda pagar (#) pada dua baris ini
```
en_US.UTF-8 UTF-8
en_US.ISO-8859-1 ISO-8859-1
```
### Generate dan terapkan locale:
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> buka locale.conf dan pastikan isinya
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

### Buat Akun Pengguna (Sudoers)
```
useradd -m <nama_user>
```
```
passwd <nama_user>
```
```
echo "<nama_user> ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
```
### Buat Parameter Kernel (Cmdline)
```
mkdir -p /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/<nama_partisi>)=<nama_perangkat> root=/dev/<nama_grup>/<nama_volume_root>" > /etc/cmdline.d/01-boot.conf
```
> Ganti variabel sesuai dengan nama partisi & grup LVM kamu

```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

### Bersihkan File Lama & Konfigurasi Initramfs
```
rm -fr /boot/initramfs-linux-*
```
```
nvim /etc/mkinitcpio.conf
```
> isi dengan:
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```
### Konfigurasi Preset Kernel LTS
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"
ALL_kerneldest="/boot/vmlinuz-linux-lts"

PRESETS=('default')
default_uki="/boot/EFI/Linux/arch-linux-lts.efi"
```
> jika tidak ada di command diatas berarti semua di disable atau ditambahka # didepannya

## Terapkan Bootloader
```
exit
```
```
bootctl --path=/mnt/boot install
```
```
arch-chroot /mnt
```
```
mkinitcpio -P
```
## Aktivasi Layanan Latar Belakang (Services) & Restart
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```
```
systemctl enable firewalld
```
```
exit
```
```
umount -R /mnt
```
```
reboot
```

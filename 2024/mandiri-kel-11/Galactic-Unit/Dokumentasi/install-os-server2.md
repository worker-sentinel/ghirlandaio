Step pertama sama seperti tugas-tugas sebelumnya, yaitu menyambung wifi dan membagi partisi
```
boot = 3G (EFI system)
root = 80G (Linux filesystem)
```
Melihat partisi yang telah dibuat 
```
lsblk
```

# Enkripsi LUKS
```
cryptsetup luksFormat /dev/(partisi root)
```
```
cryptsetup luksOpen /dev/(partisi root) [galactic]
```

# Konfigurasi LVM
## Pertama, siapkan physical volume untuk membuat volume group
```
pvcreate /dev/mapper/[galactic]
```
```
vgcreate proc /dev/mapper/[galactic]
```
Cek partisi
```
lsblk
```
## Membuat logical volume root
```
lvcreate -L 15G proc -n root
```
```
mkfs.ext4 /dev/proc/root
```
Format partisi boot
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
```
Mounting partisi grup root
```
mount /dev/proc/root /mnt
```
Mounting partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi boot) /mnt/boot
```

## Membuat logical volume
```
lvcreate -L 15G proc -n vars
```
```
lvcreate -L 1G proc -n vlog
```
```
lvcreate -L 1G proc -n vaud
```
```
lvcreate -L 2G proc -n vtmp
```
```
lvcreate -l50%FREE proc -n home
```
```
lvcreate -L 10G proc -n podman
```

# Format Partisi
```
mkfs.ext4 /dev/proc/vars
```
```
mkfs.ext4 /dev/proc/vlog
```
```
mkfs.ext4 /dev/proc/vaud
```
```
mkfs.ext4 /dev/proc/vtmp
```
```
mkfs.ext4 /dev/proc/home
```
```
mkfs.ext4 /dev/proc/podman
```

# Mounting Partisi
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/log/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/podman /mnt/var/lib/containers
```
```
lsblk
```

# Install Packages
```
pacstrap /mnt linux-hardened linux-hardened-headers lvm2 mkinitcpio linux-firmware-intel linux-firmware-realtek linux-firmware-atheros git neovim less intel-ucode firewalld pacman sudo iwd bash iputils iproute2
```
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
cat /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

# Membuat hostname, set localtime dan locale
```
arch-chroot /mnt
```
```
nvim /etc/hostname
```
> ketik hostname yang ingin dibuat (bebas), lalu klik esc dan ketik :wq
```
cat /etc/hostname
```
## Set localtime
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
> hapus tanda pagar di en_US. Klik esc dan ketik :wq
## Generate bahasa
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> ubah LANG=C menjadi LANG=en_US dan LC_ALL= menjadi LC_ALL=en_US.UTF-8. Klik esc dan ketik :wq

# Membuat home user directory
```
mkdir -p /home/user
```
```
useradd -d /home/user unit
```
```
passwd
```
```
chown -R unit:unit /home/user
```
```
passwd unit
```
```
echo “unit ALL=(ALL:ALL) ALL” >> /etc/sudoers.d/unit
```
```
cat /etc/sudoers.d/unit
```

# Konfigurasi bootloader
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/(partisi root))=unit root=/dev/proc/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
> cari hooks yang tidak ada pagar. Lalu samakan HOOKS=(base system autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)

Berpindah ke directory boot
```
cd /boot
```
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
mv vmlinuz-linux-hardened intel-ucode.img kernel/
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
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
Samakan isinya:
```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.d/default.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/EFI/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```
```
mkinitcpio -P
```

# Systemd boot
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

# Booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```











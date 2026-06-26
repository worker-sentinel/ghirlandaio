# lsblk
Untuk mengecek partisi yang tersedia biasanya menggunakan nvme/sda pastikan dengan benar

# partition
```
cryptsetup luksFormat /dev/partition_name
```
```
cryptsetup luksOpen /dev/partition_name device_name
```
```
pvcreate /dev/mapper/device_name
```
```
vgcreate group_name /dev/mapper/device_name
```

# logical volume create
membuat logical volume yang dibuthkan admin
## root
```
lvcreate -L size (G | M) group_name -n root
```
## boot
```
mkfs.vfat -F32 -n BOOT /dev/partition
```
## var
```
lvcreate -L size (G | M) group_name -n volume_name (example:vars)
```

## vtmp
```
lvcreate -L size (G | M) group_name -n volume_name (example:vtmp)
```

## vlog
```
lvcreate -L size (G | M) group_name -n volume_name (example:vlog)
```

## vaud
```
lvcreate -L size (G | M) group_name -n volume_name (example:vaud)```
```

## home
```
lvcreate -l50%FREE group_name -n home
```
# makes format (mkfs) dan mounting (/mnt)
## root
```
mkfs.ext4 -b 4096 /dev/group_name/root
```
```
mount /dev/group_name/root /mnt
```
## boot
```
mkfs.vfat -F32 -n BOOT /dev/partition
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/paritition /mnt/boot
```
## var
```
mkfs.ext4 -b 4096 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/volume_name /mnt/var
```
## vtmp
```
mkfs.ext4 -b 4096 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/tmp
```
## vaud
```
mkfs.ext4 -b 4096 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/log
```
# home
```
mkfs.ext4 -b 4096 /dev/group_name/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/home /mnt/home
```
# tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
# chroot
```
arch-chroot /mnt
```

## hostname
```
echo [nama komputer] > /etc/hostname
```

## localtime
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## locale

```
nvim /etc/locale.gen
```
uncommentin both `en_US.UTF-8`
```
en_US.UTF-8
en_US.ISO-8859-1
```
```
locale-gen
```

```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
add value like below
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

## user admin
```
useradd -m [user_name]
```
```
passwd [user_name]
```
```
echo "[user_name] ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
```
## user operator
```
useradd -m [user_name]
```
```
passwd [user_name]
```

## cmdline
```
mkdir -p /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partition_name)=device_name root=/dev/group_volume/volume_name" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## boot
```
rm -fr /boot/initramfs-linux-*
```

## initramfs
```
nvim /etc/mkinitcpio.conf
```
add `sd-encrypt` and `lvm` after `sd-vconsole`
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
the same as the configuration below
```
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
```
touch /etc/vconsole.conf
```
```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
## desktop
```
pacman -S xfce4 sddm networkmanager
```
## service
```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
```

```
systemctl enable NetworkManager
```

```
systemctl enable firewalld
```

```
systemctl enable sddm
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

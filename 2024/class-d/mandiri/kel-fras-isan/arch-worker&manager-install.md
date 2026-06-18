# nyalakan asciinema
```
asciinema rec [nama file].cast
```
# setup luks
```
cryptsetup luksFormat /dev/[partisi]
```
```
cryptsetup luksOpen /dev/[partisi] [nama device]
```

# partition
```
pvcreate /dev/mapper[nama device]
```
```
vgcreate [nama grup] /dev/mapper/[nama device]
```

## root
```
lvcreate -L size (G | M) [nama grup] -n root
```
```
mkfs.ext4 /dev/[nama grup]/root
```
```
mount /dev/[nama grup]/root /mnt
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
lvcreate -L size (G | M) [nama grup] -n vars
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```


## vtmp
```
lvcreate -L size (G | M) [nama grup] -n vtmp
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```

## vlog
```
lvcreate -L size (G | M) [nama grup] -n vlog
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```

## vaud
```
lvcreate -L size (G | M) [nama grup] -n vaud
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```

## home 
```
lvcreate -L size (G | M) [nama grup] -n home
```
```
mkfs.ext4 /dev/[nama grup]/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/home /mnt/home
```

## dock

```
lvcreate -L size (G | M) [nama grup] -n dock
```
```
mkfs.ext4 /dev/[nama grup]/dock
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/dock /mnt/var/lib/docker
```

## pacstrap
```
pacstrap /mnt intel-ucode linux-lts linux-firmware linux-lts-headers docker curl base which grep sudo pacman firewalld lvm2 iwd neovim
```

## fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

## tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

## chroot
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
> add value like below
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```
## useradd
```
useradd -m [user name]
```

```
passwd [user name]
```
```
echo 'nama_user ALL=(ALL:ALL) ALL' > /etc/sudoers.d/none
```
# KERNEL PARAMETER
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

## CONFIG KERNEL PARAMETER

### 01-boot.conf
```
nvim /etc/cmdline.d/01-boot.conf
```
#### ISI DENGAN
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root)=[nama device] root=/dev/[nama grup]/root" > /etc/cmdline.d/01-boot.conf 
```
### 02-misc.conf
```
nvim /etc/cmdline.d/02-misc.conf
```
#### ISI DENGAN
```
rw quiet
```
****

## Prepare boot and config mkinit
```
cd boot/
```
```
rm initramfs-linux-lts.img
```
### mkinit
```
nvim /etc/mkinitcpio.conf
```
```
# vim:set ft=sh:
# MODULES
# The following modules are loaded before any boot hooks are
# run.  Advanced users may wish to specify all system modules
# in this array.  For instance:
#     MODULES=(usbhid xhci_hcd)
MODULES=()

# BINARIES
# This setting includes any additional binaries a given user may
# wish into the CPIO image.  This is run last, so it may be used to
# override the actual binaries included by a given hook
# BINARIES are dependency parsed, so you may safely ignore libraries
BINARIES=()

# FILES
# This setting is similar to BINARIES above, however, files are added
# as-is and are not parsed in any way.  This is useful for config files.
FILES=()

# HOOKS
# This is the most important setting in this file.  The HOOKS control the
# modules and scripts added to the image, and what happens at boot time.
# Order is important, and it is recommended that you do not change the
# order in which HOOKS are added.  Run 'mkinitcpio -H <hook name>' for
# help on a given hook.
# 'base' is _required_ unless you know precisely what you are doing.
# 'udev' is _required_ in order to automatically load modules
# 'filesystems' is _required_ unless you specify your fs modules in MODULES
# Examples:
##   This setup specifies all modules in the MODULES setting above.
##   No RAID, lvm2, or encrypted root is needed.
#    HOOKS=(base)
#
##   This setup will autodetect all modules for your system and should
##   work as a sane default
#    HOOKS=(base udev autodetect microcode modconf block filesystems fsck)
#
##   This setup will generate a 'full' image which supports most systems.
##   No autodetection is done.
#    HOOKS=(base udev microcode modconf block filesystems fsck)
#
##   This setup assembles a mdadm array with an encrypted root file system.
##   Note: See 'mkinitcpio -H mdadm_udev' for more information on RAID devices.
#    HOOKS=(base udev microcode modconf keyboard keymap consolefont block mdadm_udev encrypt filesystems fsck)
#
##   This setup loads an lvm2 volume group.
#    HOOKS=(base udev microcode modconf block lvm2 filesystems fsck)
#
##   This will create a systemd based initramfs which loads an encrypted root filesystem.
#    HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt filesystems fsck)
#
##   NOTE: If you have /usr on a separate partition, you MUST include the
#    usr and fsck hooks.
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)

# COMPRESSION
# Use this to compress the initramfs image. By default, zstd compression
# is used for Linux ≥ 5.9 and gzip compression is used for Linux < 5.9.
# Use 'cat' to create an uncompressed image.
#COMPRESSION="zstd"
#COMPRESSION="gzip"
#COMPRESSION="bzip2"
#COMPRESSION="lzma"
#COMPRESSION="xz"
#COMPRESSION="lzop"
#COMPRESSION="lz4"

# COMPRESSION_OPTIONS
# Additional options for the compressor
#COMPRESSION_OPTIONS=()

# MODULES_DECOMPRESS
# Decompress loadable kernel modules and their firmware during initramfs
# creation. Switch (yes/no).
# Enable to allow further decreasing image size when using high compression
# (e.g. xz -9e or zstd --long --ultra -22) at the expense of increased RAM usage
# at early boot.
# Note that any compressed files will be placed in the uncompressed early CPIO
# to avoid double compression.
#MODULES_DECOMPRESS="no"
```
## perlu diperhatikan
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)
```
> yang harus ditambahkan

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"
ALL_kerneldest="/boot/vmlinuz-linux-lts"

PRESETS=('default')                                                                                                                                                   #PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"                                                                                                                                #default_image="/boot/initramfs-linux-lts.img"                                                                                                                        default_uki="/EFI/Linux/arch-linux-lts.efi"                                                                                                                           #default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"   

#fallback_config="/etc/mkinitcpio.conf"                                                                                                                              #fallback_image="/boot/initramfs-linux-lts-fallback.img"                                                                                                              #fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"     
```
> harus mirip dengan yang diatas

## BOOTCTL
```
touch /etc/vconsole.conf
```
```
exit
```
```
bootctl --path=/mnt/boot install
```
### masuk lagi ke arch-chroot
```
arch-chroot /mnt
```

****


## GENERATE UKI

```
mkinitcpio -P
```
****

## SERVICE
```
systemctl enable systemd-networkd.socket
```

```
systemctl enable systemd-resolved
```

```
systemctl enable iwd
```
****
```
exit
```
> tekan ctrl + d untuk mematikan asciinema

```
asciinema upload [nama file].cast
```
# UNMOUNT

```
umount -R /mnt
```

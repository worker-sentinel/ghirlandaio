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

## root
```
lvcreate -L size (G | M) group_name -n root
```
```
mkfs.ext4 /dev/group_name/root
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
lvcreate -L size (G | M) group_name -n volume_name (example:vars)
```
```
mkfs.ext4 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/volume_name /mnt/var
```


## vtmp
```
lvcreate -L size (G | M) group_name -n volume_name (example:vtmp)
```
```
mkfs.ext4 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/tmp
```

## vlog
```
lvcreate -L size (G | M) group_name -n volume_name (example:vlog)
```
```
mkfs.ext4 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/log
```

## vaud
```
lvcreate -L size (G | M) group_name -n volume_name (example:vaud)
```
```
mkfs.ext4 /dev/group_name/volume_name
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/volume_name /mnt/var/log/audit
```

## home
```
lvcreate -l50%FREE group_name -n home
```
```
mkfs.ext4 /dev/group_name/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/home /mnt/home
```


## podi
```
lvcreate -l50%FREE group_name -n podi
```
```
mkfs.ext4 /dev/group_name/podi
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/podi /mnt/var/lib/containers
```

# packages
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman openssh
```
# fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
# tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
# network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network/
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


## user
```
useradd -m [user_name]
```
```
passwd [user_name]
```
```
echo "[user_name] ALL=(ALL:ALL) ALL" > /etc/sudoers.d/none
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
## service
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

## after booting
### setup firewalld
1. check zone. example in below
```
sudo firewall-cmd --list-all-zone
```
2. allow port. example in below

```
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```
3. allow service. example in below
```
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```
4. allow port rich rules
```
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="ip_admin" port port="port_apk" protocol="tcp" accept'
```
5. remove service. example in below
```
sudo firewall-cmd --zone=internal --remove-service=ssh --permanent
```
>[Note]
> hapus semua service dan port selain di zone public
```
sudo firewall-cmd --reload
```

### setup hardening kernel
#### example for disale module kernel
for check wireless device
```
lspci -knnd ::0280
```
value
```
02:00.0 Network controller [0280]: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak] [8086:24fb] (rev 10)
	Subsystem: Intel Corporation Device [8086:2110]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```
> iwlwifi is a module
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
value
```
install usb-storage /bin/false
blacklist usb-storage
install iwlwifi /bin/false
blacklist iwlwifi
```
```
sudo modprobe -r usb-storage
```
```
sudo mkinitcpio -P
```
### setup rootless podman
#### adding subuid and subgid

```
sudo nvim /etc/subuid
```

```/etc/subuid
[user]:100000:65536
```

```
sudo nvim /etc/subgid
```

```/etc/subgid
[user]:100000:65536
```

## enable global podman

```
sudo systemctl enable --global podman
```

## configure storage

```
mkdir -p .config/containers/
```

```
nvim .config/containers/storage.conf
```

```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

#### registries podman
```
nvim /etc/containers/registries.conf
```
#### cari
```
unqualified-search-registries = ["example.com"] 
```
menjadi ["docker.io"]

save and quit
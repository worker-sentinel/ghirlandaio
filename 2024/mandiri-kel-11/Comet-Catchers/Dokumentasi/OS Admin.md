# Dokumentasi Install OS

## Menghubungkan internet
```
iwctl
device list
station wlan0 scan
station wlan0 get-network
station wlan0 connect “nama wifi”
exit
ping 8.8.8.8
```

## Cek partisi
```
lsblk
```

## Physical volume, volume group, logivcal volume
```
pvcreate /dev/nvme0n1p8
vgcreate proc /dev/nvme0n1p8
lvcreate -L 10G proc -n root
mkfs.ext4 /dev/proc/root 
mount /dev/proc/root  /mnt
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p7
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```

```
lvcreate -L 10G  proc -n vars
mkfs.ext4 /dev/proc/vars
mount –mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```

```
lvcreate -L 1G  proc -n vtmp
mkfs.ext4 /dev/proc/vtmp
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

```
lvcreate -L 1G proc -n vlog
mkfs.ext4 /dev/proc/vlog
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

```
lvcreate -L 1G  proc -n vaud
mkfs.ext4 /dev/proc/vaud
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

```
lvcreate -L 1G proc -n home
mkfs.ext4 /dev/proc/home
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
```

```
lvcreate -l150%FREE proc -n priv
```

```
cryptsetup luksFormat /dev/proc/priv
```

## Cek partisi
```
lsblk
```
## Install package
```
pacstrap /mnt intel-ucode linux-hardened linux-hardened-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```
## After install
```
genfstab -U /mnt > /mnt/etc/fstab
echo “tmpfs /tmp /tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
arch-chroot /mnt
echo comet > /etc/hostname
```

## Set waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock –systohc 
nvim /etc/locale.gen
```

## Hapus hastag
```
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1
```

Menjadi

```
en_US.UTF-8 UTF-8
en_US ISO-8859-1
```

```
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
```

```
LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)
```

```
cryptsetup luksOpen /dev/proc/priv internal
enter password
mkdir -p /home/user
useradd -d /home/user catchers
chown -R catchers:catchers /home/user
passwd catchers
enter password
```

```
echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/none
nvim /etc/security/pam_mount.conf.xml
<volume 
users=”catchers" 
fstype= “crypt”
path= “/dev/prov/priv”
mountpoint=”/home/user”
```

```
/>
nvim /etc/security/pam.d/system-login
Sesuaikan dengan foto ini
nvim /etc/booster.yaml
```

```
network:
dhcp: on
universal: false 
modules: -*,ext4,nvme
extra_files: fsck,fsck,ext4
strip: true
enable_lvm: true
```

```
cd /boot
ls /usr/lib/modules
booster build –kernel-version 6.18.35-1-lts /boot/booster-linux-lts-new.img
rm -fr booster-linux-lts.img
bootctl –path=/boot install
```

```
nvim /boot/loader/entries/booster.conf
title   arch with booster
linux /vmlinuz-linux-lts
initrd /amd-ucode. img
initrd /booster-linux-lts-new. img 
options root=/dev/proc/root rw
nvim /boot/loader/loader.conf
```
```
#timeout 3 
#console-mode keep 
#default booster.conf
```

```
bootctl –graceful update
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-alsa-pipewire-jack network-manager-applet
```

## Keluar
```
exit

umount -R /mnt
reboot
```

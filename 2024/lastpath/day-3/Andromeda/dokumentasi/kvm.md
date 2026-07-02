# install atom

## mengaktifkan asciinema
```
asciinema rec [nama file].cast
```

## partisi
```
pvcreate /dev/nvme0n1p6
```
```
vgcreate proc /dev/nvme0n1p6
```

## membuat lv, format, dan mount
### root
```
lvcreate -L 20G andromeda -n root
```
```
mkfs.ext4 /dev/andromeda/root
```
```
mount /dev/andromeda/root /mnt
```

### boot
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p5
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p5 /mnt/boot
```


### var
```
lvcreate 15G andromeda -n vars
```
```
mkfs.ext4 /dev/andromeda/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/andromeda/vars /mnt/var
```


### vtmp
```
lvcreate -L 2G andromeda -n vtmp
```
```
mkfs.ext4 /dev/andromeda/vtmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vtmp /mnt/var/tmp
```

### vlog
```
lvcreate -L 5G andromeda -n vlog
```
```
mkfs.ext4 /dev/andromeda/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vlog /mnt/var/log
```

### vaud
```
lvcreate -L 5G andromeda -n vaud
```
```
mkfs.ext4 /dev/andromeda/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/vaud /mnt/var/log/audit
```

### home public
```
lvcreate -L 10G andromeda -n home
```
```
mkfs.ext4 /dev/andromeda/home
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/home /mnt/home
```

### podman
```
lvcreate -l50%FREE andromeda -n podman
```
```
mkfs.ext4 /dev/andromeda/podman
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/andromeda/podman /mnt/var/lib/containers
```

### home internal
```
lvcreate -l50%FREE andromeda -n andromeda
```
```
cryptsetup luksFormat /dev/andromeda/andromeda
```

# packages
```
pacstrap /mnt intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman openssh podman-compose asciinema
```
# fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

# tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
# network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

# chroot
```
arch-chroot /mnt
```

## hostname
```
echo hpallyssa > /etc/hostname
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
hapus # pada
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
masukkan command seperti yang dibawah
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

## PAM Mount
```
cryptsetup luksOpen /dev/andromeda/andromeda internal
```
```
mkfs.ext4 /dev/mapper/internal
```
## User
```
mkdir -p /home/user
```

```
useradd -d /home/user andromeda6
```

```
chown -R andromeda6:andromeda6 /home/user
```

```
passwd andromeda6
```


```
echo 'andromeda6 ALL=(ALL:ALL) ALL' > /etc/sudoers.d/none
```
                                                                                 
### configure the Volume

```
nvim /etc/security/pam_mount.conf.xml
```
tambahkan seperti yang dibawah setelah logout

```
<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/andromeda/andromeda" 
    mountpoint="/home/user" 
/>
```

### mengupdate Konfigurasi PAM

```
nvim /etc/pam.d/system-login
```

sesuai seperti yang dibawah

```/etc/pam.d/system-login
#%PAM-1.0

auth       required   pam_shells.so
auth       requisite  pam_nologin.so
auth       include    system-auth
auth       required   pam_mount.so

account    required   pam_access.so
account    required   pam_nologin.so
account    include    system-auth

password   include    system-auth

session    optional   pam_loginuid.so
session    optional   pam_keyinit.so       force revoke
session    include    system-auth
session    optional   pam_lastlog2.so      silent
session    optional   pam_motd.so
session    optional   pam_mail.so          dir=/var/spool/mail standard quiet
session    optional   pam_umask.so
session    optional  pam_mount.so
-session   optional   pam_systemd.so
session    required   pam_env.so
```

## Kernel Command Line
```
mkdir -p /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "root=/dev/group_volume/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

```
rm -fr /boot/initramfs-linux-*
```

```
nvim /etc/mkinitcpio.conf
```

tambahkan `lvm2` setelah `sd-vconsole`
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block lvm2 filesystems fsck)
```
`esc` lalu `:wq`
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
ikuti yang dibawah
```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
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
## Service
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
systemctl enable sshd
```
## mematikan asciinema

ctrl+D

```
asciinema upload [nama file].cast
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

## install kvm packages
```
sudo pacman -Syy

sudo reboot 

sudo pacman -S archlinux-keyring

sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```
```
sudo pacman -S ebtables iptables
```
```
sudo pacman -S libguestfs
```
```
sudo systemctl enable libvirtd.service

sudo systemctl start libvirtd.service
```
```
$ systemctl status libvirtd.service

 ● libvirtd.service - Virtualization daemon

    Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: disabled)

    Active: active (running) since Thu 2019-04-18 20:55:34 EAT; 11h ago

      Docs: man:libvirtd(8)

            https://libvirt.org

  Main PID: 646 (libvirtd)

     Tasks: 26 (limit: 32768)

    Memory: 74.0M

    CGroup: /system.slice/libvirtd.service

            ├─  646 /usr/bin/libvirtd

            ├─  754 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leases>

            ├─  755 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leases>

            ├─  777 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/docker-machines.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir>

            ├─  778 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/docker-machines.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir>

            ├─25924 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/vagrant-libvirt.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir>

            ├─25925 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/vagrant-libvirt.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir>

            ├─25959 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/fed290.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leasesh>

            └─25960 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/fed290.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leasesh>
```

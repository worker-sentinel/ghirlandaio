# Dokumentasi Install OS Admin
## koneksi jaringan
```
iwctl
```
## cek partisi
```
lsblk
```
## buat volume fisik
```
pvcreate /dev/nvme0n1p8
```
## buat volume grub
```
vgcreate proc /dev/nvme0n1p8
```
```
lvcreate -L 10G proc -n root
```
```
mkfs.ext4 /dev/proc/root
```
```
mount /dev/proc/root  /mnt
```
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p7
```
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```
```
lvcreate -L 10G  proc -n vars
```
```
mkfs.ext4 /dev/proc/vars
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```
```
lvcreate -L 1G  proc -n vtmp
```
```
mkfs.ext4 /dev/proc/vtmp
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```
```
lvcreate -L 1G proc -n vlog
```
```
mkfs.ext4 /dev/proc/vlog
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```
```
lvcreate -L 1G  proc -n vaud
```
```
mkfs.ext4 /dev/proc/vaud
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```
```
lvcreate -L 1G proc -n home
```
```
mkfs.ext4 /dev/proc/home
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
```
```
lvcreate -l150%FREE proc -n priv
```
```
cryptsetup luksFormat /dev/proc/priv
```
## cek kembali
```
lsblk
```
## install
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepaassxc secrets booster networkmanager pam_mount 
```
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo “tmpfs /tmp /tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
## masuk ke dalam sistem
```
arch-chroot /mnt
```
```
echo comet > /etc/hostname
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock –systohc
```
```
nvim /etc/locale.gen
```

## hapus hastag 
# en_US.UTF-8 UTF-8
		#en_US ISO-8859-1
    
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```

LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)

```
cryptsetup luksOpen /dev/proc/priv internal
```
## masukan kata sandi
```
enter password
```
```
mkdir -p /home/user
```
```
useradd -d /home/user catchers
```
```
chown -R catchers:catchers /home/user
```
```
passwd catchers
```

enter password

```
echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/none
```
```
nvim /etc/security/pam_mount.conf.xml
```
## tambahkan ini
<volume 
users=”catchers" 
fstype= “crypt”
path= “/dev/prov/priv”
mountpoint=”/home/user”
/>

```
nvim /etc/security/pam.d/system-login
```
```
nvim /etc/booster.yaml
```
## tambahkan ini
network:
dhcp: on
universal: false 
modules: -*,ext4,nvme
extra_files: fsck,fsck,ext4
strip: true
enable_lvm: true

```
cd /boot
```
```
ls /usr/lib/modules
```
```
booster build –kernel-version 6.18.35-1-lts /boot/booster-linux-lts-new.img
```
```
rm -fr booster-linux-lts.img
```
```
bootctl –path=/boot install
```
```
nvim /boot/loader/entries/booster.conf
```
## tambahkan ini
title   arch with booster
linux /vmlinuz-linux-lts
initrd /amd-ucode. img
initrd /booster-linux-lts-new. img 
options root=/dev/proc/root rw

```
nvim /boot/loader/loader.conf
```

#timeout 3 
#console-mode keep 
#default booster.conf

```
bootctl –graceful update
```
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-alsa-pipewire-jack network-manager-applet
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


# Dokumentasi OS Server
## Hubungkan ke internet dulu
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-network
```
```
station wlan0 connect “nama wifi”
```
```
exit
```
```
ping 8.8.8.8
```
## Aktifkan asciinema
```
cryptsetup dan format luks
```
```
cryptsetup luksFormat /dev/partisi sistem
```
```
cryptsetup luksOpen /dev/partisi sistem proc
```
```
pvcreate /dev/mapper/proc
```
```
vgcreate [nama volume grup] /dev/mapper/proc
```
## Cek partisi
```
lsblk
```
## Buat logical volume
```
lvcreate -L 8G comet -n root
```
```
lvcreate -L 5G comet -n vars
```
```
lvcreate -L 1G comet -n vlog
```
```
lvcreate -L 1G comet -n vaud
```
```
lvcreate -L 1.5G comet -n vtmp
```
```
lvcreate -l50%FREE comet -n home
```
```
lvcreate -l100%FREE comet -n podman
```
```
lsblk
```
## Format partisi
```
mkfs.ext4 /dev/comet/root
```
```
mkfs.vfat -F32 /dev/partisi boot
```
```
mkfs.ext4 /dev/comet/vars
```
```
mkfs.ext4 /dev/comet/vlog
```
```
mkfs.ext4 /dev/comet/vaud
```
```
mkfs.ext4 /dev/comet/vtmp
```
```
mkfs.ext4 /dev/comet/home
```
```
mkfs.ext4 /dev/comet/podman
```
```
mount /dev/comet/root /mnt
```
```
lsblk
```
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi boot /mnt/boot
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/vars /mnt/var
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vlog /mnt/var/log
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vaud /mnt/var/log/audit
```
```
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vtmp /mnt/var/tmp
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/home /mnt/home
```
```
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/podman /mnt/var/lib/containers
```
```
lsblk
```
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
```
genfstab -U /mnt/etc/fstab
```
```
echo ‘“tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/*  /mnt/etc/systemd/network
```
```
arch-chroot /mnt
```
```
echo server > /etc/hostname
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock –systohc
```
```
nvim /etc/locale.gen
```

hapus hastag # en_US.UTF-8 UTF-8
		#en_US ISO-8859-1
    
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```

LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)

```
```
useradd -m catchers
```
```
passwd catchers
```
```
echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/catchers
```
```
su catchers
```
```
sudo su
```
```
enter password
```
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
lsblk
```
```
cat /etc/cmdline.d/01-boot.conf
```
```
echo “rd.luks.name=$(blkid -s UUID -o value /dev/partisi boot)=proc root=/dev/comet/root” > /etc/cmdline.d/01-boot.conf
```
```
echo rw > /etc/cmdline.d/02-misc.conf
```
```
nvim /etc/mkinitcpio.conf
```
bagian HOOKS akhir tambahin (sd-encrypt lvm2)
```
nvim /etc/mkinitcpio.d/linux-lts.conf.preset
```

= mkinitcpio preset file for the 'linux-lts' package
ALL_config="/etc/mkinitepio.conf"
ALL_kver="/boot/vmlinuz-linux-Its"
ALL_kerneldest="/boot/vmlinuz-linux-lts"


PRESETS=( 'default')
#PRESETS=('default' ‘fallback')

#default_config="/etc/mkinitcpio.conf”
#default_image="/boot/initramfs-linux-lts.img”
 default_uki="/boot/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /us/share/systend/bootctl/splash-arch.bmp”
#fallback_config=”/etc/mkinitcpio.conf”
#fallback_image="/boot/initramfs-linux-lts-fallback.img “
#fallback_uki=”/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect”


```
bootctl –path=/boot install
```
```
mkinitcpio -P 
```
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
```
exit
```
```
umount -R /mnt
```
```
reboot
```


# Dokumentasi OS Client

## Menghubungkan internet
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-network
```
```
station wlan0 connect “nama wifi”
```
```
exit
```
```
ping 8.8.8.8
```

Cek partisi

```
lsblk
```
```
cfdisk /dev/sda
```

syncing disk atau bagi partisi
format partisi

```
lsblk
```
```
mkfs.fat -F 32 /dev/sda5
```
```
mkswap /dev/sda7
```
```
swapon /dev/sda6
```
```
mkfs.ext /devsda7
```
```
mkfs.ext4 /dev/sda7
```
```
mount /dev/sda7 /mnt
```
```
lsblk
```
```
mkfs.ext4 /dev/sda6
```
```
lsblk
```
```
mkfs.fat -F 32 /dev/sda5
```
```
mount /dev/sda7 /mnt
```
```
mkdir -p /mnt/boot
```
```
mount /dev/sda5 /mnt/boot
```
```
pacstrap -K /mnt base-devel linux linux-firmwarenvim intel-ucode
```
```
genfstab -U /mnt >> /mnt/etc/fstab
```
```
arch-chroot /mnt
```
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
locale-gen
```
```
echo LANG=en_US.UTF-8 > /etc/locale.conf
```
```
cat /etc/locale.conf
```
```
echo comet > /etc/hostname
```
```
mkinitcpio -P
```
```
passwd
```
```
pacman -S grub efibootmgr networkmanager grub-install --target=x86_65-efi --efi-dictionary=/boot --bootloader-id=GRUB
```
```
pacman -S networkmanager
```
```
systemctl enable NetworkManager.service
```
```
pacman -S grub efibootmgr
```
```
grub-install --target=x86_64-efi --efi-dictionary=/boot --bootloader-id=GRUB
```
```
grub-mkconfig -o /boot/grub/grub.cfg
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


# Setup Firewalld
```
systemctl status firewalld 	
```
```
firewall-cmd --list-all-zone | grep drop
```
```
firewall-cmd --list-all-service 
```
```
firewall-cmd --info-zone=drop	
```
```
firewall-cmd --info-zone=block   
```
```
firewall-cmd --info-zone=public 
```
```
firewall-cmd --info-zone=external
```
```
firewall-cmd --info-zone=internal
```
```
firewall-cmd --info-zone=trusted 
```
```
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```
```
firewall-cmd --reload  
```
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent 
```
```
firewall-cmd --reload
```

success

```                                                                                              
firewall-cmd --zone=dmz --remove-service=ssh --permanent 
```
```
firewall-cmd --reload
```
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```
```
firewall-cmd --reload
```
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
```
firewall-cmd --reload
```
```
exit                      
```


# Disable kernel
```
lsmod | grep cramfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
```
```
lsmod | grep jffs2
```
```
lsmod | grep overlayfs
```
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep usb-storage
```
```
nvim /etc/modprobe.d/disable-module.conf
```
```
install usb-storage /bin/false
```
```
blacklist usb-storage
```
```
lsmod | grep afs
```
```
lsmod | grep ceph
```
```
lsmod | grep cifs
```
```
lsmod | grep exfat
```
```
lsmod | grep ext
```
```
lsmod | grep fat
```
```
lsmod | grep fscache
```
```
lsmod | grep fuse
```
```
lsmod | grep gfs2
```
```
lsmod | grep nfs_common
```
```
lsmod | grep nfsd
```
```
lsmod | grep smbfs_common
```
```
nvim /etc/modprobe.d/disable-module.conf
```
```
install usb-storage /bin/false
```
```
blacklist usb-storage
```
```
mkinitcpio -P
```
```
exit
```


# Dokumentasi Install Slims
## Download Source SLiMs

```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip 
```
```
unzip master.zip
```

## Rename folder
```
mv docker-compose-for-slims-master compose
```
```
cd compose
```

## Kernel

```
sudo nvim /etc/sysctl.d/99-custom.conf
```
```
sudo chmod -R 777 app/slims
```
```                                                                                                     
sudo nvim /etcsubid   
```

## masukin     
[user]:100000:65536      
sudo nvim /etcsugid   
masukin
[user]:100000:65536

## configure storage
```
nvim ~/.config/cleo/storage.conf
```

[storage]  
driver = "overlay"  
[storage.options.overlay]
mount_program = ""   
mountopt = "userxattr"

```
sudo nvim /etc/containers
```

## Lanjutan Install SLiMS
```
podman pull docker.io/mysql:5.7
```
```
podman compose up -d
```
```
podman ps -a
```
```
exit
```


# Connecting admin
```
sudo nmcli device status  
```
```
sudo nmcli device status 
```
```
sudo nmcli connection add type ethernet ifname enp3s0f4u2c2
con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.10.10/24 ipv
4.gateway 10.10.10.1 ipv4.dns 8.8.8.8   
```

enter password

```
sudo nmcli connection modify admin-connection connection.autoconnect yes 
```

enter password

```
exit
```


# Connecting Operator
```
nmcli device status
```
```
sudo nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4 gateway 10.10.10.1 ipv4.dns 8.8.8.8
```

masukin password for operator:......

```
nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4. gateway 10.10.10.1 ipv4.dns 8.8.8.8
```
```
nmcli connection up operator-connection
```
```
ip a
```
```
ip r
```
```
ping 8.8.8.8
```
```
exit
```





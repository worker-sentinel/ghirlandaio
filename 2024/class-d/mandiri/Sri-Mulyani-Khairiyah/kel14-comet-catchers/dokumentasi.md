# Dokumentasi OS Server

## Hubungkan ke internet dulu

``` bash iwctl

device list

station wlan0 scan

station wlan0 get-network

station wlan0 connect “nama wifi”
exit

ping 8.8.8.8
```
aktifkan asciinema

## cryptsetup dan format luks
```bash cryptsetup luksFormat /dev/partisi sistem
cryptsetup luksOpen /dev/partisi sistem proc
pvcreate /dev/mapper/proc
vgcreate [nama volume grup] /dev/mapper/proc

cek partisi
lsblk
```

## Buat logical volume
``` bash lvcreate -L 8G comet -n root
lvcreate -L 5G comet -n vars
lvcreate -L 1G comet -n vlog
lvcreate -L 1G comet -n vaud
lvcreate -L 1.5G comet -n vtmp
lvcreate -l50%FREE comet -n home
lvcreate -l100%FREE comet -n podman

lsblk
```

## Format partisi
```bash mkfs.ext4 /dev/comet/root
mkfs.vfat -F32 /dev/partisi boot
mkfs.ext4 /dev/comet/vars
mkfs.ext4 /dev/comet/vlog
mkfs.ext4 /dev/comet/vaud
mkfs.ext4 /dev/comet/vtmp
mkfs.ext4 /dev/comet/home
mkfs.ext4 /dev/comet/podman
```

## Mounting Partisi
```bash mount /dev/comet/root /mnt
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi boot /mnt/boot
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/vars /mnt/var
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vlog /mnt/var/log
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vaud /mnt/var/log/audit
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/comet/vtmp /mnt/var/tmp
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/home /mnt/home
mount –mkdir -o rw,nodev,nosuid,relatime /dev/comet/podman /mnt/var/lib/containers

lsblk
```

```bash pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```

```bash genfstab -U /mnt/etc/fstab

echo ‘“tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
```bash cp /etc/systemd/network/*  /mnt/etc/systemd/network
arch-chroot /mnt
echo server > /etc/hostname
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock –systohc
nvim /etc/locale.gen

hapus hastag # en_US.UTF-8 UTF-8
		#en_US ISO-8859-1

locale-gen

locale > /etc/locale.conf
nvim /etc/locale.conf

LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)
```

```bash useradd -m catchers
passwd catchers

echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/catchers
su catchers
sudo su
enter password
```

```bash mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
lsblk

cat /etc/cmdline.d/01-boot.conf
echo “rd.luks.name=$(blkid -s UUID -o value /dev/partisi boot)=proc root=/dev/comet/root” > /etc/cmdline.d/01-boot.conf
echo rw > /etc/cmdline.d/02-misc.conf
nvim /etc/mkinitcpio.conf

bagian HOOKS akhir tambahin (sd-encrypt lvm2)

nvim /etc/mkinitcpio.d/linux-lts.conf.preset
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

```bash hbootctl –path=/boot install
mkinitcpio -P 
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd 
systemctl enable firewalld
systemctl enable sshd
exit
umount -R /mnt
reboot
```

# Install OS Admin
```bash iwctl
```

```bashlsblk
pvcreate /dev/nvme0n1p8
vgcreate proc /dev/nvme0n1p8
```

```bash lvcreate -L 10G proc -n root
mkfs.ext4 /dev/proc/root 
mount /dev/proc/root  /mnt
```

```bashmkfs.vfat -F32 -n BOOT /dev/nvme0n1p7
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```

```bash lvcreate -L 10G  proc -n vars
mkfs.ext4 /dev/proc/vars
mount –mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```

```bash lvcreate -L 1G  proc -n vtmp
mkfs.ext4 /dev/proc/vtmp
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

```bash lvcreate -L 1G proc -n vlog
mkfs.ext4 /dev/proc/vlog
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

```bash lvcreate -L 1G  proc -n vaud
mkfs.ext4 /dev/proc/vaud
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

```bash lvcreate -L 1G proc -n home
mkfs.ext4 /dev/proc/home
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
```

```bash lvcreate -l150%FREE proc -n priv
cryptsetup luksFormat /dev/proc/priv
lsblk
```

```bash pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepaassxc secrets booster networkmanager pam_mount 
```

```bash genfstab -U /mnt > /mnt/etc/fstab
echo “tmpfs /tmp /tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
arch-chroot /mnt
echo comet > /etc/hostname
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock –systohc 
nvim /etc/locale.gen

hapus hastag # en_US.UTF-8 UTF-8
		#en_US ISO-8859-1
locale-gen

locale > /etc/locale.conf
nvim /etc/locale.conf

LANG=C.UTF-8 (LANG=en_US.UTF-8)
LC_ALL= (ditambahin en_US.UTF-8)

cryptsetup luksOpen /dev/proc/priv internal
enter password
mkdir -p /home/user
useradd -d /home/user catchers
chown -R catchers:catchers /home/user
passwd catchers
enter password

echo “catchers ALL=(ALL:ALL) ALL” > /etc/sudoers.d/none
nvim /etc/security/pam_mount.conf.xml
<volume 
users=”catchers" 
fstype= “crypt”
path= “/dev/prov/priv”
mountpoint=”/home/user”

/>
nvim /etc/security/pam.d/system-login
Sesuaikan dengan foto ini
nvim /etc/booster.yaml

network:
dhcp: on
universal: false 
modules: -*,ext4,nvme
extra_files: fsck,fsck,ext4
strip: true
enable_lvm: true

cd /boot
ls /usr/lib/modules
booster build –kernel-version 6.18.35-1-lts /boot/booster-linux-lts-new.img
rm -fr booster-linux-lts.img
bootctl –path=/boot install
```

```bash nvim /boot/loader/entries/booster.conf
title   arch with booster
linux /vmlinuz-linux-lts
initrd /amd-ucode. img
initrd /booster-linux-lts-new. img 
options root=/dev/proc/root rw
nvim /boot/loader/loader.conf
#timeout 3 
#console-mode keep 
#default booster.conf
```

```bash bootctl –graceful update
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-alsa-pipewire-jack network-manager-applet
```

```bash exit

umount -R /mnt
reboot
```
											
# Setup Firewalld
```bashsystemctl status firewalld 	

firewall-cmd --list-all-zone | grep drop

firewall-cmd --list-all-service 

firewall-cmd --info-zone=drop		
firewall-cmd --info-zone=block   
firewall-cmd --info-zone=public 
firewall-cmd --info-zone=external  								
firewall-cmd --info-zone=internal   
firewall-cmd --info-zone=trusted 
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload  
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent                                                                                                                                                                                                                                                                                                                                                                                                                                                  
firewall-cmd --zone=dmz --remove-service=ssh --permanent                                                                                                                                                               
firewall-cmd --reload                                                                                                                                                                                                  
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent                                                                                                                                              
firewall-cmd --reload                                                                                                                                                                                                  
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent                                                                                                                            
firewall-cmd --reload                                                                                                                                                                                                  
exit                      
```

# disable kernel
```bash lsmod | grep cramfs                                                                                                                                                                                                     
lsmod | grep freevxfs                                                                                                                                                                                                   
lsmod | grep hfs                                                                                                                                                                                                        
lsmod | grep hfsplus                                                                                                                                                                                                    
lsmod | grep jffs2                                                                                                                                                                                                      
lsmod | grep overlayfs                                                                                                                                                                                                  
lsmod | grep squashfs                                                                                                                                                                                                   
lsmod | grep udf                                                                                                                                                                                                        
lsmod | grep usb-storage                                                                                                                                                                                                 
nvim /etc/modprobe.d/disable-module.conf
install usb-storage /bin/false                                                                                                                                                                                                                 
blacklist usb-storage 

lsmod | grep afs                                                                                                                                                                                                        
lsmod | grep ceph                                                                                                                                                                                                       
lsmod | grep cifs                                                                                                                                                                                                       lsmod | grep exfat                                                                                                                                                                                                      lsmod | grep ext                                                                                                                                                                                                        lsmod | grep fat                                                                                                                                                                                                       lsmod | grep fscache                                                                                                                                                                                                    lsmod | grep fuse                                                                                                                                                                                                       lsmod | grep gfs2                                                                                                                                                                                                       lsmod | grep nfs_common                                                                                                                                                                                                 lsmod | grep nfsd      
lsmod | grep smbfs_common
nvim /etc/modprobe.d/disable-module.conf
install usb-storage /bin/false                                                                                                                                                                                                                 
blacklist usb-storage 
mkinitcpio -P      
exit
```

# Install OS Server App

## Menghubungkan internet
```
iwctl
device list
station wlan0 scan
station wlan0 get-network
station wlan0 connect "nama wifi"
exit
```

## Cek koneksi
```
ping 8.8.8.8
```

## Aktifkan asciinema
```
asciinema rec [nama rekaman].cast
```

## Cek partisi
```
lsblk
```
## Luks
```
cryptsetup luksFormat /dev/nvme0n1p8
cryptsetup luksOpen /dev/nvme0n1p8 yaps
```

```
pvcreate /dev/mapper/yaps 
```

## Buat Volume Grup
```
vgcreate yay /dev/mapper/yaps
```

## Buat Logical Volume
```
lvcreate -L 10G yay -n root
lvcreate -L 10G yay -n vars
lvcreate -L 1G yay -n vlog
lvcreate -L 1G yay -n vaud
lvcreate -L 1.5G yay -n vtmp
lvcreate -l50%FREE yay -n home
lvcreate -l100%FREE yay -n podman
```

## Format partisi
```
mkfs.ext4 /dev/yay/root
mkfs.vfat -F32 /dev/partisi boot
mkfs.ext4 /dev/yay/vars
mkfs.ext4 /dev/yay/vlog
mkfs.ext4 /dev/yay/vaud
mkfs.ext4 /dev/yay/vtmp
mkfs.ext4 /dev/yay/home
mkfs.ext4 /dev/yay/podman
```

```
mount /dev/yay/root /mnt
```
```
lsblk
```
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi boot /mnt/boot
mount –mkdir -o rw,nodev,nosuid,relatime /dev/yay/vars /mnt/var
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/yay/vlog /mnt/var/log
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/yay/vaud /mnt/var/log/audit
mount –mkdir -o rw,nodev,nosuid,noexec,relatime /dev/yay/vtmp /mnt/var/tmp
mount –mkdir -o rw,nodev,nosuid,relatime /dev/yay/home /mnt/home
mount –mkdir -o rw,nodev,nosuid,relatime /dev/yay/podman /mnt/var/lib/containers
```

## Install package
```
pacstrap /mnt base amd-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman which
```

## After install
```
genfstab -U /mnt/etc/fstab
```
```
echo ‘“tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/*  /mnt/etc/systemd/network
arch-chroot /mnt
```
```
echo serverapp > /etc/hostname
```

```
## Set waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock –systohc
nvim /etc/locale.gen
```
## Hapus hastag atau aktifkan
```
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1
```
menjadi
```
en_US.UTF-8 UTF-8
en_US ISO-8859-1
```
```
locale-gen
```
```
locale > /etc/locale.conf
nvim /etc/locale.conf
```
```
LANG=C.UTF-8 menjadi (LANG=en_US.UTF-8)
LC_ALL= tambahkan (en_US.UTF-8)
```
```
## Membuat username
```
useradd -m comet
passwd comet
```
```
echo “comet ALL=(ALL:ALL) ALL” > /etc/sudoers.d/comet
su comet
sudo su
enter password
```
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
lsblk
```
```
cat /etc/cmdline.d/01-boot.conf
echo “rd.luks.name=$(blkid -s UUID -o value /dev/partisi boot)=proc root=/dev/yay/root” > /etc/cmdline.d/01-boot.conf
echo rw > /etc/cmdline.d/02-misc.conf
nvim /etc/mkinitcpio.conf
```
```
pacman -S pam_mount
```
```
nvim /etc/security/pam_mount.conf.xml
```

Lalu tambahkan ini dibawah <volume
```
user="comet"
fstype="crypt" 
path="/dev/yay/yaps"  
mountpoint="/home/user"
```

Setelah itu esc
 :wq

```
nvim /etc/pam.d/system-login
```

Masukkan seperti ini
```
#%PAM-1.0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       auth       required   pam_shells.so    
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
session    optional   pam_mount.so
-session   optional   pam_systemd.so   
session    required   pam_env.so      
```

```
nvim /etc/mkinitcpio.conf
```

bagian HOOKS akhir tambahin
```
(sd-encrypt lvm2)
```

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

Sesuaikan seperti ini
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
bootctl –path=/boot install
```

```
mkinitcpio -P
```

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable sshd
```

## Keluar
```
exit
umount -R /mnt
```

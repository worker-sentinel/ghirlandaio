# Connect Wifi

## iwctl
```
station wlan0 get-networks
```
```
station wlan0 connect (nama jaringan)
```
exit
```
ping 8.8.8.8
```
## Membagi Partisi
```
cfdisk /dev/(nama partisi masing masing bisa sda atau nvme0n1p)
```
Bisa disesuaikan dengan penyimpanan yang tersedia

boot= 3G

root= sisanya

```
lsblk
```
## Setup cretae
```
pvcreate /dev/(sda atau nvem0n1p…. (root))
```
```
vgcreate way /dev/(sda atau nvem0n1p…. (root))
```
## Setup LVM
```
lvcreate -L 10G way -n root 
lvcreate -L 10G way -n vars 
lvcreate -L 1G way -n vlog
lvcreate -L 1G way -n vaud
lvcreate -L 1.5G way -n vtmp
lvcreate -L 1.5G way -n home
lvcreate -L 10G way -n podman
lvcreate -l50%FREE way -n lit (bebass)
```
lsblk
```
```
##  Formating LV
```
mkfs.ext4 /dev/way/root
mkfs.ext4 /dev/way/vars
mkfs.ext4 /dev/way/vtmp
mkfs.ext4 /dev/way/vlog
mkfs.ext4 /dev/way/vaud
mkfs.ext4 /dev/way/home
mkfs.ext4 /dev/way/podman
```
## Setup Luks
```
cryptsetup luksFormat /dev/way/lit (bebas)
```
lsblk

## Mounting
```
mount /dev/wc/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/wc/vars /mnt/var
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vlog /mnt/var/log
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/home /mnt/home
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,noexec,nosuid,relatime /dev/wc/podman /mnt/var/lib/containers                                                  
```
lsblk -o name,fstype

## Install Packages
```
pacstrap /mnt base intel-ucode base linux-hardened linux-hardened-headers  linux-firmware  mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman which
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab                                                                                                                      
```
 ## Format tmpfs ke tmp
```
echo "tmpfs  /tmp  tmpfs  defaults,nodev,nosuid,noexec,size=1G  0  0" >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network 
```
## Chroot
```
arch-chroot /mnt
```

## Membuat Hostname
```
echo "server" > /etc/hostname
```
## Mengatur Waktu Dan Bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
>Hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
>Pada LANG=en_US.UTF-8 ubah menjadi LANG=en_US.UTF-8 kemudian dibagian LC_ALL= tambahkan juga en_US.UTF-8
```
```
## luks open
```
cryptsetup luksOpen /dev/way/lit lay
```
```
mkfs.ext4 /dev/mapper/lay
```
```
lsblk
```
## Menambahkan User
```
mkdir /home/user
```
```
useradd -d /home/user milky
```
```
chown -R milky:milky /home/user
```

## Mengatur Password User
```
passwd milky
```
## Konfigurasi Sudo
```
echo 'milky ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/milky
```
```
nvim /etc/security/pam_mount.conf.xml
```
tambahin teks dibawah logout

><volume

user="milky"

fstype="crypt"

path="/dev/wc/lit"

mountpoint="/home/user" 
```
nvim /etc/pam.d/system-login
```
                                                                                                                                 session    optional   pam_keyinit.so       force revoke                                                                                                                   session    include    system-auth                                                                                                                                         session    optional   pam_lastlog2.so      silent                                                                                                                         session    optional   pam_motd.so                                                                                                                                         session    optional   pam_mail.so          dir=/var/spool/mail standard quiet                                                                                             session    optional   pam_umask.so                                                                                                                                        session    optional   pam_mount.so         (tambahan)                                                                                                                               -session   optional   pam_systemd.so                                                                                                                                      session    required   pam_env.so                                                                                                                                          ~                                                   

## mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
>Pada hooks paling bawah, tambahkan lvm2
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole lvm2 block filesystems fsck)  
```
nvim /etc/mkinitcpio./linux-hardened.preset
```
>Hapus semua pagar pada baris ALL, kemudian ubah baris pertama ALL jadi "/etc/mkinitcpio.d/default.conf", untuk dua baris setelahnya tambahkan 'kernel' setelah 'boot'.

>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus 'EFI' dan ubah 'Linux' jadi 'linux'


```
lsblk
```
bootctl --path=/boot install
```
```
## mkinitcpio
```
mkinitcpio -P
```
## Kernel
```
rm -fr /etc/cmdline.conf                                                                                                                              mkdir /etc/cmdline.d                                                                                                                                  nvim /etc/cmdline.d/01-boot.conf  
>ketik root=/dev/way/root rw
```
```
systemctl enable iwd
```
systemctl enable systemd-networkd
```
systemctl enable systemd-resolved
```
systemctl enable firewalld 
```
systemctl enable sshd   
```
systemctl enable --global podman 
```
```
# Selesai
```
exit
```
umount -R /mnt
```
reboot
```

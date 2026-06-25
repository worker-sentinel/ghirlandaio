# HOW TO INSTALL OS SERVER 1

## 1. Cek partisi
```
lsblk
```
## 2. Physical volume
```
pvcreate /dev/sda6
```
## 3. Volume group
```
vgcreate server1 /dev/sda6
```
## 4. Logical volume
```
lvcreate -L 5G server1 -n root
lvcreate -L 5G server1 -n vars
lvcreate -L 1G server1 -n vlog
lvcreate -L 1G server1 -n vaud
lvcreate -L 1.5G server1 -n vtmp
lvcreate -L 1G server1 -n home
lvcreate -L 9G server1 -n podman
```
```
lsblk
```
```
lvcreate -l50%FREE server1 -n user
```
```
mkfs.vfat -F32 -n BOOT /dev/sda5
```
```
mkfs.ext4 /dev/server1/root
mkfs.ext4 /dev/server1/vars
mkfs.ext4 /dev/server1/vlog
mkfs.ext4 /dev/server1/vaud
mkfs.ext4 /dev/server1/vtmp
mkfs.ext4 /dev/server1/home
mkfs.ext4 /dev/server1/podman
```
```
lsblk
```
```
cryptsetup luksFormat /dev/server1/test
```
```
YES
```
```
mount /dev/server1/root /mnt  
mount --mkdir -o uid=0,gid=0,dmask=0077,fmask=0077 /dev/sda5 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/server1/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server1/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server1/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server1/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server1/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/server1/podman /mnt/var/lib/containers
```
```
lsblk -o name,fstype
```
```
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 openssh podman pacman sudo wget git neovim iwd grepwhich firewalld 
```
```
genfstab -U /mnt > /mnt/etc/fstab 
```
```
echo "/tmpfs /tmp tmpfs defaults,nosuid,nodev,noexec,size=1G  0 0" >> /mnt/etc/fstab 
```
```
arch-chroot /mnt 
```
```
pacman -S pam_mount 
```
```
nvim /etc/hostname 
```
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime 
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen  
```
```
Uncommenting en_US
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
```
Tambahkan en_US di konfig yang atas dan bawah
```
```
cryptsetup luksOpen /dev/server1/user user
```
```
mkdir /home/user
```
```
useradd -d /home/user hakim
```
```
chown -R hakim:hakim /home/user 
```
```
passwd hakim
```
```
nvim /etc/sudoers.d/ 
```
```
echo "hakim ALL=(ALL:ALL) ALL" /etc/sudoers.d/hakim
```
```
nvim /etc/security/pam_mount.conf.xml 
```
## Tambahkan konfig dibawah <logout
```
<volume
    user="hakim"fstype="crypt"
    path="/dev/system/user"
    mountpoint="home/user"
/>
```
```
nvim /etc/pam.d/system-login
```
## Tambahkan konfig
```
auth   required   pam_mount.so 
session    optional   pam_mount.so
```
```
nvim /etc/mkinitcpio.conf  
```
```
Tambahkan konfig di HOOKS lvm2
```
```
exit
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```
arch-chroot /mnt  
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
```
Uncommenting yang ALL
```
```
Di default_uki, efi ganti dengan boot
```
```
exit 
```
```
bootctl --path=/mnt/boot install
```
```
arch-chroot /mnt   
```
```
nvim /etc/cmdline.conf
```
```
mkinitcpio -P  
```
```
rm -fr /etc/cmdline.conf  
```
```
mkdir /etc/cmdline.d 
```
```
nvim /etc/cmdline.d/01-boot.conf 
```
```
Tambahkan konfig root=/dev/server1/root rw
```
```
mkinitcpio -P 
```
```
systemctl enable iwd
```
```
systemctl enable systemd-networkd 
```
```
systemctl enable systemd-resolved  
```
```
pacman -S openssh
```
```
systemctl enable sshd 
```
```
systemctl enable --global podman
```
```
exit
```




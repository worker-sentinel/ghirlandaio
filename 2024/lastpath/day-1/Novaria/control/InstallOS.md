# HOW TO INSTALL OS


## 1. Sambung ke jaringan
```
iwctl
station wlan0 get-networks
station wlan0 connect "WIFI"
exit
ping 8.8.8.8
```
## 2. Cek partisi
```
lsblk
```
## 3. Physical Volume 
```
pvcreate /dev/nvme0n1p7
```
## 4. Volume group
```
vgcreate server1 /dev/nvme0n1p7
```
## 5. Logocal volume
```
lvcreate -L 5G server1 -n root
lvcreate -L 5G server1 -n vars
lvcreate -L 1G server1 -n vlog
lvcreate -L 1G server1 -n vaud
lvcreate -L 1.5G server1 -n vtmp
lvcreate -L 1G server1 -n home
lvcreate -L 9G server1 -n podman
```
## 6. Format
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p6
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
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 openssh podman pacman sudo wget git neovim iwd grepwhich firewalld
```




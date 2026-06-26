# Install OS Admin
```
Sudo Cryptsetup luksFormat /dev/nvme0n1p5
YES
Masukin passwd
```
```
Cryptsetup luksOpen /dev/nvme0n1p5 novaria 
Masukin passwd
```
```
Pvcreate /dev/mapper/Novaria
```
```
Vgcreate kel4 /dev/mapper/Novaria 
```
```
Lvcreate -L 8G kel4 -n root
Mkfs.ext4 -b 4-96 /dev/kel4/root
Mount /dev/kel4/root /mnt
```
```
Mkfs.vfat -F32 -n BOOT /dev/nvme0n1p4
Mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p4 /mnt/boot
```
```
Lvcreate -L 8G kel4 -n vars
Mkfs.ext4 -b 4096 /dev/kel4/vars
Mount --mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vars /mnt/var
lvcreate -L 1G kel4 -n vlog
mkfs,ext4 -b 4096 /dev/kel4/vtmp
mount –mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vtmp /mnt/var/tmp
lvcreate -L 1G kel4 -n vlog
mkfs.ext4 -b 4096 /dev/kel4/vlog
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log
lvcreate -L 1G kel4 -n vaud
mkfs.ext4 -b 4096 /dev/kel4/vaud
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log/audit
lvcreate -L 8G kel4 /dev/kel4/home
mkfs.ext4 -b 4096 /dev/kel4/home
mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/home /mnt/home
```
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base base=devel neopvim firewalldopenssh
```
```
iwctl
station wlan0 connect “Kelawai Ciputat 01_ 5G”
exit 
systemctl enable iwd
systemctl start iwd
systemctl enable system-networkd
systemctl start system-networkd
systemctl restart system-networkd
iwctl
```

# CONNECT WIFI

```
iwctl
```
> untuk mengelola koneksi WIFI 
```
station wlan0 scan
```
> untuk memindai jaringan WIFI di sekitar
```
station wlan0 get-networks
```
> untuk menampilkan daftar WIFI yang ditemukan
```
station wlan0 connect "nama_jaringan"
```
> untuk menghubungkan ke jaringan wifi, kemudian dilanjutkan dengan memasukkan password
```
exit
```
untuk keluar dari iwctl
## TEST JARINGAN
```
ping google.com
```
> untuk memastikan sudah terhubung WIFI
#  PARTISI

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi]
```
> untuk membentuk layout yang akan diinstall
### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 3G [EFI system]
system = 50G [linux filesystem]
```
> membuat partisi EFI sebesar 3 GB dan membuat partisi utama Linux sebesar 50 GB
#   SETUP LUKS

```
cryptsetup luksFormat --sector-size 4096 /dev/partisi_system
```
> untuk menyesuaikan format partisi dengan file system yang sudah dibuat
```
cryptsetup luksOpen /dev/partisi_system [nama partisi]
```
> untuk membuka partisi yang sudah diformat
```
pvcreate /dev/mapper/[nama partisi]
```
> membuat physical volume
```
vgcreate [nama grup] /dev/mapper/[nama partisi]
```
> membuat volume grup
#  SETUP LVM
##  root
lvcreate -L 15G [nama grup] -n root
```
> membuat logical volume dan memberikan size pada root
```
mkfs.ext4 /dev/[nama grup]/root
```
> membuat format file system dengan ext4 untuk partisi root
```
mount /dev/pudding/root /mnt
```
> membuat mounting untuk partisi mnt
```
## boot
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
> membuat format 
```
> mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
```
> membuat mounting partisi boot
```
## vars
lvcreate -L 15G [nama grup] -n vars
```
> membuat logical volume dan memberikan size pada vars
```
mkfs.ext4 /dev/[nama grup]/vars
```
> membuat format file system dengan ext4 untuk partisis vars
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
>  membuat mounting var, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,
```
## tmp 
lvcreate -L 2G [nama grup] -n tmp
```
> membuat logical volume dan memberikan size pada tmp
```
mkfs.ext4 /dev/[nama grup]/tmp
```
> membuat format file system dengan ext4 untuk partisi tmp
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/tmp /mnt/tmp
```
>  membuat mounting tmp, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,
```
## vtemp
lvcreate -L 1G [nama grup] -n vtemp
```
> membuat logical volume dan memberikan size pada vtemp
```
mkfs.ext4 /dev/[nama grup]/vtemp
```
> membuat format file system dengan ext4 untuk partisi vtemp
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtemp /mnt/var/tmp
```
> membuat mounting untuk partisi var tmp,  nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,  noexec untuk tidak dapat menjalankan semua binary di partisi tersebut

```
## vlog
lvcreate  -L 1G pudding -n vlog
```
> membuat logical volume dan memberikan size pada vlog
```
mkfs.ext4 /dev/[nama grup]/vlog
```
> membuat format file system dengan ext4 untuk partisi vlog
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
> membuat mounting vlog, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,  
```
## vaud
lvcreate -L 1G [nama grup] -n vaud
```
> membuat  logical volume dan memberikan size pada vaud
```
mkfs.ext4 /dev/[nama grup]/vaud
```
> membuat format file system dengan ext4 untuk partisi vaud
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
>  membuat mounting untuk partisi audit,  nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,  noexec untuk tidak dapat menjalankan semua binary di partisi tersebut
```
## podman
lvcreate -L 10G [nama grup] -n podman
```
> membuat  logical volume dan memberikan size pada podman
```
mkfs.ext4 /dev/[nama grup]/podman
```
> membuat format file system dengan ext4 untuk partisi podman
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```
> membuat mounting podman, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut, relatime untuk menghemat dan mempercepat kinerja hard disk
```
## home 
lvcreate -l70%FREE [nama grup] -n home
```
> membuat  logical volume dan memberikan size pada home
```
mkfs.ext4 /dev/[nama grup]/home
```
> membuat format file system dengan ext4 untuk partisi home
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/home /mnt/home
```
> membuat mounting home, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut, relatime untuk menghemat dan mempercepat kinerja hard disk
```
# Install Package
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema linux-firmware-realtek firewalld
```
>
```
# Regist Partisi

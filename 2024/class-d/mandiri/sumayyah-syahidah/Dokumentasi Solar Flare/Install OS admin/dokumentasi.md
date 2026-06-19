# Konfigurasi dan Instalasi Server Linux

## 1.Pemeriksaan struktur partisi disk
```
cfdisk /dev/nvme0n1
```
> tujuannya disini tuh buat ngeliat dan kelola partisi pada media penyimpanan yang akan digunakan sebagai server dengan alamat IP 10.10.3.4
## 2.Membuka partisi terenkripsi LUKS
```
cryptsetup luksOpen /dev/nvme0n1p6 server
```
> nah disini kita buka partisi terenkripsi biar bisa digunain buat media penyimpanan server
## 3.Membuat physical volume (PV)
```
pvcreate /dev/mapper/server
```
> tujuannya buat menginisialisasi partisi terenkripsi sebagai physical volume untuk kebutuhan LVM
## 4.Membuat Volume Group (VG)
```
vgcreate SERVER /dev/mapper/server
```
> disini bikin volume group bersama SERVER sebagai wadah logical volume
## 5.Membuat Logical Volume Root
```
lvcreate -L 8G SERVER -n root
```
> tujuan: bikin partisi root untuk sistem operasi server
## 6.Membuat Logical Volume Var
```
lvcreate -L 8G SERVER -n vars
```
> tujuan: sediain ruang penyimpanan direktori /var
## 7.Membuat Logical Volume Log
```
lvcreate -L 1G SERVER -n vlog
```
> tujuan: sediain partisi khusus penyimpanan log sistem
## 8.Membuat Logical Volume Temporary
```
lvcreate -L 1G SERVER -n vtmp
```
> tujuan: sediain ruang penyimpanan sementara pada direktori /var/tmp
## 9.Membuat Logical Volume Audit 
```
lvcreate -L 1G SERVER -n vaud
```
> tujuan: saving log audit keamanan sistem
## 10.Membuat Logical Volume Home
```
lvcreate -L 5G SERVER -n home
```
> tujuan: sediain ruang penyimpanan direktori home pengguna
## 11.Membuat Logical Volume Podman
```
lvcreate -L 5G SERVER -n podman
```
> tujuan: sediain penyimpanan khusus untuk container podman
## 12.Format Partisi Boot
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p5
```
> tujuan: bikin filesystem FAT32 untuk partisi boot server
## 13.Format Filesystem Root
```
mkfs.ext4 /dev/SERVER/root
```
> tujuan: bikin filesystem EXT4 pada partisi root
## 14.Format Filesystem Var
```
mkfs.ext4 /dev/SERVER/vars
```
> tujuan: bikin filesystem EXT4 pada partisi /var
## 15.Format Filesystem Var Tmp
```
mkfs.ext4 /dev/SERVER/vtmp
```
> tujuan: bikin filesystem EXT4 pada direktori sementara
## 16.Format Filesystem Audit
```
mkfs.ext4 /dev/SERVER/vaud
```
> tujuan: bikin filesystem buat penyimpanan audit log
## 17.Format Filesystem Log
```
mkfs.ext4 /dev/SERVER/vlog
```
> tujuan: bikin filesystem khusus log sistem
## 18.Format Filesystem Podman
```
mkfs.ext4 /dev/SERVER/home
```
> tujuan: bikin filesystem direktori home pengguna
## 19.Format Filesystem Podman
```
mkfs.ext4 /dev/SERVER/podman
```
> tujuan: bikin filesystem buat penyimpanan container
## 20.Verifikasi Struktur Penyimpanan
```
lsblk
```
> tujuan: wes pokokne cek ulang tuh yang udh dibuat td aph aja
## 21.Mount Root Filesystem
```
mount /dev/SERVER/root /mnt
```
> tujuan: pasang partisi root ke direktori instalasi
## 22.Mount Partisi Boot
```
mount --mkdir /dev/nvme0n1p5 /mnt/boot
```
> tujuan: pasang partisi boot ke sistem instalasi
## 23.Mount Direktori Var
```
mount --mkdir /dev/SERVER/vars /mnt/var
```
> tujuan: pasang partisi /var
## 24.Mount Direktori Var Tmp
```
mount --mkdir /dev/SERVER/vtmp /mnt/var/tmp
```
> tujuan: pasang partisi /var/tmp
## 25.Mount Direktori Log
```
mount --mkdir /dev/SERVER/vlog /mnt/var/log
```
> tujuan: pasang partisi log sistem
## 26.Mount Direktori Audit
```
mount --mkdir /dev/SERVER/vaud /mnt/var/log/audit
```
> tujuan: pasang partisi audit log
## 27.Mount Direktori Home
```
mount --mkdir /dev/SERVER/home /mnt/home
```
> tujuan: pasang partisi home pengguna
## 28.Mount Direktori Podman
```
mount --mkdir /dev/SERVER/podman /mnt/var/lib/containers
```
> tujuan: pasang partisi penyimpanan container
## 29.Verifikasi Hasil Mount
```
lsblk
```
> tujuan: cek ulang wae
## 30.Instalasi Sistem Operasi dan Paket yang Diperlukan
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose firefox
```
> tujuan: instal dah tu sistem operasi arch linux bareng paket berkaitan buat server dengan alamat IP 10.10.3.4

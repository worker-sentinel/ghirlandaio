# MENYALAKAN ASCIINEMA
```
asciinema rec namafile.cast (nama file bebas)
```
# CONNECT WIFI

```bash
iwctl
```
#### cek driver wifi setiap laptop
---
```bash
device list
```
#### melihat jaringan yang tersedia
---
```bash
station (driver wifi) get-network
```
#### memindai jaringan
---
```bash
station (driver wifi) scan
```
 
#### menghubungkan ke jaringan
---
```bash
station {device wifi} connect "{nama wifi}"
```
```
exit
```

# Memeriksa jaringan 

```bash
ping 1.1.1.1
```



****

# CEK PARTISI
```
lsblk
```

## BAGI PARTISI
```
cfdisk /dev/[partisi] (sesuaikan dengan laptop, sda/nvme0n1)
```

### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 2G [EFI system]
root = 48G [linux filesystem/]. (sisanya)
```

```
lsblk (lagi)
```
****
### ENKRIPSI PARTISI DENGAN LUKS
#### Enkripsi Partisi
```
cryptsetup luksFormat /dev/sda6 (sesuaikan dengan partisi yg paling besar / root)
```
#### Membuka partisi terenkripsi dan membuat pemetaan device yang dipasang ke partisi terenkripsi luks
```
cryptsetup luksOpen /dev/sda6 amelia (partisi root & nama device)
```
### Membuat LVM
#### Physical Vlume (PV)
```
pvcreate /dev/mapper/amelia (nama devuce)
```
#### Volume Group (VG)
```
vgcreate system /dev/mapper/amelia (system bebas)
```
#### Logical Volume (LV)
```
lvcreate -L 10G system -n root
lvcreate -L 10G system -n vars
lvcreate -L 1G system -n vlog
lvcreate -L 1G system -n vaud
lvcreate -L 1G system -n vtmp
lvcreate -L 10G system -n home
lvcreate -l50%FREE system -n dock
```
### Memformat Lv yang sudah dibuat
```
mkfs.vfat -F32 -n BOOT /dev/sda5 (partisi boot/yg kecil)
mkfs.ext4 /dev/system/root
mkfs.ext4 /dev/system/vars
mkfs.ext4 /dev/system/vlog
mkfs.ext4 /dev/system/vaud
mkfs.ext4 /dev/system/vtmp
mkfs.ext4 /dev/system/home
mkfs.ext4 /dev/system/dock
```
### Mount root
```
mount /dev/system/root /mnt
```
### Mount boot / EFI Partition
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot
```
### Mounting
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/home /mnt/home
```
#### Mount lv ke docker
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/dock /mnt/var/lib/docker
```
```
lsblk
```
untuk cek lagi 
### Instalasi Sistem Dasar Blackbird Amanda Linux
```
pacstrap /mnt base linux-hardened linux-hardened-headers intel-ucode linux-firmware neovim lvm2 mkinitcpio sudo pacman curl which iwd grep firewalld
```
(sesuaikan jenis processor laptop klau amd amd-ucode)

### File system table (fstab)
```
gen fstab -U /mnt > /mnt/etc/fstab
```
#### Konfigurasi tmpfs
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
#### copy konfigurasi jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
### Masuk ke chroot
```
arch-chroot /mnt
```
### Konfigurasi Hostname
```
nvim /etc/hostname
```
ketik i untuk insert kemudian masukkan nama hostname kalian, klik ESC kemudian ketik :wq untuk keluar

### Set Localtime
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

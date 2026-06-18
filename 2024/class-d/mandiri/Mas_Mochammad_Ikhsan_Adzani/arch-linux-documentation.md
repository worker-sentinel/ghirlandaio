# Dokumentasi install arch manual
## jika sudah masuk kedalam install arch linux nya langsung ikutin langkah dibawah

# CHECKING PARTISI
## jika ingin melihat partisi beserta type nya
```
lsblk -o name,fstype,size 
```
## Jika ingin melihat partisinya saja
```
lsblk
```

# MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```

### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 1G [EFI system]
swap = 4G [linux swap]
root = 460G [linux filesystem]
```

# CONNECT WIFI

```bash
iwctl
```
---
```bash
device list
```
#### Catatan : Untuk cek driver wifi setiap laptop
---
```bash
station (driver wifi) get-network
```
#### Catatan : untuk melihat jaringan yang tersedia
---
```bash
station (driver wifi) scan
```
#### Catatan : Untuk memindai jaringan yang ada
---
```bash
station {device wifi} connect "{nama wifi}"
```
```
exit
```
#### Catatan : Untuk menghubungkan ke jaringan yang sudah ditentukan



# Memeriksa jaringan 

```bash
ping 1.1.1.1
```
# jika sudah masuk kedalam install arch linux nya langsung ikutin langkah dibawah


****

# CHECKING PARTISI
## jika ingin melihat partisi beserta type nya
```
lsblk -o name,fstype,size 
```
## Jika ingin melihat partisinya saja
```
lsblk
```

## MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```

### MINIMAL PARTISI 
#### **MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA**
```
boot = 1G [EFI system]
root = 4G [linux swap]
home = 40G [linux filesystem/linux server data]
```

#### kalo salah satu partisi PENTING ke hapus, langsung QUIT aja jangan di WRITE

```
lsblk (lagi)
```
****

# FORMATING

## BOOT
```
mkfs.fat -F32 /dev/[partisi boot]
```

## ROOT
```
mkfs.ext4 /dev/[partisi root]
```
# format swap dan mengaktifkannya
```
mkswap /dev/[partisi swap]
```
```
swapon /dev/[partisi swap]
```
****

# MOUNTING

## MOUNTING ROOT
```
mount /dev/[pertisi root] /mnt
```

## MOUNTING BOOT

```
mkdir /mnt/boot
```

```
mount /dev/[partisi boot] /mnt/boot
```
****

# INSTALL CORE

```
pacstrap -K /mnt base base-devel linux linux-firmware iwd git neovim 
```
****

# FSTAB

```
genfstab -U /mnt > /mnt/etc/fstab
```
****

## ARCH-CHROOT
```
arch-chroot /mnt
```

# USER KOMPUTER

## jika 1 kata tidak perlu pake `""` kalo lebih menggunakan petik `""`
```
echo [nama komputer] > /etc/hostname
```

# LOCALTIME
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
****
# LOCALE

```
nvim /etc/locale.gen
```

## lalu pencarian di nvim menggunakan `/`

```
lalu uncommenting kedua en_US
```

### generate bahasa yg di uncommenting 
```
locale-gen
```

```
locale > /etc/locale.conf
```

# config locale
```
nvim /etc/locale.conf
```
## config file locale 
```
isi lang=C menjadi lang=en_US.UTF-8
dan isi ALL=en_US.UTF-8
```
****
# USERADD
```
useradd -m [user]
passwd [user]
```
```
echo 'nama_user ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
```
****
# Prepare BOOT dengan GRUB
```
grub-install --target=x86_64-efi --efi-directory=[letak direktori yang menyimpan kernal dan image] --bootloader-id=GRUB```
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```


# Generate image awal linux
```
mkinitcpio -P
```

****
# UNMOUNT

```
umount -R /mnt
```

```
reboot
```

# Jika sudah berada dalam fresh install, kita akan install plasma

```
systemctl enable NetworkManager
```

```
systemctl start NetworkManager
```

```
nmtui
```
> pilih wifi yang ingin di hubungkan

```
sudo pacman -S plasma sddm pipewire pipewire-alsa pipewire-jack kitty firefox
```

```
systemctl enable sddm
```
```
reboot
```

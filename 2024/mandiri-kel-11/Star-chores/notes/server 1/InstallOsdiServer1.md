## konek internet
jika menggunakan kabel lan tinggal colok


jika menggunakan wifi:
iwctl
```
iwctl
```
```
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect NamaWifi
```
alternatif jika phy0 belum dinyalakan
Keluar dulu dari iwctl:
```
exit
```
```
rfkill list
```
Kalau ada tulisan Soft blocked: yes atau Hard blocked: yes
```
rfkill unblock all
rfkill unblock wifi
```
```
ip link set wlan0 up
```
Update sistem waktu
```
timedatectl
```

## Partisi Disk
Di live environment anda dapat melihat disk yang dibagi menjadi *block device* seperti /dev/sda, /dev/nvme0n1. Untuk mengidentifikasi device ini gunakan lsblk atau fdisk
```
fdisk /dev/nvme0n1
```
buat satu disk untuk lvm dan boot jika dibutuhkan


### lalu format disk

jika buat boot, jangan diformat ulang kalau sebelumnnya sudah ada karena dapat merusak sistem
### untuk boot
```
mkfs.fat -F 32 /dev/nvme0n1p4
```
lalu buat folder dan mount bootnya


buat enkripsi
```
sudo cryptsetup luksFormat /dev/nvme0n1p5
```
liat status
```
sudo cryptsetup status /dev/nvme0n1p5
```
open untuk membuat virtual device yang mapping enkripsi (cryptroot=nama virtual) 
```
cryptsetup open /dev/nvme0n1p5 cryptroot
```
untuk lihat luks header information
```
cryptsetup luksDump /dev/nvme0n1p5
```

lalu buat lvm

untuk pv contoh di disk saya
```
pvcreate /dev/mapper/cryptdisk
```
untuk vg
```
vgcreate sawit /dev/mapper/cryptdisk
```
buat lv sesuai layout cis
```
lvcreate -L 30G sawit -n lroot
lvcreate -L 8G  sawit -n lvar
lvcreate -L 4G  sawit -n lvtmp
lvcreate -L 4G  sawit -n lvlog
lvcreate -L 2G  sawit -n lvaud
lvcreate -L 20G sawit -n docker
lvcreate -l 100%FREE sawit -n home
```
liat lvm
```
sudo lvdisplay
```
format partisi
```
mkfs.ext4 /dev/sawit/lroot
mkfs.ext4 /dev/sawit/lvar
mkfs.ext4 /dev/sawit/lvtmp
mkfs.ext4 /dev/sawit/lvlog
mkfs.ext4 /dev/sawit/lvaud
mkfs.ext4 /dev/sawit/docker
mkfs.ext4 /dev/sawit/home
```
tambahkan format boot jika perlu
## Mount partisi
di laptop saya
```
mount /dev/sawit/lroot /mnt
mount --mkdir -o rw,nodev,nosuid,relatime /dev/sawit/lvar /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sawit/lvtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sawit/lvlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sawit/lvaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sawit/docker /mnt/var/lib/containers
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sawit/home /mnt/home
```
mount bootloader jika belum



## Mirror
jika diperlukan semisalnya downloadnya masih lambat
```
nano /etc/pacman.d/mirrorlist
```
## Install packages penting
paket yang sudah dikustomisasi untuk laptop saya
```
pacstrap -K /mnt base linux-hardened amd-ucode mesa linux-firmware networkmanager nano sudo grub efibootmgr os-prober lvm2 cryptsetup git nvim asciinema
```
### fstab
agar Agar nanti setelah Arch Linux di-boot, sistem tahu format partisi disk yang disk ada dimana
dibuatlah file
```
/etc/fstab
```
```fstab``` adalah daftar partisi yang harus otomatis dimount saat Linux menyala
```
genfstab -U /mnt >> /mnt/etc/fstab
```
### chroot
### untuk ke system environmt 
```
arch-chroot /mnt
```
set time
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

### localization
karena sudah install nano

masuk ke folder
```
nano /etc/locale.gen
```
dibuka komentarnya 
```
en_US.UTF-8 UTF-8
```
jalankan
```
locale-gen
```

ketik
```
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```
hostname
```
nano /etc/hostname
```
adduser
```
useradd -m -G wheel -s /bin/bash kautsar
```
masuk
```
EDITOR=nano visudo
```
uncomment
```
# %wheel ALL=(ALL:ALL) ALL
```
EDITOR=nano visudo
```
passwd
```
nano

### ketik nama hostname
### start networkmanager
```
systemctl enable NetworkManager
```
root password
```
passwd
```
### boot loader

### install grub package
sesuaikan dengan folder boot
di laptop saya
```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=slims
```
sesuaikan directory sesuai kebutuhan bootloader


update grub
```
echo $(blkid -o UUID -s value /dev/nvme0n1p5 >> /etc/default/grub
```
``` 
nano /etc/default/grub
```
buat seperti ini
cut uuidnya lalu dibagian ini ketik seperti itu
cmmd defaut grub
```
rd.luks.name=device-UUID=cryptroot root=/dev/mapper/cryptroot
```
paste uuid dibagian ini ```device-UUID```
```
grub-mkconfig -o /boot/grub/grub.cfg
```

### initframs

masuk ke 
```
nano /etc/mkinitcpio.conf
```
lalu cari sperti ini 
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard block lvm2 filesystems fsck)
```
tambahkan cryptsetup setelah lvm
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard block sd-encrypt lvm2 filesystems fsck)
```

```
mkinitcpio -P
```

Use the ```grub-mkconfig tool``` to generate ```/boot/grub/grub.cfg```
nyalakan os-prober
```
nano /etc/default/grub
```
setting sendiri malas ngetik


update bootloader
```
grub-mkconfig -o /boot/grub/grub.cfg
```
Reboot
```
exit
```
```
umount -R /mnt
```
jangan lupa stop dan upload asciinema
```
reboot

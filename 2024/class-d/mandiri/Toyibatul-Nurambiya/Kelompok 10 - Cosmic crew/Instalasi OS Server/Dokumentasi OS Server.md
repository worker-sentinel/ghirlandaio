# Connect Wifi
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 scan
```
```
station wlan0 connect (nama jaringan)
```
```
exit
```
```
ping 8.8.8.8
```

# Partisi
## Melihat Partisi
```
lsblk
```
## Membagi Partisi
```
cfdisk /dev/(nama partisi masing masing bisa sda atau nvme0n1p)
```
Sesuaikan dengan penyimpanan yang ada
```
boot= 3G
root= 56G
```
```
lsblk
```

# Setup Luks
```
cryptsetup luksFormat /dev/p.root
```
```
cryptsetup luksOpen /dev/p.root proc (nama device - boleh apa aja)
```
```
pvcreate /dev/mapper/proc
```
```
vgcreate slims /dev/mapper/proc
```

# Setup LVM
```
lvcreate -L 15G slims -n root 
lvcreate -L 10G slims -n vars 
lvcreate -L 1G slims -n vlog
lvcreate -L 1G slims -n vaud
lvcreate -L 1G slims -n vtmp
lvcreate -L 8G slims -n home
lvcreate -l70%FREE slims -n podman 
lsblk
```

# Formating LV
```
mkfs.vfat -F32 -n BOOT /dev/p.boot
mkfs.ext4 /dev/slims/root
mkfs.ext4 /dev/slims/vars
mkfs.ext4 /dev/slims/vtmp
mkfs.ext4 /dev/slims/vlog
mkfs.ext4 /dev/slims/vaud
mkfs.ext4 /dev/slims/home
mkfs.ext4 /dev/slims/podman
lsblk
```

# Mounting
```
mount /dev/slims/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/p.boot /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/vars /mnt/var         
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vtmp /mnt/var/tmp     
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vlog /mnt/var/log  
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vaud /mnt/var/log/audit      
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/podman /mnt/var/lib/container
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/home /mnt/
lsblk
```

# Install Packages
```
lspci
```
>Untuk melihat jenis hardware

AMD
```
pacstrap /mnt amd-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel podman neovim git networkmanager asciinema linux-firmware-realtek firewalld
```

# Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

# Format tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

# Chroot
```
arch-chroot /mnt
```

# Membuat Hostname
```
echo "cosmic-crew" > /etc/hostname
```

# Mengatur Waktu Dan Bahasa
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

# Membuat User
## Membuat Home Directory
```
mkdir -p /home/user
```
## Menambahkan User
Menambahkan user 'kel10' dengan home directory
```
useradd -d /home/user kel10
```
## Mengatur Password User
```
passwd
```
## Mengatur Hak Kepemilikan Home Directory
```
chown -R kel10:kel10 /home/user
```
## Mengatur Password User
```
passwd kel10
```
## Konfigurasi Sudo
```
echo 'kel10 ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/kel10
```
```
cat /etc/sudoers.d/kel10
```

# Kernel Parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/p.root)=proc root=/dev/slims/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
# mkinitcpio
```
cd /boot
```
```
mkdir kernel efi
```
```
cd efi
```
```
mkdir linux
```
```
cd ..
```
```
mv vmlinuz-linux-lts amd-ucode.img kernel
```
```
rm -fr inittramfs-linux-lts.img
```
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
>Pada hooks paling bawah, tambahkan lvm2 sd-encrypt setelah sd-vconsole
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>Hapus semua pagar pada baris ALL, kemudian ubah baris pertama ALL jadi "/etc/mkinitcpio.d/default.conf", untuk dua baris setelahnya tambahkan 'kernel' setelah 'boot'.

>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus 'EFI' dan ubah 'Linux' jadi 'linux'
```
mkinitcpio -P 18L, 639B written
```
```
bootctl --path=/boot install
```
```
systemctl enable NetworkManager
```
```
systemctl enable systemd-networkd.socket
```
```
systemctl enable systemd-resolved
```

# Selesai
```
exit
```
```
umount -R /mnt
```
```
lsblk
```
```
reboot
```

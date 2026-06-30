
### Recording Asciinema 
```
asciinema rec [bebas dah].cast 
```
### Membagi Partisi 
Untuk pembagian partisi disesuaikan dengan aplikasi yang akan di install

Cek partisi: 
```
lsblk 
```

```
cfdisk /dev/[sesuai jenis partisi]  
```

Partisi Boot : 4G (EFI System)
Partisi LVM : sisanya (Linux File System)

Cek partisi: 
```
lsblk 
```

### Menghubungkan WIFI 

```
iwctl
device list 
station wlan0 get-networks
station wlan0 connect [nama wifi]
```
### 
```
cryptsetup luksFormat /dev/[sesuai dengan partisi yang sudah di buat untuk system] 
```
```
cryptsetup luksOpen /dev/[sesuai dengan partisi yang sudah di luksformat] orion 
```
### Membuat Physical Volume 
```
pvcreate /dev/mapper/orion 
```
### Membuat Volume Grup 
```
vgcreate belt /dev/mapper/orion 
```
### Membuat Logical Volume 
bisa disesuaikan dengan kebutuhan
```
lvcreate -L 10G belt -n root
lvcreate -L 10G belt -n vars
lvcreate -L 1G belt -n vlog
lvcreate -L 1G belt -n vaud 
lvcreate -L 1,5G belt -n vtmp
lvcreate -L 10G belt -n home
lvcreate -l50FREE% belt -n db
lvcreate -l100FREE% belt -n podman
```
### Memformat Partisi 

Partisi root
```
mkfs.ext4 /dev/belt/root
```
Partisi boot 
```
mkfs.vfat -F32 /dev/nvme0n1p8
```
Partisi vars 
```
mkfs.ext4 /dev/belt/vars
```
Partisi vlog 
```
mkfs.ext4 /dev/belt/vlog
```
Partisi vaud 
```
mkfs.ext4 /dev/belt/vaud
```
Partisi vtmp 
```
mkfs.ext4 /dev/belt/vtmp
```
Partisi home 
```
mkfs.ext4 /dev/belt/home 
```
Partisi mysql
```
mkfs.ext4 /dev/belt/db
```
Partisi Podman
```
mkfs.ext4 /dev/belt/podman 
```
setelah memformat, lakukan cek partisi
```
lsblk
```
### Mounting Partisi 

Partisi root 
```
mount /dev/belt/root /mnt
```

Partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p8 /mnt/boot
```

Partisi vars
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/vars /mnt/var
```

Partisi vlog
```
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/belt/vlog /mnt/var/log
```

Partisi vaud
```
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/belt/vaud /mnt/var/log/audit
```

Partisi vtmp
```
mount --mkdir -o rw,nosuid,nodev,      relatime /dev/belt/vtmp /mnt/var/tmp
```

Partisi home
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/home /mnt/home
```

Partisi mysql
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/db /mnt/var/lib/mysql
```
Partisi podman
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/db /mnt/var/lib/containers
```
setelah mounting, lakukan cek partisi
```
lsblk
```
### Installasi Package
```
pacstrap /mnt base linux-hardened linux-hardened-headers lvm2 mkinitcpio sudo pacman iwd firewalld linux-firmware audit lynis intel-ucode neovim git asciinema 
```
### Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Mounting RAM
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab 
```
### Konfigurasi Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
Masuk arch chroot
```
arch-chroot /mnt 
```
### Membuat Hostname
```
echo "kel5" > /etc/hostname
```
### Setting Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
### Generate Waktu
```
hwclock --systohc
```
### Setting Bahasa
```
nvim /etc/locale.gen
```
Search: /en_US lalu enter, 
Mode insert (i), 
Hapus # pada en_US.UTF-8 UTF-8 dan
Hapus # pada en_US ISO-8859-1
Lalu esc untuk keluar dari mode insert
:wq untuk save write and quit

Lalu generate 
```
locale-gen
```

```
locale > /etc/locale.conf
```

```
nvim /etc/locale.conf
```

### Membuat User
```
useradd -m data
passwd data
```
Memberikan hak akses sudo 
```
echo "data ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/data
```
Cek uji coba user 
```
su server
```
Cek hak akses sudo
```
sudo su
```
lalu exit.

### Setup Kernel Parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/satu-boot.conf
```
```
touch /etc/cmdline.d/dua-misc.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p9)=orion root=/dev/belt/root" > /etc/cmdline.d/satu-boot.conf
```

```
cat /etc/cmdline.d/satu-boot.conf
```

```
lsblk -o name,UUID
```

```
echo "rw" > /etc/cmdline.d/dua-misc.conf 
```

### Initramfs
```
nvim /etc/mkinitcpio.conf
```
Masuk mode insert, lalu cari HOOKS tanpa #
Lalu menambahkan sd-encrypt lvm2 setelah sd-vconsole
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
Masuk mode insert, lalu hapus # pada
ALL_config
ALL_kerneldesh
ALL_default uki (menghapus /efi diganti menjadi /boot)
Menambah # pada
ALL_default image

### Generate file boot
```
bootctl --path=/boot install
```

```
cd /boot/
```

```
ls
```
### Generate initram
```
mkinitcpio -P
```
### Aktivasi

```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
```

```
systemctl enable iwd
```

```
exit
```

```
umount -R /mnt
```

```

### Stop Recording Asciinema 

Klik ctrl+d 

Lalu upload asciinema
```
upload asciinema [bebas dah].cast

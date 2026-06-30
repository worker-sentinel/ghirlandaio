
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
Fungsi 
- Menampilkan seluruh hard disk, SSD, partisi, dan mount point.
- Digunakan untuk memastikan nama disk yang akan dipartisi.

Membuat partisi
```
cfdisk /dev/[sesuai jenis partisi]  
```
Fungsi:
- Membuka aplikasi partisi berbasis terminal
- Digunakan untuk membuat, menghapus, atau mengubah ukuran partisi

> Partisi Boot : 4G (EFI System)
> Partisi LVM : sisanya (Linux File System)

Cek partisi: 
```
lsblk 
```

### Menghubungkan WiFi 

Masuk ke utilitas WiFi:
```
iwctl
```

Melihat perangkat WiFi:
```
device list 
```

Melihat jaringan yang tersedia:
```
station wlan0 get-networks
```

Menghubungkan WiFi:
```
station wlan0 connect [nama wifi]
```
### Membuat Enkripsi LUKS

Memformat partisi menjadi terenkripsi:
```
cryptsetup luksFormat /dev/[sesuai dengan partisi yang sudah di buat untuk system] 
```

Membuka partisi terenkripsi:
```
cryptsetup luksOpen /dev/[sesuai dengan partisi yang sudah di luksformat] orion 
```
### Membuat Physical Volume (LVM)
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
lvcreate -L 8G belt -n vars
lvcreate -L 1G belt -n vlog
lvcreate -L 1G belt -n vaud 
lvcreate -L 1,5G belt -n vtmp
lvcreate -L 20G belt -n cache
lvcreate -l100%FREE belt -n backup
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
Partisi cache
```
mkfs.ext4 /dev/belt/cache
```
Partisi backup 
```
mkfs.ext4 /dev/belt/backup 
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
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/orion/vaud /mnt/var/log/audit
```

Partisi vtmp
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/vtmp /mnt/var/tmp
```
Partisi cache 
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/cache /mnt/var/lib/redish 
```

Partisi backup
```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/belt/backup /mnt/var/backup
```

setelah mounting, lakukan cek partisi
```
lsblk
```
### Installasi Package
```
pacstrap -K /mnt linux-hardened linux-hardened-headers iwd neovim git sudo pacman base linux-firmware redis rsync firewalld audit lynis
```
### Generate FSTAB
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Mounting RAM 
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab 
```
### Menyalin Konfigurasi Jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
Masuk arch chroot
```
arch-chroot /mnt 
```
### Membuat Hostname
```
echo "internal" > /etc/hostname
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
useradd -m internal
passwd internal 
```
### Memberikan hak akses sudo 
```
echo "control ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/control
```
Cek uji coba user 
```
su internal
```
Cek hak akses sudo
```
sudo su
```
lalu exit.

### Setup Kernel Parameter

Membuat folder:
```
mkdir /etc/cmdline.d
```

Membuat file konfigurasi:
```
touch /etc/cmdline.d/satu-boot.conf
```

```
touch /etc/cmdline.d/dua-misc.conf
```

Isi konfigurasi:
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p8)=orion root=/dev/belt/root" > /etc/cmdline.d/satu-boot.conf
```

Melihat isi file:
```
cat /etc/cmdline.d/satu-boot.conf
```

```
lsblk -o name,UUID
```

Isi konfigurasi
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
Instalasi Bootloader Systemd
```
bootctl --path=/boot install
```

Untuk berpindah folder:
```
cd /boot/
```
**cd** berarti Change Directory dari /root ke /boot

Melihat Isi Direktori
```
ls
```
### Generate initram
```
mkinitcpio -P
```
### Aktivasi Service

Network:
```
systemctl enable systemd-networkd
```

DNS:
```
systemctl enable systemd-resolved
```

WiFi:
```
systemctl enable iwd
```

SDDM:
```
systemctl enable sddm
```

Keluar dari arch-chroot
```
exit
```

Melepas semua mount:
```
umount -R /mnt
```

### Stop Recording Asciinema 

```
ctrl+d 
```

Lalu upload asciinema
```
upload asciinema [bebas dah].cast
```

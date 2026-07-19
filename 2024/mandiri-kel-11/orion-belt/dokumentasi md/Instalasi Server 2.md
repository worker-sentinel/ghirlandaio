
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

Partisi Boot : 1G (EFI System)
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
cryptsetup luksOpen /dev/[sesuai dengan partisi yang sudah di luksformat] orion2 
```
### Membuat Physical Volume 
```
pvcreate /dev/mapper/orion2 
```
### Membuat Volume Grup 
```
vgcreate belt2 /dev/mapper/orion2 
```
### Membuat Logical Volume 
bisa disesuaikan dengan kebutuhan
```
lvcreate -L 8G belt2 -n root
lvcreate -L 8G belt2 -n vars
lvcreate -L 1G belt2 -n vlog
lvcreate -L 1G belt2 -n vaud 
lvcreate -L 1,5G belt2 -n vtmp
lvcreate -l50FREE% belt2 -n home
lvcreate -l100FREE% belt2 -n podman
```
### Memformat Partisi 

Partisi root
```
mkfs.ext4 /dev/belt2/root
```
Partisi boot 
```
mkfs.vfat -F32 /dev/sda4
```
Partisi vars 
```
mkfs.ext4 /dev/belt2/vars
```
Partisi vlog 
```
mkfs.ext4 /dev/belt2/vlog
```
Partisi vaud 
```
mkfs.ext4 /dev/belt2/vaud
```
Partisi vtmp 
```
mkfs.ext4 /dev/belt2/vtmp
```
Partisi home 
```
mkfs.ext4 /dev/belt2/home 
```
Partisi podman 
```
mkfs.ext4 /dev/belt2/podman
```
setelah memformat, lakukan cek partisi
```
lsblk
```
### Mounting Partisi 

Partisi root 
```
mount /dev/belt2/root /mnt
```

Partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda4 /mnt/boot
```

Partisi vars
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt2/vars /mnt/var
```

Partisi vlog
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt2/vlog /mnt/var/log
```

Partisi vaud
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt2/vaud /mnt/var/log/audit
```

Partisi vtmp
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt2/vtmp /mnt/var/tmp
```

Partisi home
```
mount --mkdir -o rw,nodev,relatime /dev/belt2/home /mnt/home
```

Partisi podman
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt2/podman /mnt/var/lib/container
```

setelah mounting, lakukan cek partisi
```
lsblk
```
### Installasi Package
```
pacstrap -K /mnt base intel-ucode git neovim wget openssh firewalld pacman sudo linux-hardened linux hardened-headers linux-firmware lvm2 uget curl iwd podman asciinema 
```
### Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Mounting RAM
```
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,
	relatime,size=1G 0 0" >> /mnt/etc/fstab 
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
echo "server2" > /etc/hostname
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
useradd -m obel2
passwd obel2
```
Memberikan hak akses sudo 
```
echo "obel2 ALL=(ALL:ALL) ALL" > /etc/sudoers.d/obel2
```
Cek uji coba user
```
su obel2
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
untuk file yang di dalam { } bisa bebas
```
touch /etc/cmdline.d/{satu-boot.conf,dua-misc.conf}
```

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda5)=orion2 root=/dev/belt2/root" > /etc/cmdline.d/satu-boot.conf
```

```
cat /etc/cmdline.d/satu-boot.conf
```

```
lsblk
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
nvim /etc/mkinitcpio.d/linux-hardened.preset
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
systemctl enable firewalld
```

```
systemctl enable sshd
```

```
exit
```

### Stop Recording Asciinema 

Klik ctrl+d 

Lalu upload asciinema
```
upload asciinema [bebas dah].cast
```


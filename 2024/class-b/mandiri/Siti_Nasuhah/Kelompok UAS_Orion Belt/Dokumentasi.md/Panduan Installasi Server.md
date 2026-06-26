
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

Partisi Boot : 3G (EFI System)
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
lvcreate -L 8G belt -n root
lvcreate -L 5G belt -n vars
lvcreate -L 1G belt -n vlog
lvcreate -L 1G belt -n vaud 
lvcreate -L 1,5G belt -n vtmp
lvcreate -l50FREE% belt -n home
lvcreate -l100FREE% belt -n podman
```
### Memformat Partisi 

Partisi root
```
mkfs.ext4 /dev/belt/root
```
Partisi boot 
```
mkfs.vfat -F32 /dev/nvme0n1p7
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
Partisi podman 
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
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```

Partisi vars
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt/vars /mnt/var
```

Partisi vlog
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vlog /mnt/var/log
```

Partisi vaud
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vaud /mnt/var/log/audit
```

Partisi vtmp
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vtmp /mnt/var/tmp
```

Partisi home
```
mount --mkdir -o rw,nodev,relatime /dev/belt/home /mnt/home
```

Partisi podman
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt/podman /mnt/var/lib/container
```

setelah mounting, lakukan cek partisi
```
lsblk
```
### Installasi Package
```
pacstrap -K /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman uget curl iwd podman asciinema 
```
### Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Mounting RAM
```
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab 
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
echo "orion" > /etc/hostname
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
useradd -m server
passwd server
```
Memberikan hak akses sudo 
```
echo "server ALL=(ALL:ALL) ALL" > /etc/sudoers.d/server
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
untuk file yang di dalam { } bisa bebas
```
touch /etc/cmdline.d/{satu-boot.conf,dua-misc.conf}
```

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p8)=orion root=/dev/belt/root" > /etc/cmdline.d/satu-boot.conf
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

Memperbaiki iwd:
```
sudo nvim /etc/resolve.conf
```
Masuk ke mode insert (i), lalu tambahkan:
name server 8.8.8.8
name server 1.1.1.1
Lalu esc, simpan hasil perubahan dengan :wq

Restart systemd networkd resolved
```
sudo systemctl restart systemd-networkd
```

```
sudo systemctl restart systemd-resolved
```

Jika masih belum bisa, setup iwd
```
sudo nvim /var/lib/iwd/main.conf
```
masuk mode insert (i):
tambahkan 
[General]
EnableNetworkConfiguration=true
lalu esc, simpan hasil perubahan dengan :wq

lalu install asciinema
```
sudo pacman -S asciinema
```

### Recording Asciinema
```
asciinema rec [bebas dah].cast
```

### Setup Firewalld

Cek status
```
sudo systemctl status firewalld
```

Untuk memeriksa setiap zona firewall
```
sudo firewall-cmd --info-zone-drop
```

```
sudo firewall-cmd --info-zone-block
```

```
sudo firewall-cmd --info-zone-public
```

```
sudo firewall-cmd --info-zone-home
```

```
sudo firewall-cmd --info-zone-work
```

```
sudo firewall-cmd --info-zone-internal
```

```
sudo firewall-cmd --info-zone-external
```

```
sudo firewall-cmd --info-zone-trusted
```

```
sudo firewall-cmd --info-zone-dmz
```

Menghapus service yang tidak diperlukan dalam zona (secara satu persatu)
```
sudo firewall-cmd --zone=[nama zona] --remove-service=nama service  --permanent
```

Menghapus service yang tidak diperlukan dalam zona (sekaligus)
```
sudo firewall-cmd --zone=[nama zona] --remove-service={nama service}  --permanent
```

Jika melakukan perubahan pada informasi zona, perlu dilakukan reload seperti berikut ini
```
sudo firewall-cmd --reload
```

Untuk memastikan kembali, terkait informasi baru yang dirubah sudah terbaca atau belum
```
sudo firewall-cmd --info-zone-[nama zona]
```

### Disable Module Kernel

Verifikasi keberadaan module kernel 
```
sudo lsmod | grep cramfs
```

```
sudo lsmod | grep freevxfs
```

```
sudo lsmod | grep hfs
```

```
sudo lsmod | grep hfsplus
```

```
sudo lsmod | grep jffs2
```

```
sudo lsmod | grep overlayfs
```

```
sudo lsmod | grep squashfs
```

```
sudo lsmod | grep udf
```

```
sudo lsmod | grep usb-storage
```

Cara memblokir module 
```
sudo nvim /etc/modprobe.d/disable-module.conf
```

Mode insert (i), lalu ketik:
install usb-storage /bin/false
blacklist usb-storage

Verifikasi filesystem harus disable semua:
```
sudo lsmod | grep afs
```

```
sudo lsmod | grep ceph
```

```
sudo lsmod | grep cifs
```

```
sudo lsmod | grep exfat
```

```
sudo lsmod | grep fat
```

```
sudo lsmod | grep fscache
```

```
sudo lsmod | grep fuse
```

```
sudo lsmod | grep gfs2
```

```
sudo lsmod | grep nfs_common
```

```
sudo lsmod | grep nfsd
```

```
sudo lsmod | grep smbfs_common
```

Cara memblokir
```
sudo nvim /etc/modprobe.d/disable-module.conf
```

Mode insert (i), lalu ketik:
install [nama filesystem] /bin/false
blacklist [nama filesystem]

```
sudo mkinitcpio -P
```

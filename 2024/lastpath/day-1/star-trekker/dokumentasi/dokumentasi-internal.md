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
```
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
## boot
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
> membuat format 
```
mount --mkdir -o uid=0,gid=0 /dev/partisi_boot /mnt/boot
```
> membuat folder sekaligus mounting untuk partisi boot
## vars
```
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
>  membuat folder sekaligus mounting untuk partisi var, nodev untuk tidak mengizinkan device lainnya di partisi tersebut, nosuid untuk tidak dapat mengatur akses suid/sgid dipartisi tersebut,

## tmp 
```
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

## vtemp
```
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


## vlog
```
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

## podman
```
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

## home 
```
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

# Install Package
```
pacstrap /mnt intel-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel docker neovim git iwd asciinema linux-firmware-realtek firewalld
```
> install package yang di perlukan untuk os 

# Regist Partisi
```
genfstab -U /mnt > /mnt/etc/fstab
```
> mendaftarkan partisi 
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
> memasukkan  tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0 ke dalam folder /mnt/etc/fstab

# Chroot
```
arch-chroot /mnt
```
> masuk supaya tidak perlu menggunakan /mnt di sertiap mau masuk ke dalam suatu folder

# Buat Hostname
```
echo "hostnameanda" > /etc/hostname
```
> untuk membuat hostname

# time 
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
> data yang ada di /usr/share/zoneinfo/asia/jakarta di simlink ke /etc/localtime
```
hwclock --systohc
```
> untuk generate waktu
```
nvim /etc/locale.gen
```
> nvim untuk menjalankan program neovim
cari en_US lalu uncommenting keduanya
locale-gen
```
> untuk generate bahasa
```
locale > /etc/locale.conf
```
> untuk memasukkan data locale ke /etc/locale.conf
```
nvim /etc/locale.conf
```
> edit file directory (di kasus ini edit file locale.conf)
```
di line paling atas ganti 'C' menjadi en_US di line paling bawah tambahkan en_US.UTF-8
# User
```
useradd -m 'nama_user'
```
> membuat user (-m artinya untuk membuat folder otomatis untuk user)
```
passwd 'nama_user'
```
> membuat password
```
echo "nama_user ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/nama_user
```
> untuk memberikan akses sudo kepada user yang dituju

# Kernel Parameter
```
mkdir /etc/cmdline.d
```
> membuat folder
```
touch /etc/cmdline.d/{01-boot.conf,02-rw.conf}
```
> membuat file 
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi_luks])=creamy root=/dev/[nama grup]/root" > /etc/cmdline.d/01-boot.conf
```
> untuk mendeklareasikan uuid partisi luks dan partisi root
```
echo "rw" > /etc/cmdline.d/02-rw.conf
```
> untuk menambahkan "rw" di file rw.conf
```
nvim /etc/mkinitcpio.conf
```
> nvim untuk menjalankan program neovim, /etc/mkinitcpio.confadalah folder tempat menyimpan profile konfigurasi untuk alat bernama mkinitcpio, default.conf adalah file yang berisi instruksi bagaimana kernel utama harus dibuat 
```
> tambahkan lvm2 dan sd-encrypt setelah sd-vcondole di hooks paling bawah
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>  nvim untuk menjalankan program neovim, /etc/mkinitcpio.d/ adalah folder tempat menyimpan profile konfigurasi untuk alat bernama mkinitcpio
```
> sesuaikan

```
bootctl --path=/boot install
```
> untuk mendaftarkan linux di boot option 
```
mkinitcpio -P
```
> generate image kernel
```
systemctl enable iwd
```
> untuk mengaktifkan aplikasi yang dituju
```
systemctl enable systemd-networkd.socket
```
> untuk mengaktifkan aplikasi yang dituju
```
systemctl enable systemd-resolved
```
> untuk mengaktifkan aplikasi yang dituju

# finish installation
```
exit
```
> untuk keluar
```
umount -R /mnt
```
> menghapus mounting
```
reboot
```
> menyalakan ulang

# after installation

## network
```
nvim /etc/iwd/main.conf
```
>  nvim untuk menjalankan program neovim
```
> tambahkan
```
[General]
EnableNetworkConfiguration=true

## disable module
```
nvim /etc/modprobe.d/hardening.conf
```
> isi
```
install    cramfs           /bin/false


install    freexfs          /bin/false


install    hfs              /bin/false


install    hfsplus          /bin/false


install    jffs2            /bin/false


install    udf              /bin/false


install    fire-wire-core   /bin/false


install    usb_storage      /bin/false
selanjutnya ketik : 
```
mkinitcpio -P
```
> generate image kernel
cek list module :
```
lsmod
```
> mencantumkan semua modul kernel yang lagi dimuat
lsmod | grep namamodule
```
> mencamtukan modul yang dicari
## setup firewalld
```
systemctl enable --now firewalld
```
sudo firewall-cmd --zone=public --add-service=http --permanent 
```
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
sudo firewall-cmd --zone=public --add-port=7946/tcp --permanent
```
sudo firewall-cmd --zone=public --add-port=4789/tcp --permanent
```
sudo firewall-cmd --zone=public --add-port=8000/tcp --permanent
```
sudo firewall-cmd --zone=public --add-port=5432/tcp --permanent
```
sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
```
sudo firewall-cmd --reload
```
Selanjutnya, untuk melihat list-list port dan sistem yang sudah di enable ketik:
```
sudo firewall-cmd --list-ports






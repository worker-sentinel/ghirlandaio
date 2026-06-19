# **Instalasi OS Admin**

## Mengaktifkan rekaman asciinema
```
asciinema record adminandromeda.cast
```

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```

```
station wlan0 get-networks
```
menampilkan hasil scan berupa daftar SSID yang ditemukan

```
station wlan0 scan
```
meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar
```
station wlan0 connect (nama wifi)
```
```
station wlan0 connect "wifi sekolah"
```
jika nama wifi lebih dari 1 kata. gunakan tanda "..."
```
exit
```
## Memeriksa partisi
```
lsblk
```

## Membuat dan mengatur partisi
```
cfdiks /dev/nvme0n1p
```

## Jumlah partisi
```
boot = 3g [efi system] nvme0n1p7
root = 75.1g [linux filesystem] nvme0n1p8
```

## Cek kembali partisi
```
lsblk
```

## Setup lvm
```
pvcreate /dev/mapper/sayyida
```
```
vgcreate proc /dev/mapper/nvme0n1p8
```

## Membuat logical volume
```
lvcreate -L 15G proc -n root
lvcreate -L 20G proc -n vars
lvcreate -L 2G proc -n vlog
lvcreate -L 4G proc -n vtmp
lvcreate -L 2G proc -n vaud
lvcreate -L 15G proc -n home
```
## mengecek kembali partisi
```
lsblk
```

## Format
```
mkfs.vfat -F32 -n boot /dev/nvmeon1p7
mkfs.ext4 /dev/proc/root
mkfs.ext4 /dev/proc/vars
mkfs.ext4 /dev/proc/vlog
mkfs.ext4 /dev/proc/vtmp
mkfs.ext4 /dev/proc/vaud
mkfs.ext4 /dev/proc/home
```

## Periksa partisi Kembali untuk memastikan
```
lsblk
```
## Mount
```
mount /dev/proc/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/proc/home /mnt/home
```
## Periksa partisi Kembali
```
lsblk
```
```
lvcreate -l50%FREE proc -n andromeda
```
```
cryptsetup luksFormat /dev/proc/andromeda
```

## Instal package
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```
## Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Tambahkan
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
 ```
## Masuk ke system
```
arch-chroot /mnt


echo admin2 > /etc/hostname
```
 
## Sinkronisasi Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime


hwclock --systohc
```

## Atur Bahasa dan lokasi
```
nvim /etc/locale.gen
```
Ketik tanda "/" agar pencarian lebih cepat
/en_US
```
Tekan enter lalu tekan "i"

Lalu hapus tanda "#" pada
en_US.UTF-8 UTF-8
en_US ISO-8859-1

Tekan esc setelah itu :wq
```
## Menghasilkan dan mengatur konfigurasi locale sistem agar menggunakan bahasa dan format regional en_US.UTF-8
```
locale-gen


locale > /etc/locale.conf


nvim /etc/locale.conf
```

isi lang=C menjadi lang=en_US.UTF-8

isi ALL=en_US.UTF-8

```

cryptsetup luksOpen /dev/proc/andromeda andromeda
```
```
mkfs.ext4 /dev/mapper/andromeda
```
## Membuat user
```
mkdir /home/user
```
```
useradd -d /home/user andromeda


passwd andromeda
```
```
chown -R andromeda:andromeda /home/user


passwd
 

echo "andromeda ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/none
```
## Config Volume
```
nvim /etc/security/pam_mount.xml
```
<img width="723" height="820" alt="b3dc3992-4dbf-4965-8901-565d14dd2e89" src="https://github.com/user-attachments/assets/5897cf1d-17db-457c-86fb-c3696bc4fc47" />

sesuaikan dengan foto

## Mengupdate konfigurasi pam_mount
```
nvim /etc/pam.d/system-login
```
<img width="565" height="403" alt="4e43d842-b035-4a3f-a903-b6e982642d80" src="https://github.com/user-attachments/assets/9ada6bd6-0fbe-46c9-bcc1-5e38b7acf7f7" />


## Booster
```
nvim /etc/booster.yaml
```
<img width="217" height="114" alt="ac2e9e0c-4f69-40af-87e4-3686cf115491" src="https://github.com/user-attachments/assets/173d3208-d342-4ef9-84aa-66a315aa7b46" />



## Prepare Boot
```
cd /boot
```
cek kernel melalui:
```
ls /usr/lib/modules
```
```
booster build --kernel-version <version> /boot/booster-linux-lts-new.img
```
```
rm -fr booster-linux-lts.img
```

## System-boot
```
bootctl --path=/boot install
```
```
nvim /boot/loader/entries/booster.conf
```
```
title    arch with booster
linux    /vmlinuz-linux-lts
initrd   /intel-ucode.img
initrd   /booster-linux-lts-new.img
options  root=/dev/proc/root rw
```
```
nvim /boot/loader/loader.conf

add value

default  booster.conf


bootctl --graceful update
```

## Booting
```
exit
umount -R /mnt
```

## Menghentikan asciinema
```
CTRL+D
asciinema upload adminandromeda.cast
```
## Selesai
```
reboot
```

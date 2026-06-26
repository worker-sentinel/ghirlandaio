## Installasi OS Hardened (arsip) luks on lvm

## Sambungkan ke WIFI
```
iwctl
```
## Mengecek Wifi 

```
station wlan0 get-networks
```
## Menyambungkan Wifi 
```
station wlan0 connect (nama wifi) 
```
```
exit
```

## Tes Jaringan

```
ping google.com
```

## Membuat partisi
```
cfdisk /dev/[nama partisi]
```
## Membagi partisi sesuai penyimpanan yang ada

```
boot = 3G [EFI System]
root = 47G (menyesuaikan sisa penyimpanan yang ada) [Linux filesystem] 
```


## Membagi partisi

```
lsblk
```
```
pvcreate /dev/[nama partisi]
```
```
vgcreate [nama grup] /dev/[nama partisi]
```
```
lvcreate -L [size G | M] [nama grup] -n root
```
```
lvcreate -L [size G | M] [nama grup] -n vars
```
```
lvcreate -L [size G | M] [nama grup] -n vlog
```
```
lvcreate -L [size G | M] [nama grup] -n vaud
```
```
lvcreate -L [size G | M] [nama grup] -n vtmp
```
```
lvcreate -L [size G | M] [nama grup] -n home
```
```
lvcreate -l50%FREE [nama grup] -n podman
```
```
lvcreate -l50%FREE [nama grup] -n [nama home untuk internal]
```
```
lsblk
```


*luks on lvm home nya ada dua (eksternal dan internal)*

## Format partisi

```
mkfs.ext4 /dev/[nama grup]/root
```
```
mkfs.ext4 /dev/[nama grup]/vars
```
```
mkfs.ext4 /dev/[nama grup]/vlog
```
```
mkfs.ext4 /dev/[nama grup]/vaud
```
```
mkfs.ext4 /dev/[nama grup]/vtmp
```
```
mkfs.ext4 /dev/[nama grup]/home
```
```
mkfs.ext4 /dev/[nama grup]/podman
```
```
kalo lupa partisi pake command lsblk -o name,fstype
```
```
mkfs.vfat -F32 /dev/[nama partisi boot]
```

## Mounting

```
mount /dev/[nama grup]/root /mnt
```
```
mount —mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot /mnt/boot
```
```
mount —mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
```
```
mount —mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/home /mnt/home
```
```
mount —mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers
```

*(home untuk internal ga perlu dimounting, karena nanti mounting otomatis pake aplikasi pam_mount)*

```
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]
```

**bikin password, password nya harus sama kayak password user** 


## Install package

```
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema
```

## After install

```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo “tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=512M 0 0” >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Masuk ke dalam arch chroot

```
arch-chroot /mnt
```

## Membuat hostname

```
echo [hostname] > /etc/hostname
```

## Time/sinkronisasi waktu

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock —systohc
```
```
nvim /etc/locale.gen 
```
**Setelah masuk ketik “I” kemudian (cari en_US lalu hapus hashtag dua-duanya)**
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

## Melakukan generate lokasi

```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
**Setelah masuk ketik “I” pada bagian paling atas "C" diubah menjadi en_US dan pada bagian paling bawah tambahkan en_US.UTF-8**
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

## Install pam mount

```
pacman -S pam_mount
```
```
lsblk
```

## Membuka Enkripsi LUKS 

```
cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
```

**masukin password yg tadi udah dibuat**

## Membuat system
```
mkfs.ext4 /dev/mapper/[device name]
```
```
mkdir /home/[nama folder]
```
## Membuat user

```
useradd -d /home/[nama folder] [nama user]
passwd [nama user]
```
**(masukin password. password harus sama kayak pas cryptsetup luksFormat)**

```
- echo “[nama user] ALL‎ = (ALL:ALL)” > /etc/sudoers.d/[nama user]
```
## Mengatur kepemilikan home
```
- chown -R [nama user]:[nama user] /home/[nama folder]
```
```
nvim /etc/security/pam_mount.conf.xml
```
**disamain kaya gambar di bawah ini**
*jangan sampai salah ya*

<img width="837" height="639" alt="Screenshot 2026-06-23 155018" src="https://github.com/user-attachments/assets/476a802e-2f26-4b57-844e-78d79caa03ca" />

```
nvim /etc/pam.d/system-login
```
**disamain kaya gambar di bawah ini**
*jangan sampai salah ya*

<img width="1176" height="684" alt="Screenshot 2026-06-23 120829" src="https://github.com/user-attachments/assets/637da8c4-0d8e-4302-b697-df53555ddf02" />

## Konfigurasi MKINITCPIO


```
mkdir /etc/mkinitcpio.d
```
```
nvim /etc/cmdline.d/boot.conf
```
**setelah masuk ke tampilan itu isi nya kosong, lalu pencet insert dan ketik root=/dev/namagrup/root rw**

```
nvim /etc/mkinitcpio.conf
```
**cari bagian HOOKS yang aktif (tidak memiliki tagar) dan posisinya di atas kata “COMPRESSION” di file itu, kemudian dibaris “HOOKS” setelah kata sd-vconsole spasi sekali dan ketik lvm2**
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
**disamain kaya gambar di bawah ini**

<img width="1058" height="601" alt="Screenshot 2026-06-23 154725" src="https://github.com/user-attachments/assets/ded2c537-5ce9-4b23-b178-3914994248f0" />

## Instalasi systemd-boot 

```
bootctl —path=/boot install
```

## generate initramfs

```
mkinitcpio -P
```

## Mengaktifkan layanan jaringan, Wi-Fi, firewall dan SSH

```
systemctl enable systemd-networkd
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
```
```
exit
```

**Untuk Lenovo bootctl sekali lagi di luar arch-chroot**

```
bootctl —path=/mnt/boot install
```
```
reboot
```
## Selesai

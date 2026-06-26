# Dokumentasi Instalasi OS untuk Server

## Sambungkan internet terlebih dahulu
```
iwctl
```
## Periksa partisi
```
lsblk
```
## Cryptsetup dan format luksnya
```
cryptsetup luksFormat /dev/partisi_root
```
> masukin password
> kalau muncul tulisan 'in use', apusin dulu partisi grupnya pake command:
```
vgsremove /dev/nama grup/nama partisinya
```
>setelah di vgsremove, cryptsetup luksFormat lagi
## Buka partisi luks
```
cryptsetup luksOpen /dev/(partisi_root) (nama_device)
```
## Bikin physical volume
```
pvcreate /dev/mapper/(nama_device)
```
## Bikin volume grub
```
vgcreate nama_grup /dev/mapper/(nama_device)
```
## Bikin logical volume
```
lvcreate -L 8G (nama_grup) -n root
```

```
lvcreate -L 8G (nama_grup) -n vars
```

```
lvcreate -L 1G (nama_grup) -n vlog
```

```
lvcreate -L 1G (nama_grup) -n vtmp
```

```
lvcreate -L 1G (nama_grup) -n vaud
```

```
lvcreate -L 5G (nama_grup) -n home
```

```
lvcreate  -L 5G (nama_grup) -n podman
```
## Formatting partisi
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```

```
mkfs.ext4 /dev/namagrup/root
```

```
mkfs.ext4 /dev/namagrup/vars
```

```
mkfs.ext4 /dev/namagrup/vtmp
```

```
mkfs.ext4 /dev/namagrup/vaud
```

```
mkfs.ext4 /dev/namagrup/vlog
```

```
mkfs.ext4 /dev/namagrup/home
```

```
mkfs.ext4 /dev/namagrup/podman
```
## Cek ulang
```
lsblk
```
> mountingnya ini harus berurutan
> root
> boot
> vars
> vtmp
> vaud
> vlog
> home
> docker
```
mount /dev/nama grup/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/namagrup/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/namagrup/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/namagrup/home /mnt/home 
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/podman /mnt/var/lib/containers
```
## Cek lagi
```
lsblk
```
## Install package yang diperlukan
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman 
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Mengcopy jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Memformat tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
## Cek lagi yups
```
cat /mnt/etc/fstab
```
## Login ke sistem
```
arch-chroot /mnt
```
## Sinkronin waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
## Setting bahasa dan waktu
```
nvim /etc/locale.gen
```
> search en_US menggunakan simbol / kemudian hapus tada pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
> LANG=C.UTF-8 diganti menjadi LANG=en_US.UTF-8 kemudian bagian LC_ALL= tambahkan en_US.UTF-8
```
## Membuat user
```
```
useradd -m nama user
```
```
passwd nama user
```
```
echo "nama user ALL=(ALL:ALL) ALL" > /etc/sudoers.d/nama user
```
## Kernel
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdine.d /{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi root)=nama device root=/dev/nama grup/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
## Mengatur mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
> bagian hooks tambahkan sd-encrypt dan lvm2
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
> hapus tanda pagar pada semua baris ALL kemudian tambahkan # pada baris kedua default lalu hapus pagar pada baris ketiga default ganti efi kecil dengan boot
```
mkinitcpio -P
```
## Mengaktifkan dan mejalankan jaringan
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```
## Keluar dari terminal
```
exit
```

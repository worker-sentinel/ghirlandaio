# Install OS

## **Hubungkan ke internet terlaebih dahulu:**

```
iwctl 
```

**Aktifkan asciinema**

## Periksa partisi terlebih dahulu

```
lsblk
```

## cryptsetup dan format luks

```
cryptsetup luksFormat /dev/partisi_root
```

**lalu, masukin pw.**

### Jika ada tulisan in use, hapus dulu PARTISI grupnya : vgsremove /dev/nama grup/nama partisinya. setelah di vgsremove cryptsetup luksFormat lagi

## Buka partisi luks

```
cryptsetup luksOpen /dev/(partisi_root) (nama_device)
```

## Buat physical volume

```
pvcreate /dev/mapper/(nama_device)
```

## Buat volume grub

```
vgcreate nama_grup /dev/mapper/(nama_device)
```

## Buat logical volume 

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
lvcreate -L 5G (nama_grup) -n podman
```

## Format Partisi

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

## Periksa kembali 

```
lsblk
```


## mounting ini harus berurutan
### root
### boot
### vars
### vtmp 
### vaud
### vlog 
### home
### docker

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

## Periksa kembali

```
lsblk
```

## Install package yang dibutuhkan

```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

## Fstab

```
genfstab -U /mnt > /mnt/etc/fstab
```

## salin setingan jaringan

```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Tambahkan

```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

## Periksa kembali

```
cat /mnt/etc/fstab
```

## Masuk ke sistem

```
arch-chroot /mnt
```

## sinkronisasi waktu

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

## atur bahasa dan lokalisasi

```
nvim /etc/locale.gen
```

**cari dua en_US dan uncommenting**

```
locale-gen 
```

```
locale > /etc/locale.conf
```

```
nvim /etc/locale.conf
```

**Bagian atas "C" ubah jadi en_US dan tambahkan en_US.UTF-8**

## Buat user, contohnya luqman

```
useradd -m luqman 
```

```
passwd luqman
```

```
echo "luqman ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/luqman
```

## Atur parameter

```
mkdir /etc/cmdline.d
```

```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
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

***cari bagian Hooks pas ada tulisan block itu di spasi sekali lalu ketik "sd-encrypt" spasi lagi sekali lalu ketik "lvm2"***

## Sesuaikan ini dengan poto

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/65ea0b77-d56b-42ee-be4a-7e66af5982a3" />

## Install bootctl

```
bootctl --path=/mnt/boot install 
```

***(ini kalo bukan laptop Lenovo, Lenovo bootctl nya harus di luar dari arch-chroot)***

```
mkinitcpio -P
```

## Aktifkan sebagai berikut

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 
```

## Penyesuaian terakhir

```
exit
umount -R /mnt
```

### **STOP REKAMAN**

**CTRL+D**

**asciineme upload nama_file.cast**

**setelah itu, foto link yang tampil**

## Selesai


```
reboot
```


sumber
| kelompok 8-9 dan kelompok 10 hanya mengganti volume_grup_partisi-docker > volume_grup_podma(containers)

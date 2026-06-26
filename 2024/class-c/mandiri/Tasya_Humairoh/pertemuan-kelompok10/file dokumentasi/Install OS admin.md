# Installasi OS

## **Menghubungkan ke internet:**

```
iwctl 
```

**Mengaktifkan asciinema**

## Periksa partisi terlebih dahulu

```
lsblk
```

## cryptsetup dan format luks

```
cryptsetup luksFormat /dev/mmcblk0p6
```

**Kemudian, masukan password.**

## Membuka partisi luks

```
cryptsetup luksOpen /dev/mmcblk0p6 advan7
```

## Membuat physical volume (PV)

```
pvcreate /dev/mapper/advan7
 ```

## Selanjutnya membuat volume grub (VG)

```
vgcreate nebula /dev/mapper/advan7
```

## Membuat logical volume (LV)
**Jumlah partisi root yang kelompok kami miliki sebanyak 30G, kemudian kami membagi partisi root sesuai dengan yang ada dibawah ini**
```
lvcreate -L 8G nebula -n root
```
```
lvcreate -L 8G nebula -n vars
```
```
lvcreate -L 1G nebula -n vlog
```
```
lvcreate -L 1G nebula -n vtmp
```
```
lvcreate -L 1G nebula -n vaud
```
```
lvcreate -L 5G nebula -n home
```

## Kemudian dilanjutkan dengan Format Partisi

```
mkfs.vfat -F32 -n BOOT /dev/mmcblk0p5
```

```
mkfs.ext4 /dev/nebula/root
```

```
mkfs.ext4 /dev/nebula/vars
```

```
mkfs.ext4 /dev/nebula/vlog
```

```
mkfs.ext4 /dev/nebula/vtmp
```

```
mkfs.ext4 /dev/nebula/vaud
```

```
mkfs.ext4 /dev/nebula/home
```


## Lalu kami melakukan pemeriksaan kembali 

```
lsblk
```


## mounting harus dilakukan secara berurutan
```
mount /dev/nebula/root /mnt        
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/mmcblk0p5 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/nebula/vars /mnt/var
mount --mkdir -o rw,nosuid,noexec,relatime /dev/nebula/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/nebula/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/nebula/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/nebula/home /mnt/home 
```

## Setelah kami melakukan mounting, kami periksa kembali

```
lsblk
```

## Menginstall linux package

```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

## Kemudian kami melakukan Fstab (Menyimpan partisi yang telah dibuat)

```
genfstab -U /mnt > /mnt/etc/fstab
```

## Melakukan penyalinan setingan jaringan

```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Dilanjutkan kami menambahkan partisi tmpfs ke dalam partisi

```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

## Kemudian kami melakukan pemeriksaan kembali

```
cat /mnt/etc/fstab
```

## Setelah itu masuk ke system 

```
arch-chroot /mnt
```

## Melakukan sinkronisasi waktu

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

## Selanjutnya kami mengatur bahasa dan lokalisasi

```
nvim /etc/locale.gen
```

**Setelah masuk ketik “I” kemudian cari dua (en_US) dengan menghapus tagar untuk mengatur bahasa dan lokalisasi**
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

## Membuat root dan user, kalua kelompok kami menggunakan (force)
```

passwd 12345

```

```
useradd -m force
```

```
passwd force 12345

```

```
echo "force ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/force

```

## Mengatur parameter

```
mkdir /etc/cmdline.d
```

```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/mmcblk0p6=advan7 root=/dev/nebula/root" > /etc/cmdline.d/01-boot.conf
```

```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## Selanjutnya mengatur mkinitcpio

```
nvim /etc/mkinitcpio.conf
```

***cari bagian Hooks yang tidak memiliki tagar dan berada di atas kata “COMPRESSION”, kemudian cari tulisan block lalu berikan spasi sekali dan ketik "sd-encrypt" lalu berikan spasi sekali lagi dilanjutkan dengan mengetik "lvm2"***

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
 

## Menginstall bootctl

```
bootctl --path=/mnt/boot install 
```
```
mkinitcpio -P
```

## Mengaktifkan aplikasi yang ingin digunakan 

```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 
```

## Melepas mounting yang telah dibuat

```
exit
umount -R /mnt
```

### **STOP REKAMAN**

**CTRL+D**

**asciineme upload nebula7.cast**

**setelah melakukan upload rekaman asciinema, lalu foto link yang ditampilkan**

```
reboot
```

## Selesai

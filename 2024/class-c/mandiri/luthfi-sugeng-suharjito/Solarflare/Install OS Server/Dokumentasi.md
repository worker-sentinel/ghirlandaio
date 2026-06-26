## Konek Internet dlu
```
iwctl
```
> fungsinya: buat hubbungin arch linux ke jaringan wifi.

## Cek penyimpanan
```
lsblk
```
> fungsinya: yaa cek ulang ntar ditampilin semua tu SSD, HDD, NVMe beserta partisinya.

## Bikin Enkripsi LUKS
```
cryptsetup luksFormat /dev/partisi_root
```
> fungsinya: bikin LUKS pada partisi root

## Open LUKS
```
cryptsetup luksOpen /dev/partisi_root server
```
> fungsinya: buka partisi yang udah di partisi

## Membuat Physical Volume 
```
pvcreate /dev/mapper/server
```
> mengganti device menjadi Physical Volume (PV) milik LVM

## Membuat Volume Group
```
vgcreate SERVER /dev/mapper/server
```
> fungsinya: bikin volume group bernama SERVER

## Bikin Logical Volume
```
lvcreate -L 8G SERVER -n root
```
> bikin partisi virtual

```
lvcretae -L 8G SERVER -n vars
```
> membuat /var

```
lvcreate -L 1G SERVER -n vlog
```
> membuat /var/log

```
lvcreate -L 1G SERVER -n vaud
```
> membuat /var/log/audit

```
lvcreate -L 1G SERVER -n vtmp
```
> membuat /var/tmp

```
lvcreate -L 5G SERVER -n home
```
> untuk /home

```
lvcreate -L 5G SERVER -n podman
```
> untuk
```
/ver/lib/containers
```

## Format Partisi
```
mkfs.ext4 /dev/SERVER/root
```
> bikin filesystem EXT4

## Cek kembali
```
lsblk
```

## Mounting
```
mount /dev/SERVER/root /mnt
```
> udah kitu ntar root, home, vars, vlog semua muanya di mounting yak.

## Install dah tu Arch linux lu
```
pacstrap /mnt base intel-ucode linux lts.. bla bla
```
> bisa base, linux-lts, linux-frimware, terus driver hardware lvm2, firewalld, sudo.

## Bikin fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
> fungsinya: bikin file /etc/fstab saat komputer boot

## Salin config Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
> fungsinya: nyalin konfig jaringan dari live installer ke sistem baru.

## Tambahin tmpfs
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```

## Masuk ke Sistem baru
```
arch-chroot /mnt
```

## Sinkronin waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

## Locale
```
locale-gen
```

```
locale > /etc/locale.conf
```

## Bikin user
```
useradd -m SERVER
```

```
passwd SERVER
```

```
echo "SERVER ALL=(ALL:ALL) ALL" > /etc/sudoers.d/SERVER
```

## Setting parameter boot
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi_root)=server root=/dev/SERVER/root" > /etc/cmdline.d/01-boot.conf
```

## Mensetting mkinitcpio
> di file mkinitcpio.conf, cari hook terus tambahin:
```
sd-encrypt
```

```
lvm2
```

## Install Bootloader
```
bootctl --path=/mnt/boot install
```

## Bikin initramfs
```
mkinitcpio -P
```

## Aktifin layanan
```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
```

```
systemctl enable iwd
```

## Selese
```
exit
```

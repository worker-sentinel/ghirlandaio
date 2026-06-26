# lts ganti kernel hardened
## hubungkan ke internet

```
iwctl
```
```
device list
```

## Untuk melihat jaringan yang tersedia

```
station wlan0 scan
```
```
station wlan0 get-network
```
```
station wlan0 connect “nama wifi”
```
```
exit
```

## Memeriksa jaringan yang terhubung

```
ping 8.8.8.8
```
```
lsblk
```

## Install kernel hardened

```
pacman -S linux-hardened-headers
```

## Mengedit file konfigurasi

```
nvim /etc/fstab
```
```
nvim /etc/mkinitcpio.d/linux
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

Hapus semua hastag # dan tambahin seperti dibawah ini:
ALL_config=”/etc/mkinitcpio.conf”
ALL_kver=”/boot/vmlinuz-linux-hardened”
ALL_kerneldest=”/boot/kernel/vmlinuz-linux-hardened”

#default_image=”/boot/initramfs-linux-hardened.img”
default_uki=”/efi/Linux/arch-linux-hardened.efi”

## Mengenerate ulang initramfs

```
mkinitcpio -P
```

## Mengecek hasil build kernel

```
ls /boot/efi/Linux/arch-linux–hardened.efi
```
```
ls /boot/efi/Linux
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

## Tambahkan pada bagian ini
default_uki=”/boot/efi/Linux/arch-linux-hardened.efi”

## Cek ulang

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
```
mkinitcpio -P
```
```
ls /boot/efi/Linux/arch-linux–hardened.efi
```
```
exit
```

# Setup Linux Hardened
```
sudo su
```
```
pacman -S linux-harderned linux-hardened-headers
```

## Menyesuaikan Preset Mkinitcpio
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>Cek konfigurasi kernel lts, sebagai referensi sebelum mengonfigurasi kernel hardened

```
nvim /etc/mkinitcpio.d/linux-harderned.preset
```
```
mkinitcpio -P
```
```
ls /boot/efi/linux/linux-harderned-headers
```

## Menyesuaikan Konfigurasi Boot
```
nvim /etc/mk
```
```
nvim /etc/mkinitcpio.d/
```
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
```
cat /etc/mkinitcpio.d/linux-lts.preset
```

## Menempatkan Kernel ke Direktori Boot
```
cd /boot/
```
```
ls
```
```
mv vmlinuz-linux-hardened kernel/
```
```
ls
```

## Verifikasi EFI Image
```
mkinitcpio -P
```
```
ls /boot/efi/linux/arch-linux-hardened.efi
```

## Selesai
```
exit
```

# Hardening Module Kernel
```
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep jffs2
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep bluetooth
```

## Menonaktifkan Module Yang Tidak Diperlukan
```
nvim /etc/modprobe.d/01-custum.conf
```
ADD
```
install bluetooth /bin/false
blacklist bluetooth
install usb-storage /bin/false
blacklist usb-storage
```

```
mkinitcpio -P
```

## Selesai
```
exit
```

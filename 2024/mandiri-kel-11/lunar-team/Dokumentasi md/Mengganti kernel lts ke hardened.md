# Mengganti kernel linux-lts ke linux-hardened
### Konfigurasi Koneksi Jaringan
```
iwctl
```
```
device list
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
station wlan0 connect nama_wifi
```
```
exit
```
### Memeriksa koneksi jaringan
```
ping 8.8.8.8
```
### Menginstall Kernel Linux Hardened
```
pacman -S linux-hardened linux-hardened-headers
```
### Mengedit file konfigurasi
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

 samakan dengan yang dibawah :

```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/efi/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```
### Generate initramfs
  ```
mkinitcpio -P
```
### Verifikasi Hasil Kompilasi Kernel
```
ls /boot/efi/Linux/arch-linux-hardened.efi
ls /boot/efi/Linux
```
### Keluar dari akses Superuser
```
exit
```
```
reboot
```

harusnya pas di boot menu terdapat 2 pilihan :
1. linux hardened (pilih yang ini)
2. linux lts

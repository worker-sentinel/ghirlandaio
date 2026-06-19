# Setup Kernel Server
## Membuat file konfigurasi
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
>Insert (klik I)
```
>ketik install usb-storage /bin/false
```
>blacklist usb-storage
```
>ESC :wq
```
## Memuat ulang konfigurasi modul
```
sudo modprobe -r usb-storage
```
## Initramfs
```
mkinitcpio -P
```
## Mengedit file preset kernel LTS
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
## Melihat partisi
```
lsblk
```
## melihat isi boot
```
ls /boot/efi/
```
## Initramfs
```
mkinitcpio -P
```

exit

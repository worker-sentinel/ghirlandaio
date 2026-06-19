
# Kernel Admin
## Membuat file konfigurasi
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
>Insert (klik I)

>ketik install usb-storage /bin/false

>blacklist usb-storage

>ESC :wq
## Memuat ulang konfigurasi modul
```
sudo modprobe -r usb-storage
```
## Initramfs
```
mkinitcpio -P
```
```
exit
```

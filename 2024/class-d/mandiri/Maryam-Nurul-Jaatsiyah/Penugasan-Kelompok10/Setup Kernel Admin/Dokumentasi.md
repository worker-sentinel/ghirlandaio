# Kernel Admin
## Membuat file konfigurasi
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
>Insert (klik I)

>ketik install usb-storage /bin/false

>blacklist usb-storage

>ESC :wq
<img width="720" height="667" alt="WhatsApp Image 2026-06-19 at 4 00 10 AM" src="https://github.com/user-attachments/assets/68967ec8-3a01-4d83-935d-39215f0a3aba" />

## Memuat ulang konfigurasi modul
```
sudo modprobe -r usb-storage
```
## Initramfs
```
mkinitcpio
```
```
exit
```

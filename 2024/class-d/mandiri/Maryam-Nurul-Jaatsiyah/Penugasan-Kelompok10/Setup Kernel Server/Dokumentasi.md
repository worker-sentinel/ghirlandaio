# Setup Kernel Server
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
## Mengedit file preset kernel LTS
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
<img width="683" height="980" alt="WhatsApp Image 2026-06-19 at 4 17 42 AM" src="https://github.com/user-attachments/assets/fc9b3cd1-45d0-4456-b5ac-82964c8b982e" />

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
```
exit
```
 

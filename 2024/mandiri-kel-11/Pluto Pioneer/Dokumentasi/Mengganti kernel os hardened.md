# Mengganti Linux-lts ke Linux-hardened

## Merekam Asciinema
```
asciinema rec nama_file.cast
```
## Connect Jaringan
```
iwctl
```
**Cek jaringan terdekat ada apa aja**
```
station wlan0 get-networks
```
**Masukin nama jaringannya**
```
station wlan0 connect (nama jaringan)
```
```
masukin pass
```
```
exit
```
## Periksa jaringan
```
ping 1.1.1.1
```
## Cek partisi
```
lsblk
```
## Instal packages linux hardened
```
pacman -S linux-hardened linux-hardened-headers
```
## Mengedit konfigurasi mkinitcpio
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
> klik I untuk menghapus tagar pertama ALL_config, ALL_kerneldest. Menambahkan tagar pada default_image. Lalu menghapus tagar pada default_uki. Selanjutnya menjadi seperti ini = “/boot/EFI/Linux/arch-linux-hardened.efi” kemudian klik esc
```
:wq
```
## Konfigurasi yang sudah ditentukan sebelumnya
```
mkinitcpio -Preset
```
## Mengecek file kernel
```
ls /boot/EFI/Linux/arch-linux-hardened.efi
```

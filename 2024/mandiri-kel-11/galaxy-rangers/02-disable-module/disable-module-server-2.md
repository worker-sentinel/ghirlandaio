# Root
```
sudo su
```
# Disable Module
```
lsmod | grep cramfs 
lsmod | grep freevxfz 
lsmod | grep hfs 
lsmod | grep hfsplus 
lsmod | grep jffs2 
lsmod | grep overlayfs
lsmod | grep squashfs
lsmod | grep udf
lsmod | usb-storage 
```
> Walaupun usb-storage kosong, harus tetep di blokir
```
sudo nvim /etc/modprobe.d/[custom name].conf
install usb-storage /bin/false 
```
> False = disable 
> True = enable (aktif)
```
blacklist usb-storage abis itu
```
> esc 
```
:wq
sudo mkinitcpio -P
```
---
# CATATAN
> Jika setiap command disable module tidak ada output apa-apa, maka artinya memang tidak aktif

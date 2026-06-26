# Disable Module
```
nvim /etc/modprobe.d/hardening.conf
```
```
install    cramfs           /bin/false


install    freexfs          /bin/false


install    hfs              /bin/false


install    hfsplus          /bin/false


install    jffs2            /bin/false


install    udf              /bin/false


install    fire-wire-core   /bin/false


install    usb_storage      /bin/false
```
## Pemeriksaan Modul CramFS
```
sudo lsmod | grep c
```
## Pemeriksaan Modul FreeVXFS
```
sudo lsmod | grep freevxfs
```
## Pemeriksaan Modul HFS
```
sudo lsmod | grep hfs
```
## Pemeriksaan Modul HFS+
```
sudo lsmod | grep hfsplus
```
## Pemeriksaan Modul JFS
```
sudo lsmod | grep jfs
```
## Pemeriksaan Modul SquashFS
```
sudo lsmod | grep squashfs
```
## Pemeriksaan Modul UDF
```
sudo lsmod | grep jfs
```
## Pemeriksaan Driver USB Storage
```
sudo lsmod | usb-storage
```
## Pemeriksaan Modul OverlayFS
```
sudo lsmod | grep overlayfs
```
## Konfigurasi Penonaktifkan Modul Kernel
```
sudo nvim/etc/modprobe.id/disable-module.conf
```
## Identifikasi Perangkat dan Partisi Disk
```
lsblk
```
## Regenerasi Initramfs
```
mkinitcpio -P
```

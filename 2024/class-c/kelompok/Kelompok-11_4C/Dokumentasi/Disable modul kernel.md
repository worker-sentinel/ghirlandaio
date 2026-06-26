## Disable modul kernel

## Menonaktifkan beberapa modul kernel

**Cek module kernel yang sedang aktif maupun tidak**
```
lsmod | grep cramfs
```
```
lsmod | grep freexfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep jffs2
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
``` 
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep usb-storage
```
```
lsmod | grep bluetooth
```
**Jika mucul output atau keterangan yang menandakan module itu aktif maka kita harus menonaktifkannya dengan mengedit file konfigurasi**

```
nvim /etc/modprobe.d/01-custom.conf

```
```
install nama_modul /bin/false
blacklist nama_modul
```

**Artinya: modul-modul tersebut dilarang dimuat oleh kernel**

## Rebuild initramfs 
```
mkinitcpio -P
```

**untuk memperbarui initramfs**

## Selesai


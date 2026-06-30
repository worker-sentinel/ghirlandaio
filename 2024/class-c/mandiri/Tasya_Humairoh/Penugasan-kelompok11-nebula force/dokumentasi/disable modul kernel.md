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

```
nvim /etc/modprobe.d/01-custom.conf

```
```
install usb-storage /bin/false
blacklist usb-storage
install usb-storage /bin/false
blacklist usb-storage
```

```
mkinitcpio -P
```
## Selesai

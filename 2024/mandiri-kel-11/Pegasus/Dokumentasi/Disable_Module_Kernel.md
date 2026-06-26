### Disable modul kernel

### Nonaktifkan modul kernel

```
lsmod | grep cramfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep jffs2
```
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep overlay
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
``` 
```
lsmod | grep usb-storage
```
```
lsmod | grep bluetooth
```
```
nvim /etc/modprobe.d/namafile.conf

```
**Lalu ketik**
```
install usb-storage /bin/false
blacklist usb-storage
```
```
install bluetooth /bin/false
blacklist bluetooth
```

### Rebuild
```
mkinitcpio -P
```

### SELESAI

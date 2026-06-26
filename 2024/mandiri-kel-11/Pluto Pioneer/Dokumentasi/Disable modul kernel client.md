# Disable module

## Mengedit file konfigurasi

```
nvim /etc/modprobe.d/01-custom.conf
```
```

# Disable module

```
lsmod | grep crampfs
lsmod | grep freevxfs
lsmod | grep jffs2
lsmod | grep hfs
lsmod | grep hsplus
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep bluetooth
```
## Mengedit file konfigurasi

```
nvim /etc/modprobe.d/01-custom.conf
```

```
install bluetooth /bin/false 
blacklist bluetooth
install usb-storage /bin/false
blacklist usb-storage
```

# Mkinitcpio

```
mkinitcpio -P
```

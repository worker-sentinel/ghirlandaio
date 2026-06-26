## Cek semua modul kernel

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

jika muncul modul aktif maka edit dengan perintah

```
nvim /etc/modprobe.d/01-custom.conf
```

Klik i

```
install bluethooth /bin/false
blacklist bluetooth
install usb-storage /bin/false
blacklist usb-storage
```

Klik esc kemudian :wq

## Ulang initramfs

```
mkinitcpio -P
```

exit

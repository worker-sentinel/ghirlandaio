# Dokumentasi Disable Kernel

```
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep overlayfs
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
```
```
nvim /etc/modprobe.d/disable-module.conf
```

## Ketik
```
install usb-storage /bin/false                             
blacklist usb-storage 
```

```
lsmod | grep afs
lsmod | grep ceph
lsmod | grep cifs
lsmod | grep exfat
lsmod | grep ext
lsmod | grep fat
lsmod | grep fscache
lsmod | grep fuse
lsmod | grep gfs2
lsmod | grep nfs_common
lsmod | grep nfsd      
lsmod | grep smbfs_common
```

```
nvim /etc/modprobe.d/disable-module.conf
```

```
install usb-storage /bin/false
blacklist usb-storage 
```

```
mkinitcpio -P      
```

## Keluar
```
exit
```

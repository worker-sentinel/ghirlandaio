## Disable Module
```
lsmod | grep cramfs
```
```
lsmod | grep freevxfs
```
```
grep jffs2
```
```
grep hfs
```
```
grep hfsplus
```
```
grep squashfs
```
```
grep udf
```
```
grep usb-storage
```
```
grep bluetooth
```
```
nvim /etc/modprobe.d/01-custom.conf
```
## add configuration below
```
install bluetooth /bin/false
blacklist bluetooth 
install usb-storage /bin/false
blacklist usb-storage 
```
```
mkinitcpio -P 
```

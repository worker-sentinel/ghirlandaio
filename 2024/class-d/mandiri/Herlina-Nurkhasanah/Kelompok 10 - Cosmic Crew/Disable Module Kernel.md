# Menonaktifkan Modul Kernel
```
nvim /etc/modprobe.d/hardening.conf
```
## Add Value
```
install cramfs /bin/false

install freexfs /bin/false

install hfs /bin/false

install hfsplus /bin/false

install jffs2 /bin/false

install udf /bin/false

install usb-storage /bin/false

install firewire-core /bin/false
```

## Memperbarui Initramfs
```
nvim /etc/modprobe.d/hardening.conf
```
```
mkinitcpio -P
```
## Verifikasi Modul Kernel
```
lsmod
```
```
lsmod | grep cramfs
```
```
lsmod | grep freexfs
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
```
```
lsmod | grep jffs2
```
```
lsmod | grep udf
```
```
lsmod | grep firewire-core
```
```
lsmod | grep usb-storage
```
```
exit
```

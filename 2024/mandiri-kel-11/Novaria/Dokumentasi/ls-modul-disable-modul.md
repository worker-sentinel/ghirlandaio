# Cek modul yang ingin kita disable
```
lsmod | grep cramfs
```
```
lsmod | grep freexfs
```
```
lsmod | grep hfsplus
```
```
lsmod | grep jffs2
```
```
lsmod | grep squashhfs
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
lsmod | grep bluetooth
```

# bila setelah di lsmod ada systax, itu perlu disable
```
nvim /etc/modprobe.d/hardenings.conf
```
### isi yang perlu disable
```
install usb-storage /bin/false
blacklist usb-storage
```
```
install bluetooth /bin/false
blacklist bluetooth
```



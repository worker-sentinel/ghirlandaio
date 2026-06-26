```
ssh nayla@10.219.56.6
masukin password (untuk mencoba lagi)
```
```
lsmod | grep cramfs
lsmod | grep frefexfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep overlayfs
lsmod | grep squashhfs
lsmod | grep hfsplus
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
```
```
sudo nvim /etc/modeprobe.d/disable.module.conf
masukin passwd
klik i
install usb-storage /bin/false
blacklist
ternyata ada yang kurang
```
```
lsmod | grep unused filesystem
itu salah karena tidak ada file or directory
lsmod | grep unused
```
```
logout
exit
```

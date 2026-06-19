# Disable Module Kernel pada Server
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Salfa Firyal Hasanah
3. Fatma Ramadhani
4. Aditya Pangruwating Dhiyu
5. Fauzan Azhiimi
6. Ahmad Hafiz Baihaqi
## Check status module
```
modprobe -n -v cramfs
```
```
modprobe -n -v treevxfs
```
```
modprobe -n -v hfs
```
```
modprobe -n -v hfsplus
```
```
modprobe -n -v jffs2
```
```
modprobe -n -v overlay
```
```
modprobe -n -v overlayss
```
```
modprobe -n -v squashfs
```
```
modprobe -n -v udf
```
```
modprobe -n -v firewire-core
```
```
modprobe -n -v usb-storage
```
```
modprobe -n -v atm
```
```
modprobe -n -v can
```
```
modprobe -n -v dccp
```
```
modprobe -n -v rds
```
```
modprobe -n -v sctp
```
```
modprobe -n -v tipc
```
```
nvim /etc/modprobe.d/01-block.conf
```
**isi dengan:**
```
install cramfs /bin/false
blacklist cramdfs
install hfs /bin/false
blacklist hfs
install hfsplus /bin/false
blacklist hfsplus
install jffs2/bin/false
blacklist jffs2
install squashfs /bin/false
blacklist squashfs
install udf /bin/false
blacklist udf
install firewire-core /bin/false
blacklist firewire-core
install usb-storage /bin/false
blacklist usb-storage
install atm /bin/false
blacklist atm
install can /bin/false
blacklist can
install rds /bin/false
blacklist rds
install sctp /bin/false
blacklist sctp
install tipc /bin/false
blacklist tipc
```
```
:wq
```
## Lanjut, kita akan verifikasi
```
modprobe -r cramfs 2> /dev/null
```
```
modprobe -r hfs 2> /dev/null
```
```
modprobe -r hfsplus 2> /dev/null
```
```
modprobe -r jffs2 2> /dev/null
```
```
modprobe -r squashfs 2> /dev/null
```
```
modprobe -r udf 2> /dev/null
```
```
modprobe -r firewire-core 2> /dev/null
```
```
modprobe -r usb-storage 2> /dev/null
```
```
modprobe -r atm 2> /dev/null
```
```
modprobe -r can 2> /dev/null
```
```
modprobe -r rds 2> /dev/null
```
```
modprobe -r sctp 2> /dev/null
```
```
modprobe -r tipv 2> /dev/null
```
```
modprobe -r tipc 2> /dev/null
```
```
mkinitcpio -P
```
```
exit
```

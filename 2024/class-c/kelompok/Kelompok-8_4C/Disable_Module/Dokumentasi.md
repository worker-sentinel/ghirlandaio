# DOKUMENTASI DISABLE MODULE KERNEL

## Blacklist modul kernel via Modprobe
```
nvim /etc/modprobe.d/modprobe.conf
```
Masukan ini semua
```
blacklist hfs

install hfsplus /bin/false
blacklist hfsplus

install jffs2 /bin/false
blacklist jffs2

install overlay /bin/false
blacklist overlay

install squashfs /bin/false
blacklist squashfs

install udf /bin/false
blacklist udf

install firewire_core /bin/false
blacklist firewire_core

install usb_storage /bin/false
blacklist usb_storage

install ceph /bin/false
blacklist ceph

install cifs /bin/false
blacklist cifs

install exfat /bin/false
blacklist exfat

install fscache /bin/false
blacklist fscache

install fuse /bin/false
blacklist fuse

install gfs2 /bin/false
blacklist gfs2

install nfs_common /bin/false                                                       blacklist nfs_common

install nfsd /bin/false
blacklist nfsd

install smbfs_common /bin/false
blacklist smbfs_common

install atm /bin/false                                                            blacklist atm

install can /bin/false
blacklist can

install dccp /bin/false
blacklist dccp                                                                         

install rds /bin/false
blacklist rds

install sctp /bin/false
blacklist sctp

install tipc /bin/false
blacklist tipc 

install bluetooth /bin/false
blacklist bluetooth
``` 
>lalu esc :wq

---
## Filesystem Hardening Verification
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+cramfs\b'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+cramfs\b'
```
```
lsmod | grep 'freevxfs'
```
```
lsmod | grep 'freevxfs'
```
```
 modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+freevxfs\b'
```
```
lsmod | grep 'hfsplus'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+hfsplus\b' 
```
```
lsmod | grep 'jffs2'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+jffs2\b'
```
```
lsmod | grep 'overlay'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+overlay\b'
```
```
lsmod | grep 'squashfs'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+squashfs\b'
```
```
lsmod | grep 'udf'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+udf\b'
```
```
lsmod | grep 'firewire-core'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+firewire-core\b'           ```
```
lsmod | grep 'usb-storage'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+usb-storage\b'
```
```
lsmod | grep 'ceph'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+ceph\b'
```
```
lsmod | grep 'cifs'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+cifs\b'
```
```
lsmod | grep 'exfat'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+exfat\b'
```
```
lsmod | grep 'fscache'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+fscache\b'
```
```
lsmod | grep 'fuse'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+fuse\b'
```
```
lsmod | grep 'gfs2'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+gfs2\b'
```
```
lsmod | grep 'nfs_common'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+nfs_common\b'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+nfsd\b'
```
```
lsmod | grep 'nfsd'
```
```
lsmod | grep 'smbfs_common'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+smbfs_common\b
```
```
lsmod | grep 'atm
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+atmb'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+can\b'
```
```
lsmod | grep 'can'
```
```
lsmod | grep 'dccp'                                                                    ```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+dccp\b'
```
```
lsmod | grep 'tipc'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+tipc\b'
```
```
lsmod | grep 'rds'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+rds\b' 
```
```
lsmod | grep 'sctp'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+sctp\b'
```
```
lsmod | grep 'bluetooth'
```
```
modprobe --showconfig | grep -P -- '\b(install|blacklist)\h+bluetooth\b'
```
```
mkinitcpio -P
```

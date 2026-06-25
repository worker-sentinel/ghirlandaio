## Mengecheck module kernel yang belum disable

sudo su
– lsmod | grep cramfs (kalau kosong gausah)
–sudo lsmod | grep freevxfz
Module Kernel hfs
- sudo lsmod | grep hfs
Module kernel hfsplus
- sudo lsmod | grep hfsplus
Module kernel jffs2
- sudo lsmod | grep jffs2
Module kernel overlayfs
- sudo lsmod | grep overlayfs
Module kernel squashfs
- sudo lsmod | grep squashfs
Module kernel udf
- sudo lsmod | grep udf
Module kernel usb-storage
- sudo lsmod | usb-storage
Walaupun usb-storage kosong, harus tetep di blokir
- sudo nvim /etc/modprobe.d/01-custom.conf

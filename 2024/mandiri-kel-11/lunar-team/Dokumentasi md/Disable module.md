# Dokumentasi disable module
## Cek Module Kernel
### Module Kernel Cramfs
```
sudo lsmod | grep cramfs
```
### Module Kernel Freevxfs
```
sudo lsmod | grep freevxfs
```
### Module Kernel hfs
```
sudo lsmod | grep hfs
```
### Module Kernel hfsplus
```
sudo lsmod | grep hfsplus
```
### Module Kernel jffs2
```
sudo lsmod | grep jffs2
```
### Module Kernel overlayfs
```
sudo lsmod | grep overlayfs
```
### Module Kernel squashfs
```
sudo lsmod | grep squashfs
```
### Module Kernel udf
```
sudo lsmod | grep udf
```
### Module Kernel usb-storage
```
sudo lsmod | grep usb-storage
```
## Blokir Module usb-storage
1. Buka/edit file konfigurasi modprobe:
```
sudo nvim /etc/modprobe.d/01-custom.conf
```
2. Tambahkan baris berikut di dalam file:
```
install usb-storage /bin/false
blacklist usb-storage
```
3. Simpan dan keluar dari nvim:
```
Esc lalu :wq
```
4. Regenerate initramfs agar perubahan diterapkan:
```
sudo mkinitcpio -P
```

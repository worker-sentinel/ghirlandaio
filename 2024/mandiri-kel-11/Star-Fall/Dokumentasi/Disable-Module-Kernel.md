## Mengecheck module kernel yang belum disable

```
sudo su
```
> Module Kernel Cramfs
```
lsmod | grep cramfs
```
> Module Kernel Freevxfz
```
lsmod | grep freevxfz
```
> Module Kernel hfs
```
lsmod | grep hfs
```
> Module Kernel hfsplus
```
lsmod | grep hfsplus
```
> Module Kernel jffs2
```
lsmod | grep jffs2
```
> Module Kernel overlayfs
```
lsmod | grep overlayfs
```
> Module Kernel squashfs
```
lsmod | grep squashfs
```
> Module Kernel udf
```
lsmod | grep udf
```
> Module Kernel usb-storage
```
lsmod | grep usb-storage
```
> Module Kernel Bluetooth
```
lsmod | grep bluetooth
```
<img width="2304" height="1440" alt="Screenshot_20260625_224238_Samsung Browser" src="https://github.com/user-attachments/assets/312e52b5-79bc-4d6c-adda-60812c7859ba" />

> Disable module yang tidak diperlukan
```
sudo nvim /etc/modprobe.d/01-custom.conf
```

> Disable bluetooth dan USB
<img width="2304" height="1440" alt="Screenshot_20260626_010245_Samsung Browser" src="https://github.com/user-attachments/assets/1179cca0-b155-4b00-b736-b24de81ed4b9" />

```
mkinitcpio -P
```























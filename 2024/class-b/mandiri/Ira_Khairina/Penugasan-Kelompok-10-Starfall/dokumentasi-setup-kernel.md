Nonaktifkan module kernel yang tidak dipakai untuk memperkecil celah keamanan.

#### 1. Buat file konfigurasi 
```bash
	sudo nvim /etc/modprobe.d/01-hard.conf
```

#### Isi dengan: 
```
install usb-storage /bin/false
blacklist usb-storage
```

Lalu esc :wq

```bash
sudo modprobe -r usb-storage
````

#### Lalu rebuild initramfs

```bash
mkinitcpio -P
```

#### reboot
`sudo reboot`

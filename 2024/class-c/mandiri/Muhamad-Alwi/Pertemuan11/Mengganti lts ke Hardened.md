## CARA MENGUBAH KERNEL LTS KE HARDENED

### install Linux Hardened (pastikan sudah sudo su terlebih dahuhu)
```
pacman -S linux-hardened linux-hardened-headers
```

### kemudian masuk ke Nvim untuk mengATUR Preset linux hardened
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
# mkinitcpio preset file untuk 'linux-hardened'

```
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/efi/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```

### Generate ulang initramfs
```
mkinitcpio -P
```


### Cek lagi apakah file sudah terbentuk
```
ls /boot/efi/linux/arch-linux-hardened.efi
```
### Jika sudah berhasil, keluar dan reboot
```
exit
```
```
reboot
```




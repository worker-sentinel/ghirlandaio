## Install Package Kernel Linux-Hardened
```
pacman -S linux-hardened linux-hardened-headers
```

## Lalu cek config apakah sudah benar 
```
nvim /etc/fstab
```

## Cek juga
```
nvim /etc/mkinitcpio.d/linux
```

## Cek
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```

## Masukan value
samakan dengan value ini
```
# mkinitcpio preset file for the 'linux-hardened' package
ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"
PRESETS=('default')
#PRESETS=('default' 'fallback')
#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/efi/Linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"
#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"
```

## Simpan config
```
mkinitcpio -P
```

## Cek apakah sudah terinstall Linux-Hardened
```
ls /boot/efi/Linux/arch-linux-hardened.efi
```

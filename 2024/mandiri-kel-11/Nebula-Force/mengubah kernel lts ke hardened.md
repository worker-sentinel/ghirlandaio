## MENGUBAH KERNEL LTS KE HARDENED
Langkah Pertama
```
sudo su
```
## Melakukaan instalasi linux hardened
```
pacman -S linux-hardened linux-hardened-headers 
```
## Mengatur preset linux hardened
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
```
# mkinitcpio preset file for the 'linux-hardened' package
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
## Melakukan Generate ulang initramfs
```
mkinitcpio -P
```
## Melakukan pengecekan ulang apakah file sudah terbentuk
```
ls /boot/efi/linux/arch-linux-hardened.efi
```
## Setelah Keluar:
```
exit
```
## Selesai:
```
reboot
```

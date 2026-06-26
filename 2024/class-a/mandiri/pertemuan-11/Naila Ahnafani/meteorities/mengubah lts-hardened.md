## Mengubah Lts-Hardened
```
sudo su
```
## Install linux hardened
```
pacman -S linux-hardened linux-hardened-headers
```
## Melakukan Konfigurasi Preset Linux Hardened
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
```
# mkinitcpio preset file for the 'linux-hardened' package 

ALL_config="/etc/mkinitcpio.conf"                                                                        
ALL_kver="/boot/vmlinuz-linux-hardened"                                                                  
ALL_kerneldest="/boot/vmlinuz-linux-hardened"                   

PRESETS=('default')                                                                                      
#PRESETS=('default' 'fallback')    

#default_config="/etc/mkinitcpio.conf"                                                                  
#default_image="/boot/initramfs-linux-hardened.img"           
default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"                                                    
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"                 

#fallback_config="/etc/mkinitcpio.conf"                                                                  
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"                                           
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"                                       
#fallback_options="-S autodetect"

## Mengonfugurasi ulang initramfs
```
mkinitcpio -P 
```
## Memastikan File Telah Berhasil dibuat
```
ls /boot/EFI/linux/arch-linux-hardened.efi
```
## Keluar
```
exit
```
```
reboot
```

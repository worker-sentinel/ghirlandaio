# Mengubah kernel lts menjadi hardened

## Mengaktifkan rekaman asciinema
```
asciinema record (nama file).cast
```

## Menghubungkan ke jaringan wifi
```
sudo su
```
```
iwctl
```
```
device list
```

```
station wlan0 get-networks
```
menampilkan hasil scan berupa daftar SSID yang ditemukan

```
station wlan0 scan
```
meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar
```
station wlan0 connect (nama wifi)
```
```
station wlan0 connect "wifi sekolah"
```
jika nama wifi lebih dari 1 kata. gunakan tanda "..."
```
exit
```
## Menginstall Linux-Hardened
```
pacman -S linux-hardened linux-hardened-headers
```
## Mengatur preset
```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
samakan dengan yang di bawah ini
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
## Generate 
```
mkinitcpio -P
```
## Selesai
```
exit
reboot
```

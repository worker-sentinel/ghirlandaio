# Mengubah kernel linux-lts ke linux-hardened

## 1. Menghubungkan ke internet 
```
iwctl
station wlan0 get-networks
station wlan0 connect "nama wifi"
masukin password
exit
ping 8.8.8.8
```

## 2. Install Kernel Linux Hardened
```
pacman -S linux-hardened-headers
```

## 3. Mengedit file konfigurasi
```
nvim /etc/fstab
nvim /etc/mkinitcpio.d/linux
nvim /etc/mkinitcpio.d/linux-lts.preset
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
> hapus semua # dan ditambahkan seperti ini:
- ALL_config="/etc/mkinitcpio.conf"
- ALL_kver="/boot/vmlinuz-linux-hardened"
- ALL_kerneldest="/boot/kernel/vmlimuz-linux-hardened"

- BAGIAN ENIH GANTI #default_image="/boot/initramfs-linux-hardened.img"
- MENJADI default_uki="/boot/efi/Linux/arch-linux-hardened.efi"

## 4. Men-generate initramfs
```
mkinitcpio -P
```
## 5. Mengecek hasil build kernel
```
ls /boot/efi/Linux/arch-linux-hardened.efi
ls /boot/efi/Linux
```

## 6. FOR THE LAST is
```
exit
```

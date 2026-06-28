# Ganti Linux lts ke hardened

## 1.Membuka file
```
nvim /etc/fstab
```
## 2.Cek ulang
```
lsblk
```
## 3.Menghubungkan Linux ke WiFi 
```
[iwd]# station wlan0 get-networks
```

```
[iwd]# station wlan0 connect scan
```

```
[iwd]# station wlan0 connect Galaxy
```

```
exit
```
## 4.Cek udah connect blm
```
ping 1.1.1.1
```
## 5.Install Package kernel Linux versi hardened manager arch linux
```
pacman -s linux-hardened linux-hardened-headers
```
> jika muncul tulisan Proceed with installation? [Y/n]
> kwtik y aja
## After install
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
> terus, apus tanda pagar di ALL_kerneldest="/boot/EFI/Linux
> sekarang, tambahin tanda pagar di default_image="/boot/iniramfs-linux-hardened.img"
> klo udh, apus tanda pagar dan ganti efi jadi boot di default_uki="/boot/EFI/Linux/arch-linux-hardened.efi"
> klik Esc, abis tu ketik :wq

```
mkinitcpio -P
```

## 6.Cek file boot kernel ny udh ke install blm
```
ls /boot/EFI/Linux/arch-linux-hardened.efi
```
## 7.Selese
```
exit
```



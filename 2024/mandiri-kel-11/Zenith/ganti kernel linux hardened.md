### Download Kernel dan Header

```
pacman -S linux-hardened linux-hardened-headers
```

### Mengedit File fstab
```
nvim /etc/fstab
```

### Mengedit Preset mkinitcpio
```
nvim /etc/mkinitcpio.d/linux
```

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

### Membuat Ulang Initramfs
```
mkinitcpio -P
```

### Memeriksa File EFI Kernel
```
ls /boot/efi/Linux/arch-linux-hardened.efi
```

```
ls /boot/efi/Linux arch-linux-lts.efi
```

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

```
ls /boot/efi/Linux/arch-linux-hardened.efi
```

### Konfirmasi dan Cek Hasil
```
mkinitcpio -P
```

```
ls /boot/efi/Linux/arch-linux-hardened.efi /boot/efi/Linux/arch-linux-hardened.efi
```

### Keluar dari Root
```
exit
```

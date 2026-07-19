# Install Admin
```
lvremove /dev/namavolumegrup/dock
```
```
lsblk
```

## mounting 
```
mount /dev/proc/root /mnt
```
```
mount /dev/namapartisi /mnt/boot
```

## masuk root
```
arch-chroot /mnt
```
```
nvim /etc/fstab
```

## install harus mounting var
```
mount /dev/proc/vars/mnt/var/arch-chroot/mnt
```

## install desktop 
```
pacman -S xfce sddm kitty
```
```
systemctl enable --global sddm
```
```
mkinitcpio -P
```
```
exit
```

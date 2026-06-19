# Install Admin

## 1. Hapus Logical Volume Docker

```bash
lvremove /dev/namavolumegrup/dock
```

## 2. Cek Struktur Disk

```bash
lsblk
```

## 3. Mount Partisi Root

```bash
mount /dev/proc/root /mnt
```

## 4. Mount Partisi Boot

```bash
mount /dev/namapartisi /mnt/boot
```

Contoh:

```bash
mount /dev/sda1 /mnt/boot
```

## 5. Masuk ke Sistem Arch Linux

```bash
arch-chroot /mnt
```

## 6. Edit File fstab

```bash
nvim /etc/fstab
```

- Cari entri `dock`.
- Hapus baris yang mengarah ke volume tersebut.
- Simpan perubahan lalu keluar dari editor.

## 7. Mount Partisi `/var` (jika dipisah)

```bash
mount /dev/proc/var /mnt/var
```

## 8. Install Desktop Environment

```bash
pacman -S xfce4 sddm kitty
```

## 9. Aktifkan SDDM

```bash
systemctl enable sddm
```

## 10. Generate Ulang Initramfs

```bash
mkinitcpio -P
```

## 11. Keluar dari Chroot

```bash
exit
```

## 12. Restart Sistem

```bash
reboot
```

---

# Ringkasan Command

```bash
lvremove /dev/namavolumegrup/dock

lsblk

mount /dev/volumegroup/root /mnt

mount /dev/sda1 /mnt/boot

arch-chroot /mnt

nvim /etc/fstab

pacman -S xfce4 sddm kitty

systemctl enable sddm

mkinitcpio -P

exit

reboot
```

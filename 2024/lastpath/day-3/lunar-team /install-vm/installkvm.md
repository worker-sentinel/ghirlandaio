# Install KVM — Persiapan (Sync & Reboot)

## Sync ulang database
```
sudo pacman -Syy
```
Sync ulang database paket dari repo `core` dan `extra` (force refresh, bukan cuma cek versi lokal). Dipakai sebelum instalasi paket baru biar pacman tau versi terbaru yang tersedia di mirror.

```
[rapiip@susarrr ~]$ sudo pacman -Syy
[sudo] password for rapiip:
:: Synchronizing package databases...
 core       126.5 KiB   40.8 KiB/s 00:03 [####################] 100%
 extra        8.3 MiB    288 KiB/s 00:29 [####################] 100%
```

## Reboot
```
sudo reboot
```
Reboot node setelah sync database, sebelum lanjut instalasi paket KVM (`qemu`, `libvirt`, dkk).

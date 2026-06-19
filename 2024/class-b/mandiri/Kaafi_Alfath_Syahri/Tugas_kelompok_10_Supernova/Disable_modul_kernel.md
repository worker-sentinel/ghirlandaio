# Menonaktifkan Kernel Module yang Tidak Digunakan

## Memeriksa Status Modul Kernel

Jalankan perintah berikut untuk mengecek keberadaan dan status modul yang akan dinonaktifkan:

```
modprobe -n -v cramfs
modprobe -n -v freevxfs
modprobe -n -v hfs
modprobe -n -v hfsplus
modprobe -n -v jffs2
modprobe -n -v overlayfs
modprobe -n -v squashfs
modprobe -n -v udf
modprobe -n -v firewire-core
modprobe -n -v usb-storage
modprobe -n -v atm
modprobe -n -v can
modprobe -n -v dccp
modprobe -n -v rds
modprobe -n -v sctp
modprobe -n -v tipc
```

> Catatan: Pesan seperti `FATAL: Module not found` menunjukkan bahwa modul tersebut tidak tersedia pada kernel sehingga tidak perlu dinonaktifkan.

## Membuat File Konfigurasi Blacklist

Buka file konfigurasi baru:

```
sudo nvim /etc/modprobe.d/01-block.conf
```

Masukkan konfigurasi berikut:

```
install cramfs /bin/false
blacklist cramfs

install hfs /bin/false
blacklist hfs

install hfsplus /bin/false
blacklist hfsplus

install jffs2 /bin/false
blacklist jffs2

install squashfs /bin/false
blacklist squashfs

install udf /bin/false
blacklist udf

install firewire-core /bin/false
blacklist firewire-core

install usb-storage /bin/false
blacklist usb-storage

install atm /bin/false
blacklist atm

install can /bin/false
blacklist can

install rds /bin/false
blacklist rds

install sctp /bin/false
blacklist sctp

install tipc /bin/false
blacklist tipc
```

Simpan perubahan dan keluar dari editor:

```
:wq
```

## Melepas Modul yang Sedang Aktif

Jalankan perintah berikut untuk menghapus modul dari kernel yang sedang berjalan:

```
modprobe -r cramfs 2>/dev/null
modprobe -r hfs 2>/dev/null
modprobe -r hfsplus 2>/dev/null
modprobe -r jffs2 2>/dev/null
modprobe -r squashfs 2>/dev/null
modprobe -r udf 2>/dev/null
modprobe -r firewire-core 2>/dev/null
modprobe -r usb-storage 2>/dev/null
modprobe -r atm 2>/dev/null
modprobe -r can 2>/dev/null
modprobe -r rds 2>/dev/null
modprobe -r sctp 2>/dev/null
modprobe -r tipc 2>/dev/null
```

## Memperbarui Initramfs

Agar konfigurasi diterapkan saat sistem melakukan booting, bangun ulang initramfs dengan perintah:

```
sudo mkinitcpio -P
```

## Referensi 
Dokumentasi Kelompok 9

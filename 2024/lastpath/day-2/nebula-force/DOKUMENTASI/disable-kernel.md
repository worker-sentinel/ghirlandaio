## Disable modul kernel

## Menonaktifkan beberapa modul kernel

**Cek module kernel yang sedang aktif maupun tidak**
Melihat semua daftar modul kernel yang masih aktif dengan menggunakan command

```
lsmod | grep cramfs
```
```
lsmod | grep freexfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep jffs2
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
``` 
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep usb-storage
```
```
lsmod | grep bluetooth
```
**Jika mucul output atau keterangan yang menandakan module itu aktif maka kita harus menonaktifkannya dengan mengedit file konfigurasi**
Membuka buku catatan system dan mengedit aturan baru yang ada disana dengan menggunakan command

```
nvim /etc/modprobe.d/01-custom.conf

```
```
install nama_modul /bin/false
blacklist nama_modul
```

**Artinya: modul-modul tersebut dilarang dimuat oleh kernel**

## Rebuild initramfs 
Untuk merakit ulang system restart dengan menggunakan command

```
mkinitcpio -P
```

**untuk memperbarui initramfs**

## Selesai

bikinin pengertian dan fungsinya untuk apa tiap commandnya dengan bahasa yang mudah

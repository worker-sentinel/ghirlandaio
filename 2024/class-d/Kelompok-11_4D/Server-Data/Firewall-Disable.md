# Firewall
## Masuk ke root

```
sudo su
```

## Cek status

```
systemctl status firewalld
```

jika belum aktif atau status disable maka lakukan perintah

```
systemctl enable -now firewall
```

## Untuk cek semua zona 

```
firewall-cmd --list-all-zone
```

## Hapus service yang tidak dibutuhkan

```
firewall-cmd --zone=(sesuaiin zonanya yang mau dihapus) --remove-service=(masukkan service yang mau dihapus) --permanent
```

## Cek ulang firewall

```
firewall-cmd --reload
```


# Disable kernel

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

jika semua modul aktif maka edit

```
nvim /etc/modprobe.d/01-custom.conf
```
```
install nama_modul /bin/false
blacklist nama_modul
```

selesai

## Ulang initramfs

```
mkinitcpio -P
```

exit

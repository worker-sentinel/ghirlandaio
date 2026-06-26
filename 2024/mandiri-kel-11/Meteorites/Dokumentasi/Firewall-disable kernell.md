# Firewall
## masuk root
```
sudo so
```
## Cek Status
```
systemctl status firewalld
```
## Mengecek Daftar Zona
```
firewalld-cmd --list-all-zone
```
## Hapus Service yang tidak dibutuhkan
```
firewalld-cmd --zone=menyesuaikanzonanya --remove-service={service yang akan dihapus} --permanen
```
karena pada konfigurasi yang digunakan service firewall sudah diatur sesuai kebutuhan, maka tidak ada service tambahan yang perlu dihapus 

## Reload firewall
```
firewall-cmd --reload
```
## Memeriksa Kembali
```
firewall-cmd --list-old-zone
```

# Disable modul kernel
## Menonaktifkan modul kernel yang aktif
cek modul kernel
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
## Apabila module terdeteksi aktif, maka langkah selanjutnya adalah menonaktifkannya melalui pengaturan konfigurasi sistem
```
nvim /etc/modprobe.d/01-custom.conf
```
```
install nama_modul /bin/false
blacklist nama_modul

contoh:
insall bluetooth /bin/false
blacklist bluetooth
```
## mengkonfigurasi ulang initramfs
```
mkinitcpio -P
```
## Selesai

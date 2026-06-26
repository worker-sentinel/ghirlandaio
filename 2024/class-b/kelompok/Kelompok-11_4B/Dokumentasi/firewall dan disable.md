# Tahap Setup Firewall 
## Masuk mode root
```
sudo su
```
## Cek status firewall
```
systemctl status firewall
```
```
systemctl enable firewall
```
```
systemctl start firewall
```
## Melihat semua zona
```
firewall-cmd –list-all-zone
```
## Menghapus service yang tidak dibutuhkan
```
firewall-cmd –zone=menyesuaikanzonanya –remove-service={service yang akan dihapus} --permanent
```
**zona= public, internal, eksternal,work dll sesuai perangkat kalian**
**sisakan service=ssh di zona public**
**Kalau service lebih dari satu gunakan {}**
**Kalau service hanya satu tidak perlu menggunakan {}**
## Reload firewall
```
firewall-cmd --reload
```
**untuk mengaktifkan perubahan konfigurasi yang sudah dibuat**
## Cek ulang
```
firewall-cmd --list-all-zone
```
**untuk memastikan apakah services yang tidak dibutuhkan sudah terhapus**
## Cek modul kernel
**mengecek modul kernel yang aktif**
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
## Disable modul kernel
**menonaktifkan modul yang tidak diperlukan**
```
nvim /etc/modprobe.d/01-custom.conf

```
```
install nama_modul /bin/false
blacklist nama_modul
```
```
contoh:
install bluetooth /bin/false
blacklist bluetooth
```
**setelah itu esc lanjut :wq**
## Update Initramfs
```
mkinitcpio -P
```
**untuk menerapkan perubahan modul**
## Finish

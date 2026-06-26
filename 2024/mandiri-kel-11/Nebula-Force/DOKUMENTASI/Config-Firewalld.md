## CONFIG FIREWALLD 
```
## Untuk Melihat dan memulai zone di firewall
```
firewall-cmd --list-all-zone
```
```
systemctl enable firewalld
```
```
systemctl start firewalld
```
## Mengecek info zone  yang aktif


```
firewall-cmd --info-zone=public
```
```
##Menghapus service dari zone public
```
firewall-cmd --zone=public  --remove-service=dhcpd6-client --permanent
```
```
##Mengecek dan mereload zone 
firewall-cmd --info-zone=public
```
```
firewall-cmd --reload
```
```
firewall-cmd --info-zone=public
```
```
##Menampilkan konfigurasi zone home dan menghapusnya
```
firewall-cmd --info-zone=home
```
```
firewall-cmd --zone=home --remove-service={dhcpd6-client,mdns,samba-client} --permanent
```
```
##Mengcek kembali zone public setelah reload dan reload kembali 
```
firewall-cmd --info-zone=public
```
```
firewall-cmd –reload
```
```
**setelah itu sudo nvim
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
```
insert
```
```
install usb-storage /bin/false
```
```
blacklist usb-storage
```
```
esc
```
```
:wq
```
```
sudo modprobe -r usb-storage
```
```
mkinitcpio -P
```

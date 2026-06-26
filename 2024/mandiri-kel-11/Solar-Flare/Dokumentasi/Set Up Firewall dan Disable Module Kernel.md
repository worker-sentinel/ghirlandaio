# firewall sekalian disable module
## Cek firewall
```
systemctl status firewalld
```
> untuk liat status service firewall
## Melihat Konfigurasi firewall
```
firewall-cmd --list-all-zone
```
> nampilin semua zona firewall
> tiap zona punya aturan beda
## Reload firewall
```
firewall-cmd --reload
```
> nerapin ulang konfigurasi firewall nya tanpa di restart
## Hapus service firewall public
```
firewall-cmd --zone=public --remove-service={cockpit,dhcpv6-client} --permanent
```
> untuk ngapus izin service pada zona public
> yang di hapus:
#### web interface administrasi linux
```
dhcpv6-client
```
#### layanan DHCP IPv6
```
-- permanent
```
## Ngatur zona internal
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,mdns,samba-client} 
```
> menghapus service dari zona internal
> yang di happus:
```
DHCP IPv6
mDNS
Samba client
```
> untuk memperkecil akses jaringan nya
## Mengatur zona home
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client}
```
> untuk ngurangin service yang kebuka pada jaringan home
## Mengatur zona external
```
firewall-cmd --zone=external --remove-service=ssh --permanent
```
> untuk menutup akses SSH dari jaringan external
> SSH digunakan untuk remote login
## Mengatur zona DMZ
```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```
> untuk menutup SSH pada zona DMZ
> DMZ biasanya itu untuk server yang menghadap internet
## Melihat zona lain
```
firewall-cmd --zone=trusted --list-all
```
```
firewall-cmd --zone=drop --list-all
```
```
firewall-cmd --zone=block --list-all
```
> untuk ngeliat aturan masing-masing zona firewall
## Mengecek modul kernel
```
lsmod | grep cramfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep jffs2
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
lsmod | grep usb_storage
```
> untuk mengecek apakah modul kernel tertentu lagi akfif
> tujuan nya untuk ngeliat modul yang gak kepake atau yang mau di nonaktifkan
## Membuat konfigurasi blacklist module
```
nvim /etc/modprobe.d/01-custom.conf
```
> untuk membuka file konfigurasi modul kernel

> ketik commant
```
install bluetooth /bin/false
```
```
blacklist bluetooth
```
```
install usb_storage /bin/false
```
```
blacklist usb_storage
```
## Membuat ulang initramfs
```
mkinitcpio -P
```
> untuk regenerasi initrams semua kernel
# keluar terminal
```
exit
```

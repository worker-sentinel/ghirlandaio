## Konfigurasi Firewalld di Arch Linux

## Mengecek Status Firewalld
```
systemctl status firewalld
```
##  Menampilkan Seluruh Zona Firewall
```
firewall-cmd --list-all-zone
```
## Masuk sebagai Root
```
sudo su
```
## Menampilkan Semua Zona Firewall
```
firewall-cmd --list-all-zone
```
## Reload Konfigurasi Firewalld
```
firewall-cmd --reload
```
## Menghapus Service Cockpit dan DHCPv6 dari Zona Public
```
firewall-cmd --zone=public --remove-service={cockpit,dhcpv6-client} --permanent
```
## Menghapus Service dari Zona Public
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,mdns,samba-client}
```
##  Menghapus Service dari Zona Home
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client}
```
## Menghapus SSH dari Zona External
```
firewall-cmd --zone=external --remove-service=ssh --permanent
```
## Menghapus SSH dari Zona DMZ
```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```
## Melihat Konfigurasi Zona Trusted
```
firewall-cmd --zone=trusted --list-all
```
## Melihat Konfigurasi Zona Drop
```
firewall-cmd --zone=drop --list-all
```
## Melihat Konfigurasi Zona Block
```
firewall-cmd --zone=block --list-all
```
##  Keluar dari Root
```
exit
```

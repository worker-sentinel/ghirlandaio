# Setup Firewall Server
## Mengaktifkan Firewall
```
systemctl enable firewalld
```
## Menjalankan firewall
```
systemctl start firewalld
```
## menampilkan semua zone firewall
```
firewall-cmd --list-all-zones
```
## untuk melihat firewall di zone public
```
firewall-cmd --info-zone=public
```
## menghapus izin service client
```
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```
## memuat ulang firewall agar perubahannya bisa diterapkan
```
firewall-cmd --reload
```
## mengecek firewall zone public
```
firewall-cmd --info-zone=public
```
## Melihat firewall di zone trusted
```
firewall-cmd --info-zone=trusted
```
## Melihat firewall di zone home
```
firewall-cmd --info-zone=home
```
## menghapus service client
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client} --permanent
```
## memuat firewall agar perubahannya bisa diterapkan
```
firewall-cmd --reload
```
## mengecek firewall zone home 
```
firewall-cmd --info-zone=home
```
## melihat firewall dizone work
```
firewall-cmd --info-zone=work
```
## menghapus service client di zone work
```
firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
```
## memuat ulang firewall agar perubahannya bisa diterapkan
```
firewall-cmd --reload
```
## mengecek firewall zone work apakah sudah ke remove service clientnya
```
firewall-cmd --info-zone=work
```
## melihat firewall dizone internal
```
firewall-cmd --info-zone=internal
```
## menghapus service client di zone internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client} --permanent
```
## memuat ulang firewall agar perubahannya bisa diterapkan
```
firewall-cmd --reload
```
## mengecek firewall zone internal apakah sudah ke remove service clientnya
```
firewall-cmd --info-zone=internal
```
## melihat firewall dizone nm-shared
```
firewall-cmd --info-zone=nm-shared
```
```
exit
```

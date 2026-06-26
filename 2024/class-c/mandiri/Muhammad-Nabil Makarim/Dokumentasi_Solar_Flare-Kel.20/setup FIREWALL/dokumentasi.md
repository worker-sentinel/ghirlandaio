# Setup Firewall 

## Akses server melalui SSH
```
ssh SERVER@10.10.3.4
```
## Periksa Konfigurasi Jaringan
```
ip a
```
## Membuat zona firewall baru untuk admin
```
firewall-cmd --permanent --new-zone=admin
```
## Menjalankan service firewalld
```
sudo systemctl start firewalld
```
## Tambahkan IP admin ke zona admin
```
sudo firewall-cmd --permanent --zone=admin --add-source-10.10.3.2
```
## Membuka akses SSH untuk admin
```
sudo firewall-cmd --permanent --zone=admin --add-service=ssh
```
## Membuka port MySQL
```
sudo firewall-cmd --permanent --zone=admin --add-port=330/tcp
```
## Membuka port redis
```
sudo firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
## Membuka port HTTP
```
sudo firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
##Membuka port HTTPS
```
sudo firewall-cmd --permanent --add-port=443/tcp
```
# Konfigurasi zona operator
## Membuat zona operator
```
sudo firewall-cmd --permanent --new-zone=operator
```
## Menambahkan IP operator 
```
sudo firewall-cmd --permanent --zone=operator --add-source=10.10.3.3
```
## Membuka akses SSH untuk operator
```
sudo firewall-cmd --permanent --zone=operator --add-service=ssh
```
## Membuka port redis
```
sudo firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```
## Membuka port HTTP
```
sudo firewall-cmd --permanent --zone=operator --add-port=80/tcp
```
## Konfigurasi zona WIFI
## Membuka zona wifi
```
sudo firewall-cmd --permanent --new-zone=wifi
```
## Menambahkan source jaringan WIFI
```
sudo firewall-cmd --permanent --zone=wifi --add-source=10.10.4.1/24
```
## Membuka HTTP untuk WIFI
```
sudo firewall-cmd --permanent --zonne=wifi --add-port=80/tcp
```
## Membuka HTTPS untuk WIFI
```
sudo firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
## Verifikasi konfigurasi firewall
## Menampilkan seluruh zona firewall
```
sudo firewall-cmd --list-all-zone
```
## Menghapus service dari zona work
```
sudo firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
```
## Menghapus service dari zona home
```
sudo firewall-cmd --zone=home --remove-service=dhcpv6-client,mdns,samba-client,ssh --permanent
```
## Menghapus service dari zona internal 
```
sudo firewall-cmd --zone-internal --remove-service=dhcpv6-client,mdns,samba-client,ssh --permanent
```
## Menghapus SSH dari zona external
```
sudo firelwall-cmd --zone=external --remove-service=ssh --permanent
```
## Membuat ulang konfigurasi firewall
```
sudo firewall-cmd --reload
```

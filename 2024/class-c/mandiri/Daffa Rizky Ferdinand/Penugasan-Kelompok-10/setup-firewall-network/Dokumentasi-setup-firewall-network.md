# Setup Firewall Network
## Login ssh
```
ssh sagitarius@14.26.26.14
```
## Zona firewall admin
```
firewall-cmd --permanent --new-zone=admin
firewall-cmd --permanent --zone=admin --add-source=14.26.26.14
```
## Zona firewall operator
```
firewall-cmd --permanent --new-zone=operator
firewall-cmd --permanent --zone=operator --add-source=14.26.26.13
```
## Zona wifi
```
firewall-cmd --permanent --new-zone=wifi
firewall-cmd --permanent --zone=wifi --add-source=14.14.14.0/24
```
## Membuka ssh pada zona admin
```
firewall-cmd --permanent --zone=admin --add-service=ssh
```
## Membuka port 3306 pada zona admin
```
firewall-cmd --permanent --zone=admin --add-port=3306/tcp
```
## Membuka port 6379 pada zona admin
```
firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
## Membuka HTTP pada admin
```
firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
## Membuka HTTPS pada admin
```
firewall-cmd --permanent --zone=admin --add-port=443/tcp
```
## Membuka ssh pada zona operator
```
firewall-cmd --permanent --zone=operator --add-service=ssh
```
## Membuka port 6379 pada operator
```
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```
## Membuka HTTP pada operator
```
firewall-cmd --permanent --zone=operator --add-port=80/tcp
```
## Membuka HTTPS pada operator
```
firewall-cmd --permanent --zone=operator --add-port=443/tcp
```
## Membuka HTTP
```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
## Membuka HTTPS
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
## Memuat ulang firewall agar perubahan diterapkan
```
firewall-cmd --reload
```
## Keluar
```
exit
```
## Masuk folder
```
cd config/containers/docker-compose-for-slims-master/
```
## Menjalankan containers 
```
podman compose up -d
```
## Mengecek status hostapd
```
systemctl status hostapd
```
## Menjalankan hostapd
```
systemctl start hostapd
```
## Mengecek status hostapd
```
systemctl status hostapd
```
>hostapd (Host Access Point Daemon) untuk mengubah adaptor wifi menjadi access point sehingga perangkat lain dapat terhubung ke perangkat melalui wifi
## logout

logout
```

exit

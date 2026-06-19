# Setup Firewalld Operator

## 1. Connect dan jadi root
```
ssh kel10@10.119.250.30
```
```
sudo su -
```
## 2. Buat zone, lalu reload SEBELUM dikonfigurasi
```
firewall-cmd --permanent --new-zone=operator2
```
```
firewall-cmd --reload
```
## 3. Konfigurasi zone-nya
```
firewall-cmd --permanent --zone=operator2 --add-source=10.10.10.11
```
```
firewall-cmd --permanent --zone=operator2 --add-service=ssh
```
```
firewall-cmd --permanent --zone=operator2 --add-port=6379/tcp
```
```
firewall-cmd --permanent --zone=operator2 --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=operator2 --add-port=443/tcp
```
```
firewall-cmd --reload
```
## 4. Verifikasi
```
firewall-cmd --list-all-zones
```

## 5. Bersihkan zone admin (hapus source, bukan service)
```
firewall-cmd --permanent --zone=admin --remove-source=15.15.5.2
```
```
firewall-cmd --reload
```
```
firewall-cmd --list-all-zones
```
```
exit
```

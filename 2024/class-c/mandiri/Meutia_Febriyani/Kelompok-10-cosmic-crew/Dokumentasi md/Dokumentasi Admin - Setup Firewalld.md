# Setup Firewalld Admin

```
ssh kel10@10.119.250.30
```

```
sudo su
```

```
firewall-cmd --permanent --zone=admin --add-source=10.10.10.10
```
Daftarkan IP admin ke zone admin.

```
firewall-cmd --permanent --zone=admin --add-service=ssh
```
Izinkan akses SSH dari zone admin.

```
firewall-cmd --permanent --zone=admin --add-port=3306/tcp
```
Izinkan akses port MySQL.

```
firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
Izinkan akses port Redis.

```
firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
Izinkan akses HTTP.

```
firewall-cmd --permanent --zone=admin --add-port=443/tcp
```
Izinkan akses HTTPS.

```
exit
```

## Referensi
David aka joshua aka baihaqi

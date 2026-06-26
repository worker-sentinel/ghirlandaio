# Koneksi Server ke Admin
```
ssh F123@10.38.202.71 (ip server)
```

# Melihat Konfigurasi Firewall Saat Ini
```
sudo firewall-cmd --list-all-zone
```

# Menghapus Service DHCP, DNS dan SSH dari Zona nm-shared
```
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dhcp
```
```
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dns
```
```
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=ssh
```
```
sudo firewall-cmd --reload
```

# Memverifikasi Konfigurasi Firewall
```
sudo firewall-cmd --list-all-zone
```

# Menambahkan Service HTTP pada Zona Public dan Membuka Port
```
sudo firewall-cmd --zone=public --add-service=http --permanent
```
```
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
sudo firewall-cmd --reload
```
```
sudo firewall-cmd --list-all-zone
```
```
exit
```

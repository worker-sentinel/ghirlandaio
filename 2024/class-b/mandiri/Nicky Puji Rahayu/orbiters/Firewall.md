### untuk melihat informasi zona
```
firewall-cmd --list-all-zones
```

gunakan ini kalau ingin melihat daftar lengkap seluruh zona beserta aturan (port yang dibuka,layanan yang dizinkan) dimasing-masing zona

### untuk melihat informasi zona secara spesifik 
```
firewall-cmd --info-zone=[nama_zona]
```
ganti nama_zona dengan zona yang mau kamu lihat contohnya: public, home, work

### untuk mengubah zona default
```
firewall-cmd --get-default-zone
```

```
firewall-cmd --set-default-zone= [zone]
```


## service

### untuk melihat list service 
```
firewall-cmd --get-services
```

### untuk melihat informasi service

```
firewall-cmd --info-service [service_name]
```

### untuk menambahkan service ke zone tertentu

```
firewall-cmd --zone=[zone_name] --add-service [service_name]
```

### untuk menghapus service di zone tertentu

```
firewall-cmd --zone=[zone_name] --remove-service [service_name]
```
## ports

### menambahkan port di zona tertentu
```
firewall-cmd --zone= [zone_name] --add-port [port_num] / [protocol]
```

### menghapus port di zona tertentu 

```
firewall-cmd --zone= [zone_name] --remove-port [port_num] / [protocol]
```

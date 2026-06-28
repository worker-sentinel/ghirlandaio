# Menghubungkan Server Aplikasi dan Server Nginx

## 1. Restart service network
 ```bash
 systemctl restart systemd-networkd
 systemctl restrat resolved
 ```

## 2. Cek file yang ada di direktori
 ```bash 
 ls
```

## 3. Cek seluruh konfigurasi firewall
 ```bash
 firewall-cmd --list-all-zones
```

## 4. Cek konfigurasi zona public
 ```bash 
 firewall-cmd --zone=public --list-all
```

## 5. Edit file host
 ```bash 
 nvim /etc/hosts
```
Simpan:
 ```bash
 :wq
```

## 6. Edit konfigurasi SLiMS
 ```bash 
 nvim /etc/slims.conf
```
Simpan:
 ```bash
 :wq
``` 

## 7. Edit virtual host nginx
 ```bash 
 nvim /etc/nginx/sites-available/slims.conf
```
Simpan:
 ```bash 
 :wq
``` 

## 8. Menambahkan rule firewall untuk akses web
 ```bash 
 firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" port port="80" protocol="tcp" accept'
```

## 9. Reload firewall
 ```bash 
 firewall-cmd --reload
```

## 10. Restart nginx
 ```bash 
 systemctl restart nginx
```

## 11. Edit file nginx yang aktif
 ```bash 
 nvim /etc/nginx/sites-enabled/slims.conf
```
simpan:
 ```bash
 :wq
``` 

## 12. Cek alamat IP server
 ```bash 
 ip a
``` 

## 13. Test koneksi ke server lain
 ```bash 
 ping 10.10.1.20
``` 

## 14. Cek status nginx
 ```bash 
 sudo systemctl status nginx
``` 

## 15. Stop firewalld
 ```bash 
 sudo systemctl stop firewalld
``` 

## 16. Cek IP lagi
 ```bash 
 ip a
``` 

## 17. Masuk ke IWD
 ```bash 
 iwctl
``` 

## 18. Lihat konfigurasi hotspot
 ```bash 
 cat /var/lib/iwd/ap/hani.ap
``` 

## 19. Edit konfigurasi hotspot
 ```bash 
 nvim /var/lib/iwd/ap/hani.ap
``` 
Simpan:
 ```bash 
 :wq
``` 

## 20. Restart IWD
 ```bash 
 systemctl restart iwd
``` 

## 21. Masuk lagi ke IWD
 ```bash 
 iwctl
``` 

## 22. Cek konfigurasi nginx
 ```bash
 nginx -t
``` 
output:
 ```bash 
 nginx: configuration file /etc/nginx/nginx.conf test is successful
``` 

## 23. Restart nginx lagi
 ```bash
 systemctl restart nginx
``` 

## 24. Cek status firewalld
 ```bash
systemctl status firewalld
 ``` 

## 25. Uji akses web server
 ```bash 
 curl -v http://10.10.1.20:8080
``` 

## 26. Restart firewalld
 ```bash 
 systemctl restart firewalld
``` 

## 27. Buka port 80 permanen
 ```bash 
 firewall-cmd --zone=public --add-port=80/tcp --permanent
```
 
## 28. Reload firewall
 ```bash 
 firewall-cmd --reload
```

## 29. Edit lagi konfigurasi hotspot
 ```bash 
 nvim /var/lib/iwd/ap/hani.ap
```

## 30. Restart IWD
 ```bash
 systemctl restart iwd
``` 

## 31. Masuk ke IWD
 ```bash
 iwctl
``` 

## 32. Verifikasi IP
 ```bash 
 ip a
``` 

## 33. Selesai / keluar
 ```bash
 exit
```

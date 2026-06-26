# Server 1
```
cd/var/lib/iwd/
```

## Membuat Direktori Access Point 
```
mkdir -p ap
```

## Masuk ke Direktori AP 
```
cd ap
```

##Membuat File Konfigurasi Access Point 
```
nvim hotspot1.ap
```

## Mengisi Konfigurasi Access Point 
```
[General]
Enable=true                                                                                                                                                              SSID=hotspot1
[Security]
Passphrase=12345678                                                                                                                                                                                                                                                                                                                               [IPv4]
Address=15.15.2.3
Netmask=255.255.255
```

## Membuka iwd / jaringna
```
iwctl
```

## Mengubah Mode Perangkat Wi-Fi Menjadi Access Point 
```
device wlan0 set-property Mode ap
ap wlan0 start-profile hotspot1
exit
```

## Menjalankan Firewalld 
```
sudo systemctl start firewalld
```

## Menambahkan Aturan Firewall untuk Akses SSH 
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.234" port port="22" protocol="tcp" accept'              
```
```
sudo firewall-cmd --reload        
```   

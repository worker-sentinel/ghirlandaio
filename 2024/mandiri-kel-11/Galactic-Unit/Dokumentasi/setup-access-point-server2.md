# Masuk ke root
```
sudo su
```

# Navigasi dan buat direktori
```
cd /var/lib/iwd
```
```
mkdir -p ap
```
```
cd ap
```

# Membuat file konfigurasi AP
```
nvim hotspot2.ap
```
ketik:
```
[General]
Enable=true
SSID=hotspot2

[Security]
Passphrase=12344678

[IPv4]
Address=16.16.2.3
Netmask=255.255.255.0
```

# Restart iwd
```
sudo systemctl restart iwd
```

# Masuk ke iwctl dan aktifkan mode AP
```
iwctl
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile hotspot2
```
```
exit
```

# Konfigurasi firewall
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.100.79" port port="80" protocol="tcp" accept'
```
```
sudo firewall-cmd --reload
```

# Bukti sudah ada hotspot2:
<img width="720" height="1600" alt="WhatsApp Image 2026-06-26 at 08 58 32" src="https://github.com/user-attachments/assets/e887eac7-06bd-4a83-a41d-dbc6fe338901" />

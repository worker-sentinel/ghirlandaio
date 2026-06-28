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
nvim hotspot1.ap
```
ketik:
```
[General]
Enable=true
SSID=hotspot1

[Security]
Passphrase=12344678

[IPv4]
Address=15.15.2.3
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
ap wlan0 start-profile hotspot1
```
```
exit
```

# Konfigurasi firewall
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.100.79" port port="22" protocol="tcp" accept'
```
```
sudo firewall-cmd --reload
```

# Bukti sudah ada hotspot1
<img width="720" height="1600" alt="WhatsApp Image 2026-06-26 at 08 58 32" src="https://github.com/user-attachments/assets/6fcf05c7-78af-48cd-82e1-223b8514e33a" />

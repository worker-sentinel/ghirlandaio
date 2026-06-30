# SETUP AKSES POINT HOSTAPD
```
sudo pacman -S hostapd systemd-networkd
```
## Buat file hostapd:
```
sudo nano /etc/hostapd/hostapd.conf
```
**Isi:**
```
interface=wlan0
```
```
driver=nl80211
ssid=dika
hw_mode=g
channel=6
wmm_enabled=1

auth_algs=1
wpa=2
wpa_passphrase=12345
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
```
## Edit service hostapd:
```
sudo mkdir -p /etc/systemd/system/hostapd.service.d
```
```
sudo nano /etc/systemd/system/hostapd.service.d/override.conf
```

**Isi:**

```
[Service]
ExecStart=
ExecStart=/usr/bin/hostapd /etc/hostapd/hostapd.conf
```
## Buat konfigurasi network:
```
sudo nano /etc/systemd/network/02-wireless-ap.network
```
**Isi:**
```
[Match]
Name=wlan0

[Network]
Address=192.168.6.1/24
DHCPServer=yes
IPv4Forwarding=yes
IPv6Forwarding=no
LinkLocalAddressing=no

[DHCPServer]
PoolOffset=10
PoolSize=100
EmitDNS=yes
DNS=192.168.6.1
```
## Aktifkan IP Forward:
```
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/30-ipforward.conf
```
```
sudo sysctl --system
```
## Aktifkan service:
```
sudo systemctl enable systemd-networkd
```
```
sudo systemctl enable hostapd
```
## Restart:
```
sudo systemctl restart systemd-networkd
```
```
sudo systemctl restart hostapd
```
## Cek:
```
networkctl status wlan0
atau
ip a
```

**ganti semua wlan0 menjadi wlan1 kalo pas cek ip berubah ke wlan1:**
```
sudo nano /etc/hostapd/hostapd.conf
```
```
interface=wlan1
```

dan
```
sudo nano /etc/systemd/network/
02-wireless-ap.network
```
```
[Match]
Name=wlan1
```
```
sudo systemctl restart systemd-networkd
```
```
sudo systemctl restart hostapd
```
```
iw dev
```
```
systemctl status hostapd --no-pager
```
```
journalctl -u hostapd -n 30 --no-pager
```

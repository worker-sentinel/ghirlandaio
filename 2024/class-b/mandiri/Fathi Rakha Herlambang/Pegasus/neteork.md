# Konfigurasi Admin, Operator, Access Point, dan Firewall

## 1. Tambahkan User ke Grup Wheel

```bash
sudo usermod -aG wheel user1
sudo usermod -aG wheel user2
```

---

## 2. Cek Interface Jaringan

Lihat status perangkat:

```bash
nmcli device status
```

Lihat alamat IP:

```bash
ip a
```

---

## 3. Konfigurasi Koneksi Admin

Buat koneksi ethernet untuk admin:

```bash
nmcli connection add type ethernet ifname enp2s0 con-name "admin-connection" ipv4.method manual ipv4.addresses 212.122.4.6/24 ipv4.gateway 212.122.4.1 ipv4.dns 8.8.8.8
```

Aktifkan otomatis saat login:

```bash
echo "nmcli connection up admin-connection" >> /home/user1/.bash_profile
```

Logout:

```bash
logout
```

---

## 4. Konfigurasi Koneksi Operator

Buat koneksi ethernet untuk operator:

```bash
nmcli connection add type ethernet ifname enp2s0 con-name "operator-connection" ipv4.method manual ipv4.addresses 10.20.30.56/24 ipv4.gateway 10.20.30.1
```

Aktifkan otomatis saat login:

```bash
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```

Logout:

```bash
logout
```

---

## 5. Login ke Server

```bash
ssh rakha@192.168.2.145
```

---

## 6. Konfigurasi Ethernet Server

Edit file:

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Isi file:

```ini
[Match]
Name=enp2s0

[Network]
Address=212.122.4.7/24
Gateway=212.122.4.1
```

---

## 7. Install Hostapd

```bash
sudo pacman -S hostapd
```

Cek interface wireless:

```bash
ip a
```

---

## 8. Konfigurasi Hostapd

Edit file:

```bash
sudo nvim /etc/hostapd/hostapd.conf
```

Isi file:

```ini
interface=wlan0
driver=nl80211

ssid=test
channel=7

auth_algs=1

wpa=2
wpa_passphrase=12345678

wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

---

## 9. Konfigurasi Wireless Access Point

Edit file:

```bash
sudo nvim /etc/systemd/network/02-wireless-ap.network
```

Isi file:

```ini
[Match]
Name=wlan0

[Network]
Address=214.123.6.1/24
DHCPServer=yes
```

---

## 10. Aktifkan IP Forwarding

Edit file:

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi file:

```text
net.ipv4.ip_forward=1
```

Terapkan konfigurasi:

```bash
sudo sysctl --system
```

---

## 11. Aktifkan Service

Aktifkan systemd-networkd:

```bash
sudo systemctl enable --now systemd-networkd
```

Aktifkan hostapd:

```bash
sudo systemctl enable --now hostapd
```

---

## 12. Konfigurasi Firewall

### Buat Zona Admin

```bash
sudo firewall-cmd --new-zone=admin --permanent

sudo firewall-cmd --zone=admin --add-source=212.122.4.7 --permanent
```

Tambahkan service SSH:

```bash
sudo firewall-cmd --zone=admin --add-service=ssh --permanent
```

Tambahkan port:

```bash
sudo firewall-cmd --zone=admin --add-port=3306/tcp --permanent
sudo firewall-cmd --zone=admin --add-port=6379/tcp --permanent
sudo firewall-cmd --zone=admin --add-port=80/tcp --permanent
sudo firewall-cmd --zone=admin --add-port=443/tcp --permanent
```

### Buat Zona Operator

```bash
sudo firewall-cmd --new-zone=operator --permanent
```

Tambahkan port:

```bash
sudo firewall-cmd --zone=operator --add-port=6379/tcp --permanent
sudo firewall-cmd --zone=operator --add-port=80/tcp --permanent
sudo firewall-cmd --zone=operator --add-port=443/tcp --permanent
```

### Buat Zona WiFi

```bash
sudo firewall-cmd --new-zone=wifi --permanent
```

Tambahkan source:

```bash
sudo firewall-cmd --zone=wifi --add-source=214.123.6.0/24 --permanent
```

Tambahkan port:

```bash
sudo firewall-cmd --zone=wifi --add-port=80/tcp --permanent
sudo firewall-cmd --zone=wifi --add-port=443/tcp --permanent
```

---

## 13. Set Default Zone

```bash
sudo firewall-cmd --set-default-zone=drop --permanent
```

Reload firewall:

```bash
sudo firewall-cmd --reload
```

---

## 14. Verifikasi

Lihat seluruh zona:

```bash
sudo firewall-cmd --list-all-zones
```

Cek status hostapd:

```bash
systemctl status hostapd
```

Cek status systemd-networkd:

```bash
systemctl status systemd-networkd
```

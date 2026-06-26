# Setup Network, WiFi Access Point, dan Firewall

## Konsep Dasar IP Address

IP Address adalah kode unik yang dimiliki setiap perangkat dalam jaringan.

### Jenis IP Address

* IP Publik → diberikan oleh ISP dan digunakan untuk terhubung ke internet.
* IP Lokal (Private IP) → digunakan dalam jaringan internal dan tidak dapat diakses langsung dari internet.

Agar perangkat dapat berkomunikasi secara langsung, perangkat tersebut harus berada dalam satu jaringan (network) yang sama.

### Komponen IP

* **IP Network** → IP yang mewakili seluruh jaringan.
* **IP Host** → IP yang dimiliki oleh satu perangkat, bersifat unik dan tidak boleh sama.
* **IP Broadcast** → IP terakhir dalam subnet, digunakan untuk mengirim data ke seluruh host.
* **IP Gateway** → IP yang menjadi pintu keluar masuk jaringan.

### Contoh Jaringan

```text
192.168.2.0/24
```

Maka:

```text
IP Network   : 192.168.2.0
IP Gateway   : 192.168.2.1
IP Host      : 192.168.2.2 - 192.168.2.254
IP Broadcast : 192.168.2.255
```

Rentang IP:

```text
192.168.2.1 - 192.168.2.255
```

Rentang host yang dapat digunakan:

```text
192.168.2.2 - 192.168.2.254
```

### Netmask / CIDR

Menunjukkan jumlah bit yang digunakan sebagai Network ID.

Contoh:

```text
10.10.3.76/24
10.10.3.2/25
```

---

# Setup Network Manager (Admin)

## Install NetworkManager

```bash
sudo pacman -S networkmanager network-manager-applet
```

## Hapus iwd

```bash
systemctl stop iwd
pacman -R iwd
```

## Aktifkan NetworkManager

```bash
systemctl enable NetworkManager
systemctl start NetworkManager
systemctl status NetworkManager
```

## Tambahkan User ke Group Wheel

```bash
usermod -aG wheel username
```

## Membuat User Operator

```bash
useradd -m operator
usermod -aG wheel operator
sudo passwd operator
```

## Reboot

```bash
reboot
```

---

# Konfigurasi Koneksi Network

## Cek Interface

```bash
nmcli device status
```

## Membuat Koneksi Admin

```bash
nmcli connection add type ethernet ifname [interface] con-name "admin-connection" ipv4.method manual ipv4.addresses [ip-admin]/[CIDR] ipv4.gateway [gateway] ipv4.dns 8.8.8.8
```

## Membuat Koneksi Operator

```bash
nmcli connection add type ethernet ifname [interface] con-name "operator-connection" ipv4.method manual ipv4.addresses [ip-operator]/[CIDR] ipv4.gateway [gateway] ipv4.dns 8.8.8.8
```

---

# Automated Login Script

## Admin

```bash
echo "nmcli connection up admin-connection" >> /home/admin/.bash_profile
```

## Operator

```bash
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```

Logout lalu login kembali.

Cek:

```bash
ip a
```

---

# SSH ke Server

```bash
ssh [username-server]@[ip-server]
```

---

# Static IP Server

Edit file:

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Masuk root:

```bash
sudo su
```

Restart service:

```bash
systemctl restart systemd-networkd
```

Tes koneksi:

```bash
ping 8.8.8.8
```

---

# Install Hostapd

```bash
sudo pacman -S hostapd
```

## Cek Interface Wireless

```bash
ip link
```

## Konfigurasi Hostapd

```bash
nvim /etc/hostapd/hostapd.conf
```

Isi:

```conf
interface=wlan0
driver=nl80211
ssid=NamaWifi
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=PasswordWifi
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

---

# Konfigurasi Wireless Network

```bash
nvim /etc/systemd/network/02-wireless-ap.network
```

Isi:

```ini
[Match]
Name=wlan0

[Network]
Address=12.12.12.1/24
DHCPServer=yes
```

Restart service:

```bash
systemctl restart systemd-networkd
```

Aktifkan:

```bash
sudo systemctl enable --now systemd-networkd
```

---

# Enable IP Forwarding

```bash
sudo nvim /etc/sysctl.d/30-ipforward.conf
```

Isi:

```conf
net.ipv4.ip_forward=1
```

Terapkan:

```bash
sudo sysctl --system
```

---

# Aktifkan Hostapd

```bash
sudo systemctl enable --now hostapd
```

## Reboot

```bash
reboot
```

---

# Setup Firewall

## Zona Admin

```bash
firewall-cmd --permanent --new-zone=admin
firewall-cmd --permanent --zone=admin --add-source=10.10.10.10
```

## Zona Operator

```bash
firewall-cmd --permanent --new-zone=operator
firewall-cmd --permanent --zone=operator --add-source=13.25.168.55
```

## Zona WiFi

```bash
firewall-cmd --permanent --new-zone=wifi
firewall-cmd --permanent --zone=wifi --add-source=12.12.12.0/24
```

---

# Menjalankan Container

Keluar dari root:

```bash
exit
```

Cek container:

```bash
podman ps
```

Masuk ke folder compose:

```bash
cd ~/.config/celadon/compose/
```

Jalankan:

```bash
podman compose up -d
```

Cek:

```bash
podman ps
```

---

# Hak Akses Tiap Zona

## Admin

```bash
firewall-cmd --permanent --zone=admin --add-service=ssh
firewall-cmd --permanent --zone=admin --add-port=3306/tcp
firewall-cmd --permanent --zone=admin --add-port=6379/tcp
firewall-cmd --permanent --zone=admin --add-port=80/tcp
firewall-cmd --permanent --zone=admin --add-port=443/tcp
```

## Operator

```bash
firewall-cmd --permanent --zone=operator --add-service=ssh
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
firewall-cmd --permanent --zone=operator --add-port=80/tcp
firewall-cmd --permanent --zone=operator --add-port=443/tcp
```

## WiFi Client

```bash
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```

---

# Default Policy Drop

```bash
firewall-cmd --set-default-zone=drop --permanent
firewall-cmd --reload
```

---

# Konfigurasi Manual WiFi Client

Advanced Options → Static IP

```text
Alamat IP        : 12.12.12.2
Gateway (Router) : 12.12.12.1
CIDR             : 24
DNS              : 8.8.8.8
```

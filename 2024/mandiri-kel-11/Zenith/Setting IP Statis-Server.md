

# **1. Konfigurasi Ethernet**

Membuka file konfigurasi jaringan Ethernet.

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Isi file:

```ini
[Match]
Type=ether
# Exclude virtual Ethernet interfaces
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=10.10.1.20/24
Gateway=10.10.1.1
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

# systemd-networkd does not set per-interface-type default route metrics
# Explicitly set route metrics so Ethernet is preferred over Wi-Fi.
[DHCPv4]
RouteMetric=100
```

Restart layanan jaringan.

```bash
sudo systemctl restart systemd-networkd
sudo systemctl restart systemd-resolved
```

---

# **2. Konfigurasi IWD**

Membuka file konfigurasi IWD.

```bash
sudo nvim /etc/iwd/main.conf
```

Isi file:

```ini
[General]
EnableNetworkConfiguration=true
```

Nonaktifkan NetworkManager.

```bash
sudo systemctl disable NetworkManager
```

Aktifkan dan restart IWD.

```bash
sudo systemctl enable iwd
sudo systemctl restart iwd
```

---

# **3. Membuat Profil Access Point**

Masuk ke direktori penyimpanan profil IWD.

```bash
cd /var/lib/iwd
mkdir -p ap
ls
```

Buat file profil Access Point.

```bash
sudo nvim zenith.ap
```

Isi file:

```ini
[General]
Enable=true
SSID=sarah

[Security]
Passphrase=sarahsalsabila

[IPv4]
Address=192.192.1.9
Netmask=255.255.255.0
```

---

# **4. Menjalankan Access Point**

Masuk ke IWD.

```bash
iwctl
```

Ubah mode perangkat menjadi Access Point.

```bash
device wlan0 set-property Mode ap
```

Jalankan profil Access Point.

```bash
ap wlan0 start-profile zenith
```

Keluar dari IWD.

```bash
exit
```

---

# **5. Konfigurasi Firewall**

Tambahkan aturan firewall untuk mengizinkan akses.

```bash
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.10.1.253" port port="8080" protocol="tcp" accept'

sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.192.1.10" port port="22" protocol="tcp" accept'
```

Reload konfigurasi firewall.

```bash
sudo firewall-cmd --reload
```

Lihat konfigurasi firewall.

```bash
sudo firewall-cmd --list-all-zones
```

---

# **6. Pengujian Koneksi**

Uji koneksi ke perangkat tujuan.

```bash
ping 10.10.1.253
```

Periksa alamat IP perangkat.

```bash
ip a
```

Periksa container yang sedang berjalan.

```bash
podman ps
```

---


Jika diperlukan, hentikan firewall sementara.

```bash
sudo systemctl stop firewalld
```

Edit file lain jika diperlukan.

```bash
sudo nvim /home/zenith/
```





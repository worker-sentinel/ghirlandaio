## Akses Server Melalui SSH

Masuk ke server menggunakan SSH:

```bash
ssh hostname@192.168.2.110
```

## Konfigurasi Alamat IP Server

Buka file konfigurasi jaringan:

```bash
sudo nvim /etc/systemd/network/20-ethernet.network
```

Isi file dengan konfigurasi berikut:

```ini
[Match]
Type=ether
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=15.15.5.7/24
Gateway=15.15.5.8
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

[DHCPv4]
RouteMetric=100

[IPv6AcceptRA]
RouteMetric=100
```

Simpan dan keluar dari editor.

## Instalasi Hostapd

Pasang paket Hostapd untuk membuat Access Point:

```bash
sudo pacman -S hostapd
```

## Identifikasi Interface Wireless

Periksa nama interface jaringan nirkabel yang tersedia:

```bash
ip link
```

## Konfigurasi Hostapd

Buka file konfigurasi Hostapd:

```bash
sudo nvim /etc/hostapd/hostapd.conf
```

Masukkan konfigurasi berikut:

```conf
interface=wlan0
driver=nl80211
ssid=MyArchAP
hw_mode=g
channel=7
auth_algs=1

wpa=2
wpa_passphrase=Pejil123
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Simpan perubahan dan keluar dari editor.

## Konfigurasi Interface Access Point

Buat atau edit konfigurasi interface wireless:

```bash
sudo nvim /etc/systemd/network/02-wireless-ap.network
```

Sesuaikan konfigurasi sesuai kebutuhan jaringan Access Point.

## Mengaktifkan dan Merestart Layanan Network

Restart layanan jaringan:

```bash
sudo systemctl restart systemd-networkd
```

Aktifkan layanan agar berjalan otomatis saat boot:

```bash
sudo systemctl enable --now systemd-networkd
```

## Mengaktifkan IP Forwarding

Buat file konfigurasi IP forwarding:

```bash
sudo nvim /etc/sysctl.d/30-ipforward.conf
```

Isi file dengan konfigurasi berikut:

```conf
net.ipv4.ip_forward=1
```

Terapkan konfigurasi ke sistem:

```bash
sudo sysctl --system
```

## Keluar dari Server

Setelah seluruh konfigurasi selesai, keluar dari sesi terminal:

```bash
logout
```

atau

```bash
exit
```

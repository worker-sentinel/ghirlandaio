# Buat dan Buka File Konfigurasi Jaringan
```
nvim /etc/systemd/network/20-ethernet.network
```

## Masukkan Pengaturan IP
```
[Match]
Type=ether
# Mengabaikan virtual Ethernet interface
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=<ip_server>/<angka_cidr>
Gateway=<ip_gateway>
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

# systemd-networkd does not set per-interface-type default route metrics
# https://github.com/systemd/systemd/issues/17698
# Explicitly set route metric, so that Ethernet is preferred over Wi-Fi and Wi-Fi is preferred over mobile broadband.
# Use values from NetworkManager. From nm_device_get_route_metric_default in
# https://gitlab.freedesktop.org/NetworkManager/NetworkManager/-/blob/main/src/core/devices/nm-device.c
[DHCPv4]
RouteMetric=100

[IPv6AcceptRA]
RouteMetric=100
```
# Membuat IP Saluran systemd 
## Instal Aplikasi Pemancar (Hostapd)
Aplikasi ini bertugas mengelola pancaran sinyal Wi-Fi dan kata sandinya.
```
sudo pacman -S hostapd
```
## Cek Nama Perangkat Wi-Fi
Cari tahu nama kartu jaringan Wi-Fi kamu (biasanya berawalan wl atau wlan, contoh: wlan0).
```
ip link
```
### Atur Nama Wi-Fi (SSID) dan Kata Sandi
```
nvim /etc/hostapd/hostapd.conf
```
Isi dengan teks berikut. Kamu bisa mengubah nama Wi-Fi pada baris ssid= dan kata sandinya pada baris wpa_passphrase=. (Pastikan interface= diisi dengan nama Wi-Fi kamu dari langkah sebelumnya).
```
interface=wlan0
driver=nl80211
ssid=MyArchAP
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=MySecurePassword
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```
## Atur IP untuk Jaringan Hotspot
Kita perlu memberi tahu sistem untuk membagikan IP ke perangkat yang terhubung ke hotspot ini.
```
nvim /etc/systemd/network/02-wireless-ap.network
```
```
systemctl restart systemd-networkd
```
Isi file tersebut dengan teks di bawah ini. Ubah <nama_interface_wifi> (contoh: wlan0) dan <ip_hotspot>/<angka_cidr> (contoh: 192.168.4.1/24).
```
[Match]
Name=<nama_interface_wifi>

[Network]
Address=<ip_hotspot>/<angka_cidr>
DHCPServer=yes
```
```
sudo systemctl enable --now systemd-networkd
```
### Izinkan Berbagi Koneksi Internet (IP Forwarding)
```
nvim /etc/sysctl.d/30-ipforward.conf
```
Terapkan pengaturan tersebut langsung ke sistem:
```
net.ipv4.ip_forward=1
```
```
sudo sysctl --system
```
```
Reboot
```
```
sudo systemctl enable --now hostapd
```

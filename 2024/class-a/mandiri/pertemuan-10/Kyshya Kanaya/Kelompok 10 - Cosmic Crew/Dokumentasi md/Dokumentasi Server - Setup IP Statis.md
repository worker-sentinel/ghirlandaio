# Setup IP Statis Server

```
pacman -Syu openssh
```
Update sistem dan install OpenSSH server.

```
systemctl enable sshd
```
Aktifkan SSH agar otomatis jalan saat boot.

```
systemctl start sshd
```
Jalankan SSH sekarang.

```
systemctl status sshd
```
Cek status SSH, pastikan active (running).

```
nmtui
```
Buka Network Manager TUI untuk konek ke WiFi.
Pilih Activate a connection, cari WiFi gass, masukkan password, Back, Quit.

```
ping 8.8.8.8
```
Cek koneksi internet. Tekan Ctrl+C untuk berhenti.

```
ssh kel10@10.119.250.30
```
Remote login ke server via SSH.

```
nvim /etc/systemd/network/20-ethernet.network
```
Buat konfigurasi IP statis untuk interface ethernet. Isi dengan:

```
[Match]
Type=ether
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=10.10.10.12/24
Gateway=10.10.10.1
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

[DHCPv4]
RouteMetric=100

[IPv6AcceptRA]
RouteMetric=100
```

```
sudo su
```
Masuk ke root. Masukkan password saat diminta.

```
nvim /etc/systemd/network/20-ethernet.network
```
Buka ulang dan pastikan konfigurasi ethernet sudah benar (sama seperti di atas).

```
pacman -S hostapd
```
Install hostapd untuk membuat WiFi Access Point.

```
ip link
```
Lihat nama interface WiFi yang tersedia, contoh wlp3s0.

```
nvim /etc/hostapd/hostapd.conf
```
Konfigurasi Access Point. Ketik :3400 di nvim untuk loncat ke baris bawah, hapus dua # terakhir, lalu tambahkan di bawah #bssid=00:03:7f:12:84:85:

```
interface=wlp3s0
driver=nl80211
ssid=MyArchAP
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=111
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

```
nvim /etc/systemd/network/02-wireless-ap.network
```
Konfigurasi IP untuk interface WiFi AP. Isi dengan:

```
[Match]
Name=wlp3s0

[Network]
Address=10.10.2.1/24
DHCPServer=yes
```

```
systemctl restart systemd-networkd
```
Restart network agar konfigurasi diterapkan.

```
systemctl enable --now systemd-networkd
```
Aktifkan dan jalankan systemd-networkd.

```
nvim /etc/sysctl.d/30-ipforward
```
Aktifkan IP forwarding agar traffic bisa diteruskan antar interface. Tambahkan:

```
net.ipv4.ip_forward=1
```

```
sysctl --system
```
Terapkan konfigurasi sysctl tanpa reboot.

```
systemctl enable --now hostapd
```
Aktifkan dan jalankan hostapd sebagai Access Point.


## Referensi
David aka joshua aka baihaqi

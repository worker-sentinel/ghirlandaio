# Membuat IP statis 
```
nvim /etc/systemd/network/20-ethernet.network
```
```
[Match]
Type=ether
# Exclude virtual Ethernet interfaces
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=[ip address server]/CIDR
Gateway=[ip address gateway]
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
## Install Package
```
pacman -S hostapd
```
## cek interface
```
ip link
```
```
nvim /etc/hostapd/hostapd.conf
```
isi
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
```
nvim /etc/systemd/network/02-wireless-ap.network
```
```
systemctl restart systemd-networkd
```
isi
```
[Match]
Name=[wireless interface]

[Network]
Address=[ip address]/CIDR
DHCPServer=yes
```
```
sudo systemctl enable --now systemd-networkd
```
```
nvim /etc/sysctl.d/30-ipforward.conf
```
isi
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

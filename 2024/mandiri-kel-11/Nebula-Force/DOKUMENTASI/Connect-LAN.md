# CONNECT LAN
```
ip a
```
```
ip a
```
```
ip addr show enpOs2OfOu4
```
```
nvim /etc/systemd/network/20-ethernet.network
```
**Kemudian ikuti pada gambar dibawah ini**
<img width="1032" height="426" alt="lan connect" src="https://github.com/user-attachments/assets/85d93a1f-077d-4975-bc0d-b75faa5e1c12" />
**atau ketik command berikut**
```
[Match]
Type=ether

# Exclude virtual Ethernet interfaces
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=192.168.1.6/24
Gateway=192.168.1.1
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

# systemd-networkd does not set per-interface-type default route metrics
# https://github.com/systemd/systemd/issues/17698

# Explicitly set route metric, so that Ethernet is preferred over Wi-Fi and Wi-Fi is preferred over mobile broadband.
# Use values from NetworkManager. From nm_device_get_route_metric_default in
# https://gitlab.freedesktop.org/NetworkManager/NetworkManager/-/blob/main/src/core/devices/nm-device.c

[DHCPv4]
RouteMetric=100
```
```
exit
```

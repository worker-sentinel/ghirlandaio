## di sini diantara kedua server harus terkoneksi dengan kabel lan
**di sisi server 1**

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
Address=13.13.2.3/24 #ini bebas namun tidak boleh 1 dan harus 4 oktad dan /24
Gateway=13.13.2.1 # yang ini oktad ke 4 harus 1
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
```
systemctl restart systemd-networkd
```

**di sisi server 2**
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
Address=13.13.2.5/24 #ini 3 oktad pertama samakan dengan server 1 oktad terkahir bedakan namun tidak boleh 1
Gateway=13.13.2.1 # yang ini oktad ke 4 harus 1
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
```
systemctl restart systemd-networkd
```

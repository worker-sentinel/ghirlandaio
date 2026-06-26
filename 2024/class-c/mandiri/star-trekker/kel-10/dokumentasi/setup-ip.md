## di sisi admin

### set ip admin

```
nmcli connection add type ethernet ifname [nama driver ethernet] con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.1.3/24 ipv4.gateway 10.10.1.1 ipv4.dns 8.8.8.8
```
```
echo "nmcli connection up admin-connection" >> /home/[username_admin]/.bash_profile
```

## di sisi operator

```
nmcli connection add type ethernet ifname [nama driver ethernet] con-name "operator-connection" ipv4.method manual ipv4.addresses 10.10.1.4/24 ipv4.gateway 10.10.1.1 ipv4.dns 8.8.8.8
```
```
echo "nmcli connection up admin-connection" >> /home/[username_operator]/.bash_profile
```


## di sisi server

```
nvim /etc/systemd/network/20-ethernet.network
```
isi
```
[Match]
Type=ether
# Exclude virtual Ethernet interfaces
Kind=!*

[Link]
RequiredForOnline=routable

[Network]
Address=10.10.1.5/24
Gateway=10.10.1.1
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


## setup ip untuk public

```
pacman -S hostapd
```
```
sudo systemctl enable --now systemd-networkd
```
```
nvim /etc/hostapd/hostapd.conf
```
> isi
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
> isi dengan
```
[Match]
Name=nama_ethernet

[Network]
Address=11.11.1.3/24
DHCPServer=yes
```

```
systemctl restart systemd-networkd
```

```
nvim /etc/sysctl.d/30-ipforward.conf
```
> isi dengan
```
net.ipv4.ip_forward=1
```

```
sudo sysctl --system
```
```
sudo systemctl enable --now hostapd
```
```
reboot
```

# create static ip

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
Address=[ip address server 3 digit awal samain kayak admin]/CIDR
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

### note
> ip address server dan admin harus dalam satu network contoh:  
> ip server : 192.168.1.12  
> maka  
> ip admin : 192.168.1.13  
> ip operator : 192.168.1.14  
> ip gateway : 192.168.1.1  
> dan notasi CIDR harus sama yakni 24  

# create systemd ip broadcast
## install package 
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
reboot
```

```
sudo systemctl enable --now hostapd
```

```
sudo systemctl edit hostapd
```

Add these lines inside the drop-in file:  
```
[Unit]
BindsTo=sys-subsystem-net-devices-[wireless interface].device
After=sys-subsystem-net-devices-[wireless interface].device
```

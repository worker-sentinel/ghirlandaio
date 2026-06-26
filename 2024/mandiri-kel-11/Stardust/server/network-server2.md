# IP Static
```
ssh untuk server1 ip address=10.43.23950 (contoh)
```
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
samakan dengan command dibawah:
```
[Match]                                                                                                                               
Type=ether                                                                                                                            
# Exclude virtual Ethernet interfaces
Kind=!*

[Link]
RequiredForOnline=routable

[Network]                                                                                                                             
Address=[Buat IP Address bebas]/24                                                                                                                
Gateway=[3 digit awal disamakan dengan IP Address].1
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
sudo systemctl restart systemd-networkd
```

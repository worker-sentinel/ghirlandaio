## SSH dan Konfigurasi IP Server <br>
### SSH ke server <br> 
ssh kel10@192.168.2.110 <br>
### Konfigurasi IP Server <br>
sudo nvim /etc /system/network/20-ethernet.network <br>
[Match] <br>                                                                                                                                                                                      Type=ether  <br>                                                                                                                                                                                  Kind=!* <br>
[Link]  <br>                                                                                                                                                                                      RequiredForOnline=routable <br>
[Network] <br>                                                                                                                                                                                    Address=15.15.5.7/24  <br>                                                                                                                                                                        Gateway=15.15.5.8         <br>                                                                                                                                                                    DNS=1.1.1.1 8.8.8.8          <br>                                                                                                                                                                 MulticastDNS=yes <br>
[DHCPv4]  <br>                                                                                                                                                                                    RouteMetric=100  <br>
[IPv6AcceptRA]  <br>                                                                                                                                                                              RouteMetric=100  <br>
#### Instalasi hostapd <br>
sudo pacman -S hostapd <br>
#### Identifikasi interface wireless <br>
ip link <br>
#### Konfigurasi hostapd <br>
sudo nvim /etc/hostapd/hostapd.conf <br>
Interface=wlan0 <br>                                                                                                                                                                              driver=nl80211       <br>                                                                                                                                                                         ssid=MyArchAP        <br>                                                                                                                                                                         hw_mode=g           <br>                                                                                                                                                                          channel=7                 <br>                                                                                                                                                                    auth_algs=1                <br>                                                                                                                                                                   wpa=2                 <br>                                                                                                                                                                        wpa_passphrase=Pejil123 <br>                                                                                                                                                                      wpa_key_mgmt=WPA-PSK    <br>                                                                                                                                                                      wpa_pairwise=TKIP         <br>                                                                                                                                                                    rsn_pairwise=CCMP <br>
##### Konfigurasi Interface Access Point <br>
sudo nvim /etc/systemd/network/02-wireless-ap.network <br>
##### Restart dan enable system-networkd <br> 
sudo systemctl restart systemd-networkd <br>
sudo systemctl enable --now systemd-networkd <br>
##### Mengaktifkan IP Forwading <br>
sudo nvim /etc/sysctl.d/30-ipforward.conf <br>
net.ipv4.ip_forward=1 <br>


sudo sysctl –system
logout
exit

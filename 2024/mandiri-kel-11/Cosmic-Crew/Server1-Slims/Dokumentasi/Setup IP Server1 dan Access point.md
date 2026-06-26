# Setup IP Server1

## Masuk sebagai root
```
sudo su
```

## Konfigurasi IP Statis Ethernet
```
nvim /etc/systemd/network/20-ethernet.network
```
insert
```
[Match]                                                                        
Type=ether                                                                     
Kind=!*                                                                        
                                                                              
[Link]                                                                         
RequiredForOnline=routable                                                     
                                                                              
[Network]                                                                      
Address=10.10.1.20/24                                                          
Gateway=10.10.1.1                                                              
DNS=1.1.1.1 8.8.8.8                                                            
MulticastDNS=yes                                                               
                                                                              
[DHCPv4]                                                                       
RouteMetric=100                                                                
                                                                              
[IPv6AcceptRA]                                                                 
RouteMetric=100
```

# Konfigurasi Access Point

## Konfigurasi IWD
```
nvim /etc/iwd/main.conf
```
insert
```
[General]                                                                      
EnableNetworkConfiguration=true
```

## Installasi IWD
```
pacman -S iwd
```

## Menonaktifkan NetworkManager
```
systemctl stop NetworkManager
```

## Mengaktifkan dan Menjalankan IWD
```
systemctl enable iwd
```
```
systemctl start iwd
```
```
systemctl stop iwd
```

## Membuat Direktori Access Point
```
cd /var/lib/iwd
mkdir -p ap
ls
```

## Menghubungkan ke Wi-Fi
```
iwctl
station wlan0 connect nama_wifi
```

## Membuat Konfigurasi Wireless AP
```
nvim /etc/systemd/network/02-wireless-ap.network
```

## Menyalin Konfigurasi Ethernet ke WLAN
```
cp /etc/systemd/network/20-ethernet.network /etc/systemd/network/20-wlan.network
```

## Konfigurasi WLAN
```
nvim /etc/systemd/network/20-wlan.network
```
insert
```
[Match]
Type=wlan

[Link]
RequiredForOnline=routable

[Network]
DHCP=yes
MulticastDNS=yes

[DHCPv4]
RouteMetric=100

[IPv6AcceptRA]
RouteMetric=100
```

## Memuat Ulang Konfigurasi Jaringan
```
systemctl restart systemd-networkd
systemctl restart systemd-resolved
```

## Menguji Koneksi Internet
```
ping 1.1.1.1
```

## Membuat Profil Hotspot
```
cd ap/
ls
```
```
nvim (nama hotspot).ap
```
insert
```
[General]
Enable=true
SSID=bila

[Security]
Passphrase=bila0201

[IPv4]
Address=192.168.2.10
Netmask=255.255.255.0
```

## Menjalankan Hotspot
```
iwctl
ap wlan0 start-profile (nama hotspot)
exit
```

## Menguji Koneksi ke Host Lain
```
ping 10.10.1.253
```

## Memuat Ulang Jaringan
```
systemctl restart systemd-networkd
systemctl restart systemd-resolved
```

## Memeriksa Alamat IP
```
ip a
```

## Memeriksa Container Podman
```
podman ps -a
```

## Memeriksa File dalam Direktori
```
ls
```

## Melakukan reboot
```
reboot
```

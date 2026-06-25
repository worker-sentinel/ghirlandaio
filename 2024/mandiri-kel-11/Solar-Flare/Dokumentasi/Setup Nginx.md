# Dokumentasi Server Nginx

---
## Edit file konfigurasi
```
nvim /etc/sysctl.d/99-nginx-custom.conf
```
> isi dengan
> 
> kernel.unprivileged_userns_clone=1
```
```

## Install nginx
```
pacman -S nginx
```
## Membuat konfigurasi jaringan ethernet
```
nvim /etc/systemd/20-ethernet.network  
```
> isi dengan
> 
> [Match]
> Type=ether
> 
> #Exclude virtual Ethernet interfaces  
> Kind=!*
>
> [Link]
> RequiredForOnline=routable
>
> [Network]
> Addres=10.10.1.253/24
> Gateway=10.10.1.1
> DNS=1.1.1.1.8.8.8.8
> MulticastDNS=yes
>
> [DHCPv4]
> RouteMetric=100

> [IPv6AcceptRA]
> RouteMetric=100
```
```
# Mengedit konfigurasi iwd
```
nvim /etc/iwd/main.conf
```
> isi dengan
> 
> [General]
> EnableNetworkConfiguration=true 
```
```
## Restart layanan iwd
```
systemctl restart iwd 
```
## Masuk ke direktori profil wifi
```
cd /var/lib/iwd
```
## Membuat profil access point
```
nvim myhotspot.ap
```
> isi dengan
> [General]
> Enable=true
> SSID=Nama hotspot
>
> [Security]
> Passphrase=passwordwifi
>
> [IPv4]
> Adress=alamat ip
> Netmask=alamat ip
```
```
## Melihat isi folder
```
ls
```
## Membuat folder AP
```
mkdir ap
```
## Restart iwd lagi
```
systemctl restart iwd
```
## Membuka shell interaktif iwd
```
iwctl
```
## Menampilkan perangkat wifi
```
device list
```
## Mengubah mode adapter menjadi access point
```
device wlan0 set-property Mode ap 
```
## Jalankan hotspot
```
ap wlan0 start-profile hani (nama hotspot)
```
## Keluar dari iwctl
```
exit
```

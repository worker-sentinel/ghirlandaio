# Dokumentasi Setup server 

---
## Login ke server

ssh SERVER@10.113.208.188

## Administrator

sudo su

## Mengedit dan mengatur file konfigurasi ethernet

nvim /etc/systemd/network/20-ethernet.network

> [Network]
> Adress=10.10.3.4/24
> Gateway=10.10.3.1
> DNS=1.1.1.1 8.8.8.8
> MulticastDNS=yes

```
## Install Hostapd

pacman -S hostapd

## Cek interface jaringan

ip link

## Konfigurasi access point

nvim /etc/hostapd/hostpad.conf

> tambahkan
> interface=wlan0
> driver=nl80211
> ssid=MyArchAP
> hw_mode=g
> channel=7
> auth_algs=1
> wpa=2
> wpa_passphrase=12345678
> wpa_key_mgmt=WPA-PSK
> wpa_pairwise=TKIP
> rsn_pairwise=CCMP


## Mengatur konfigurasi jaringan access point

nvim /etc/systemd/network/02-wireless-ap.network

> isi
> [Match]
> Name=[wireless interface]

> [Network]
> Address=10.10.4.1/24
> DHCPServer=yes


## Restart network

systemctl restart systemd-networkd

## Aktifkan network service

sudo systemctl enable --now systemd-networkd

## Mengatur ip forwading

nvim /etc/systcl.d/30-ipforward.conf

> isi
> net.ipv4.ip_forward=1     


## Terapkan konfigurasi sysctl

sudo sysctl --system 

## Menjalankan hostapd

sudo sysctl enable --now hostapd

## Keluar

exit

# Setup IP Statis 

 Konfigurasi IP statis pada interface Ethernet menggunakan **systemd-networkd**.

## Edit File Konfigurasi

```
/etc/systemd/network/20-wired.network
```

## Isi Konfigurasi

```
[Match]
Type=ether
Kind=!*
Name=(nama jaringan lan)

[Link]
RequiredForOnline=routable

[Network]
Address=10.10.1.253/24
Gateaway=10.10.1.1
DNS=1.1.1.1 8.8.8.8
MulticastDNS=yes

[DHCPv4]
RouteMetric=100

[IPv6AcceptRA]
```

> [!NOTE]
> contohnya: 'eno1' atau 'enp0s25'. Intinya yang didepannya ada en. Cara cek nya yaitu 'ip a'
## Terapkan Konfigurasi

```
sudo systemctl restart systemd-networkd
sudo systemctl restart systemd-resolved
```

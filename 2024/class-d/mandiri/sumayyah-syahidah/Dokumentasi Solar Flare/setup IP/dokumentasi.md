# Membuat IP Statis pada Server

## Pertama, periksa status perangkat jaringannya dulu guys

```
nmcli device status
```
> nah, kan outputnya gini ntar:

```
DEVICE   TYPE      STATE                   CONNECTION
enp7s0   ethernet  connected (externally) enp7s0
lo       loopback  connected (externally) lo
```
> inian yak:
> enp7s0 = nama interface ethernet/LAN fisik
> ethernet = jenis koneksi
> connected = internet aktif dan tersambung
> lo = loopback interface (127.0.0.1)
  
> fungsi/tujuan: kasi liat daftar (perangkat jaringan0 network interface yang kepasang di sistem beserta status koneksinya.

## Lanjut, periksa konfigurasi alamat IP

```
ip a
```

> fungsinya: nampilin IP address, MAC address, status interface, IPv4 & IPv6.

## bagian loopback
```
1: lo: <LOOPBACK,UP,LOWER_UP>
```
> artinye:
> LOOPBACK =interface internal komputer sendiri
> UP = interface aktif

> tujuan/fungsi: nampilin semua info alamat IP yang dimiliki setiap interface jaringan pada sistem secara menyeluruh.
  
## IP loopback
```
inet 127.0.0.1/8 scope host lo
```
> artinya:
> 127.0.0.1 = localhost
> /8 = subnet mask

## Detail interface ethernet
```
2: enp7s0 <BROADCAST, MULTICAST, UP, LOWER_UP>
```
## penjelasan flag:
> BROADCAST = bisa kirim broadcast
> MULTICAST = support multicast
> UP = interface aktif
> LOWER_UP = kabel/network detected

## MAC Address
```
link/ether 40:c2:ba:79:fb:e5
```
> fungsinya: alamat fisik NIC/LAN card

## IPv4 address
```
inet 10.10.1.253/24 brd 10.10.1.255 scope global enp7s0
```
> artinya:
> 10.10.1.253 = IP address disesuaikan dengan device yang sedang digunakan
> /24 = broadcast address
> scope global = bisa dipakai komunikasi jaringan

## Bikin Koneksi Statik
```
nmcli connection add type ethernet ifname enp7s0 con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.3.2/24 ipv4.gateway 10.10.3.1 ipv4.dns 8.8.8.8
```
> tujuannya: bikin koneksi ethernet baru dengan IP statik, gateaway manual, dan DNS manual.
## Per-bagian:
> nmcli = program CLI NetworkManager
> connection add = tambah profile koneksi baru
> type ethernet = jenis koneksi ethernet kabel
> ifname enp7s0 = koneksi dipasang ke interface enp7s0
> con-name "admin-connection" = nama profile koneksi
> ipv4.method manual = IP gapake DHCP tapi diiisi manual/statik
> ipv4.address 10.10.3.2/24 = set IP statik jadi IP-> 10.10.3.2, terus subnet-> /24
> ipv4.gateaway 10.10.3.1 = default gateaway/router
> ipv4.dns 8.8.8.8 = DNS server google

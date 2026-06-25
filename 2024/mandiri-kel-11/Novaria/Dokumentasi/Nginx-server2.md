# HOW TO INSTALL NGINX FOR SERVER 2

## Membuat file konfigurasi
```
nvim /etc/sysctl.d/99-custom.conf
```
>Mengaktifkan fitur unprileged user namespace
```
kernel.unprivilaged_userns_clone=1
```
## Meng-install pakey nginx
```
pacman -S nginx
```
## Membuat file konfigurasi systemd-networkd
>Untuk mengatur jaringan ethernet, seperti alamat IP, gateway dan DNS
```
nvim /etc/systemd/network/20-ethernet.network
```
## Membuka file konfigurasi iwd (ined wireless daemon)
```
nvim /etc/iwd/main.conf
```
Ketik
```
[General]
EnableNetworkConfiguration=true
```
## RESTART
```
systemctl restart iwd
```
## Berpindah direktori tempat iwd menyimpan jaringan WIFI dan access point
```
cd /var/lib/iwd
```
## Edit file access point bernama myconnect.ap
```
nvim myconnect.ap
```
## Aktifkan profil access point
>SSID=nginx Menentukan nama jaringan Wi-Fi (SSID) yang akan dipancarkan. Dalam contoh ini nama hotspot adalah nginx
## Atur password WIFI yang harus dimasukkan oleh pengguna untuk connect ke access point
Ketik
```
[General}
Enable=true
SSID=nginx

[Security]
Passphrase=123
```
## Memnentukan alamat IP yg akan digunkan oleh Access Point
>IP tersebut menjadi gateway bagi prangkat yang terhubung. Netmask=255.255.255.0 kemudian menentukan subnet mask supaya jaringan ada di subnet 192.168.1.0/24
```
[IPv4]
Adress=192.168.1.1
Netmask=255.255.255.0
```
## Membuka interaktif iwd untuk mengelola koneksi WIFI
```
iwctl
device list
device wlan0 set-property Mode ap
```
## Menjalankan Access point di perangkat wlan0 menggunkan konfogurasi myconnect.ap
```
ap wlan0 start-profile myconnect
```

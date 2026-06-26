# Nginx server
## membuat file konfigurasi kernel Linux menggunakan editor Neovim

```
nvim /etc/sysctl.d/99-custom.conf
```

## Mengaktifkan fitur unprivileged user
Ketik: 

```
kernel.unprivilaged_userns_clone=1
```

## menginstall paket nginx

```
pacman -S nginx
```

## Membuka file konfigurasi systemd-networkd

```
nvim /etc/systemd/20-ethernet.network
nvim /etc/systemd/network/20-ethernet.network
```

## Membuka file konfigurasi iwd (iNet Wireless Daemon)

```
nvim /etc/iwd/main.conf
```

## Mengaktifkan iwd untuk mengelola konfigurasi jaringan secara otomatis (enable)

Ketik:

```
[General]
EnableNetworkConfiguration=true
```

## Memulai ulang layanan iwd

```
systemctl restart iwd
```

## Berpindah ke direktori tempat iwd

```
cd /var/lib/iwd
```

## Membuat file profil Access Point bernama myconnect.ap

```
nvim myconnect.ap
```

## Mengaktifkan profil Access Point
Ketik:

```
[General]
Enable=true
SSID=nginx

[Security]
Passphrase=123

[IPv4]
Address=192.168.1.1
Netmask=255.255.255.0
```

## Membuka iwd untuk mengelola koneksi Wi-Fi

```
iwctl
```

## Menampilkan daftar perangkat jaringan nirkabel (Wi-Fi)

```
device list
```

## Mengubah mode perangkat Wi-Fi wlan0 menjadi Access Point (AP)

```
device wlan0 set-property Mode ap
```

## Menjalankan Access Point pada perangkat wlan0 menggunakan profil konfigurasi myconnect.ap 

```
ap wlan0 start-profile myconnect
```

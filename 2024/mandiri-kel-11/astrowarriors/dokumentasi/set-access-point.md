# Set Access Point

```
cd /var/lib/iwd
```

buat direktori ap
```
mkdir ap
```

```
cd ap/
```

buat file .ap
```
nvim [terserah].ap
```
isi [untuk address hanya contoh]
```
[General]
Enable=true
SSID=[terserah]
[Security]
Passphrase=[terserah]
[IPv4]
Address=17.17.17.17[hanya contoh]
Netmask=255.255.255.0
```

```
iwctl
```

```
device list
```

ubah mode station menjadi ap
```
device wlan0 set-property Mode ap
```

start ap dengan nama file .ap yang sebelumnya telah dibuat
```
ap wlan0 start-profile [terserah]
```

cek di wifi di device client apakah hotspot telah muncul.

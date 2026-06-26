## Di Server 1 

```bash
Sudo su
```

Masuk ke direktori iwd
```bash
cd /var/lib/iwd
```

Buat folder ap
```bash
mkdir ap
```

Masuk ke folder ap
```bash
cd ap
```

```bash
nvim hotspot1.ap
```

Isi dengan 
```bash
[General]
Enable=true
SSID=hotspot1

[Security]
Passphrase=12345678

[IPv4]
Address=15.15.2.3
Netmask=255.255.255.0
```

Lalu `esc` dan save `:wq`

Aktifkan 
```bash
iwctl
```

Kalau tidak ada maka install dan aktifkan dulu iwd. 

```bash
device wlan0 set-property Mode ap
```

```bash
ap wlan0 start-profile hotspot1
```

Cek apakah hotspot sudah ada di device lain

Sambungkan admin dengan hotspot yang dibuat

```bash
Sbcjb
```


## Di Server 2 

```bash
Sudo su 
```


Masuk ke direktori iwd
```bash
cd /var/lib/iwd
```

Buat folder ap
```bash
mkdir ap
```

Masuk ke folder ap
```bash
cd ap
```

```bash
nvim hotspot2.ap
```


Isi nvim 
```bash
[General]
Enable=true
SSID=myhotspot

[Security]
Passphrase=12345678

[IPv4]
Address=18.18.3.5
Netmask=255.255.255.0
```

Lalu save dan restart iwd 

```bash
Sudo systemctl restart iwd
```

```bash
Iwctl
```

```bash
device list
```

```bash
device wlan0 set-property Mode ap
```

```bash
ap wlan0 start-profile hotspot2
```

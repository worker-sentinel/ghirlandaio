# Konfigurasi Access Point (iwd)

### 1. Masuk sebagai root

```
sudo su
```

### 2. Edit konfigurasi iwd

```
nvim /etc/iwd/main.conf
```

Isi file:

```
[General]
EnableNetworkConfiguration=true
```

### 3. Restart service iwd

```
systemctl restart iwd
```

### 4. Masuk ke direktori profil iwd

```
cd /var/lib/iwd
```

### 5. Buat profil Access Point

```
nvim myhospot.ap
```

Isi file:

```
[General]
Enable=true
SSID=nginx

[Security]
Passphrase=12345678

[IPv4]
Address=192.168.2.11
Netmask=255.255.255.0
```

### 6. Konfigurasi Access Point

```
iwctl
device list
device wlan0 set-property Mode ap
device list
ap wlan0 start-profile nginx
exit
```

### 7. Ganti nama profil

```
mv myhospot.ap nginx.ap
```

### 8. Buat direktori AP dan pindahkan profil

```
mkdir ap
mv nginx.ap ap/
```

### 9. Restart iwd

```
systemctl restart iwd
```

### 10. Aktifkan kembali Access Point

```
iwctl
device list
device wlan0 set-property Mode ap
device list
ap wlan0 start-profile nginx
exit
```

# Hotspot

```
sudo su
```

```
cd var/lib/iwd/
```

```
cd iwd
```

```
mkdir -p ap
```

```
nvim hostpot1.ap
```

Isi
```
[General]
Enable=true
SSID=hostpot1

[Security]
Passphrase=12345678

[IPv4]
Adress=15.15.2.3
Netmask=255.255.255.0
```

Setelah itu esc :wq

```
sudo systemctl restart iwd
```

```
iwctl
station wlan1 get-networks
device wlan1 set-property Mode ap
ap wlan1 start-profile hotspot1
```

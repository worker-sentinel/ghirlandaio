### Akses Point
```
sudo nvim /etc/iwd/main.conf
```

```
[General]
EnableNetworkConfiguration=true
```

**kalau udh esc :wq**

```
sudo su
```
```
cd /var/lib/iwd
```
```
mkdir ap
```
```
cd ap
```
```
nvim hotspot.ap
```

```
[General]
Enable=true
SSID=pegasus

[Security]
Passphrase=12345678

[IPv4]
Address=14.14.14.9 
Netmask=255.255.255.0
```

**esc kemudian ketik :wq**

```
sudo systemctl restart iwd
```
```
sudo iwctl
```
```
device list
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile hotspot
```

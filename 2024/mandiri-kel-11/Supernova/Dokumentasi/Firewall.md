## Firewall

```
nvim /etc/systemd/network/20-ethernet.network
```
```
systemctl restart systemd-networkd
```
```
nvim /etc/iwd/main.conf
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
nvim myhotspot.ap
```
```
systemctl restart iwd
```
```
iwctl
```

masuk ke iwd

```
device wlan0 set-property Mode ap
```
```
station wlan0 scan
```
```
station wlan0 get-networks
```
```
exit
```

keluar dari iwd

```
nvim myhotspot.ap
```
```
nmtui
```
```
iwctl
```
```
exit
```
```
ip a
```
```
address="10.32.25.105" port="8080" protocol="tcp"
```

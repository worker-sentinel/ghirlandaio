## Set up ip static server 1
```
sshusername@ipserver1
```
```
nano /etc/systemd/network/20-ethernet.network
```
```
Address=10.10.10.2/24
Gateway=10.10.10.1
DNS=1.1.1.1 8.8.8.8
```
```
systemctl restart systemd-networkd
```
## Set up ip static server 2
```
sshusername@ipserver2
```
```
nano /etc/systemd/network/20-ethernet.network
```
```
Address=10.10.10.3/24
Gateway=10.10.10.1
DNS=1.1.1.1 8.8.8.8
```
```
systemctl restart systemd-networkd
```
## Set up akses point
```
nano /etc/iwd/main.conf
```
```
[General]
EnableNetworkConfiguration=true
```
```
Esc :wq
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
nano myhotspot.ap
```
```
[General]
Enable=true
SSID=lunar

[Security]
Passphrase=12345678

[IPv4]
Address=5.5.5.3
Netmask=255.255.255.0
```
```
Esc :wq
```
```
systemctl restart iwd
```
```
iwctl
```
```
device list
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile myhotspot
```
```
exit
```

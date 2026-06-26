# Atur config IWD
```
1.sudo nvim /etc/iwd/main.conf
```
```
- kosong dan diisi [General] enter EnableNetworkConfiguration=true lalu esc :wq
```
```
- sudo su
- cd /var/lib/iwd
- mkdir ap
- cd ap
- nvim myhotspot.ap diisi [General] enter Enable=true enter SSID=blackrog enter 2kali [Security] enter Passphrase=12345678 (pw harus 8 karakter) enter 2kali lagi [IPv4] enter Address=9.9.9.3 enter Netmask=255.255.255.0 esc :wq
- sudo systemctl restart iwd
- iwctl
- device list
- device wlan0 set-property Mode ap
```

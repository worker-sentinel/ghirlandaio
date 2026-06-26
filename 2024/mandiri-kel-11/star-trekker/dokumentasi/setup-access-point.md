## di sisi server 1
```
nvim /var/lib/iwd
```
```
mkdir ap
```
```
cd ap
```
```
nvim hostpot.ap
```
> isi
```
[General]
Enable=true
SSID=agoy

[Security]
Passphrase=passwordminimal8karakter

[IPv4]
Address=10.10.1.10 #ini bebas tapi harus 4 oktad
Netmask=255.255.255.0 
```
```
iwctl
```
```
device list
```
```
station wlan0 
## di sisi server 2

```
nvim /var/lib/iwd
```
```
mkdir ap
```
```
cd ap
```
```
nvim bebas.ap
```
> isi
```
[General]
Enable=true
SSID=agoy

[Security]
Passphrase=passwordminimal8karakter

[IPv4]
Address=99.99.99.99 #ini bebas tapi harus 4 oktad
Netmask=255.255.255.0 
```

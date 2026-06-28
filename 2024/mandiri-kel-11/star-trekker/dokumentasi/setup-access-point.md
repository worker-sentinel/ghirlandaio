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
SSID=agoy #ini bebas

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
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile hostpot.ap
```

## uji penggunaaan

*cari di wifi device lain, jika muncul wifi dengan nama hostpot maka setup acces poin telah berhasil*
### di sisi server 2

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
SSID=agoy #ini bebas

[Security]
Passphrase=passwordminimal8karakter

[IPv4]
Address=99.99.99.99 #ini bebas tapi harus 4 oktad
Netmask=255.255.255.0 
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
ap wlan0 start-profile bebas.ap
```

## uji penggunaaan

*cari di wifi device lain, jika muncul wifi dengan nama bebas maka setup acces poin telah berhasil*

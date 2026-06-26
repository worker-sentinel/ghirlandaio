## *Setup Admin Local Test*
```
ping 1.1.1.1
```
```
ip a
```
```
pacman -S iwd
```
```
systemctl disable NetworkManager
```
```
systemctl stop NetworkManager
```
```
systemctl enable iwd
```
```
systemctl start iwd
```
```
systemctl status iwd
```
```
sambungkan jaringan

iwctl
```
```
station wlan0 get-networks 
```
```
station wlan0 connect (nama wifi)
```
```
nvim /var/lib/iwd/(nama wifi).psk
```
```
# add configuration below 
```

[Security]
```
PreSharedKey=07409fc91b7d66d914529af1eb1d74c9d2538cfa561d3593d 
Passphrase-=
SAE-PT-Group19=ff700b20ff1dce97b0df773194aee24b7bef4aa14719daafi bd93ad00 
SAE-PT-Group20=50829b3d9e6af5939189411ddfb739b6d0e99f 1b847c46ee66f3939 
```
 [IPv4]
```
Address=192.168.1.26
Netmask=255.255.255.0
Gateway=192.168.1.1
```

### SET UP IP STATIK BUAT SERVER 2 
```
ssh username@ipserver1
```
```
sudo nvim /etc/systemd/network/01-ethernet.network
```

**Bagian 01-ethernet diisi sesuai kemauan kita**
```
ip address : 14.14.14.10/24
ip gateaway: 14.14.14.1
ip broadcast: 14.14.14.255
```
**esc lalu :wq**
```
sudo systemctl restart systemd-networkd
```

### IP SERVER1 BUAT LAN

```
ssh username@ipserver
```
```
sudo nvim /etc/systemd/network/01-ethernet.network
```

**Ubah bagian 01-ethernet di addressnya itu diisi sesuai kemauan kita**
```
ip address : 14.1.4.19/24
ip gateaway: 14.14.14.1
```
**esc lalu :wq**
```
sudo systemctl restart systemd-networkd
```

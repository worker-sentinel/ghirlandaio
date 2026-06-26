### IP SERVER1 BUAT LAN

```
ssh username@ipserver1
```
```
sudo nvim /etc/systemd/network/01-ethernet.network
```

**Ubah bagian 01-ethernet di addressnya itu diisi sesuai kemauan kita**

**Buat IP Gateaway 3 digit depannya itu ngikutin ip address tapi blkgnya 1**

ip bikinan kita itu ip adressnya : 2.3.4.17/24
ip gateaway: 2.3.4.1
ip broadcast: 2.3.4.255

**esc lalu :wq**
```
sudo systemctl restart systemd-networkd
```

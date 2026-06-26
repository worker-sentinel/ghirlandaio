## Masuk ssh dari admin ke server 1
```
ssh force @192.168.6.1
```
```
ssh force @127.0.0.1
```

**masukkin password**

```
sudo su
```

**Masukkan password**
Cek status firewall aktif atau tidak
```
systemctl status firewalld
```
Cek list semua zona firewall
```
firewall-cmd - -list-all-zone
```
Untuk cek staus podman
```
systemctl status podman
```
## Masuk ke file .config/containers/slims
```
cd .config
```
```
cd containers
```
```
cd slims
```
```
ls
```
```
ip a show wlan0
```
```
whoami && hostname
```
```
ls
```
```
ssh force @192.168.6.1
```
```
exit
```
```
ssh force @192.168.6.1
```
```
sudo su 
```
Masukan password

```
ip addr add 192.168.6.50/24 dev wlan0
```
```
ip a
```
```
ssh force @192.168.6.1
```
```
cd .config
```
```
cd containers
```
```
ls
```
```
cd slims
```
```
systemctl podman
```
```
systemctl enable podaman
```
```
systemctl start podman
```
```
systemctl status podman
```
```
ip a 
```
```
Read form remote host 192.168.6.1: connection reset by peer
```
```
cd .config
```
```
cd containers/
```
```
ssh force @192.168.6.1
```
Password
```
cd .config
```
```
sudo su
```
password 
```
systemctl
```
```
ip a
```
```
systemctl status firewalld
```
```
firewall -cmd –list-all-zone
```

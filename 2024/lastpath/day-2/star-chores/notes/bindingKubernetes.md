sinkronisasi waktu

```
sudo timedatectl set-ntp true
```
install k3s
```
sudo curl -sfL https://get.k3s.io | sh -s
```
cek service
```
sudo systemctl status k3s.service
```
masuk root
```
sudo su
```
```
cd /var/lib/rancher/k3s/server/
```
```
ls
```
```
ip a
```
```

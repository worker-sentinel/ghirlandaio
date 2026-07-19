# Dokumentasi google kubernetes engine
# Instalasi K3s Kubernetes

## 1. Set NTP Time Sync
```
sudo timedatectl set-ntp true

```
## 2. Install K3s Server
```
sudo curl -sfL https://get.k3s.io | sh -s server

```
## 3. Cek Status Service
```
sudo systemctl status k3s.service
```
## 4. Masuk ke Direktori Data K3s
```
sudo su
cd /var/lib/
cd /var/lib/rancher/k3s/server/
ls
```
## 5. Ambil Node Token 
```
cat node-token
```
## 6. Cek IP Address
```
ip a
```
## 7. Buka Port Firewall 
```
sudo systemctl status firewalld
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
sudo firewall-cmd --reload
```
## 8. Verifikasi Node Cluster
```
sudo kubectl get node
```

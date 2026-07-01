# Install K3s kubernetes
```
sudo timedatectl set-ntp true
```
```
masukin pw 
```
## instalasi K3
```
sudo curl -sfL https://get.k3s.io | sh -s server
```
## pengecekan status K3
```
sudo systemctl status k3s.service
```
```
sudo su
```
```
cd /var/lib/
```
```
cd /var/lib/rancher/k3s/server/
```
## melihat daftar file
```
ls
```
## kode token
```
cat node-token
```
## cek alamat ip
```
ip a
```
## cek status firewall
```
sudo systemctl status firewalld
```
## komunikasi cluster
```
sudo firewall-cmd –zone=public –add-port=6443/tcp –permanent
```
```
sudo firewall-cmd –reload
```
## daftar komputer terhubung 
```
sudo kubectl get node
```
```
ssh <nama grup> @(IP) 
```
## penggabungan komputer
```
sudo curl -sfL https://get.k3s.io | K3S_URL=”https://(IP)” K3S_TOKEN=”<TOKEN>
```
```
masukin pw
```
```
sudo systemctl status k3s-agent.service
sudo systemctl restart k3s-agent.service
sudo systemctl status k3s-agent.service
sudo systemctl status k3s-agent.service
```
```
ls
```
```
asciinema upload
```
```
exit
```

# CONNECT WIFI

```
iwctl
```
> untuk mengelola koneksi wifi
```
station wlan0 scan
```
> untuk memindai jaringan wifi disekitar
```
station wlan0 get-networks
```
> untuk menampilkan daftar wifi yang ditemukan
```
station wlan0 connect "namajaringan"
```
> untuk menghubungkan ke jaringan wifi, kemudian dilanjutkan dengan memasukkan password
```
exit
```
> untuk keluar dari iwctl
# TES JARINGAN 
```
ping 8.8.8.8
```
> untuk ngecek udah tersambung belom
# 
curl -sfL https://get.k3s.io | sh -
```
> untuk generate token 
```
sudo cat /var/lib/rancher/k3s/server/node/token
```
> cek token
```
sudo systemctl enable --now firewalld
```
sudo firewall-cmd --zone=public --add-port=6443/tcp -permanent
```
sudo firewall-cmd --reload 
```
sudo curl -sfL https://get.k3s.io | K3S _URL="https://10.171.188.232:6443" K3S_TOKEN="K106640dd7b4d90fbdc8ef6b6171825913743b3c62ca23da3665ef190ddd098756e::server:507c3a2a2b93f95f7dba84e95c064a41"sh -s -agent
```

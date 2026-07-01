## Instalasi Curl
```
pacman -Q curl
```
## Lalu kita drop token
```
curl -sfl https://get.k3s.io | sh -
```
## Lalu kita cek kodenya
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
## Bila sudah ada tokennya
> kita coba join kode tersebut


```
curl -sfl https://get.k3s.io | sudo sh -s - agent --server https://[ip a dari kontrol:6443 --token [kode token::server:kode token servernya]
```
## Setelah itu

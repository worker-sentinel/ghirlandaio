## Instalasi Curl
```
pacman -Q curl
```
## Lalu kita drop token
## Install K3s sebagai SERVER (control plane)
```
curl -sfl https://get.k3s.io | sh -
```

>* -s = silent, tidak tampilkan progress bar -f = fail silently jika server error, biar script rusak tidak dieksekusi
>* Hasil download langsung dieksekusi via "sh -"

## Lalu kita cek kodenya
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
## Bila sudah ada tokennya
> kita coba join kode tersebut


```
curl -sfl https://get.k3s.io | sudo sh -s - agent --server https://[ip a dari kontrol:6443 --token [kode token::server:kode token servernya]
```

# set up
## Mengatur waktu
```
sudo timedatectl set-ntp true
```
> mengaktifkan sinkronisasi waktu otomatis
```
sudo timedatecetl set-timezone Asia/Jakarta
````
> mengubah zona waktu sistem ke waktu Indonesia Barat
```
sudo timedatectl
```
> untuk cek status

## Server region control
pada ada bagian admin ssh region control untuk menjadi control-plane
````
curl -sfl https://get.k3s.io | sh -s server --disable traefik
```
> install pada hasil yang resmi
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
> untuk mengecek tokennya
K1038fb91da987e64ac1d98b948bc00520b51f8ef4f1dca73ea14a2391a4c1fb76c::server:e08f645e38889f1dfab0766a250aceee
````

## Server region data
> pada bagian laptop admin di region control
curl -sfl https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_PADA_TOKEN_DARI_SERVER" sh -s - agent
```
sudo kubectl label node [hostnamae_server] role=[rolenya]
```

## Server region internal
> dari laptop admin di region control
```
curl -sfl https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_PADA_TOKEN_DARI_SERVER" sh -s - agent
```
sudo kubectl label node [hostnamae_server] role=[rolenya]
```

## Server region public
> dari laptop admin di region control
```
curl -sfl https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_PADA_TOKEN_DARI_SERVER" sh -s - agent
```
sudo kubectl label node [hostnamae_server] role=[rolenya]
```

## cek status region
> dari laptop admin diregion control
```
sudo k3s kubectl get nodes
```
> [note]
> command diatas di jalankan di server control

# langkah awalnya 
## mengsinkronkan waktu
```
sudo timedatectl set-ntp true
```
> mengaktifkan sinkronisasi waktu otomatis
```
sudo timedatectl set-timezone Asia/Jakarta
```
> mengubah zona waktu (timezone) sistem Anda ke Waktu Indonesia Barat (WIB
```
sudo timedatectl
```
> cek statusnya

## region internal 
> harus dari laptop admin di region control
```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 
```
```
sudo kubectl label node [hostname_server] role=[rolenya]
```

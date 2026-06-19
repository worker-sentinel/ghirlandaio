## Login ke Server
```
ssh kel10@192.168.2.110
```
Saat pertama kali terhubung, sistem akan meminta konfirmasi fingerprint host:
```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
jawab 
```
yes
```
Kemudian masukkan password user.
## Mengedit Konfigurasi Network
File yang diedit:
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
## Mengaktifkan IP Forwarding
Membuat file:
```
sudo nvim /etc/sysctl.d/30-ipforward.conf
```
Isi file:
```
net.ipv4.ip_forward = 1net.ipv4.ip_forward = 1
```
## Menerapkan Konfigurasi
```
sudo sysctl --system
```
## Verifikasi
```
sysctl net.ipv4.ip_forward
```

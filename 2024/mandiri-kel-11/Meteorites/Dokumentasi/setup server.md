# Melakukan konfigurasi IP statis
## server 1

```
sshusername@ipserver1

contoh: ssh sebelas@10.99.68.138
password
```
lanjut
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
Menyesuaikan konfigurasi network dan address sesuai kebutuhan. Pada bagian gateway, tiga angka awal mengikuti alamat IP yang digunakan, sedangkan angka terakhir ditetapkan menjadi 1
ip kelompok kita untuk server 1: Address=2.3.4.17/24, Gateway=2.3.4.1

## server 2

```
sshusername@ipserver2

contoh: ssh sebelas@10.99.68.84
password
```

lanjut
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
ip kelompok kita untuk server 2: Address=2.3.4.16/24, Gateway=2.3.4.1

lanjut
```
sudo systemctl restart systemd-networkd
```
masukan lagi sshusername@ipserver 
  
## next exit

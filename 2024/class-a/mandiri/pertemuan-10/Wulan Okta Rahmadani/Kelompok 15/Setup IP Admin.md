## Memeriksa Status Jaringan
```
nmcli device status
```
## Menampilkan Informasi IP
```
ip
```
## Membuat Koneksi Jaringan Baru
```
 nmcli connection add type ethernet ifname enp2s0 con-name "admin-connection" ipv4.method manual ipv4.addresses 15.15.5.2/24 ipv4.gateway 15.15.5.1 ipv4.dns 8.8.8.8
```
## Membuat User Baru
```
echo "nmcli connection up admin-connection" >> ~/.bash_profile
```
## Membuat Pengguna Baru
```
sudo useradd -m operator
```
## Mengatur Password Pengguna
```
sudo passwd operator
```
## Keluar dari Terminal
```
exit
```

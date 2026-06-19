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
Connection 'admin-connection' successfully added.
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

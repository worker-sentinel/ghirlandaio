# Setup IP Admin

## 1. periksa Status Jaringan menggunakan kode : 
```
nmcli device status
```
## 2. lihat Informasi IP, menggunakan kode : 
```
ip
```
## 3. Membuat Koneksi Jaringan Baru
```
Connection 'admin-connection'
```
## 4. Membuat User Baru
```
echo "nmcli connection up admin-connection" >> ~/.bash_profile
```
## 5. Membuat Pengguna Baru
```
sudo useradd -m operator
```
## 6. Mengatur Password Pengguna
```
sudo passwd operator
```
## 7. logout admin kemudian masuk server
```
sudo reboot
```
kemudian pada masuk kedalam operator

# referensi 

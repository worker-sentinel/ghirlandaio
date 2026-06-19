# Setup Network Admin
## Membuat user baru
```
sudo user add -m sagioperator
```
## Passowrd user
```
sudo passwd sagioperator
```
>masukkan password,retype password
## Status jaringan (memeriksa apakah sudah terhubung ethernet) 
```
nmcli device status
```
>nmcli adalah NetworkManager Command Line Interface

>ethernet adalah jaringan kabel untuk menghubungkan perangkat dalam suatu jaringan lokal
## Membuat koneksi ethernet admin
```
nmcli connection add type ethernet ifname eno1 con-name "adminsagi" ipv4.method manual ipv4.addresses 14.26.26.14/24 ipv4.gateway 14.26.26.1 ipv4.dns 8.8.8.8
```
## Membuat koneksi ethernet operator
```
sudo nmcli connection add type ethernet ifname eno1 con-name "operatorsagi" ipv4.method manual ipv4.addresses 14.26.26.13/24 ipv4.gateway 14.26.26.1 ipv4.dns 8.8.8.8
```
## Bash_profile (agar adminsagi otomatis diaktifkan ketika user sagitariusadmin login)
```
echo "nmcli connection up adminsagi" >> /home/sagitariusadmin/.bash_profile
```
## Menampilkan isi file
```
cat /home/sagitariusadmin/.bash_profile
```
## Bash_profile (agar adminsagi otomatis diaktifkan ketika user sagitariusadmin login)
```
echo "nmcli connection up adminsagi" >> /home/sagitariusadmin/.bash_profile
```
```
exit
```

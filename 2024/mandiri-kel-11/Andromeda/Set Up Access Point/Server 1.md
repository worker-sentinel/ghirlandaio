# Set Up Access Point Server 1
## Masuk sebagai root
```
sudo su
```
## Melihat daftar profil Access Point
```
ls /var/lib/imd/ap
```
## Masuk ke direktori profil AP
```
cd /var/lib/imd/ap
```
## Membuat/mengedit konfigurasi AP hotspot2.ap 
```
sudo nvim hotspot1.ap
```
insert

<img width="217" height="201" alt="server accest point 1" src="https://github.com/user-attachments/assets/575ef0d5-c460-41ee-b19c-6bddea3d71b9" />


## Masuk ke iwd

```
iwctl
```
## Mengubah wlan0 ke mode Access Point
```
device wlan0 set-property Mode ap
```
## Menjalankan AP dengan profil hotspot2
```
ap wlan0 start-profile hotspot1
```
## Keluar dari iwd
```
exit
```
## Melihat konfigurasi IP jaringan
```
ip a
```
## mengecek/menghubungkan jaringan
```
iwctl
```
```
device list
```
```
station wlan0 get-networks
```
```
station wlan0 scan
```
## Mengizinkan IP 10.77.249.246 mengakses port 80
```
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.77.249.246" port port="22" protocol="tcp" accept'
```
## Menerapkan perubahan firewall
```
firewall-cmd --reload
```
## Keluar dari sesi root
```
exit
```

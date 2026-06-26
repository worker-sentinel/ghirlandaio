server 2

# Cek Partisi dan IP Address
```
lsblk
```
```
ip a
```
Membuka dan mengedit file konfigurasi jaringan ethernet
```
sudo nvim /etc/systemd/network/20-ethernet.network
``` 
```
Masukin pass
```
```
esc + :wq
```
```
sudo systemctl restart systemd-networkd
```
```
sudo su
```
# Konfigurasi dan Manajemen Web Server Nginx
```
cd /etc/nginx/
```
Melihat daftar file dan folder yg ada di dalam direktori /etc/nginx/
```
ls
```
Mengubah nama folder
```
mv sites-enable sites-enabled
```
Membuat atau mengedit file konfigurasi server virtual host bernama slims.conf untuk apk SLiMS
```
nvim sites-enabled/slims.conf
```
```
server{
    listen 80;
    server_name slims.test;

    localtime / {
         proxy_pass http://[ipserver2]:80;
         proxy_set_header Host $host;
         proxy_set_header X-Real-IP $remote_addr;
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header X-Forwarded-Protk $scheme;
    }
}
```
```
esc + :wq
```
Mengedit file konfigurasi utama nginx (nginx.conf) untuk melakukan penyesuaian global
```
sudo nvim /etc/nginx/nginx.conf
```
```
#user http; menjadi user http;
```
tambahkan paling akhir
```
include /etc/nginx/sites-enabled/*;
}
```
```
esc + :wq
```
Melakukan uji coba
```
nginx -t
```
Menghentikan layanan firewall (firewalld) untuk memastikan tidak ada lalu lintas data yang diblokir ke server. 
```
sudo systemctl stop firewalld
```
Menjalankan layanan web server nginx
```
systemctl start nginx
```
```
systemctl status nginx
```
Membuka kembali konfigurasi slims.conf dengan hak akses sudo untuk melakukan perbaikan (kemungkinan memperbaiki masalah Network is unreachable ke arah upstream). 
```
sudo nvim /etc/nginx/sites-enabled/slims.conf
```
Memuat ulang total layanan Nginx agar konfigurasi terbaru yang telah diperbaiki bisa langsung aktif. 
```
sudo systemctl restart nginx
```
# Konfigurasi Access Point Wi-Fi
Melihat daftar profil Access Point yang tersimpan di direktori konfigurasi iwd. 
```
ls /var/lib/iwd/ap/
```
Menjalankan iwd untuk jaringan Wi-Fi
```
systemctl start iwd
```
```
iwctl
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile hotspot2
```
Keluar dari iwd
```
exit
```
Keluar dari root
```
exit
```
Keluar dari asciinema
```
ctrl + d / exit
```

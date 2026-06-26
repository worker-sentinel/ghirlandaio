# Dokumentasi Setup firewall

## Login ip baru

ssh SERVER@10.10.3.4

## Mengecek alamat ip

ip a

## Menjalankan Firewall

systemctl start firewalld

## Membuat zona admin

sudo firewall-cmd --permanent --new-zone=admin   

## Menambahkan ip admin

sudo firewall-cmd --permanent --zone=admin --add-source=10.10.3.2 

## Memberikan hak akses admin


> SSH 


sudo firewall-cmd --permanent --zone=admin --add-service=ssh 


> MySQL

sudo firewall-cmd --permanent --zone=admin --add-port=3306/tcp 

> Remote dictionary server

sudo firewall-cmd --permanent --zone=admin --add-port=6379/tcp

> HTTP

sudo firewall-cmd --permanent --zone=admin --add-port=80/tcp                                                      

> Membuka port HTTPS


sudo firewall-cmd --permanent --zone=admin --add-port=443/tcp


## Membuat zona operator

sudo firewall-cmd --permanent --new-zone=operator

## Menambahkan ip operator

sudo firewall-cmd --permanent --zone=operator --add-source=10.10.3.3

## Memberikan hak akses operator


> sudo firewall-cmd --permanent --zone=operator --add-service=ssh
> sudo firewall-cmd --permanent --zone=operator --add-port=6379/tcp
> sudo firewall-cmd --permanent --zone=operator --add-port=80/tcp
> sudo firewall-cmd --permanent --zone=operator --add-port=443/tcp 


## Membuat zona wifi

sudo firewall-cmd --permanent --new-zone=wifi

## Menambahkan network wifi

sudo firewall-cmd --permanent --zone=wifi --add-source=10.10.4.1/24    

## Memberikan hak akses wifi


> HTTP

sudo firewall-cmd --permanent --zone=wifi --add-port=80/tcp

> HTTPS

sudo firewall-cmd --permanent --zone=wifi --add-port=443/tcp

## Melihat konfigurasi firewall

sudo firewall-cmd --list-all-zone 

## Membersihkan service default firewall


> sudo firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
> sudo firewall-cmd --zone=work --remove-service=ssh --permanent


## Reload firewall

sudo firewall-cmd --reload

## Melihat container

podman ps -a

## Masuk ke folder porject

cd slimadmin

## Masuk ke folder compose

cd compose

## Menghentikan container

podman compose down

## Menjalankan container

podman compose up -d

## Verifikasi container

podman ps -a

## Verifikasi akhir jaringan

ip a
```

# Set Up Access Point Server 2
## Masuk sebagai root

sudo su

## Melihat daftar profil Access Point

ls /var/lib/imd/ap

## Masuk ke direktori profil AP

cd /var/lib/imd/ap

## Membuat/mengedit konfigurasi AP hotspot2.ap 

sudo nvim hotspot2.ap

insert

<img width="244" height="181" alt="WhatsApp Image 2026-06-26 at 07 41 47" src="https://github.com/user-attachments/assets/00ea1629-6c02-49ec-bef7-c3cc5afa98e8" />


## Masuk ke iwd


iwctl

## Mengubah wlan0 ke mode Access Point

device wlan0 set-property Mode ap

## Menjalankan AP dengan profil hotspot2

ap wlan0 start-profile hotspot2

## Keluar dari iwd

exit

## Melihat konfigurasi IP jaringan

ip a

## mengecek/menghubungkan jaringan

iwctl


device list


station wlan0 get-networks


station wlan0 scan

## Mengizinkan IP 10.77.249.246 mengakses port 80

sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.77.249.246" port port="80" protocol="tcp" accept'

## Menerapkan perubahan firewall

firewall-cmd --reload

## Keluar dari sesi root

exit

<img width="1080" height="245" alt="WhatsApp Image 2026-06-26 at 07 07 37" src="https://github.com/user-attachments/assets/da12cac6-46dc-47bf-b27c-013479538dc2" />

# Set Up Access Point Server 1
## Masuk sebagai root

sudo su

## Melihat daftar profil Access Point

ls /var/lib/imd/ap

## Masuk ke direktori profil AP

cd /var/lib/imd/ap

## Membuat/mengedit konfigurasi AP hotspot2.ap 

sudo nvim hotspot1.ap

insert

<img width="217" height="201" alt="WhatsApp Image 2026-06-26 at 07 49 57" src="https://github.com/user-attachments/assets/99b8f8fb-ebfe-4602-b5ef-f1e4579b88ed" />



## Masuk ke iwd


iwctl

## Mengubah wlan0 ke mode Access Point

device wlan0 set-property Mode ap

## Menjalankan AP dengan profil hotspot2

ap wlan0 start-profile hotspot1

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

sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="10.77.249.246" port port="22" protocol="tcp" accept'

## Menerapkan perubahan firewall

firewall-cmd --reload

## Keluar dari sesi root

exit

<img width="1078" height="412" alt="WhatsApp Image 2026-06-26 at 07 07 37 (1)" src="https://github.com/user-attachments/assets/91d8ddb4-be07-4909-8b50-b77c27010556" />

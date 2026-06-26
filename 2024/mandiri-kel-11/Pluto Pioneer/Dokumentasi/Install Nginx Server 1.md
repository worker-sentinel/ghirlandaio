# Install nginx server


## Mengedit file hosts 
``` 
sudo nvim /etc/hosts
``` 

## Menampilkan daftar paket
``` 
sudo pacman ps - a
``` 

## Alamat ip
``` 
ip a
``` 

## Mengedit Konfigurasi Jaringan Ethernet 
``` 
sudo nvim /etc/systemd/network/20-ethernet.network
``` 

## Masuk ke root
``` 
sudo su
``` 

## Instal iwd
``` 
sudo pacman -S iwd
``` 

## Menghentikan layanan NetworkManager 
``` 
sudo systemctl stop NetworkManager
``` 

## Menonaktifkan NetworkManager
``` 
sudo systemctl disable NetworkManager
``` 

## Menjalankam iwd
``` 
sudo systemctl start iwd
``` 

## Keluar dari root
``` 
exit
``` 

## Masuk ke direktori konfigurasi iwd 
``` 
cd var/lib/iwd
``` 

## Membuat access point
``` 
mkdir -p  ap
``` 

## Masuk ke access point
```
cd ap
``` 

## Membuat file konfigurasi access point 
``` 
nvim hospot2.ap
``` 

 ## Isi konfigurasi acces point
``` 
[General]
Enable=true
SSID=hotspot2
[Security]
Passphrase=12345678                                                                                                                                                                                                                                                                                                                                                                         [IPV4
Address=10.11.3.4
Netmask=255.255.255.0 
``` 

## Melihat kapasitas
``` 
cat /sys/class/power_supply/BAT1/capacity
``` 

## Restart iwd
``` 
sudo systemctl restart iwd
``` 

## Membuka iwd/jaringan
``` 
iwctl
``` 
## Mengubah mode perangkat Wi-Fi jadi access point
``` 
device wlan0 set-property Mode ap
``` 
``` 
ap wlan0 start-profile hotspot2 
```
``` 
exit
``` 

## Menambahkan firewall untuk layanan HTTP
``` 
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.234" port port="80" protocol=tcp accept'
``` 

## Memuat ulang konfigurasi firewall
``` 
sudo firewall-cmd --reload
``` 

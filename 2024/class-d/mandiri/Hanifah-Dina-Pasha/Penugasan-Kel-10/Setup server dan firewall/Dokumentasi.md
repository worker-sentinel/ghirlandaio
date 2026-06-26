# Dokumentasi Setup server dan firewall

---
## Login ke server
```
ssh SERVER@10.113.208.188
```
## Administrator
```
sudo su
```
## Mengedit dan mengatur file konfigurasi ethernet
```
nvim /etc/systemd/network/20-ethernet.network
```
> [Network]
> Adress=10.10.3.4/24
> Gateway=10.10.3.1
> DNS=1.1.1.1 8.8.8.8
> MulticastDNS=yes
```
```
## Install Hostapd
```
pacman -S hostapd
```
## Cek interface jaringan
```
ip link
```
## Konfigurasi access point
```
nvim /etc/hostapd/hostpad.conf
```
> tambahkan
> interface=wlan0
> driver=nl80211
> ssid=MyArchAP
> hw_mode=g
> channel=7
> auth_algs=1
> wpa=2
> wpa_passphrase=12345678
> wpa_key_mgmt=WPA-PSK
> wpa_pairwise=TKIP
> rsn_pairwise=CCMP
```
```
## Mengatur konfigurasi jaringan access point
```
nvim /etc/systemd/network/02-wireless-ap.network
```
> isi
> [Match]
> Name=[wireless interface]

> [Network]
> Address=10.10.4.1/24
> DHCPServer=yes
```
```
## Restart network
```
systemctl restart systemd-networkd
```
## Aktifkan network service
```
sudo systemctl enable --now systemd-networkd
```
## Mengatur ip forwading
```
nvim /etc/systcl.d/30-ipforward.conf
```
> isi
> net.ipv4.ip_forward=1     
```
```
## Terapkan konfigurasi sysctl
```
sudo sysctl --system 
```
## Menjalankan hostapd
```
sudo sysctl enable --now hostapd
```
## Keluar
```
exit
```
## Login ip baru
```
ssh SERVER@10.10.3.4
```
## Mengecek alamat ip
```
ip a
```
## Menjalankan Firewall
```
systemctl start firewalld
```
## Membuat zona admin
```
sudo firewall-cmd --permanent --new-zone=admin   
```
## Menambahkan ip admin
```
sudo firewall-cmd --permanent --zone=admin --add-source=10.10.3.2 
```
## Memberikan hak akses admin
```
```
> SSH 
```
```
sudo firewall-cmd --permanent --zone=admin --add-service=ssh 
```
```
> MySQL
```
sudo firewall-cmd --permanent --zone=admin --add-port=3306/tcp 
```
> Remote dictionary server
```
sudo firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
> HTTP
```
sudo firewall-cmd --permanent --zone=admin --add-port=80/tcp                                                      
```
> Membuka port HTTPS
```
```
sudo firewall-cmd --permanent --zone=admin --add-port=443/tcp
```
```
## Membuat zona operator
```
sudo firewall-cmd --permanent --new-zone=operator
```
## Menambahkan ip operator
```
sudo firewall-cmd --permanent --zone=operator --add-source=10.10.3.3
```
## Memberikan hak akses operator
```
```
> sudo firewall-cmd --permanent --zone=operator --add-service=ssh
> sudo firewall-cmd --permanent --zone=operator --add-port=6379/tcp
> sudo firewall-cmd --permanent --zone=operator --add-port=80/tcp
> sudo firewall-cmd --permanent --zone=operator --add-port=443/tcp 
```
```
## Membuat zona wifi
```
sudo firewall-cmd --permanent --new-zone=wifi
```
## Menambahkan network wifi
```
sudo firewall-cmd --permanent --zone=wifi --add-source=10.10.4.1/24    
```
## Memberikan hak akses wifi
```
```
> HTTP
```
sudo firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
> HTTPS
```
sudo firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```
## Melihat konfigurasi firewall
```
sudo firewall-cmd --list-all-zone 
```
## Membersihkan service default firewall
```
```
> sudo firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
> sudo firewall-cmd --zone=work --remove-service=ssh --permanent
```
```
## Reload firewall
```
sudo firewall-cmd --reload
```
## Melihat container
```
podman ps -a
```
## Masuk ke folder porject
```
cd slimadmin
```
## Masuk ke folder compose
```
cd compose
```
## Menghentikan container
```
podman compose down
```
## Menjalankan container
```
podman compose up -d
```
## Verifikasi container
```
podman ps -a
```
## Verifikasi akhir jaringan
```
ip a
```

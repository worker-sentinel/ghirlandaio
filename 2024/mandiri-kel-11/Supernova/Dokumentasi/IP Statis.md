## Masuk ke Mode Root
```
sudo su
```
## Mengonfigurasi Network Interface
```
nvim /etc/systemd/network/20-ethernet.network
```
## Restart Service Network
```
systemctl restart systemd-networkd
```
## Konfigurasi IWD
```
nvim /etc/iwd/main.conf
```
## Membuat Folder Access Point
```
cd /var/lib/iwd
mkdir ap
cd ap
```
## Membuat Profil Hotspot
```
nvim myhotspot.ap
```
## Restart IWD
```
systemctl restart iwd
```
## Membuka IWD CLI
```
iwctl
```
## Melihat Daftar Wireless Device
```
device list
```
### Output:
```
wlan0
Mode : ap
```
## Mengubah Mode Wireless
```
device wlan0 set-property Mode ap
```
## Menjalankan Profil Hotspot
```
device wlan0 start-profile myhotspot
```
## Keluar dari IWD
```
exit
```
## Melihat Konfigurasi IP
```
ip a
```
## Menambahkan Firewall Rule
```
firewall-cmd --zone=public \
--add-rich-rule='rule family="ipv4" source address="10.81.142.236" port protocol="tcp" port="22" accept'
```
## Keluar dari Root
```
exit
```

## Referensi
2024/class-c/kelompok/Kelompok-11_4C/Dokumentasi/setup akses point, ip static dan set firewall.md

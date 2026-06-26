# Setup ip statip server 1 dan server 2 (sambungin kabel lan ke server 1 ke 2)

```
sudo nvim /etc/systemd/network/20-ethernet.network
```
masukin pw

<img width="1200" height="1600" alt="foto aaa" src="https://github.com/user-attachments/assets/23327f68-a4e8-48e1-b432-f6431f4bd151" />

kalo kosong isi semua yg biru dan putih kecuali yg # (jangan typo)
address=13.13.2.3/24
gateway=13.13.2.1 (angka bebas max 225)

set ip statip untuk server 2
konekin wifi, 1 wifi ama admin, dicek statusnya. sambungin ssh dari admin ke server 2

(di terminal cri server di kiri atas trus tambah file baru)

## Konekin di server 2 
```
ip a
```
bagian wlan0 untuk ip server 2
```
ssh (usrname di server 2)@(ip server 2)
```
setup ip
```
sudo nvim /etc/systemd/network/20-ethernet.network
```

## Di addres, 3 digit awal disamain dgn server 1/ip address sebelumnya, digit ke 4nya harus beda. Gateway samain semua.

address=13.13.2.4/24
gateway=13.13.2.1

esc :wq
```
sudo systemctl restart systemd-networkd
```
## Masukin pw

di server 1 juga direstart
```
sudo systemctl restart systemd-networkd
```
## Masukin pw

di server 1
```
ip a
```

## Etherned itu ip nya yg enp, kalo ip keluar sesuai dgn yang kita setup itu artinya berhasil

karena direstart, ssh nya jadi terputus. Sshnya harus disambungkan lagi

ssh server 2 (kabel lan jangan dilepas)
```
ssh (usr server 2)@(ip addres server 2 wlan0)
```

## Di server 2 install nginx untuk web server dari server 1. 

-download nginx
```
sudo pacman -S nginx
```

## Set up ip static
```
sshusername@ipserver1
```
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
**Ubah bagian network di addressnya itu diisi sesuai kemauan kita**
**Buat di Gateaway 3 digit depannya itu ngikutin ip address tapi blkgnya 1**

**(ip address ada 4 digit, setiap digit itu angkanya gak boleh 0 (ip network), 1(ip gateaway), 255(broadcast) bolehnya 2-254)**
*nah ip bikinan kita itu ip adressnya : 2.3.4.17/24*
*p gateaway: 2.3.4.1*
*ip broadcast: 2.3.4.255*

*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*
```
sudo systemctl restart systemd-networkd
```

## Set up ip static untuk server 2 
```
ssh usernameserver2@ipserver2
```
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
**di network ip address 3 digit pertama harus sama kaya server 1, nah di server 2 yg diubah digit ke 4 aja**
*ip address server 1: 2.3.4.17*
*ip address server 2:2.3.4.16*

*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

```
sudo systemctl restart systemd-networkd
```

**(cek ip server 1 di laptop admin) ip a dan nanti mucul ip yang untuk server 1**

## Set up akses point

```
sudo nvim /etc/iwd/main.conf
```
**diisi seperti di bawah ini**

```
[General]
EnableNetworkConfiguration=true
```
*kalau udh esc :wq*

```
sudo su
```
```
cd /var/lib/iwd
```
```
mkdir ap
```
```
cd ap
```
```
nvim myhotspot.ap
```
**diisi seperti di bawah ini**
```
[General]
Enable=true
SSID=sebelas

[Security]
Passphrase=12345678

[IPv4]
Address=11.11.11.3 (contoh ip)
Netmask=255.255.255.0
```
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

```
sudo systemctl restart iwd
```
```
sudo iwctl
```
```
device list
```
```
device wlan0 set-property Mode ap
```
```
ap wlan0 start-profile myhotspot
```
*cek di admin*

## Cara aktifin WiFi

```
iwctl
```
```
device list pastiin modenya ap
```
```
device wlan0 set-property Mode ap (kalau modenya belum ap)
```
```
ap wlan0 start-profile myhotspot
```
```
admin mengecek ada engga wifi yang namanya myhotspot
```

## set firewall di server 1 (add) 

```
sudo firewall-cmd –zone=public –add-rich-rule='rule family=”ipv4” source address=”[ip server 2]” port port=”8080” protocol=”tcp” accept'
```
```
sudo firewall-cmd –zone=public –add-rich-rule='rule family=”ipv4” source address=”[ip wifi server 1]” port port=”22” protocol=”tcp” accept’
```
```
sudo firewall-cmd –-reload
```

## set firewall di server 2

```
ddress=”[ip server 2]” port port=”8080” protocol=”tcp” accept’
```
```
sudo firewall-cmd –zone=public –add-rich-rule='rule family=”ipv4” source address=”[ip wifi server 1]” port port=”22” protocol=”tcp” accept’
```
```
sudo firewall-cmd –-reload
```
## Selesai

NETWORK
Client data itu buat mariadb
Redis buat cache 
===

## Bagian 4 : Network 

### 4.1 : Tentukan IP

Buka terminal admin > kita tentukan dua user. Satu user admin, satu user operator. 
IP adress admin, operator, server, wifi (wifi buat sendiri di laptop server)

IP adress admin: 199.19.9.7
IP address operator: 199.19.9.9
IP address server: 199.19.9.8
IP address wifi: 199.19.10.1
IP address gateway: 199.19.9.1

### 4.2 FOKUS UNTUK USER ADMIN
	Useradd -m startop
	Sudo passwd startop :1234

#### 1. BIKIN KONEKSI UNTUK ADMIN
Mencari interface:

```bash
nmcli device status
sudo nmcli connection add type ethernet if name eno1 con-name ”admin” ipv4.method manual ipv4.adress 199.19.9.7/24 ipv4.gateway 199.19.9.1 ipv4.dns 8.8.8.8
```
	
#### 2. BIKIN KONEKSI UNTUK OPERATOR
```bash
sudo nmcli connection add type ethernet if name eno1 con-name ”operator” ipv4.method manual ipv4.adress 199.19.9.9/24 ipv4.gateway 199.19.9.1 ipv4.dns 8.8.8.8
```

#### 3. USER ADMIN OPERATOR

```bash
	sudo su
sudo echo
	echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user operator]/.bash_profile
	echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user operator]/.bash_profile
```

```bash
echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user admin]/.bash_profile
echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user admin]/.bash_profile
cat /home/starfall/.bash
```

#### 4. SAMBUNGKAN KABEL LAN DARI LAPTOP ADMIN KE LAPTOP SERVER
(lihat asciinema di laptop admin)

#### 5. SETUP SSH SERVER 
(lihat asciinema di laptop admin)

```bash
systemctl stop firewalld
systemctl restart iwd
systemctl start firewalld
```

[di laptop admin]
```bash
sudo su 
[masukkan password] pw: 123
nvim /etc/systemd/network/20-ethernet [pencet tab aja biar cepet]
Systemctl restart sytemd-networkd 
```

[colok kabel lan]
Cari ip 

```bash
Ip a
```

Masuk ke nvim lagi 
```bash
nvim /etc/systemd/network/20-ethernet [pencet tab aja biar cepet]
```

Konfigurasi Set Up Router

```bash
ssh [namauserserver]@[ip address server]
sudo su 
pacman -S hostapd
nvim /etc/hostapd/hostapd.conf
```

Search menuju ke line 3200, scroll lagi sampai ke line terbawah. Masuk ke inspection (i)
Isi : 

```bash
interface=wlan0
driver=nl80211
ssid=jatoh [nama wifi]
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=[password/bintangjatuh]
wpa_key_mgmt=WPA_PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Keluar esc :wq

Buat file konfigurasi 

```bash
nvim /etc/systemd/network/02-wireless-ap-network
```

Isi :

```bash 
[Match]
Name=wlan0

[Network]
Address=[ip wifi]
DHCPServer=yes
```
Keluar esc :wq

Restart systemd-networkd

```bash
systemctl restart systemd-networkd
```

Buat file konfigurasi 

```bash
Nvim  /etc/sysctl.d/30-ipforward.conf
```

ISI
```bash
net.ipv4.ip_forward=1
```

Esc: wq

```bash
Sudo sysctl–system
Systemctl enable hostapd
```

Ctrld sampai selesai asciinemanya terus reboot di server
	




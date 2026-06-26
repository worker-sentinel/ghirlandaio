### Recording Asciinema 
```
asciinema rec [bebas dah].cast
```
### Menghubungkan WIFI
```
iwctl
device list 
station wlan0 get-networks
station wlan0 connect [nama wifi]
```

```
crypsetup luksOpen /dev/nvme0n1p6 msi
```

Cek partisi: 
```
lsblk
```

```
lvremove /dev/dheca/dock
```

```
mount /dev/dheca/root /mnt
mount /dev/nvme0n1p5 /mnt/boot
```

```
arch-chroot /mnt
```

```
nvim /etc/fstab
```
hapus semua bagian _dock_ 

```
exit
```

```
mount /dev/dheca/vars /mnt/var
```

```
lsblk
```

```
arch-chroot /mnt
```
### Installasi Desktop 
```
pacman -S xfce4 sddm kitty
```
```
systemctl enable sddm
```

```
mkinitcpio -P
```

```
exit
```
### Stop Recording Asciinema

Klik ctrl+d 

Lalu upload asciinema 

```
upload asciinema [bebas dah].cast
```

Login useradd (admin)
lalu masukan password

Buka terminal kitty di dalam desktop xfce4
Lalu setelah itu
```
sudo su
```
jangan lupa connect wifi dulu 

### Recording Asciinema 
```
asciinema rec [bebas dah].cast
```

server ketik: 
```
ip a
```
admin ketik:
```
[ip dari server]
```

```
ssh server@[ip server]
```

```
sudo pacman -S openssh
sudo systemctl enable –now sshd

```
ssh server@[ip server yang tadi]
``` 

Buka Terminal Baru 
Installasi Firefox 

``` 
sudo pacman -S firefox
```

Buka Firefox lalu ketik: github.com/poco212/ 

Buka terminal server bagian ssh yang tadi 
```
pacman -S podman-compose wget
mkdir -p .config/[nama bebas]
cd .config/[nama bebas]

Download compose slims:
```
wget -c [https://github.com/slims/docker-compose-for-slims/archive/master.zip](https://github.com/slims/docker-compose-for-slims/archive/master.zip)
```

```
ls
```

```
pacman -S unzip
unzip [ketik tombol tab]
```

Untuk rename 
```
mv docker-compose-for-slims-master compose
```

```
ls
```

```
cd compose
```

```
sudo nvim /etc/sysctl.d/99-custom.conf
```
lalu isi dengan: net.ipv4.ip_unprivileged_port_start=80

```
sudo sysctl --system
```

```
mv docker-compose.yaml docker-compose.yaml.bck
```

```
mv docker-compose-redis.yaml docker-compose.yaml
```

```
echo '' > docker-compose.yaml
```

```
cat docker-compose.yaml
```

```
ls
```

```
nvim docker-compose.yaml
```
lalu isi dengan: 

version: "3.7"
services:
    db:
        image: mysql:5.7
        restart: always
        networks:
            - slims-net
        container_name: slims-db
        env_file:
            - db_default.env
        command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION --max_allowed_packet=1024M
        volumes:
            - "./dbdata:/var/lib/mysql"
        ports:
            - "127.0.0.1:3306:3306"
    redis:
        image: redis:latest
        restart: always
        networks:
            - slims-net
        container_name: redis
        ports:
            - "127.0.0.1:6379:6379"
    app01:
        image: slimsofficial/slims:latest
        restart: always
        networks:
            - slims-net
        container_name: slims-app
        ports:
            - "80:80"
            - "443:443"
        volumes:
            - "./app:/var/www/html"
                     -"./conf/php/php.ini:/usr/local/etc/php/conf.d/php.ini"
networks:
    slims-net:
        name: slims-net
        ipam:
            driver: default

lalu klik esc 
```
:wq
```

```
sudo chmod -R 777 app/slims
```
```
podman compose up -d
```
kalo error ketik:
```
sudo nvim /etc/subuid
```
cek isinya harus sama seperti ini:
[user] : 100000:65536

```
sudo nvim /etc/subgid
```
cek isinya harus sama seperti ini:
[user] : 100000:65536

```
sudo systemctl enable --global podman
```

```
nvim ~/.config/[nama folder yg sudah dibuat]/storage.conf
```
lalu isi dengan: 

[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"

sudo nvim /etc/containers/registries.conf
unqualified-search-registries = ["docker.io"]

lalu klik esc
```
:wq
```

```
ls
```

```
podman compose up -d
podman ps -a
```

Buka browser lalu ketik: http://ip server 

Kalo belum kebuka ketik di terminal:
```
sudo firewall-cmd -–zone=public –-add-port=80/tcp --permanent

sudo firewall-cmd --reload

sudo firewall-cmd --info-zone=public
```
Buka browser lagi ketik: 
http://ip server: [nama port]


Ketik di terminal (admin)
```
pacman -S networkmanager network-manager-applet
```

```
systemctl stop iwd
```

```
pacman -R iwd
```

```
systemctl enable NetworkManager
systemctl start NetworkManager
systemctl status NetworkManager
```

```
usermod -aG wheel [nama user admin]
```

### Stop Recording Asciinema 

Klik ctrl+d 

Lalu upload asciinema
```
upload asciinema [bebas dah].cast
```

Untuk setup network dmz (hubungkan kabel LAN):
```
nmcli device status
```

### Recording Asciinema 
```
asciinema rec [bebas dah].cast 
```

```
nmcli connection add type ethernet ifname [interface] con-name "admin-koneksi" ipv4.method manual ipv4.addresses [ip address admin]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

```
nmcli connection add type ethernet ifname [interface] con-name "server-koneksi" ipv4.method manual ipv4.addresses [ip address operator/server]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

```
echo "nmcli connection up admin-koneksi" >> /home/admin/.bash_profile
```

```
echo "nmcli connection up server-koneksi " >> /home/server/.bash_profile
```

terakhir klik LOGOUT (pojok kanan atas)

### Recording Asciinema
```
asciinema rec [bebas dah].cast
```

Bagian Server 
```
nvim /etc/systemd/network/20-ethernet.network
```
lalu isi dengan:

[Network]
Address: sesuai dengan yg kita buat
Gateway: 172.198.2.1
Dns: 1.1.1.1   8.8.8.8.8
Yes
```
systemctl restart systemd-networkd
```

Buka terminal kembali

membuat ip broadcast:
```
sudo pacman -S hostapd
```

```
nvim /etc/hostapd/hostapd.conf
```

scroll paling bawah lalu isi dengan:

interface=wlan0
driver=nl80211
ssid=MyArchAP
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=MySecurePassword
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP

```
nvim /etc/systemd/network/02-wireless-ap.network

```
lalu isi dengan:

[Match]
Name=[wireless interface]

[Network]
Address=[ip address]/CIDR
DHCPServer=yes

```
systemctl restart systemd-networkd
```
```
sudo systemctl enable --now systemd-networkd
```

membuat konfigurasi supaya kernelnya ipv4:
```
nvim /etc/sysctl.d/30-ipforward.conf
```

lalu isi dengan:

net.ipv4.ip_forward=1

```
sudo sysctl –system
sudo systemctl enable --now hostapd
```

### Stop Recording Asciinema
ctrl+d

```
asciinema upload [bebas dah].cast
```

### Setup Firewall Zone

Zona Admin
```
firewall-cmd --new-zone=admin  --permanent
firewall-cmd --zone=admin --add-source=172.198.2.2 --permanent
```

Zona Server
```
firewall-cmd --new-zone=operator --permanent
firewall-cmd --zone=operator --add-source=172.198.2.3  --permanent 
```

Zona Wifi
```
firewall-cmd --new-zone=wifi  --permanent 
firewall-cmd --zone=wifi --add-source=192.168.3.0/24 --permanent
```

```
podman ps
```

untuk menyalakan containers
```
cd.config/traktor/compose
podman compose up -d
```


admin:
```
firewall-cmd --zone=admin --add-service=ssh --permanent

firewall-cmd --zone=admin --add-port=3306/tcp --permanent

firewall-cmd --zone=admin --add-port=6379/tcp --permanent

firewall-cmd --zone=admin --add-port=80/tcp --permanent

firewall-cmd --zone=admin --add-port=443/tcp --permanent
```

operator:
```
firewall-cmd --zone=operator --add-service=ssh --permanent

firewall-cmd --zone=operator --add-port=6379/tcp --permanent

firewall-cmd --zone=operator --add-port=80/tcp  --permanent

firewall-cmd --zone=operator --add-port=443/tcp  --permanent
```

wifi client:
```
firewall-cmd --zone=wifi --add-port=80/tcp --permanent

firewall-cmd --zone=wifi --add-port=443/tcp --permanent
```

default drop:
```
firewall-cmd --set-default-zone=drop

sudo firewall-cmd –reload
```
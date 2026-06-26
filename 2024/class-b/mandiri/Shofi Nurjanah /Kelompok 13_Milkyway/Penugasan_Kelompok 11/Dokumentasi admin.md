# Install Admin

```
lvremove /dev/namavolumegrup/dock
```
```
Lsblk
```
```
mounting root= mount /dev/proc/root /mnt
```
```
mount /dev/namapartisi /mnt/boot
```
```
arch-chroot /mnt
```
```
nvim /etc/fstab
```

## install harus mounting var= 
```
mount /dev/proc/vars/mnt/var/arch-chroot/mnt
```

```
install desktop= pacman -S xfce sddm kitty
```
```
systemctl enable --global sddm
```
```
mkinitcpio -P
```
```
exit
```

## install aplikasi slims
```
di server= ip a
```
```
admin= connect ke ip a server= ping (ip server)
```
```
ssh, buat ngeremote servernya
```
```
ssh username server@ipserver
```
```
sudo pacman -S openssh (kalau blm download ssh)
```
sudo systemctl enable --now sshd (aktifin ssh)
```
```
sudo pacman -S podman--compose
```
```
## masukkan pw
```
mkdir -p .config/celadon
```
```
cd .config/celadon
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
ls 
```

## install 1 package 
```
sudo pacman -S unzip
```
```
unzip master.zip 
```
```
mv docker-compose-for-slims-master compose
```
```
ls
```
```
cd compose (masuk ke directory compose) (cd itu masuk)
```
```
config
```
```
sudo nvim /etc/sysctl.d/99-custom.conf
```

## isi
```
net.ipv4.ip_unprivileged_port_start = 80
```
```
sudo systemctl --system
```
```
mv docker-compose.yaml docker-compose.yaml.bck
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
echo ' ' > docker-compose.yaml
```
```
cat docker-compose.yaml
```
```
nvim docker-compose.yaml
```
```
version: "3.7"
```
## services: 
   ## db:
       ## image: mysql:5.7
       ##  restart: always
       ## networks: 
            ## - slims-net
       ## container_name: slims-db
      ##  env_file: 
          ##  - db_default.env
        ## command: --
```
        volumes:
```
```
            - "./dbdata:/var/lib/mysql"
```
```
        ports:
```
```
            - "127.0.0.1:3306:3306"
```
```
redis:
```
```
        image: redis:latest
```
```
        restart: always
```
```
        networks: 
```
```
            - slims-net
```
```
        container_name: redis
```
```
        ports:
```
```
            - "127.0.0.1:6379:6379"
```
```
app01:
```
        image: slimsofficial/slims:latest
```
        restart: always
```
        networks: 
```
            - slims-net
```
        container_name: slims-app
```
        ports:
```
            - "80:80"
```
            - "443:443"
```
        volumes:
```
            - "./app:/var/www/html"
```
            - "./conf/php/php.ini:/usr/local/etc/php/conf.d/php.ini"
```
networks: 
```
slims-net:
```
        name: slims-net
```
        ipam:
```
            driver: default
```

```
sudo chmod -R 777 app/slims
```
```

running container
```
```
podman compose up -d
```
```
nvim ~/.config/celadon/storage.conf
```
tambahkan:
[storage]
driver = "overlay"
[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"

##regis podman
```
sudo nvim /etc/containers/registries.conf
```
```
podman compose up -d
```
```
podman ps -a (kalo statusnya up berarti berhasil)
```

##angka2 ip itu otomatis kalo di podman
```
ip a (dicek ip servernya), copy, cek ke browser firefox
```
```
http://(ip server)
```

## kalo blm bisa kebuka, ke terminal ssh lagi
```
sudo frewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
angka 80 diambil dari port containernya slims
```
```
sudo frewall-cmd --reload
```
```
sudo frewall-cmd --info-zone=public
```

## ke browser firefox
```
http://(ip server):80
```

## netmask/cidr
10.10.3.76/24
10.10.3.2/25 (beda subnet)

## setup network

## install metwrok manager di admin
```
sudo pacman -S networkmanager network-manager-applet
```
```
remove iwd= systemctl stop iwd
```
```
pacman -R iwd
```
```
systemctl enable NetworkManager
```
```
systemctl start NetworkManager
```
```
systemctl status NetworkManager
```
```
usermod -aG wheel username
```
```
useradd -m operator
```
```
usermod -aG wheel operator
```
```
sudo passwd operator
```
```
reboot
```

## cek interface
```
nmcli device status
```

## create admin user
```
nmcli connection add type ethernet ifname [interface] con-name "admin-connection" ipv4.method manual ipv4.addresses [ip address for admin]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

## create operator
```
nmcli connection add type ethernet ifname [interface] con-name "operator-connection" ipv4.method manual ipv4.addresses [ip address for operator]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

## automated login script
```
echo "nmcli connection up admin-connection" >> /home/admin/.bash_profile
```

## automated login
```
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```
## log out, ip a di terminal operator, kalau sudah sama berarti berhasil



## ke ssh server
```
ssh [namaserver dan ip a]
```
```
create static ip
```
```
nvim /etc/system/network/20-ethernet.network
```
```
sudo su
```
```
sudo systemctl restart systemd-networkd
```
```
ping [ipv4.dns 8.8.8.8]
```

## install package
```
sudo pacman -S hostpad
```

## cek interface
```
ip link (untuk ngecek interface wirelessnya)
```
```
nvim /etc/hostapd/hostapd.conf
```
```
interface=wlan0
```
```
driver=nl80211
```
```
ssid=(nama wifi)
```
```
hw_mode=g
```
```
channel=7
```
```
auth_algs=1
```
```
wpa=2
```
```
wpa_passphrase= (bebas)
```
```
wpa_key_mgmt=WPA-PSK
```
```
wpa_pairwise=TKIP
```
```
rsn_pairwise=CCMP
```
```
nvim /etc/system/network/02-wireless-ap.network
```
```
systemctl restart 
```
```
sudo systemctl enable --now systemd-metworkd
```
```

sudo nvim /etc/sysctl.d/30-ipforward.conf
```
```
net.ipv4.ip_forward=1
```
```
sudo sysctl --system
```
```
sudo systemctl enable --now hostapd
```
```
## reboot

## setup firewall

## zona admin
```
firewall-cmd --permanent --new-zone=admin
```
```
firewall-cmd --permanent --zone=admin --add-source=10.10.10.10
```

## zona operator
```
firewall-cmd --permanent --new-zone=operator
```
```
firewall-cmd --permanent --zone=operator --add-source=13.25.168.55
```

## zona wifi-client

## buat zona
```
firewall-cmd --permanent --new-zone=wifi (wlan0)
```
```
firewall-cmd --permanent --zone-wifi --add-source=12.12.12.0/24 
```

## keluar dulu
## exit
``
podman ps
```
```
cd ~/.config/celadon/compose/
```
```
podman compose up -d
```
```
podman ps
```

## admin
```
firewall-cmd --permanent --zone=admin --add-service=ssh
```
```
firewall-cmd --permanent --zone=admin --add-port=3306/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=6379/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=admin --add-port=443/tcp
```

## operator
```
firewall-cmd --permanent --zone=operator --add-service=ssh
```
```
firewall-cmd --permanent --zone=operator --add-port=6379/tcp
```
```
firewall-cmd --permanent --zone=operator --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=operator --add-port=443/tcp
```

## wifi client
## hanya public
```
firewall-cmd --permanent --zone=wifi --add-port=80/tcp
```
```
firewall-cmd --permanent --zone=wifi --add-port=443/tcp
```

## default drop
## setelah semuanya benar:
```
firewall-cmd --set-default-zone=drop --permanent
```
```
firewall-cmd --reload
```


## ke wifi, advance options
## Pengaturan IP=
## Alamat IP= 12.12.12.2
## Router (gateway)= 12.12.12.2
## Panjang Awalan (CIDR)= 24
## DNS (PING)=8.8.8.8

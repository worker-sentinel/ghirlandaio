# PANDUAN INSTALASI SERVER #
### Recording Asciinema 
```
asciinema rec [bebas dah].cast 
```
### Membagi Partisi 
Untuk pembagian partisi disesuaikan dengan aplikasi yang akan di install

Cek partisi: 
```
lsblk 
```

```
cfdisk /dev/[sesuai jenis partisi]  
```

Partisi Boot : 3G (EFI System)
Partisi LVM : sisanya (Linux File System)

Cek partisi: 
```
lsblk 
```

### Menghubungkan WIFI 

```
iwctl
device list 
station wlan0 get-networks
station wlan0 connect [nama wifi]
```
### 
```
cryptsetup luksFormat /dev/[sesuai dengan partisi yang sudah di buat untuk system] 
```
```
cryptsetup luksOpen /dev/[sesuai dengan partisi yang sudah di luksformat] orion 
```
### Membuat Physical Volume 
```
pvcreate /dev/mapper/orion 
```
### Membuat Volume Grup 
```
vgcreate belt /dev/mapper/orion 
```
### Membuat Logical Volume 
bisa disesuaikan dengan kebutuhan
```
lvcreate -L 8G belt -n root
lvcreate -L 5G belt -n vars
lvcreate -L 1G belt -n vlog
lvcreate -L 1G belt -n vaud 
lvcreate -L 1,5G belt -n vtmp
lvcreate -l50FREE% belt -n home
lvcreate -l100FREE% belt -n podman
```
### Memformat Partisi 

Partisi root
```
mkfs.ext4 /dev/belt/root
```
Partisi boot 
```
mkfs.vfat -F32 /dev/nvme0n1p7
```
Partisi vars 
```
mkfs.ext4 /dev/belt/vars
```
Partisi vlog 
```
mkfs.ext4 /dev/belt/vlog
```
Partisi vaud 
```
mkfs.ext4 /dev/belt/vaud
```
Partisi vtmp 
```
mkfs.ext4 /dev/belt/vtmp
```
Partisi home 
```
mkfs.ext4 /dev/belt/home 
```
Partisi podman 
```
mkfs.ext4 /dev/belt/podman
```
setelah memformat, lakukan cek partisi
```
lsblk
```
### Mounting Partisi 

Partisi root 
```
mount /dev/belt/root /mnt
```

Partisi boot
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
```

Partisi vars
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt/vars /mnt/var
```

Partisi vlog
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vlog /mnt/var/log
```

Partisi vaud
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vaud /mnt/var/log/audit
```

Partisi vtmp
```
mount --mkdir -o rw,nodev,nosuid,noexec, relatime /dev/belt/vtmp /mnt/var/tmp
```

Partisi home
```
mount --mkdir -o rw,nodev,relatime /dev/belt/home /mnt/home
```

Partisi podman
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/belt/podman /mnt/var/lib/container
```

setelah mounting, lakukan cek partisi
```
lsblk
```
### Installasi Package
```
pacstrap -K /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman uget curl iwd podman asciinema 
```
### Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
### Mounting RAM
```
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab 
```
### Konfigurasi Network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
Masuk arch chroot
```
arch-chroot /mnt
```
### Membuat Hostname
```
echo "orion" > /etc/hostname
```
### Setting Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
### Generate Waktu
```
hwclock --systohc
```
### Setting Bahasa
```
nvim /etc/locale.gen
```
Search: /en_US lalu enter, 
Mode insert (i), 
Hapus # pada en_US.UTF-8 UTF-8 dan
Hapus # pada en_US ISO-8859-1
Lalu esc untuk keluar dari mode insert
:wq untuk save write and quit

Lalu generate 
```
locale-gen
```

```
locale > /etc/locale.conf
```

```
nvim /etc/locale.conf
```

### Membuat User
```
useradd -m server
passwd server
```
Memberikan hak akses sudo 
```
echo "server ALL=(ALL:ALL) ALL" > /etc/sudoers.d/server
```
Cek uji coba user
```
su server
```
Cek hak akses sudo
```
sudo su
```
lalu exit.

### Setup Kernel Parameter
```
mkdir /etc/cmdline.d
```
untuk file yang di dalam { } bisa bebas
```
touch /etc/cmdline.d/{satu-boot.conf,dua-misc.conf}
```

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p8)=orion root=/dev/belt/root" > /etc/cmdline.d/satu-boot.conf
```

```
cat /etc/cmdline.d/satu-boot.conf
```

```
lsblk
```

```
echo "rw" > /etc/cmdline.d/dua-misc.conf 
```

### Initramfs
```
nvim /etc/mkinitcpio.conf
```
Masuk mode insert, lalu cari HOOKS tanpa #
Lalu menambahkan sd-encrypt lvm2 setelah sd-vconsole
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
Masuk mode insert, lalu hapus # pada
ALL_config
ALL_kerneldesh
ALL_default uki (menghapus /efi diganti menjadi /boot)
Menambah # pada
ALL_default image

### Generate file boot
```
bootctl --path=/boot install
```
### Generate initram
```
mkinitcpio -P
```
### Aktivasi

```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
```

```
systemctl enable iwd
```

```
systemctl enable firewalld
```

```
systemctl enable sshd
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

Memperbaiki iwd:
```
sudo nvim /etc/resolve.conf
```
Masuk ke mode insert (i), lalu tambahkan:
name server 8.8.8.8
name server 1.1.1.1
Lalu esc, simpan hasil perubahan dengan :wq

Restart systemd networkd resolved
```
sudo systemctl restart systemd-networkd
```

```
sudo systemctl restart systemd-resolved
```

Jika masih belum bisa, setup iwd
```
sudo nvim /var/lib/iwd/main.conf
```
masuk mode insert (i):
tambahkan 
[General]
EnableNetworkConfiguration=true
lalu esc, simpan hasil perubahan dengan :wq

lalu install asciinema
```
sudo pacman -S asciinema
```

### Recording Asciinema
```
asciinema rec [bebas dah].cast
```

### Setup Firewalld

Cek status
```
sudo systemctl status firewalld
```

Untuk memeriksa setiap zona firewall
```
sudo firewall-cmd --info-zone-drop
```

```
sudo firewall-cmd --info-zone-block
```

```
sudo firewall-cmd --info-zone-public
```

```
sudo firewall-cmd --info-zone-home
```

```
sudo firewall-cmd --info-zone-work
```

```
sudo firewall-cmd --info-zone-internal
```

```
sudo firewall-cmd --info-zone-external
```

```
sudo firewall-cmd --info-zone-trusted
```

```
sudo firewall-cmd --info-zone-dmz
```

Menghapus service yang tidak diperlukan dalam zona (secara satu persatu)
```
sudo firewall-cmd --zone=[nama zona] --remove-service=nama service  --permanent
```

Menghapus service yang tidak diperlukan dalam zona (sekaligus)
```
sudo firewall-cmd --zone=[nama zona] --remove-service={nama service}  --permanent
```

Jika melakukan perubahan pada informasi zona, perlu dilakukan reload seperti berikut ini
```
sudo firewall-cmd --reload
```

Untuk memastikan kembali, terkait informasi baru yang dirubah sudah terbaca atau belum
```
sudo firewall-cmd --info-zone-[nama zona]
```

### Disable Module Kernel

Verifikasi keberadaan module kernel 
```
sudo lsmod | grep cramfs
```

```
sudo lsmod | grep freevxfs
```

```
sudo lsmod | grep hfs
```

```
sudo lsmod | grep hfsplus
```

```
sudo lsmod | grep jffs2
```

```
sudo lsmod | grep overlayfs
```

```
sudo lsmod | grep squashfs
```

```
sudo lsmod | grep udf
```

```
sudo lsmod | grep usb-storage
```

Cara memblokir module 
```
sudo nvim /etc/modprobe.d/disable-module.conf
```

Mode insert (i), lalu ketik:
install usb-storage /bin/false
blacklist usb-storage

Verifikasi filesystem harus disable semua:
```
sudo lsmod | grep afs
```

```
sudo lsmod | grep ceph
```

```
sudo lsmod | grep cifs
```

```
sudo lsmod | grep exfat
```

```
sudo lsmod | grep fat
```

```
sudo lsmod | grep fscache
```

```
sudo lsmod | grep fuse
```

```
sudo lsmod | grep gfs2
```

```
sudo lsmod | grep nfs_common
```

```
sudo lsmod | grep nfsd
```

```
sudo lsmod | grep smbfs_common
```

Cara memblokir
```
sudo nvim /etc/modprobe.d/disable-module.conf
```

Mode insert (i), lalu ketik:
install [nama filesystem] /bin/false
blacklist [nama filesystem]

```
sudo mkinitcpio -P
```

# PANDUAN PENYESUAIAN OS ADMIN DARI OS KELOMPOK 9 #
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
Dns: 1.1.1.1   8.8.8.8.8
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

# Link MD, Asciinema, dan Audio #
https://drive.google.com/drive/folders/1SdQ74VZQ1J7ki_KiKKteuLpiNlcic4ba

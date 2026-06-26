## Disable Kernel
### Set Firewall
### Cek zona yang ada
```
systemctl enable firewalld
systemctl start firewalld
firewall-cmd --list-all-zone
```

#### Masuk ke zona spesifik
```
firewall-cmd --info-zone=(zona yang ingin dituju)
```
```
firewall-cmd --info-zone=public
```
#### Hapus service yang tidak terpakai
```
firewall-cmd --zone=(zona yang dituju ) -- remove-service=(service yang ingin dihapus) --permanent
```
```
firewall-cmd --zone=public --remove-service=dhcpd6-client --permanent
```
Disini kelompok kami akan menghapus semua service yang ada selain ssh
#### Cek apakah service sudah terhapus
```
firewall-cmd --info-zone=(zona yang ingin dituju)
```
```
firewall-cmd --info-zone=public
```
Jika masih belum muncul, maka ketik
```
firewall-cmd --reload
```

### Setup Module Kernel
Nonaktifkan module kernel yang tidak terpakai untuk memperkecil celah keamanan
##### Buat file konfigurasi
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
Isi dengan
```
install usb-storage /bin/false
blacklist usb-storage
```
Lalu
```
sudo modprobe -r usb-storage
```
Lalu rebuild initramfs
```
mkinitcpio -P
```

## Dijalankan di cluster server
## Aplikasi
### Setup Rootless Podman
Verifikasi bahwa ada user dengan subuid 100000:65536
```
nvim /etc/subuid
```
Verifikasi grup
```
nvim /etc/subguid
```

#### Beri izin pada podman
```
systemctl enable --global podman
```

#### Buat konfigurasi storage untuk container
Membuat direktori  untuk folder dalam folder yang hanya ada di user
```
mkdir -p .config/containers
```
Lakukan verifikasi
```
ls -la
```
```
ls.config
```
Membuat file konfigurasi storage
```
nvim .config/containers/storage.conf
```
Lalu isi dengan:
```
[storage]
driver=overlay

[storage.option.overlay]
mount_program
mountopt="userxattr"
```

#### Menambahkan registry
```
sudo nvim /etc/containers/registries
```
scroll ke bawah dan buat line baru, isi dengan
```
unqualified-search-registries = ["docker.io"]
```
Cek status podman
```
systemctl status podman
```
```
systemcytl start podman
```
Reboot

### Instalasi SLiMS di cluster server yang diremote oleh admin
```
ssh [namauserserver]@[ip address server]
```
Setelah berhasil menggunakan ssh, bisa langsung install podman compose
untuk menjalankan container dalam satu file.
```
sudo pacman -S podman-compose
```
```
cd.config/containers
```
Download link zip yang disiapkan oleh developer SLiMS
```
sudo pacman -S wget
```
```
'wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip'
```
Cek apakah master.zip sudah terinstall
```
ls
```
Unzip file
```
sudo pacman -S unzip
```
```
unzip master.zip
```
Masuk ke folder
```
cd docker-compose-for-slims-master
```

Kemudian atur port 
```
sudo nvim /etc/sysctl.d/99-custom.conf
```
Isi dengan:
```
net.ipv4.ip_unprivileged_port_start=8080
```
Lalu restart konfigurasi yang dibuat
```
sudo sysctl --system
```
```
mv docker-compose.yaml docker-compose.yaml.back
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
> Hapus tagar yang terletak di sebelum ports, hapus juga tagar di line setelahnya
> Lalu tambahkan hingga "127.0.0.1:3306:336"
> Lakukan langkah yang sama di port redis

```
sudo chmod -R 777 app/slims
podman compose up -d
...
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd reload
```
> Masuk ke firefox lalu masuk ke slims
> Masukkan ip server
>
```
http://(ip server)80
```

# Network
### Tentukan IP
> Tentukan 1 user untuk admin dan 1 user untuk operator
> Buat IP Adress untuk admin, operator, server, wifi

### Jalankan di user admin
#### Membuat koneksi untuk user admin
Mencari  interface
```
nmcli device status
```
```
sudo nmcli connection add type ethernet if name eno1 con-name "admin" ipv4.method manual ipv4.adress (ip admin)/CIDR ipv4.gateway (ip gateway) ipv4.dns 8.8.8.8
```

#### Buat koneksi untuk user operator
```
sudo nmcli connection add type ethernet if name eno1 con-name "operator" ipv4.method manual ipv4.adress (ip operator)/CIDR ipv4.gateway (ip gateway) ipv4.dns 8.8.8.8
```
#### Jalankan pada user admin
```
sudo su
```
```
sudo echo
```
```
echo "nmcli connection up (nama koneksi yang dibuat)" >> home/(nama user operator)/.bash_profile
```
```
echo "nmcli connection up (nama koneksi yang dibuat)" >> /home/(nama user admin)/.bash_profile
```
```
cat /home/(nama user)/.bash
```

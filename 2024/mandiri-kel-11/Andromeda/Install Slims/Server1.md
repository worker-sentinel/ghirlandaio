# Menginstall SLiMS Server 1

## Mengaktifkan rekaman asciinema
```
asciinema record (nama file).cast
```

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```

```
station wlan0 get-networks
```
menampilkan hasil scan berupa daftar SSID yang ditemukan

```
station wlan0 scan
```
meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar
```
station wlan0 connect (nama wifi)
```
```
station wlan0 connect "wifi sekolah"
```
jika nama wifi lebih dari 1 kata. gunakan tanda "..."
```
exit
```
## Mengecek dan Mengaktifkan SSH
```
sudo systemctl status sshd
sudo systemctl start sshd
```
## Melihat informasi sistem
```
uname -a
```
## Mengaktifkan Podman 
```
sudo systemctl enable --global podman
```
## Masuk ke direktori home
```
cd ~
```
## Konfigurasi container
```
sudo nvim /etc/containers/registries.conf
```
hapus tanda # pada bagian unqualified-search-registries = ["docker.io"]
## Konfigurasi kernel
```
sudo nvim /etc/sysctl.d/custome.conf
```
tambahkan kernel.unprivileged_userns_clone=1
lalu simpan dan keluar dengan
```
esc
:wq
```
## Instalasi Podman Compose
```
sudo pacman -S podman-compose
```
## Mengunduh Docker Compose untuk SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
jika wget belum tersedia:
```
sudo pacman -S wget
## Mengekstrak File ZIP
jika unzip belum tersedia:
```
sudo pacman -S unzip
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Mengekstrak file
```
unzip master.zip
```
## Memeriksan hasil ekstrasi
```
ls
```
## Masuk ke Direktori Docker Compose SLiMS
```
cd docker-compose-for-slims-master
```
cek untuk memastikan
```
ls
```
## Mengubah hak akses direktori
```
sudo chown -R 777 app/slims
```
## Menjalankan Container SLiMS
```
sudo podman compose up -d
```
## Memeriksa status container
```
sudo podman ps -a
```
## Menon-aktifkan Firewall
```
sudo systemctl stop firewalld
```
## Memeriksan alamat IP Server
```
ip a
```
## Mengakses installer SLiMS
```
contoh:
http://192.168.1.10:80
```

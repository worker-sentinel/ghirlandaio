# Instalasi Slims

## Login ke server 
```
sshSERVER@10.113.208.188
```
> Masukan password

## Membuat direktori project
```
mkdir slimsadmin
```
## Masuk ke direktori project
```
cd slimsadmin/
```
## Install source code docker compose SLIMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Install paket unzip
```
sudo pacman -S unzip
```
## Ekstrak file ZIP
```
unzip master.zip
```
## Ubah nama folder
```
mv docker-compose-for slims-master compose
```
## Masuk ke folder compose
```
cd compose
```
## Membuat file konfigurasi sysctl
```
sudo nvim /etc/sysctl.d/composeslims.conf
```
## Menerapkan konfigurasi sysctl
```
sudo sysctl --system
```
## Backup file compose awal
```
mv docker-compose.yaml docker-compose.yaml.slims
```
## Menggunakan konfigurasi redis
```
mv docker-compose-redis.yaml docker-compose-yaml
```
## Edit file compose
```
nvim docker-compose.yaml
```
> hapus bagian ipv4_address: 172.18.1.3 , ipv4_address: 172.18.1.4 , ipv4_addres: 172.18.1.11

> hapus simbol #

> ubah "127.0.0.1:6379:6379" menjadi "80:80"

> hapus config:

> hapus - subnet: "172.18.1.0/24"

## Ubah permission folder aplikasi
```
sudo chmod -R 777 app/slims
```
## Edit registries
```
sudo nvim /etc/containers/registries.conf
```
> hapus simbol # pada unqualified-search-registries = ["example.com"]

> kemudian ubah menjadi unqualified-search-registries = ["docker.io"]
## Edit SUbUID
```
sudo nvim /etc/subuid
```
## Edit storage podman
```
nvim storage.conf
```
> ketik [storage]
> driver = "overlay"
## Aktifkan service podman
```
sudo systemctl enable --global podman
```
## Masuk kembali ke folder compose
```
cd compose
```
## Jalankan container
```
podman compose up -d
```
## Cek status container
```
podman ps -a
```
## Lihat konfigurasi jaringan
```
ip a
```

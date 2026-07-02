# Install slims
## download mariadb versi 11 
```
podman pulldocker.io/library/mariadb;11
```
> untuk mendownload image mariadb versi 11 pake podman agar bisa dijalankan jadi containers database di komputer
## menampilkan daftar image
```
podman images
```
## mengedit file konfigurasi
```
sudo nvim /etc/containers/registries.conf
```
> masukan 
> unqualified-search-registries = ["docker.io"]
## mengedit file konfigurasi
```
sudo nvim /etc/sysctl.d/custome.conf
```
## menginstall podman compose
```
sudo pacman -S podman-compose
```
## mengunduh file installasi
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## menginstall applikasi unzip
```
sudo pacman -S unzip
```
## masuk ke dalam folder
```
cd docker-compose-for-slims
```
## mengekstrak isi file
```
unzip master.zip
```
## masuk ke folder
```
cd docker-compose-for-slims-master
```
## menjalankan aplikasi SLiMS menggunakan Docker
```
docker-compose-for-slims-master

```
> untuk menjalankan aplikasi SLiMS menggunakan Docker tanpa harus menginstal PHP, MySQL, dan web server secara manual.

## masuk ke folder sagi
```
cd sagi
```
> terminal akan berpindah ke folder sagi agar kamu bisa menjalankan perintah atau mengakses file yang ada di dalam folder tersebut.
Membuat file
```
nvim /etc/sysctl.d/sagi.conf
```
> untuk mengatur parameter kernel Linux (pengaturan sistem) yang akan diterapkan melalui sysctl.
## mengganti nama file
```
mv docker-compose.yaml docker-compose.yaml.sagi-compose 
```
> agar file docker-compose.yaml asli disimpan sebagai cadangan dan tidak digunakan secara langsung.
## mengganti nama file
```
mv docker-compose-redis.yaml docker-compose.yaml
```
## untuk melihat atau mengedit isi konfigurasi Docker Compose.
```
nvim docker-compose.yaml
```
## Agar semua orang bisa mengakses dan mengubah file di app/slims.
```
chmod -R 777 app/slims
```
## menjalankan semua container yang didefinisikan di file docker-compose.yaml menggunakan Podman.
```
podman compose up -d
```
## membuat folder containers di dalam /etc
```
mkdir -p /etc/containers
```
## Untuk Membuka atau membuat file konfigurasi registries.conf di folder /etc/containers/ menggunakan Neovim nvim
```
nvim /etc/containers/registries.conf
```
## untuk melihat daftar semua container di Podman.
```
podman ps -a
```
## menampilkan semua file
```
ls -la
```
## mwnjalankan container
```
podman compose up -d
```
## membuat file registries
```
sudo nvim /etc/containers/registries.conf
```
## menampilkan daftar container
```
podman ps
```
## membuka port pada firewall
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
## memuat ulang
```
firewall-cmd -reload
```
## mengecek kembali ip a
```
ip a
```

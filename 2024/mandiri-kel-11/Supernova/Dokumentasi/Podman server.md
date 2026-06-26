## Instalasi Podman Compose
```
sudo su
pacman -S podman-compose
```
## Membuat Direktori Kerja
```
mkdir container
cd container
```
## Membuat File Konfigurasi Storage
```
nvim storage.conf
```
## Instalasi Podman
```
pacman -S podman
```
## Membuat Direktori Konfigurasi Podman
```
mkdir -p .config/containers
```
## Mengunduh Repository Docker Compose SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Mengekstrak File ZIP
```
unzip master.zip
```
## Mengubah Nama Folder Repository
```
mv docker-compose-for-slims-master slims
```
## Masuk ke Direktori Proyek
```
cd slims
ls
```
### Output
```
README.md
app
conf
db_default.env
docker-compose.yaml
phpmyadmin.env
```
## Mengedit File Docker Compose
```
nvim docker-compose.yaml
```
## Mengatur Hak Akses Aplikasi
```
chmod -R 777 app/slims
```
## Menjalankan Container
```
podman compose up -d
```
## Struktur Docker Compose
```
Isi file docker-compose.yaml:

version: "3.7"

services:
  db:
    image: mysql:5.7
    container_name: slims-db

  app01:
    image: slimsofficial/slims:latest
    container_name: slims-app
```
## Konfigurasi Port
```
ports:
  - "80:80"

Untuk database.

ports:
  - "81:81"
  - "443:443"

Untuk aplikasi SLiMS.
```
## Konfigurasi Storage Podman
```
nvim ~/.config/containers/storage.conf
```
## Optimasi Kernel Linux
Beberapa parameter yang diterapkan:
```
net.ipv4.ip_unprivileged_port_start = 80
kernel.unprivileged_userns_clone = 1
kernel.pid_max = 4194304
```

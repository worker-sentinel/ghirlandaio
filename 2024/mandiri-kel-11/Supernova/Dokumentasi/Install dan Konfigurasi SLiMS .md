##  Instalasi Podman Compose
```
sudo su
pacman -S podman-compose
```
## Membuat Direktori Container
```
mkdir container
cd container
```
##  Membuat Konfigurasi Storage Podman
```
nvim storage.conf
```
##  Instalasi Podman
```
pacman -S podman
```
## Membuat Direktori Konfigurasi Container
```
mkdir -p .config/containers
```
## Mengunduh Repository Docker Compose SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Mengekstrak File ZIP
```
bsdtar -xzf master.zip
```
## Mengubah Nama Direktori
```
mv docker-compose-for-slims-master slims
```
## Masuk ke Direktori SLiMS
```
cd slims
```
## Mengedit File Docker Compose
```
nvim docker-compose.yaml
```
## Mengatur Hak Akses Folder Aplikasi
```
chmod -R 777 app/slims
```
## Menjalankan Container
```
podman compose up -d
```
## Optimasi Kernel Linux
Mengedit file:
```
nvim /etc/sysctl.d/99-custom.conf
```
## Mengelola Konfigurasi Podman
```
nvim ~/.config/containers/storage.conf
```
## Menghapus dan Menginstal Ulang Podman
Menghapus:
```
pacman -R podman podman-compose
```
Menginstal kembali:
```
pacman -S podman podman-compose
```
## Struktur Docker Compose yang Digunakan
```yaml
version: "3.7"

services:
  db:
    image: mysql:5.7
    container_name: slims-db

  app01:
    image: slimsofficial/slims:latest
    container_name: slims-app
```

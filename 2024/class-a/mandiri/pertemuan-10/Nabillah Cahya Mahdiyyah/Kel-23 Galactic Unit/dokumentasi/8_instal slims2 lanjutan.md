## 1.   Mendownload Git
```
sudo pacman -S git
```
## 2. Menginstall Package Manager Phyton
```
pip install git+https://github.com/containers/podman-compose.git --break-systemp-packages
```
Gunanya untuk melindungi sistem dari konflik ketergantungan
## 3.   Membuat dan Memulai semua service dalam file compose dengan satu perintah.
```
podman-compose up -d
```
## 4. Mengecek kembali isi folder secara keseluruhan
```
ls -la
```
## 5. Mengunduh file Slims dari github
```
mkdir slims
cd slims
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## 6. Membuka/mengekstra isi file yang sebelumnya sudah diunduh
```
unzip master.zip
```

```
mv docker-compose-for-slims-master compose
cd compose
sudo nvim /etc/sysctl.d/99-slims.conf
```
```
net.ipv4.ip_unprivileged_port_start=80
```
## 7. Menjalankan konfigurasi kernel semua direktori bersamaan
```
sudo sysctl --system
cat /etc/containers/registries.conf
```
## 8. Mendownload MYSQL 5.7 menggunakan podman
```
podman pull docker.io/mysql:5.7
podman pull docker.io/redis:latest
podman pull docker.io/slimsofficial/slims:lates
podman compose up -d
```
## 9. Mengecek kembali container, firewall dan ip
```
podman ps -a
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
ip a
```
Pastikan terdapat beberapa ip yang nantinya akan digunakan untuk client mengecek kembali
## 10. Keluar sesi
```
exit
```

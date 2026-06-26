# Install Aplikasi (SLiMS)

## Install Compose
```
sudo pacman -S podman-compose
```
## Bikin folder untuk nyimpen file compose
```
mkdir (nama untuk folder)
```
## Cara ke foldernya
```
cd (nama folder yang udah dibuat)
```
## Download SLiMS
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Kalo ga ada wget, install dulu wget nya
```
sudo pacman -S wget unzip
```
## Pas udah install wgetnya, baru balik ke command buat install SLiMS
## Extract Zip
```
unzip master.zip
```
## Cek list folder
```
ls
```
## Hasil yang udah diunzip 
```
docker-compose-for-slims-master
```
## Untuk Rename
```
mv docker-compose-for-slims-master [nama rename-nya] (misal: compose)
```
## Cek apakah rename nya berhasil/ tidak
```
ls
```
## Masuk ke folder yang udah di rename
```
cd (nama folder yang udah di rename)
```
# Set Kernel 
## Butuh permission untuk kernel 
```
sudo nvim /etc/sysctl.d/[nama file].conf
```
## Masuk mode insert
```
net.ipv4.ip_unprivileged_port_start=80
```
```
sudo sysctl --system
```
## Pastiin ada command yang tadi ditulis waktu mode insert
```
ls
```
## Rename
```
mv docker-compose.yaml docker-compose.yaml.[nama rename nya]

```
## Rename lagi
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
## Bikin hak akses buat folder dan file yang ada di dalam folder
```
sudo chmod -R 777 app/slims
```
## Running Compose (tanpa sudo)
```
podman compose up -d
```
```
sudo nvim /etc/containers/registries.conf

```
## Cek subuid
```
sudo nvim /etc/subuid
```
## Cek subgid
```
sudo nvim /etc/subgid
```
## Keluar dari folder
```
cd ..
```
## Bikin storage simpanan
```
nvim storage.conf
```
```
sudo systemctl enable --global podman
```
```
cd  [nama direktori]
```
## Running Podman Compose
```
podman compose up -d
```
## Cek Containers
```
podman ps -a
```
## Masukin port SLiMS 
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
sudo firewall-cmd reload
```
## Cek IP Server (Inet)
```
ip a
```
## Buka browser buat cek apakah SLiMS udah masuk atau belum, masukin IP Server
``` 
http://[IP Server]: 80
```

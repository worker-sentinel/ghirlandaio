# Installasi SLiMS
## Environment Podman
``` 
sudo pacman -S podman-compose
```
```
mkdir <folder_name>
```
```
cd <folder_name
```
## Download & Ekstrak SLiMS
```
sudo pacman -S wget unzip
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
unzip master.zip
```
```
ls
```
```
mv docker-compose-for-slims-master <rename>
```
```
cd <folder_rename>
```
## Konfigurasi Sistem (Port & Hak Akses)
```
sudo nvim /etc/sysctl.d/<file_name>.conf
```
```
net.ipv4.ip_unprivileged_port_start=80
```
```
sudo sysctl --system
```
```
ls
```
```
mv docker-compose.yaml docker-compose.yaml.<name>
```
```
nvim docker-compose.yaml
```
```
sudo chmod -R 777 app/slims
```
## Konfigurasi Podman
```
sudo nvim /etc/containers/registries.conf
```
```
sudo nvim /etc/subuid
```
```
sudo nvim /etc/subgid
```
```
cd ..
```
```
nvim storage.conf
```
```
sudo systemctl enable --global podman
```
## Menjalankan kontainer & Akses SLiMS
```
cd <directory_name>
```
```
podman compose up -d
```
```
podman ps -a
```
```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
```
sudo firewall-cmd --reload
```
```
ip a
```
```
http://<Server_ip>:80
```

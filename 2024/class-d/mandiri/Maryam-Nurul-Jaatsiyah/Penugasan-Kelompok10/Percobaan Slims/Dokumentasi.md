# Slims
## Melihat IP address 
```
ip a
```
## Login ke server melalui ssh
```
ssh sagitarius@192.168.1.30
```
## menginstal OpenSSH
```
sudo pacman -S openssh
```
## Menjalankan SSH
```
sudo systemctl start sshd
```
## Mengaktifkan SSH
```
sudo systemctl enable sshd
```
## Login melalui SSH
```
ssh sagitarius@192.168.1.30
```
## Menginstal Podman Compose
```
sudo pacman -S podman-compose
```
## Masuk ke direktori konfigurasi
```
 cd .config/containers
```
## Menginstal Wget
```
 sudo pacman -S wget
```
## Download Slims
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Cek isi folder
```
ls
```
## Menginstal unzip
```
sudo pacman -S unzip
```
## Mengekstrak file zip
```
unzip master.zip
```
## mengecek isi folder
```
ls
```
## masuk ke folder
```
cd docker-compose-for-slims-master/
```
## mengecek isi 
```
ls
```
## konfigurasi kernel
```
sudo nvim /etc/sysctl.d/99-custom.conf
```
>Masukkan password

>Insert
```
net.ipv4.ip_unpriviliged_port_start=80
```
## Menerapkan konfigurasi
```
sudo sysctl --system
```
>Masukkan password
## Membuat cadangan file compose
```
mv docker-compose.yaml docker-compose.yaml.back
```
## Konfigurasi redis
```
mv docker-compose-redis.yaml docker-compose.yaml
```
## Mengedit file compose
```
nvim docker-compose.yaml
```
##  mengubah hak akses 
```
sudo chmod -R 777 app/slims
```
>Masukkan password
## Menjalankan container
```
podman compose up -d
```
## Logout
```
logout
```
```
exit
```
